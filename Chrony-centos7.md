# Cài đặt chrony trên CentOS 7
### 1. Mô hình
![Imgur](https://i.imgur.com/lHsK5Gb.png)

Sử dụng 2 server cho mô hình
 - CentOS 7
 - Có kết nối internet
 - Đăng nhập thao tác với user `root`

### 2. Chuẩn bị trước khi cài đặt
Cấu hình time zone
 
 `timedatectl set-timezone Asia/Ho_Chi_Minh`

Kiểm tra timezone sau khi cài đặt

 `timedatectl`
 ```
    [root@localhost ~]# timedatectl set-timezone Asia/Ho_Chi_Minh
[root@localhost ~]# timedatectl
      Local time: Thu 2020-05-21 13:37:10 +07
  Universal time: Thu 2020-05-21 06:37:10 UTC
        RTC time: Thu 2020-05-21 06:37:10
       Time zone: Asia/Ho_Chi_Minh (+07, +0700)
     NTP enabled: yes
NTP synchronized: yes
 RTC in local TZ: no
      DST active: n/a

 ```

Cấu hình allow Firewalld

 ```
 firewall-cmd --add-service=ntp --permanent
 firewall-cmd --reload
 ```

Cấu hình disable SElinux
 
 ```
 sudo setenforce 0
 sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
 sed -i 's/SELINUX=permissive/SELINUX=disabled/g' /etc/sysconfig/selinux
 ```

### 3. Cài đặt Chrony trên cả 2 Server
Đối với CentOS 7 mặc dù trình đồng bộ và quản lý thời gian hiện tại mặc định là Chrony thay vì NTPD nhưng đa số Chrony lại không được tích hợp sẵn trong hệ điều hành lúc chúng ta cài đặt

Truy cập SSH vào cả 2 Server. Sau đó chúng ta tiến hành cài đặt Chrony

  `yum install -y chrony`

Sau khi cài dặt chúng ta tiến hành start Chrony và cho phép khởi động cùng hệ thống
 
  `systemctl enable --now chronyd`

Kiểm tra dịch vụ đang hoạt động
  
  `systemctl status chronyd`

Mặc định trên Centos 7 file cấu hình của Chrony nằm ở `/etc/chrony.conf`, tiến hành kiểm tra file cấu hình

 `cat /etc/chrony.conf | egrep -v '^$|^#'`

 ```
 [root@serverntp ~]# cat /etc/chrony.conf | egrep -v '^$|^#'
server 0.centos.pool.ntp.org iburst
server 1.centos.pool.ntp.org iburst
server 2.centos.pool.ntp.org iburst
server 3.centos.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
 ```

Trong đó :
 * `server` Xác đinh các NTP Server bạn muốn sử dụng
 * `prefer` Đối với nhiều NTP Server chúng ta có thể chỉ định ưu tiên kết nối từ NTP Server nà trước thay vì để hệ thống tự lựa chọn VD `server 2.centos.pool.ntp.orf iburst prefer`
 * `driftfile` File lưu trữ tốc độ mà đồng hồ hệ thống tăng/giảm thời gian
 * `makestep` Cho phép đồng hồ hệ thống không cập nhật trong 3 bản cập nhật đầu tiên nếu độ lệch của nó lớn hơn 1s
 * `rycsync` Cho phép đồng bộ hóa kernel của đồng hồ thời gian thực (RTC)
 * `logdir` Vị trí file log

Các cấu hình bổ sung:
 * `hwtimestamp` Cho phép đánh dấu thời gian phần cứng trên tất cả các interface hỗ trợ nó
 * `minsources` Tăng số lượng tối thiểu các nguồn có thể chọn cần thiết để điều chỉnh đồng hồ hệ thống
 * `allow` Cho phép Client truy cập NTP từ mạng cục bộ
 * `keyfile` Tệp có chứa mật khẩu để xác thực kết nối giữa Client và Server cho phép chronyc đăng nhập vào chronyd và thông báo cho chronyd về sự hiện diện của liên kết với Internet
 * `generatecommandkey` Tạo mật khẩu ngẫu nhiên tự động khi bắt đầu chronyd đầu tiên.VD: `keyfile /etc/chrony.keys genetarecommandkey`
 * `log` Log file

### 4. Cấu hình Chrony làm NTP Server
Chrony cho phép chúng ta cấu hình Server thành 1 NTP Server. Việc này phù hợp cho các mô hình mạng LAN có nhiều máy kết nối. Thay vì phải tốn thời gian đồng bộ từ Internet thì chúng ta có thể cấu hình 1 Server làm NTP Server Local và các máy còn lại sẽ đồng bộ thời gian từ NTP Server này về

Tại Server 192.168.213.153 là Server sẽ làm NTP server. Chúng ta sẽ cấu hình bổ sung cấu hình cho phép các máy Client 192.168.213.173 phía trong có thể đồng bộ thời gian từ Server này

 `sed -i 's|#allow 192.168.0.0/16|allow 192.168.213.0/24|g' /etc/chrony.conf`

Trong đó 192.168.213.0/24 chính là dải IP Local mà chúng ta cho phép các Client kết nối vào NTP Server này để đồng bộ thời gian

Kiểm tra lại file cấu hình
 ```
 [root@serverntp ~]# cat /etc/chrony.conf | egrep -v '^$|^#'
server 0.centos.pool.ntp.org iburst
server 1.centos.pool.ntp.org iburst
server 2.centos.pool.ntp.org iburst
server 3.centos.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
allow 192.168.213.0/24
logdir /var/log/chrony
```

Restart lại dịch vụ để cập nhật cáu hình

 `systemctl restart chronyd`

Sử dụng `chronyc` để kiểm tra đồng bộ

```
[root@localhost ~]# chronyc sources -v
210 Number of sources = 4

  .-- Source mode  '^' = server, '=' = peer, '#' = local clock.
 / .- Source state '*' = current synced, '+' = combined , '-' = not combined,
| /   '?' = unreachable, 'x' = time may be in error, '~' = time too variable.
||                                                 .- xxxx [ yyyy ] +/- zzzz
||      Reachability register (octal) -.           |  xxxx = adjusted offset,
||      Log2(Polling interval) --.      |          |  yyyy = measured offset,
||                                \     |          |  zzzz = estimated error.
||                                 |    |           \
MS Name/IP address         Stratum Poll Reach LastRx Last sample                                                                                                                   
=============================================================================                                                                                                      ==
^* time.cloudflare.com           3   6    17     8    -25ms[  -16ms] +/-  139                                                                                                      ms
^- 101-210-1-103.vtx.zinnia>     3   6    17     8    -32ms[  -32ms] +/-  150                                                                                                      ms
^- ntp18.doctor.com              2   6    17     7   +138ms[ +138ms] +/-  381                                                                                                      ms
^? static.vnpt.vn                0   6     0     -     +0ns[   +0ns] +/-    0                                                                                                      ns
```

Kiểm tra đồng bộ sử dụng `timedatectl`

```
[root@localhost ~]# timedatectl
      Local time: Thu 2020-05-21 13:51:25 +07
  Universal time: Thu 2020-05-21 06:51:25 UTC
        RTC time: Thu 2020-05-21 06:51:25
       Time zone: Asia/Ho_Chi_Minh (+07, +0700)
     NTP enabled: yes
NTP synchronized: yes
 RTC in local TZ: no
      DST active: n/a
```

Set đồng bộ thời gian cho đồng hồ của BIOS (Đồng hồ phần cứng) `hwclock`

`hwclock --systohc`

Kiểm tra đồng bộ của `date` và `hwclock` đảm bảo đồng bộ

```
[root@localhost ~]# date
Thu May 21 13:54:37 +07 2020
[root@localhost ~]# hwclock
Thu 21 May 2020 01:54:42 PM +07  -0.773054 seconds
[root@localhost ~]#
```

### 5. Cấu hình Chrony Client
Thực chất sau khi cài đặt và khởi động Chrony thì Server này đã tự động đồng bộ thời gian về từ một trong những NTP Server thuocj pool `centos.pool.ntp.org`

Bây giờ thay vì đồng bộ thời gian từ Internet chúng ta sẽ đồng bộ từ NTP Server chúng ta cấu hình phía trên

Tại Server 192.168.213.175 chỉnh sửa cấu hình chrony

```
[root@localhost ~]# sed -i 's|server 0.centos.pool.ntp.org iburst|server 192.168.213.175 iburst|g' /etc/chrony.conf
[root@localhost ~]# sed -i 's|server 1.centos.pool.ntp.org iburst|#|g' /etc/chrony.conf
[root@localhost ~]# sed -i 's|server 2.centos.pool.ntp.org iburst|#|g' /etc/chrony.conf
[root@localhost ~]# sed -i 's|server 3.centos.pool.ntp.org iburst|#|g' /etc/chrony.conf
```

Kiểm tra cấu hình

```
[root@localhost ~]# cat /etc/chrony.conf | egrep -v '^$|^#'
server 192.168.213.175 iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
```

Khởi động lại chrony để cập nhật cấu hình

 `systemctl restart chronyd`

Sử dụng `chronyc` kiểm tra đồng bộ

```
[root@localhost ~]# chronyc sources -v
210 Number of sources = 1

  .-- Source mode  '^' = server, '=' = peer, '#' = local clock.
 / .- Source state '*' = current synced, '+' = combined , '-' = not combined,
| /   '?' = unreachable, 'x' = time may be in error, '~' = time too variable.
||                                                 .- xxxx [ yyyy ] +/- zzzz
||      Reachability register (octal) -.           |  xxxx = adjusted offset,
||      Log2(Polling interval) --.      |          |  yyyy = measured offset,
||                                \     |          |  zzzz = estimated error.
||                                 |    |           \
MS Name/IP address         Stratum Poll Reach LastRx Last sample             
===============================================================================
^* 192.168.213.175               4   6    17    31    +15us[  +12us] +/-   61ms
```

Kiểm tra đồng bộ sử dụng `timedatectl`

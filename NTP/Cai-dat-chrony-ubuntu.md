## Cài đặt Chrony trên Ubuntu 20.04
### 1. Mô hình chuẩn bị
![Imgur](https://i.imgur.com/dfxwDHD.png)

![Imgur](https://i.imgur.com/5Nzobw1.png)

Sử dụng 2 server cho mô hình:
 - Có kết nối internet
 - Đăng nhập với thao tác với user `root` hoặc có quyền quản trị `sudo`

### 2. Chuẩn bị trước khi cài đặt
Cấu hình timezone

 `timedatectl set-timezone Asia/Ho_Chi_Minh`

Kiểm tra timezone sau khi cài đặt 

 `timedatectl`

```
root@nam:~# timedatectl
               Local time: Sat 2020-05-23 14:14:19 +07
           Universal time: Sat 2020-05-23 07:14:19 UTC
                 RTC time: Sat 2020-05-23 07:14:19
                Time zone: Asia/Ho_Chi_Minh (+07, +0700)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```

Mặc định trên Ubuntu 20.04 thì trình đồng bộ thời gian `timesync` đã thiết lpaaj sẵn tỏng OS từ lúc cài đặt. Nó hoạt động 

Tiến hành stop dịch vụ `timesync`

 `timedatectl set-ntp no`

Kiểm tra bằng `timedatectl` lại chắc chắn `timesync` đã tắt

Cấu hình firewall `ufw`

`sudo ufw allow ntp`

### 3. Cài đặt Chrony trên cả 2 server
Truy cập SSH vào cả 2 Server. Sau đó chúng ta tiến hành cài đặt Chrony

 `apt-get install -y chrony`

Sau khi cài đặt chúng ta tiến hành start Chrony và cho phép khởi động cùng hệ thống

 `systemctl enable --now chrony`

Kiểm tra dịch vụ đang hoạt động

 `systemctl status chrony`

Kết quả: 

```
root@nam:~# systemctl status chrony
● chrony.service - chrony, an NTP client/server
     Loaded: loaded (/lib/systemd/system/chrony.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2020-05-23 14:27:07 +07; 17s ago
       Docs: man:chronyd(8)
             man:chronyc(1)
             man:chrony.conf(5)
   Main PID: 3430 (chronyd)
      Tasks: 2 (limit: 2249)
     Memory: 1.3M
     CGroup: /system.slice/chrony.service
             ├─3430 /usr/sbin/chronyd -F -1
             └─3431 /usr/sbin/chronyd -F -1

May 23 14:27:07 nam systemd[1]: Starting chrony, an NTP client/server...
May 23 14:27:07 nam chronyd[3430]: chronyd version 3.5 starting (+CMDMON +NTP +REFCLOCK +RTC
May 23 14:27:07 nam chronyd[3430]: Initial frequency -4.026 ppm
May 23 14:27:07 nam chronyd[3430]: Loaded seccomp filter
May 23 14:27:07 nam systemd[1]: Started chrony, an NTP client/server.
May 23 14:27:15 nam chronyd[3430]: Selected source 103.97.125.133
May 23 14:27:18 nam chronyd[3430]: Source 113.161.227.40 replaced with 162.159.200.1

```

Mặc định trên Ubuntu file cấu hình của Chrony nằm ở `/etc/chrony/chrony.conf`, tiến hành kiểm tra file cấu hình

 `cat /etc/chrony/chrony.conf | egrep -v '^$|^#'`

```
root@nam:~# cat /etc/chrony/chrony.conf | egrep -v '^$|^#'
pool ntp.ubuntu.com        iburst maxsources 4
pool 0.ubuntu.pool.ntp.org iburst maxsources 1
pool 1.ubuntu.pool.ntp.org iburst maxsources 1
pool 2.ubuntu.pool.ntp.org iburst maxsources 2
keyfile /etc/chrony/chrony.keys
driftfile /var/lib/chrony/chrony.drift
logdir /var/log/chrony
maxupdateskew 100.0
rtcsync
makestep 1 3
```

### 4. Cấu hình Chrony làm NTP Server
Chúng ta sẽ cấu hình Chrony thành 1 Server làm NTP Server Local và các máy còn lại sẽ đồng bộ thời gian từ NTP Server này về

Để lựa chọn pool đồng bộ thời gian chúng ta có thể truy cập vào **NTP Pool** để lựa chọn NTP Server. Ở đây chúng ta giữ nguyên đồng bộ từ `ntp.ubuntu.com`

Tại Server 192.168.11.143 là Server sẽ làm NTP Server. Chúng ta sẽ cấu hình bổ sung cấu hình cho phép các máy Client 192.168.11.0/24 phía trong có thể đồng bộ thời gian từ Server này

 `echo "allow 192.168.11.0/24 >> /etc/chrony/chrony.conf`

  * Trong đó `192.168.11.0/24` chính là dải IP Local mà chúng ta cho phép các Client kết nối vào NTP Server này để đồng bộ thời gian

Kiểm tra lại file cấu hình
```
root@nam:~# cat /etc/chrony/chrony.conf | egrep -v '^$|^#'
pool ntp.ubuntu.com        iburst maxsources 4
pool 0.ubuntu.pool.ntp.org iburst maxsources 1
pool 1.ubuntu.pool.ntp.org iburst maxsources 1
pool 2.ubuntu.pool.ntp.org iburst maxsources 2
keyfile /etc/chrony/chrony.keys
driftfile /var/lib/chrony/chrony.drift
logdir /var/log/chrony
maxupdateskew 100.0
rtcsync
makestep 1 3
allow 192.168.11.0/24
```

Restart lại dịch vụ để cập nhật cấu hình

 `systemctl restart chrony`

Sử dụng `chrony` để kiểm tra đồng bộ

 `chronyc sources -v`

Kiểm tra đồng bộ sử dụng `timedatectl`

```
Last login: Mon May 25 08:09:12 2020
namdac@server:~$ timedatectl
               Local time: Mon 2020-05-25 08:10:35 +07
           Universal time: Mon 2020-05-25 01:10:35 UTC
                 RTC time: Mon 2020-05-25 01:10:35
                Time zone: Asia/Ho_Chi_Minh (+07, +0700)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```

Set đồng bộ thời gian cho đồng hồ của BIOS (đồng hồ phần cứng) `hwclock`
 
 `hwclock --systohc`

Kiểm tra đồng bộ của `date` và `hwclock` đảm bảo đồng bộ

```
root@server:~# hwclock --systohc
root@server:~# date
Mon 25 May 2020 08:15:58 AM +07
root@server:~# hwclock
2020-05-25 08:16:02.118087+07:00
```

### 5. Cấu hình Chrony Client 
Thực chất sau khi cài đặt và khởi động Chrony thì Server này đã tự động đồng bộ thời gian về từ một trong những NTP Server thuộc pool `ntp.ubuntu.com`

Bây giờ thay vì đồng bộ thời gian từ Internet chúng ta sẽ đồng bộ từ NTP Server chúng ta cấu hình phía trên

Tại Server 192.168.11.144 chỉnh sửa cấu hình chrony

```
sed -i 's|pool ntp.ubuntu.com  iburst maxsources 4|server 192.168.11.143 iburst maxsources 1|g' /etc/chrony/chrony.conf
sed -i 's|pool 0.ubuntu.pool.ntp.org iburst maxsources 1|#|g' /etc/chrony/chrony.conf
sed -i 's|pool 1.ubuntu.pool.ntp.org iburst maxsources 1|#|g' /etc/chrony/chrony.conf
sed -i 's|pool 2.ubuntu.pool.ntp.org iburst maxsources 2|#|g' /etc/chrony/chrony.conf
```

Kiểm tra cấu hình 

`cat /etc/chrony/chrony.conf | egrep -v '^$|^#'`

Khởi động lại Chrony để cập nhật cấu hình.

`systemctl restart chrony`

Sử dụng `chrony` kiểm tra đồng bộ

`chronyc sources -v`

Kiểm tra đồng bộ sử dụng `timedatectl`

Set đồng bộ thời gian cho đồng bộ của BIOS 

`hwclock --systohc`

Kiểm tra đồng bộ của date và hwclock đảm bảo đồng bộ

`date`
`hwclock`
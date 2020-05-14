# Network Filesystem
Sử dụng **NFS** (hệ thống tệp mạng) là một trong những phương pháp được sử dụng để chia sẻ dữ liệu trên các hệ thống vật lý. Nhiều quản trị viên hệ thống gắn các thư mục của người dùng từ xa trên một máy chủ để cấp cho họ quyền truy cập vào cùng một tệp và tệp cấu hình trên nhiều hệ thống máy khách. Điều này cho phép người dùng đăng nhập vào các máy khác nhau nhưng vẫn có quyền truy cập vào cùng các tệp và tài nguyên

### Lợi ích của NFS
 * **NFS** cho phép truy cập cục bộ vào các tệp từ xa
 * Nó sử dụng kiến trúc client/server tiêu chuẩn để chia sẻ tệp giữa các máy
 * Với **NFS**, không cần thiết cả hai máy đều chạy trên một hệ điều hành
 * Với sự trợ giúp của **NFS**, có thể định cấu hình các giải pháp lưu trữ tập trung 
 * Người dùng có được dữ liệu của họ bất kể vị trí thực tế
 * Không cần làm mới thủ công cho các tập tin mới
 * Phiên bản mới hơn của NFS cũng hỗ trợ acl, mount root ảo
 * Có thể được bảo mật với tường lủa và **Kerberos**

### Dịch vụ NFS 
Đây là một System V-launched. NfS bao gồm `portmap` và `nfs-utils package` :
 * `portmap` : Nó ánh xạ các cuộc gọi được thực hiện từ các máy khác đến dịch vụ RPC chính xác (không bắt buộc với NFSv4)
 * `nfs` : Nó dịch các yêu cầu chia sẻ từ xa thành các yêu cầu trên hệ thống tệp cục bộ
 * `rpc.mountd` : Dịch vụ này có trách nhiệm lắp và unmount toàn bộ các hệ thống tập tin

### Các tệp quan trọng cho cấu hình NFS
 * `/etc/export` : Đây là tệp cấu hình chính của NFS, tất cả các tệp và thư mục đã xuất được xác định trong tệp này ở cuối máy chủ NFS
 * `/etc/fstab` : Để gắn một thư mục NFS trên hệ thống của bạn trên các lần khởi động lại, chúng ta cần tạo một thư mục trong /etc/fstab
 * `/etc/sysconfig/nfs` : Tệp cấu hình của NFS để kiểm soát cổng rpc và các dịch vụ khác đang nghe

# Network Advanced
### 1. `ping` 
 * Lệnh `ping` gửi các gói `ECHO_REQUEST` tới đỉa chỉ chỉ định
 * Lệnh này nhằm kiểm tra máy tính có thể kết nối với Internet hay một địa chỉ IP cụ thể nào đó hay không
 * Không giống lệnh `ping` trên Windows, câu lệnh `ping` trên Linux sẽ duy trì gửi các gói tin cho đến khi bạn kết thúc nó. Có thể định số lượng gói tối đã gửi đi bằng cách gõ thêm tùy chọn `-c`
  `# ping -c [number] [IP/domain_destination]`

```
[root@localhost ~]# ping -c 4 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=128 time=36.8 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=128 time=80.5 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=128 time=40.1 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=128 time=29.3 ms

--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3034ms
rtt min/avg/max/mdev = 29.379/46.744/80.560/19.911 ms
```

### 2. `tracepath / traceroute`
 * Dùng để lần dấu đường đi trên mạng tới một đích chỉ định và báo cáo về mỗi hop dọc trên đường đi 
```
[root@localhost ~]# tracepath 8.8.8.8
 1?: [LOCALHOST]                                         pmtu 1500
 1:  gateway                                               0.488ms
 1:  gateway                                               0.268ms
```

```
[root@localhost ~]# traceroute 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  gateway (192.168.213.2)  0.540 ms  0.285 ms  0.314 ms
 2  * * *
```

### 3. `mtr` 
 * Là sự kết hợp `ping` và `tracepath` trong một câu lệnh
 * `mtr` sẽ gửi liên tục các gói và hiển thị thời gian `ping` cho mỗi **hop** 
 * Câu lệnh cũng giúp phát hiện một số vấn đề mạng qua tỉ lệ **Loss%**
 * Gõ `q` để thoát khỏi tiến tình

   `# mtr [IP/Domain_destination]`
 * Tải **mtr** bằng lệnh : 

   `yum install mtr`
```
                             My traceroute  [v0.85]
localhost.localdomain (0.0.0.0)                        Wed May 13 23:30:01 2020
Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                       Packets               Pings
 Host                                Loss%   Snt   Last   Avg  Best  Wrst StDev
```

### 4. `host`
 * Lệnh `host` được thực hiện để tìm kiếm DNS
 * Nhập vào tên miền khi muốn xem địa chỉ IP đi kèm và ngược lại, nhập vào địa chỉ IP khi muốn xem tên miền đi kèm

   `# host [IP/Doamin]
 * Cài đặt `yum install bind-utils -y` để có thể dùng host
```
[root@localhost ~]# host google.com
google.com has address 172.217.25.14
google.com has IPv6 address 2404:6800:4005:809::200e
google.com mail is handled by 50 alt4.aspmx.l.google.com.
google.com mail is handled by 20 alt1.aspmx.l.google.com.
google.com mail is handled by 30 alt2.aspmx.l.google.com.
google.com mail is handled by 40 alt3.aspmx.l.google.com.
google.com mail is handled by 10 aspmx.l.google.com.
```

### 5. `whois` 
 * `whois` không được cài đặt mặc định trên **CentOS**, phải cài thủ công:

  ` # yum install -y whois`
 * Đưa các bản ghi trên server **whois** (**whois record**) của website, vì vậy bạn có thể xem thông tin về người hay tổ chức đã đăng ký và sở hữu website đó
 
  `# whois [Domain]`
```
[root@localhost ~]# whois google.com
   Domain Name: GOOGLE.COM
   Registry Domain ID: 2138514_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.markmonitor.com
   Registrar URL: http://www.markmonitor.com
   Updated Date: 2019-09-09T15:39:04Z
   Creation Date: 1997-09-15T04:00:00Z
   Registry Expiry Date: 2028-09-14T04:00:00Z
   Registrar: MarkMonitor Inc.
   Registrar IANA ID: 292
```

```
   Name Server: NS1.GOOGLE.COM
   Name Server: NS2.GOOGLE.COM
   Name Server: NS3.GOOGLE.COM
   Name Server: NS4.GOOGLE.COM
```

### 6. `dhclient`
 * Giúp làm mới địa chỉ IP trên máy bằng cách giải phóng địa chỉ IP cũ và nhận một địa chỉ mới từ DHCP server
   `# dhclient -r (release)`
   `# dhclient    (renew)`

### 7. `netstat`
 * Là một lệnh nằm trong số các tập lệnh để giám sát hệ thống Linux
 * Giám sát cả chiều in và chiều out kết nối vào server, hoặc các tuyến đường route, trạng thái của card mạng
 * Rất hữu dụng trong việc giải quyết các vấn đề về sự cố liên quan đến network như là **lượng connect kết nối, trafic, tốc độ, trạng thái của từng port, IP ...**
 * Cài đặt `yum install net-tools` để sử dụng **netstat** 
 * Cú pháp :
  `# netstat [options]`
   * **Options** :
     * `-a` : kiểm tra tổng quát tất cả các port trên Server
     * `-c` : kiểm tra tổng quát tất cả các port trên Server ở chế độ runtime
     * `-at` : kiểm tra các port chạy **TCP**
     * `au` : kiểm tra các port chạy **UDP**
     * `-l` : kiểm tra các port ở trạng thái `LISTENING`
     * `lt` : kiểm tra các port ở trạng thái `LISTENing` chạy **TCP**
     * `-plnt` : kiểm tra các port ở trạng thái `LISTENING` đang chạy dịch vụ gì
     * `-lu` : kiểm tra các port ở trạng thái `LISTENING` chạy **UDP**
     * `-rn` : hiển thị bảng định tuyến
     * `-s` : thống kế theo các bộ giao thức **TCP , UDP , ICMP , IP**
     * `-st` : thống kê theo bộ giao thức **TCP** 
     * `-su` : thống kê theo bộ giao thức **UDP**
     * `-i` : hiển thị hoạt động của các network interface
     * `-g` : hiển thị tình trạng IPv4 và IPv6

 * Hiển thị tên service cùng **PID**:
  
    `# netstat -tp`
```
[root@localhost ~]# netstat -tp
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State                                                                                                                PID/Program name
tcp        0      0 localhost.localdoma:ssh 192.168.213.1:61113     ESTABLISHED                                                                                                          1450/sshd: root@not
tcp        0      0 localhost.localdo:57314 h2162.tino.org:http     TIME_WAIT                                                                                                            -
tcp        0      0 localhost.localdoma:ssh 192.168.213.1:61112     ESTABLISHED                                                                                                          1446/sshd: root@pts
```

 * Hiển thị hoạt động của các network interface:
   
   `# netstat -i`
```
[root@localhost ~]# netstat -i
Kernel Interface table
Iface             MTU    RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OV                                                                                                         R Flg
ens33            1500    11257      0      0 0         10165      0      0                                                                                                               0 BMRU
ens37            1500       30      0      0 0            35      0      0                                                                                                               0 BMRU
ens38            1500       17      0      0 0            21      0      0                                                                                                               0 BMRU
lo              65536        3      0      0 0             3      0      0                                                                                                               0 LRU
```

 * Hiển thị thông tin bảng định tuyến:
  `# netstat -rn`
```
[root@localhost ~]# netstat -rn
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         192.168.213.2   0.0.0.0         UG        0 0          0 ens33
192.168.11.0    0.0.0.0         255.255.255.0   U         0 0          0 ens37
192.168.11.0    0.0.0.0         255.255.255.0   U         0 0          0 ens38
192.168.213.0   0.0.0.0         255.255.255.0   U         0 0          0 ens33
```

 * Hiển thị các kết nối sử dụng dịch vụ `http`:

  `# netstat -ap | grep http`

 * Hiển thị số lượng gói `SYN_REC` trên Server (nếu có quá nhiều server đang bị DDOS)
  `# netstat -np | grep SYN_REC | wc -l`

### 8. `tcpdump`
 * **Tcpdump** là phầm mềm bắt gói tin trong mạng làm việc trên hầu hết các phiên bản hệ điều hành Unix/Linux. **Tcpdump** cho phép bắt và lưu lại những gói tin bắt được, từ đó chúng ta có thể sử dụng để phân tích
 * Có thể lưu ra file và đọc bằng công cụ đồ họa **Wireshard**
 * Cài đặt `tcpdump` :

   `# yum install -y tcpdump`
 * Cấu trúc lệnh :

   `# tcpdump [options] [network_interface]`
 
   * Options:
     * `-X` : hiển thị nội dung của gói theo dạng `ASCII` và `HEX`
     * `-XX` : tương tự `-X`
     * `-D` : liệt kê các network interface có sẵn 
     * `-l` : đầu ra có thể đọc được dòng (để xem khi bạn lưu hoặc gửi đến các lệnh khác)
     * `-t` : cung cấp đầu ra dấu thời gian có thể đọc được của con người
     * `-q` : ít dài dòng hơn với đầu ra
     * `-tttt` : cung cấp đầu ra dấu thời gian tối đa có thể đọc được của con người
     * `-i` : bắt lưu lượng của một giao diện cụ thể
     * `-vv` : đầu ra cụ thể và chi tiết hơn ( nhiều v hơn cho đầu ra nhiều hơn)
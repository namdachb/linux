## Thiết lập cơ bản cho Centos7
### Đặt hostname cho centos7
Sử dụng lệnh để đặt hostname cho CentOS mà không cần phải khởi động OS theo cú pháp `hostnamectl set-hostname ten_server`. Ví dụ đặt tên hostname cho server là **namdac**

`hostnamectl set-hostname namdac`

Sau đó đăng nhập phiên mới hoặc gõ lệnh `bash` để sử dụng host name mới.

<img src="https://imgur.com/seIwPGC.png">

Có thể sửa thêm file /etc/hosts để khai báo thêm hostname mới ở dòng 127.0.0.1. Cụ thể sửa lại file đó có nội dung như sau:

`echo "127.0.0.1 localhost namdac" > /etc/hosts`

## Đề bài tạo 2 con máy ảo, mỗi máy có 3 card mạng, thiết lập ip tĩnh cho các máy

Mô hình 

![Imgur](https://i.imgur.com/lL5GgaP.png)

IP Planning

<img src="https://imgur.com/RhZhKSQ.png">

Đầu tiên chúng ta vào 2 máy ảo, ở mỗi máy ảo chúng ta tạo 3 card mạng
 * Xong chúng ta có thể check xem mạng có sẵn bằng lệnh `nmcli device`
 
```
[root@localhost ~]# nmcli device
DEVICE  TYPE      STATE      CONNECTION
ens33   ethernet  connected  ens33
ens37   ethernet  connected  Wired connection 1
ens38   ethernet  connected  Wired connection 2
lo      loopback  unmanaged  --
```

Chúng ta thấy phần connection của ens37 và ens38 khá dài, nên ta đổi "ens37" cho đồng bộ với ens37

### Thiết lập địa chỉ IP tĩnh 
Để đặt IP tĩnh cho máy CentOS ta có 2 phương án là: sửa file hoặc sử dụng lệnh. Trong bài này e sẽ sử dụng lệnh để thiết lập IP tĩnh

Việc đặt địa chỉ IP bằng lệnh trong CentOS7 được sử dụng thông qua NetworkManager, chính là gói quản lý lệnh `nmcli` .
 * Chúng ta sẽ làm với card mạng **ens37** của máy ảo 1

```
nmcli con modify ens37 ipv4.addresses 192.168.11.10/24
nmcli con modify ens37 ipv4.gateway 192.168.11.1
nmcli con modify ens37 ipv4.dns 8.8.8.8
nmcli con modify ens37 ipv4.method manual
nmcli con modify ens37 connection.autoconnect yes
```

 * Khi đổi xong chúng restart lại mạng bằng lệnh 

   `service network restart`

Sau đó dùng lệnh để kiểm tra:
  
   `ip a` 
```
[root@localhost ~]# ip a
3: ens37: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group                                                                                                    default qlen 1000
    link/ether 00:0c:29:fe:8c:db brd ff:ff:ff:ff:ff:ff
    inet 192.168.11.10/24 brd 192.168.11.255 scope global noprefixroute ens37
       valid_lft forever preferred_lft forever
    inet6 fe80::10a3:59d9:f6cd:17e8/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```

Và chúng ta tiếp tục với card ens38 của máy ảo 1 và với 2 card ens37, ens38 của máy ảo 2 ta sẽ thấy : 

 * Máy 1
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qle                                                                                                    n 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group                                                                                                     default qlen 1000
    link/ether 00:0c:29:fe:8c:d1 brd ff:ff:ff:ff:ff:ff
    inet 192.168.213.136/24 brd 192.168.213.255 scope global noprefixroute dynamic en                                                                                                    s33
       valid_lft 1491sec preferred_lft 1491sec
    inet6 fe80::7040:e666:7e44:8d75/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: ens37: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group                                                                                                     default qlen 1000
    link/ether 00:0c:29:fe:8c:db brd ff:ff:ff:ff:ff:ff
    inet 192.168.11.10/24 brd 192.168.11.255 scope global noprefixroute ens37
       valid_lft forever preferred_lft forever
    inet6 fe80::10a3:59d9:f6cd:17e8/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
4: ens38: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group                                                                                                     default qlen 1000
    link/ether 00:0c:29:fe:8c:e5 brd ff:ff:ff:ff:ff:ff
    inet 192.168.10.10/24 brd 192.168.10.255 scope global noprefixroute ens38
       valid_lft forever preferred_lft forever
    inet6 fe80::39fc:8a37:8c73:9ad7/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```

 * Máy 2
```
[root@localhost ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:0e:99:fb brd ff:ff:ff:ff:ff:ff
    inet 192.168.213.159/24 brd 192.168.213.255 scope global noprefixroute dynamic ens33
       valid_lft 1582sec preferred_lft 1582sec
    inet6 fe80::f507:ccdd:d86a:4c1d/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: ens37: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:0e:99:05 brd ff:ff:ff:ff:ff:ff
    inet 192.168.11.11/24 brd 192.168.11.255 scope global noprefixroute ens37
       valid_lft forever preferred_lft forever
    inet6 fe80::d0cf:b843:90c1:c23a/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
4: ens38: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:0e:99:0f brd ff:ff:ff:ff:ff:ff
    inet 192.168.10.11/24 brd 192.168.10.255 scope global noprefixroute ens38
       valid_lft forever preferred_lft forever
    inet6 fe80::895:2280:56d3:fecc/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```

Sau khi thiết lập IP tĩnh xong chúng ta sẽ đứng từ máy 1 để ping sang máy 2 và ngược lại để xem chúng có thể ping với nhau được không

```
[root@localhost ~]# ping 192.168.213.136
PING 192.168.213.136 (192.168.213.136) 56(84) bytes of data.
64 bytes from 192.168.213.136: icmp_seq=1 ttl=64 time=0.045 ms
64 bytes from 192.168.213.136: icmp_seq=2 ttl=64 time=0.059 ms
64 bytes from 192.168.213.136: icmp_seq=3 ttl=64 time=0.086 ms
```

```
[root@localhost ~]# ping 192.168.11.10
PING 192.168.11.10 (192.168.11.10) 56(84) bytes of data.
64 bytes from 192.168.11.10: icmp_seq=1 ttl=64 time=0.057 ms
64 bytes from 192.168.11.10: icmp_seq=2 ttl=64 time=0.064 ms
64 bytes from 192.168.11.10: icmp_seq=3 ttl=64 time=0.088 ms
64 bytes from 192.168.11.10: icmp_seq=4 ttl=64 time=0.086 ms
```

```
[root@localhost ~]# ping 192.168.10.10
PING 192.168.10.10 (192.168.10.10) 56(84) bytes of data.
64 bytes from 192.168.10.10: icmp_seq=1 ttl=64 time=0.045 ms
64 bytes from 192.168.10.10: icmp_seq=2 ttl=64 time=0.079 ms
64 bytes from 192.168.10.10: icmp_seq=3 ttl=64 time=0.085 ms
```

Và cứ như vậy chúng ta đứng từ máy 2 cũng có thể ping sang máy 1 

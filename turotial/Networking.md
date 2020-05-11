# Network Interfaces
*Giao diện mạng là kênh kết nối giữa thiết bị và mạng. Về mặt vật lý, giao diện mạng có thể tiến hành thông qua giao diện mạng (**NIC**) hoặc có thể được triển khai trừu tượng dưới dạng phần mềm. Bạn có thể có nhiều giao diện mạng hoạt động cùng một lúc. Các giao diện cụ thể có thể được đưa lên (kích hoạt) hoặc đưa xuống (khử kích hoạt) bất cứ lúc nào. Một danh sách các giao diện mạng hiện hoạt động được báo cáo bởi `ifconfig` tiện ích. Các tập tin cấu hình mạng là rất cần thiết để đảm bảo các giao diện hoạt động chính xác*

*Đối với cấu hình họ **Desbian**, tệp cấu hình mạng cơ bản là `/etc/network/interfaces`. Đối với cấu hình hệ thống gia đình **RedHat**, thông tin định tuyến và máy chủ được chứa trong `/etc/sysconfig/network`. Kịch bản cấu hình giao diện mạng cho `eth0` giao diện được đặt tại `/etc/sysconfig/network-scripts/ifcfg-eth0`. Đối với cấu hình hệ thống cho **SUSE**, tập lệnh định tuyến và thông tin máy chủ và giao diện mạng được chứa trong `/etc/sysconfig/network` thư mục*
## Lệnh `ip`:
### Lệnh trả lại thông tin trên từng thiết bị Ethernet được kết nối.
`# ip addr show`
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:4c:36:8c brd ff:ff:ff:ff:ff:ff
    inet 192.168.213.150/24 brd 192.168.213.255 scope global noprefixroute dynamic ens33
       valid_lft 1208sec preferred_lft 1208sec
    inet6 fe80::b81b:2fda:5fbf:843f/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```
*Để xem thông tin về `ens33`:*
```
[root@localhost ~]# ip addr show ens33
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:4c:36:8c brd ff:ff:ff:ff:ff:ff
    inet 192.168.213.150/24 brd 192.168.213.255 scope global noprefixroute dynamic ens33
       valid_lft 1081sec preferred_lft 1081sec
    inet6 fe80::b81b:2fda:5fbf:843f/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```
### Hiển thị bảng định tuyến
`ip route show`
```
[root@localhost ~]# ip route show
default via 192.168.213.2 dev ens33 proto dhcp metric 100
192.168.213.0/24 dev ens33 proto kernel scope link src 192.168.213.150 metric 100
```
### Gán IP cho một giao diện mạng:
`# ip addr add  192.168.213.151 dev ens33`
```
[root@localhost ~]# ip addr show dev ens33
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:4c:36:8c brd ff:ff:ff:ff:ff:ff
    inet 192.168.213.150/24 brd 192.168.213.255 scope global noprefixroute dynamic ens33
       valid_lft 1495sec preferred_lft 1495sec
    inet 192.168.213.151/32 scope global ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::b81b:2fda:5fbf:843f/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```
### Gán nhiều IP cho một giao diện mạng:
*Để gán nhiều IP cho giao diện mạng ta làm tương tự như trên*
### Gỡ bỏ IP từ giao diện mạng
`# ip addr del 192.168.213.152/32 dev ens33`
```
[root@localhost ~]# ip addr del 192.168.213.151/32 dev ens33
[root@localhost ~]# ip addr show dev ens33
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:4c:36:8c brd ff:ff:ff:ff:ff:ff
    inet 192.168.213.150/24 brd 192.168.213.255 scope global noprefixroute dynamic ens33
       valid_lft 1362sec preferred_lft 1362sec
    inet6 fe80::b81b:2fda:5fbf:843f/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```
### Hiển thị thông tin về một giao diện mạng
```
[root@localhost ~]# ip link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 00:0c:29:4c:36:8c brd ff:ff:ff:ff:ff:ff
```
### Thay đổi trạng thái giao diện mạng (up/down)
`ip link set dev {DEVICE} {up|down}`
### Hiển thị bảng định tuyến'
`ip route`
```
[root@localhost ~]# ip route
default via 192.168.213.2 dev ens33 proto dhcp metric 100
192.168.213.0/24 dev ens33 proto kernel scope link src 192.168.213.150 metric 100
```
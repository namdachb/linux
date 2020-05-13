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

```
nmcli con modify ens37 ipv4.addresses 192.168.11.10/24
nmcli con modify ens37 ipv4.gateway 192.168.11.1
nmcli con modify ens37 ipv4.dns 8.8.8.8
nmcli con modify ens37 ipv4.method manual
nmcli con modify ens37 connection.autoconnect yes
```

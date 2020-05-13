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

[Imgur](https://i.imgur.com/lL5GgaP.png)

IP Planning

<img src="https://imgur.com/RhZhKSQ.png">

### Thiết lập địa chỉ IP tĩnh 
Để đặt IP tĩnh cho máy CentOS
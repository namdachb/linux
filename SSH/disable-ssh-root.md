# Tạo user sudo và cấu hình disable ssh đối với user root (centos7)
Ngày nay, mọi người đều biết rằng các hệ thống **Linux** đi kèm với quyền truy cập của người dùng **root** và theo mặc định, quyền truy cập **root** được kích hoạt cho thế giới bên ngoài. Vì lý do bảo mật, không nên bật quyền truy cập root ssh cho người dùng trái phép. Bởi vì bất kỳ hacker nào cũng có thể có gắng ép buộc mật khẩu của bạn và giành quyền truy cập vào hệ thống của bạn 

Vì vậy, tốt hơn là nên có một tài khoản khác mà bạn thường xuyên sử dụng và sau đó chuyển sang người dùng **root** bằng cách sử dụng lệnh **su -** khi cần thiết. Hãy đảm bảo bạn có tài khoản người dùng thông thường và cùng với đó bạn **su** hoặc **sudo** để có quyền truy cập root

Các lệnh sudo cung cấp một cơ chế để cấp các đặc quyền của quản trị viên, thông thường chỉ có sẵn dành cho những người sử dụng root

## Tạo và cấp quyền sudo cho người dùng mới
Để tạo và cấp quyền sudo cho 1 người dùng mới trước tiên bạn login vào server bằng user root

### 1. Tạo user và cấp password cho user đó
Để tạo user, sử dụng `adduser` để thêm người dùng mới vào hệ thống. Và hãy thay thế `namdac123` bằng tên người dùng mới mà bạn muốn tạo

 `adduser namdac123`

Sau khi tạo user, sử dụng lệnh `passwd` để cập nhật mật khẩu cho người dùng mới. Mình sẽ tạo mật khẩu cho user vừa tạo `namdac123` như sau :

 `passwd namdac123`

Sau đó hãy nhập mật khẩu cho user tại `New password` và nhập lại mật khẩu `Retype new password`. Nếu mật khẩu trùng khớp sẽ có 1 thông báo là successfully. Tại đây chúng ta nên tạo 1 mật khẩu mạnh để tăng bảo mật

```
[root@localhost ~]# passwd namdac123
Changing password for user namdac123.
New password:
BAD PASSWORD: The password contains the user name in some form
Retype new password:
passwd: all authentication tokens updated successfully.
```

### 2. Cấp quyền sudo cho user vừa tạo
Sử dụng câu lệnh `usermod` để thêm người dùng vào nhóm `wheel`

 `usermod -aG wheel namdac123`

Theo mặc định, server sẽ có một group có tên là **wheel**, các thành viên được add vào nhóm wheel mặc định sẽ có đặc quyền sudo

### 3. Kiểm tra lại người dùng với quyền sudo
Sử dụng lệnh sau để kiểm tra xem người dùng đã được add vào nhóm wheel hay chưa

 `cat /etc/group | grep namdac123`

Nếu thông tin hiện ra như dưới đây là user **namdac123** đã nằm trong nhóm wheel và được cấp quyền sudo
```
[root@localhost ~]# cat /etc/group | grep namdac123
wheel:x:10:namdac123
namdac123:x:1001:
```

## Cấu hình disable ssh đối với tài khoản root
Tại đây, để ngăn chặn đăng nhập từ xa, bạn phải đảm bảo có một tài khoản khác trên máy chủ được cấp quyền sudo hoặc su trước khi xóa khả năng đăng nhập bằng root. Nếu không bạn sẽ không có cách nào để có quyền root trên máy chủ mình để quản lý và cấu hình đối với nó

### 1. Vô hiệu hóa đăng nhập từ xa đối với user root
Để vô hiệu hóa đăng nhập ssh liên quan đến 1 chỉnh sửa cấu hình đơn giản. Muốn vô hiệu hóa user root đăng nhập ssh, ta vào file `sshd_config` và chỉnh sửa cấu hình

Chạy lệnh sau với quyền root để mở tệp cấu hình và chỉnh sửa nó :

 `vi /etc/ssh/sshd_config`


Trong file `sshd_config`, ta tìm đến dòng chứa nội dung `#PermitRootLogin Yes` để chỉnh sửa nó

Dùng `:set nu` để hiện thị số dòng và ở đây chúng ta tìm đến dòng **46** 

Bỏ comment và sửa `yes` thành `no` sau đó lưu lại và thoát

 `PermitRootLogin no`

Khởi động lại sshd để áp dụng cấu hình
 
 `systemctl restart sshd`

### 2. Kiểm tra lại đăng nhập đối với user root
Sau khi nhập user root để đăng nhập thì ta sẽ nhận được thông báo từ chuỗi truy cập như sau :

![Imgur](https://i.imgur.com/YaRdS7I.png)

Bây giờ thì không còn ai có thể đăng nhập từ xa bằng user root nữa mà chỉ có thể truy cập từ xa bằng các user thông thường hoặc user sudo để thao tác với server

Vì vậy, chúng ta chỉ đăng nhập như người dùng bình thường và sau đó sử dụng lệnh `su` để chuyển sang người dùng root

![Imgur](https://i.imgur.com/4VOJ1i6.png)

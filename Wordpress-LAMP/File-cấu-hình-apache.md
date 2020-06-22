## Tìm hiểu file cấu hình của Apache webserver trong CentOS 7 
Khi làm việc với bất kỳ gói phần mềm nào thì chắc chắn bạn có hơn một lần thao tác với tệp cấu hình của nó. Bài viết này chia sẻ đến các bạn file cấu hình `httpd.conf` trong thư mục `/etc/httpd/conf` khi bạn cài đặt Web server trên máy chủ CentOS 7

 * `Server Root` : phần trên cùng của cây thư mục mà theo đó các tệp cấu hình, lỗi (error) và nhật ký (log) của máy chủ được lưu trữ. Mặc định đường dẫn của Server Root là `/etc/httpd`

`ServerRoot "/etc/httpd"`

 * `Listen` : cho phép bạn liên kết Apache với các địa chỉ IP và (hoặc) port cụ thể thay vì mặc định

```
#Listen 12.34.56.78:80
Listen 80
```

Mặc định giao thức http sẽ làm việc trên cổng 80 nên default Listen là 80. Khi bạn muốn Apache kết nối với máy chủ có địa chỉ IP là 192.168.100.10 trên port 8000 thì bạn chỉ cần thêm dòng `Listen 192.168.100.10:8000`

 * `User/Group` : tên (hoặc số) của user/group để thực thi httpd. Thông thường nên tạo 1 người dùng và nhóm chuyên dụng để chạy httpd như hầu hết dịch vụ hệ thống. Giá trị mặc định của user và group trong file cấu hình là `apache` 

```
User apache
Group apache
```

 * `ServerAdmin` : mỗi web sẽ có 1 admin quản trị, đây là nơi khai báo địa chỉ email của người quản trị website
 * `ServerName`: tên miền hoặc địa chỉ IP mà bạn muốn khai báo
 * `Directory` : để tăng tính bảo mật hệ thống, từ chối quyền truy cập vào toàn bộ hệ thống tệp của máy chủ, bạn cần cho phép truy cập rõ ràng vào các thư mục chứa nội dung web trong các khổi như bên dưới

```
<Directory />
    AllowOverride none
    Require all denied
</Directory>
```

Trong đó:
+ `AllowOverride` : chỉ định chỉ thị nào được khai báo trong tệp .htaccess có thể ghi đè chỉ thị cấu hình
+ `Require all denied` : từ chối tất cả các máy khác truy cập vào thư mục đang được cấu hình. Ngược lại nếu giá trị `require all granted` thì có nghĩa là cho phép
  
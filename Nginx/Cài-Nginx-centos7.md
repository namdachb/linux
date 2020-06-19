## Cách thiết lập khối máy chủ trên Centos7 

Để thêm kho lưu trữ CentOS 7, ta sử dụng lệnh

 `yum install epel-release`

Kho lưu trữ EPEL đã được cài đặt trên máy chủ, hãy cài đặt nginx:

 `yum install nginx`

Sau khi cài xong, hãy bắt đầu dịch vụ Nginx

 `systemctl start nginx`

Để cho phép Nginx khởi động khi khởi động, hãy chạy lệnh
 
 `systemctl enable nginx`

### Bước 1: Tạo cấu trúc thư mục 
Chúng ta cần tạo 1 cấu trúc thư mục chứa dữ liệu trang web để phục vụ khách truy cập 

```
mkdir -p /usr/share/namdac2.com/html
mkdir -p /usr/share/namdac3.com/html
```

 **Cấp quyền**
Bây giờ chúng ta có cấu trúc cho các tệp của chúng ta, nhưng chúng được sở hữu bởi `root`. Nếu muốn người dùng thường xuyên có thể sửa đổi các tệp trong thư mục web của mình, chúng ta có thể thay đổi quyền sở hữu với `chown`:

```
chown -R $USER:$USER /usr/share/namdac2.com/html
chown -R $USER:$USER /usr/share/namdac3.com/html
```

Chúng ta cũng nên sửa đổi các quyền của mình một chút để đảm bảo rằng quyền truy cập được phép vào thư mục web chung và tất cả các tệp và thư mục bên trong, để các trang có thể được phục vụ chính xác

 `chmod -R 755 /usr/share`

Máy chủ web của chúng ta bây giờ sẽ có các quyền cần thiết để phân phát nội dung và người dùng của chúng ta sẽ có thể tạo nội dung trong các thư mục phù hợp

### Bước 2: Tạo trang demo cho mỗi trang web
Chúng ta sẽ tạo một `index.html` trang cho mỗi trang web xác định tên miền cụ thể đó

Hãy bắt đầu với **namdac2.com**. Chúng ta có thể mở 1 `index.html` trong trình chỉnh sửa của mình 

 `vi /usr/share/namdac2.com/html/index.html`

Trong tệp này, tạo 1 tài liệu HTML đơn gian cho biết trang web mà trang được kết nối

```
<html>
  <head>
    <title>Welcome to namdac2.com!</title>
  </head>
  <body>
    <h1>Success! The namdac2.com server block is working!</h1>
  </body>
</html>
```
Lưu và đóng tệp khi hoàn thành

Chúng ta có thể sao chép tệp này để sử dụng làm cho trang web thứ của mình bằng cách:
 
 `cp /usr/share/namdac2.com/html/index.html /usr/share/namdac3.com/html/index.html`

Bây giờ hãy mở tệp đó và sửa đổi các thông tin có liên quan

 `vi /usr/share/namdac3.com/html/index.html`

```
<html>
  <head>
    <title>Welcome to namdac3.com!</title>
  </head>
  <body>
    <h1>Success! The namdac3.com server block is working!</h1>
  </body>
</html>
```

### Bước 3: Tạo tập tin chặn máy chủ mới
Chúng ta sẽ cần thiết lập thư mục mà các khối máy chủ của chúng ta sẽ được lưu trữu, cũng như thư mục cho Nginx biết rằng 1 khối má chủ đã sẵn sàng phục vụ cho khách truy cập
  * Thư mục `site-available` sẽ giữ tất cả các tệp khổi máy chủ của chúng ta
  * Trong khi thư mục `sites-enabled` sẽ giữ các liên kết tượng trưng đến các khối máy chủ mà chúng ta muốn xuất bản

Chúng ta có thể tạo cả 2 thư mục:
```
mkdir /etc/nginx/sites-available
mkdir /etc/nginx/sites-enabled
```

Tiếp theo chúng ta sẽ chỉnh sửa tệp cấu hình chính của Nginx và thêm 1 dòng khai báo 1 thư mục tùy chọn cho các tệp cấu hình bổ sung

 `vi /etc/nginx/nginx.conf`

Thêm các dòng này vào cuối `http {}` khối:
```
include /etc/nginx/sites-enabled/*.conf;
server_names_hash_bucket_size 64;
```

Dòng đầu tiên hướng dẫn Nginx tìm kiếm các khối máy chủ trong sites-enabledthư mục, trong khi dòng thứ hai tăng dung lượng bộ nhớ được phân bổ để phân tích tên miền (vì chúng ta hiện đang sử dụng nhiều tên miền)

 **Tạo tệp khối máy chủ đầu tiên**

Theo mặc định, Nginx chứa 1 khối máy chủ được gọi là mẫu `default.conf` mà chúng ta có thể sử dụng làm mẫu cho các cấu hình của mình

 `cp /etc/nginx/conf.d/default.conf /etc/nginx/sites-available/namdac2.com.conf`

Bây giờ, hãy mở tệp mới

 `vi /etc/nginx/sites-available/namdac2.com.conf`

```
server {
    listen  80;
    server_name localhost;

    location / {
        root  /usr/share/nginx/html;
        index  index.html index.htm;
    }

    error_page  500 502 503 504  /50x.html;
    location = /50x.html {
        root  /usr/share/nginx/html;
    }
}
```
 * Đầu tiên chúng ta sẽ phải điều chỉnh là `server_name`, điều này cho Nginx biết yêu cầu trỏ đến khối máy chủ nào
> Lưu ý : Mỗi câu lệnh Nginx phải kết thúc bằng dấu chấm phẩy (;)
 
 * Tiếp theo, chúng ta sửa đổi gốc tài liệu.
  
  `root /usr/share/nginx/html;`

Sau khi sửa tệp, tệp sẽ trong giống như sau :
```
server {
    listen  80;
    server_name namdac2.com www.namdac2.com;

    location / {
        root  /usr/share/namdac2.com/html;
        index  index.html index.htm;
    }

    error_page  500 502 503 504  /50x.html;
    location = /50x.html {
        root  /usr/share/nginx/html;
    }
}
```

 **Tạo tệp khối máy chủ 2**

Bây giờ chúng ta tạo tệp 2 bằng cách sao chép từ tệp 1 và điều chỉnh nếu cần

 `cp /etc/nginx/sites-available/namdac2.com.conf /etc/nginx/sites-available/namdac3.com.conf`

Mở tệp mới :

 `vi /etc/nginx/sites-available/namdac3.com.conf`

Chỉnh sửa thông tin để tham chiều tên miền thứ 2:
```
server {
    listen  80;

    server_name namdac3.com www.namdac3.com;

    location / {
        root  /var/www/namdac3.com/html;
        index  index.html index.htm;
        try_files $uri $uri/ =404;
    }

    error_page  500 502 503 504  /50x.html;
    location = /50x.html {
        root  /usr/share/nginx/html;
    }
}
```

### Bước 4: kích hoạt tệp khối máy chủ mới
Bây giờ chúng ta đã tạo các tệp chặn máy chủ của mình, chúng ta cần kích hoạt chúng để Nginx biết để phục vụ chúng cho khách truy cập. Để làm điều này, chúng ta có thể tạo một liên kết tượng trưng cho từng khối máy chủ trong sites-enabledthư mục:
```
ln -s /etc/nginx/sites-available/namdac2.com.conf /etc/nginx/sites-enabled/namdac2.com.conf
ln -s /etc/nginx/sites-available/namdac3.com.conf /etc/nginx/sites-enabled/namdac3.com.conf
```

Khởi động lại Nginx để thay đổi này có hiệu lực

 `systemctl restart nginx`


Ở máy window mình vào
 `C: Window -> System 32 -> drives -> etc -> host` để sửa thông tin

Bước 5: Kiểm tra kết quả của 


Tham khảo bài viết tại [đây](https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-server-blocks-on-centos-7#step-three-%E2%80%94-create-new-server-block-files)
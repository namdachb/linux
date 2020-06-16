## Cách cài đặt Nginx trên Ubuntu 18.04, 20.04
Nginx là một trong những máy chủ web phổ biến nhất trên thế giới và chịu trách nhiệm lưu trữ một số trang web có lưu lượng truy cập lớn nhất và lớn nhất trên internet. Nó thân thiện với tài nguyên hơn Apache trong hầu hết các trường hợp và có thể được sử dụng làm máy chủ web hoặc proxy ngược

### Bước 1: Cài đặt Nginx
Vì Nginx có sẵn trong kho lưu trữ mặc định của Ubuntu, nên có thể cài đặt nó từ kho lưu trữ bằng `apt` hệ thống đóng gói

```
apt update
apt install nginx
```

### Bước 2: Điều chỉnh tường lửa
Trước khi thử nghiệm Nginx, phần mềm tường lửa cần được điều chỉnh để cho phép truy cập dịch vụ. Nginx đăng ký chính nó như 1 dịch vụ `ufw` khi cài đặt, làm cho nó đơn giản để cho phép Nginx truy cập

Liệt kê các cấu hình ứng dụng `ufw` biêt cách làm việc bằng cách gõ:

 `ufw app list`

Bạn sẽ nhận được một danh sách các hồ sơ ứng dụng:
```
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```

Như bạn có thể thấy, có 3 hồ sơ có sẵn cho Nginx:
 * **Nginx Full** : cấu hình này mở cả cổng 80 (lưu lượng truy cập web bình thường, không được mã hóa) và cổng 443 (lưu lượng được mã hóa TLS/SSL)
 * **Nginx HTTP** : cấu hình này chỉ mở cổng 80 (lưu lượng truy cập web binhg thường, không được mã hóa)
 * **Nginx HTTPS** : cấu hình này chỉ mở cổng 443(lưu lượng được mã hóa TLS/SSL)

Bạn nên kích hoạt cấu hình hạn chế nhất vẫn sẽ cho phép lưu lượng truy cập bạn đã định cấu hình. Vì chúng ta chưa định cấu hình SSL cho máy chủ của mình, chúng ta sẽ chỉ cần cho phép lưu lượng truy cập trên cổng 80

Bạn có thể kích hoạt bằng cách gõ:
 
 `ufw allow 'Nginx HTTP'`

Bạn có thể xác minh thay đổi bằng cách nhập:

 `ufw status`

Bạn sẽ thấy lưu lượng HTTP được phép trong đầu ra được hiện tại:
```
Status: active

To                         Action      From
--                         ------      ----
Nginx HTTP                 ALLOW       Anywhere
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```

### Bước 3: Kiểm tra máy chủ web của bạn
Khi kết thúc quá trình cài đặt, Ubuntu 18.04 khởi động Nginx. Chúng ta có thể kiểm tra với `system` hệ thống init để đảm bảo dịch vụ đang chạy bằng cách gõ:

 `systemctl status nginx`

```
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2020-06-15 07:48:08 UTC; 28min ago
     Docs: man:nginx(8)
  Process: 4284 ExecStop=/sbin/start-stop-daemon --quiet --stop --retry QUIT/5 --pidfile /run/nginx.p
  Process: 4298 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUC
  Process: 4287 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, sta
 Main PID: 4300 (nginx)
    Tasks: 2 (limit: 2290)
   CGroup: /system.slice/nginx.service
           ├─4300 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
           └─4305 nginx: worker process
```

Như bạn có thể thấy ở trên, dịch vụ dường như đã bắt đầu thành công

Bạn có thể truy cập trang đích Nginx mặc định để xác nhận rằng phần mềm đang chạy đúng bằng cách điều hướng đến địa chỉ IP của máy chủ của bạn. Nếu không biết địa chỉ IP của máy chủ của mình:

Hãy thử gõ lệnh này :

`ip addr show ens33 | grep inet | awk '{ print $2; }' | sed 's/\/.*$??'`

Một cách khác :
 
 `curl -4 icanhazip.com`

Khi bạn có địa chỉ IP của máy chủ, hãy nhập địa chỉ đó vào thanh địa chỉ của trình duyệt:

 `http://your_server_ip`

![Imgur](https://i.imgur.com/iu2KCkX.png)

Trang này được bao gồm với Nginx để cho bạn thấy rằng máy chủ đang chạy chính xác

### Bước 4: Quản lý quy trình Nginx (để tham khảo)
Bây giờ bạn đã có máy chủ web của mình hoạt động, hãy xem lại một số lệnh quản lý cơ bản

Để dừng máy chủ web của bạn:
 
 `systemctl stop nginx`

Để khởi động máy chủ web khi dừng

 `systemctl start nginx`

Để dừng và sau đó bắt đầu lại dịch vụ
 
 `systemctl restart nginx`

Nếu bạn chỉ đơn giản là thực hiện thay đổi cấu hình, Nginx thường có thể tải lại mà không làm rơi kết nối. Để làm điều này

 `systemctl reload nginx`

Theo mặc định, Nginx được cấu hình để tự động khởi động khi máy chủ khởi động. Nếu bạn muốn tắt hãy gõ lệnh:
 
 `systemctl disable nginx`

Để kích hoạt lại dịch vụ để khởi động khi khởi động

 `systemctl enable nginx`

### Bước 5: Thiết lập khối máy chủ
Khi sử dụng máy chủ web Nginx, các khối máy chủ có thể được sử dụng để đóng gói chi tiết cấu hình và lưu trữ nhiều tên miên từ một máy chủ. Mình sẽ thiết lập một tên miền được gọi là **nam.com**

Tạo thư mục cho **nam.com** như sau, sử dụng `-p` để tạo bất kỳ thư mục mẹ cần thiết :

 `mkdir -p /usr/share/nam.com/html`

Tiếp theo gán quyền sở hữu của thư mục với `$USER` biến môi trường:

 `chown -R $USER:$USER /usr/share/nam.com/html`

Quyền của root web của bạn phải chính xác nếu bạn chưa sửa đổi `umask` giá trị của mình, nhưng bạn có thể đảm bảo bằng cách nhập:

 `chmod -R 755 /usr/share/nam.com`

Tiếp theo, tạo 1 `index.html`

 `vi /usr/www/nam.com/html/index.html`

Bên trong, thêm HTML mẫu sau:

![Imgur](https://i.imgur.com/OS5wilI.png)

Lưu và đóng tệp khi hoàn thành

Để Nginx phục vụ nội dung này, cần phải tạo một khối máy chủ với các chỉ thị chính xác. Thay vì trực tiếp sửa đổitệp cấu hình mặc định, hãy tạo một tệp mới tại:`/etc/nginx/sites-available/nam.com`
 
 `vi /etc/nginx/sites-available/nam.com`

Dán vào khối cấu hình sau, tương tự như mặc định, nhưng được cập nhật cho thư mục và tên miền mới của mình
```
server {
        listen 80;
        listen [::]:80;

        root /usr/share/nam.com/html;
        index index.html index.htm index.nginx-debian.html;

        server_name nam.com www.nam.com;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

Tiếp theo, hãy kích hoạt tệp bằng cách tạo một liên kết từ nó đến `sites-enabled` thư mục mà Nginx đọc từ khi khởi động

 `ln -s /etc/nginx/sites-available/nam.com /etc/nginx/sites-enabled/`

Để tránh sự cố bộ nhớ nhóm có thể phát sinh từ việc thêm tên máy chủ bổ sung, cần phải điều chỉnh một giá trị trong `/etc/nginx/nginx.conf`. Mở tập tin:

 `vi /etc/nginx/nginx.conf`

Tìm lệnh `server_names_hash_bucket_size` và xóa `#` ký hiệu để bỏ dòng:

Lưu và đóng tệp khi hoàn thành

Tiếp theo, kiểm tra để đảm bảo rằng không có lỗi cú pháp trong bất kỳ tệp Nginx nào của bạn:

 `nginx -t`

```
root@namdac:~# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Nếu không có vấn đề gì, hãy khởi động lại Nginx để kích hoạt các thay đổi của bạn:

 `systemctl restart nginx`

>Lưu ý: Ở máy Window chúng vào :

 `C: Window -> System 32 -> drives -> etc -> host ` 
để sửa thông tin 

![Imgur](https://i.imgur.com/bqi7wCP.png)

Nginx hiện đang phục vụ tên miền của bạn Bạn có thể kiểm tra điều này bằng cách điều hướng đến nơi bạn sẽ thấy như thế này `http://nam.com`

![Imgur](https://i.imgur.com/FKYXiG9.png)


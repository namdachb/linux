# Cài đặt LEMP trên Ubuntu 18.04
Việc cài đjăt dưới đây được thực hiện với quyền `root`
## 1. Cài đặt Linux
Tại bài này, ta sử dụng **Ubuntu server 18.04**
## 2. Cài đặt NGINX
  * Cài đặt Nginx:
    ```
    apt update
    apt -y install nginx
    ```

Trên Ubuntu 18.04, Nginx được cấu hình bắt đầu chạy khi cài đặt

Nếu tường lửa `ufw` đang chạy, thì chúng ta cần phải cho phép kết nối với Nginx. Nginx tự đăng ký với ufw khi cài đặt. Do đó việc thực hiện trở nên đơn giản hơn
  
  * Cho phép lưu lượng truy cập trên cổng 80
    
    `ufw allow 'Nginx HTTP'

  * Kiểm tra phiên bản nginx:
    ```
    nginx -v
    nginx version: nginx/1.14.0 (Ubuntu)
    ```

  * Kiểm tra trên trình duyệt bằng cách gõ địa chỉ IP vào thành URL

  ![Imgur](https://i.imgur.com/LwoX6F5.png)

### 3. Cài đặt hệ quản trị cơ sở dữ liệu
Trên thực tế với LEMP, chúng ta có thể sử dụng `mysql` hoặc `mariadb` đều được, bài này mình sẽ sử dụng `mariadb`

Trước khi cài đặt, ta sẽ gỡ phiên bản hiện tại của MariaDB trên máy để cài đặt mới
  * Gỡ MariaDB hiện tại trên Ubuntu:
   
   `apt-get remove mariadb-server`

  * Cài đặt mới MariaDB
   
   `apt -y install mariadb-server`

  * Dịch vụ MariaDB sẽ tự động khởi chạy. Kiểm tra trạng thái 
   
   `systemctl status mariadb`

  * Kiểm tra version của Mariadb:

   `mysql -v`

  * Cài đặt một số thông tin ban đầu:
   
   `mysql_secure_installation`

     * Cài lại mật khẩu cho quyền root của cơ sở dữ liệu:
      
      `Enter curret password for root (enter for none):`

     * Bước này yêu cầu nhập mật khẩu gần đây cho `root`. Nếu bạn mới cài lần đầu thì nhần `Enter` để tiếp tục 

      `Set root password? (Y/n)`

     * Nếu bạn cài lần đầu, hệ thống sẽ hỏi bạn muốn cài password cho quyền `root` không. Bạn gõ `y` -> **Enter**, sau đó nhập mật khẩu và xác thực mật khẩu

     * Với những máy mới cài mariadb lần đầu, hệ thống yêu cầu thêm 1 số thiết lập như sau:
        * Xóa bỏ các user khác
        * Không cho phép root đăng nhập từ xa
        * Xóa bỏ databases test
        * Khởi chạy lại bảng Privilege (bảng phân quyền)
       Chúng ta chỉ cần gõ `Y` cho những điều đó

### 4. Cài đặt PHP
Không giống với Apache, Nginx không tích hợp hỗ trợ xử  lý các tệp PHP. Vì vậy, ta cài đặt một ứng dụng riêng biệt để xử lý các tệp PHP. Chẳng hạn như PHP FPM (`fastCGI process manager`)
  * Cài đặt module `php-fpm` và `php-mysql`

    `apt -y install php-fpm php-mysql`

Bây giờ ta cài đặt đủ các thành phần của LEMP. Nhưng vẫn cần thực hiện 1 số thay đổi về cấu hình để yêu cầu **Nginx** sử dụng PHP để xử lý nội dung
  * Mở thư mục: `/etc/nginx/sites-available/`. Trong ví dụ này tên máy chủ được đặt là `nam.xyz` (chúng ta có thể đặt tên bất kì). Tạo file `nam.xyz`

   `vi /etc/nginx/sites-available/nam.xyz`

  * Thêm nội dung sau vào file vừa tạo. Lưu ý chỉnh sửa tên đúng với trường hợp của bạn

  ```
  server {
    listen 80;
    root /var/www/html;
    index index.php index.html index.htm index.nginx-debian.html;
    server_name nam.xyz;

    location / {
            try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
    }

    location ~ /\.ht {
            deny all;
    }
}
  ``` 

**Trong đó**:
  
  * `listen` : Xác định cổng Nginx lắng nghe. Trong trường hợp này là cổng 80 (mặc định của HTTP)
  * `root` : xác định gốc lưu trữ các tệp của trang web 
  * `index` : thứ tự ưu tiên các file cấu hình, nếu chúng tồn tại
  * `server_name` : Xác định khối máy chủ nào được sử dụng. Nó trỏ đến tên miền hoặc IP public của bạn
  * `location /` :Kiểm tra sự tồn tại của các tệp khớp với uri. Nếu nginx không thể tìm thấy tệp thích hợp, nó sẽ trả về 404
  * `location ~ \.php$` : khai báo nơi xử lý PHP bằng cách trỏ Nginx vào tệp cấu hình fastcgi-php.conf và tệp php7.2-fpm.sock
  * `location ~/\.ht` : xử lí các tệp .htaccess mà Nginx không xử lí. Bằng cách deny all chỉ thị, nếu bất kì tệp .htaccess xảy ra để tìm đường vào tài liệu gốc thì chúng sẽ không được phục vụ cho khách truy cập

 * Tạo liên kết tượng trưng từ file cấu hình server ở trên (trong thư mục `/etc/nginx/sites-available/`) tới thư mục `/etc/nginx/sites-enabled/`

  `ln -s /etc/nginx/sites-available/nam.xyz /etc/nginx/sites-enabled/`

 * Hủy liên kết file cấu hình mặc định khỏi thư mục `sites-enabled/` :

  `unlink /etc/nginx/sites-enabled/default`
 
  **Lưu ý**: Nếu bạn cần khôi phục cấu hình mặc định, bạn có thể tạo liên kết lại với file mặc định:

   `ln -s/etc/nginx/sites-avalable/default /etc/nginx/sites-enabled/`

 * Kiểm tra xem file cấu hình mới có lỗi gì không:

  `nginx -t`
    
   * Nếu không có lỗi gì sẽ có thông báo như sau

 ```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
 ```

 * Reload Nginx:
  
   `systemctl reload nginx`

 * Tạo file `/var/www/html/info.php`
   
   `vi /var/www/html/info.php`

 * Thêm vào nội dung sau:
   ```
   <?php
   phpinfo():
   ```

 * Dùng trinh duyệt gõ vào thành URL
   
   `<địa chỉ ip>/info.php`

   ![Imgur](https://i.imgur.com/l9ldtIC.png)

 * Sau khi caì đặt xong, bạn nên xóa file `info.php`, vì nó có thể cho người dùng trái phép 1 số gợi ý về cấu hình của trang web và giúp họ cố gắng đột nhập

  `rm /var/www/html/info.php`

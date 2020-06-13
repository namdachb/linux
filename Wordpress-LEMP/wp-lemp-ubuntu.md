# Cài đặt WordPress với LEMP trên Ubuntu 18.04
## 1. Chuẩn bị
Đầu tiên, ta cần cài LEMP trên máy, xem cách cài LEMP tại [đây](https://github.com/namdachb/linux/blob/master/Wordpress-LEMP/C%C3%A0i-LEMP-ubuntu%2018.04.md)

## 2. Tạo DB và tài khoản WP
Ở bước chuẩn bị, mình đã cài Mariadb cho cơ sở dữ liệu. Bạn cũng có thể thao tác tương tự với MySQL
  
  * Đăng nhập vào tài khoản `root` của database:

   `mysql -u root -p`
   Terminal chuyển sang MariaDB

  * Ta tạo Database cho WP. Ở đây ta đặt là `wordpress`

   `CREATE DATABASE wordpress;`

  * Tạo tài khoản riêng để quản lý DB. Tên tài khoản: `user` , Mật khẩu: `password`
    
    `CREATE USER user@localhost IDENTIFIED BY 'password';`

  * Bây giờ ta sẽ cấp quyền quản lý cơ sở dữ liệu cho user mới tạo:

   `GRANT ALL PRIVILEGES ON wordpress.* TO user@localhost IDENTIFIED BY 'password';`

  * Sau đó xác thực lại những thay đổi về quyền:
   
   `FLUSH PRIVILEGES;`

  * Sau đó, ta thoát khỏi MariaDB:

   `exit`

## 3. Tải và cài đặt WordPress
 * Cài gói hỗ trợ

  `apt install -y php-curl php-gd php-intl php-mbstring php-soap php-xml php-xmlrpc php-zip`

 * Tải xuống WordPress phiên bản mới nhất: Nếu chưa cài đặt `wget` thì có thể cài bằng câu lệnh `apt -y install wget`

  `wget https://wordpress.org/latest.tar.gz`

  **Lưu ý** : Bạn cần để ý tới thư mục đang lưu trữ file wordpress đang được tải xuống. Ở đây mình lưu tại thư mục `/root`

 * Sau khi tải xong. Ta tiến hành giải nén file `latest.tar.gz`

  `tar xvfz latest.tar.gz`

  **Lưu ý: giải nén sẽ ra thư mục `wordpress` có đường dẫn `/root/wordpress`

 * Copy các file trong thư mục `wordpress` tới đường dẫn `/var/www/html`

  `cp -Rvf /root/wordpress/* /var/www/html`

## 4. Cấu hình WP
File cấu hình wordpress: `/var/www/html/wp-config.php`

 * Di chuyển tới thư mục `/var/www/html`

  `cd /var/www/html`

 * File cấu hình wordpress là `wp-config.php`. Tuy nhiên tại đây chỉ có file `wp-config-sample.php`. Tiến hành copy laị file cấu hình như sau:

  `cp wp-config-sample.php wp-config.php`

 * Chỉnh sửa file cấu hình `wp-config.php`. Chỉnh lại tên database, username, password đã đặt ở trên. (db_name: wordpress, username: user, pass: password) và lưu lại 

 ![Imgur](https://i.imgur.com/JuKx0sy.png)

## 5. Cài đặt giao diện
Trên trình duyệt gõ địa chỉ IP server vào thành URL.

Nhập các thông tin cần thiết rồi click **Install WordPress**

## 6.Phân quyền WordPress
Phân quyền thư mục wordpress cho user `www-data` để user này được phép tạo các thư mục và lưu trữ các tệp tin tải lên

```
chown -R www-data:www-data /var/www/html/*
chmod -R 755 /var/www/html/*
```

Như vậy là bạn đã có thể tiến hành upload ảnh và đăng bài viết lên trang wordpress của mình


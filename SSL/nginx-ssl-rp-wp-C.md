# Cấu hình Nginx với SSL làm Reverse Proxy cho WordPress
Theo mặc định, wordpress sẽ đi kèm với máy chủ web tích hợp của riêng nó. Điều này thuận tiện nếu ta chạy wordpress riêng tư hoặc sử dụng chỉ hướng đến sự nhanh chóng đạt được việc gì đó mà không quan tâm đến bảo mật. Tuy nhiên, khi ta có dữ liệu cần được bảo mật và muốn tăng hiệu suất web ta nên sử dụng một máy chủ web an toàn hơn như nginx

**Mô hình cài đặt**

![Imgur](https://i.imgur.com/amuJeOg.png)

**IP Planning**

![Imgur](https://i.imgur.com/qQ5w8wg.png)

**Thao tác thực hiện trên nginx server và server cài wordpress thực hiện với quyền `root` hoặc quyền sudo**

Máy nginx server:
 * Là máy chủ reverse proxy
 * Cài chứng chỉ SSL

Máy web server:
 * Cài đặt LAMP và WordPress

## Cấu hình trên Nginx Server

### 1. Cài đặt nginx
#### Cài đặt nginx

```
yum update
yum install epel-release
yum install nginx
```

#### Backup file cấu hình

`cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bk`

### 2.Tạo file config site

**Trước khi tạo file config cho địa chỉ truy cập, ta cần kiểm tra bản ghi DNS đối với địa chỉ sẽ sử dụng**:

`yum install -y bind-utils`

`nslookup wp.namdac.xyz`

![Imgur](https://i.imgur.com/06YnfCU.png)

Nếu chưa tạo một bản ghi tên miền, hãy truy cập zonedns để tạo một bản ghi

**Tạo file config site**:

Sau khi kiểm tra bản ghi đã tồn tại, ta sẽ tạo file config của tên miền, mỗi site sẽ được khai báo tương ứng với 1 file nằm trong thư mục `/etc/nginx/conf.d/`

Tạo file có tên là tên miền của mình với phần mở rộng là `conf` và thêm vào file nội dung sau

`vi /etc/nginx/conf.d/wp.namdac.xyz.conf`

Thêm vào file nội dung sau

```
server {
 server_name wp.namdac.xyz;
     location / {
         proxy_set_header   X-Real-IP             $remote_addr;
         proxy_set_header   X-Forwarded-For     $proxy_add_x_forwarded_for;

         proxy_set_header X-Forwarded-Proto https;
         proxy_pass http://10.10.34.117;
     }
 }
```

**Kiểm tra file cấu hình và khởi động lại nginx**

Kiểm tra file cấu hình
```
[root@nginx-sv ~]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Restart nginx

`systemctl restart nginx`

hoặc 

`nginx-s reload`

**Kiểm tra cài đặt**

Vào trình duyệt web và truy cập bằng tên miền để kiểm tra. ở đây mình sẽ truy cập bằng địa chỉ `wp.namdac.xyz`

![Imgur](https://i.imgur.com/csA36zw.png)

**Cài đặt chuyển hướng về tên miền**

Tuy đã truy cập thành công bằng tên miền, nhưng khi ta truy cập bằng IP thì thanh URL vẫn hiển thị là địa chỉ IP. Để chuyền về tên miền ta thực hiện các bước sau
 
 * Truy cập và đăng nhập vào Side Admin, vào tab **Settings** và chọn **General**

![Imgur](https://i.imgur.com/jSN6SIS.png)

 * Sau đó sửa địa chỉ ip mặc định thành tên miền tại **WordPress Address** và **Site Address** (Ở bài này `https://wp.namdac.xyz`) -> click Save Changes

### 3. Thiết lập chứng chỉ Let's Encrypt

Cài đặt Certbot

`yum install python-certbot-nginx-y`

Sinh SSL bằng Let's Encrypt cho site **wp.namdac.xyz**

`certbot --nginx -d wp.namdac.xyz`

OUTPUT

```
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator nginx, Installer nginx
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): nguyenthenam1711@gmail.com
Starting new HTTPS connection (1): acme-v02.api.letsencrypt.org

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf. You must
agree in order to register with the ACME server at
https://acme-v02.api.letsencrypt.org/directory
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(A)gree/(C)ancel: A

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing to share your email address with the Electronic Frontier
Foundation, a founding partner of the Let's Encrypt project and the non-profit
organization that develops Certbot? We'd like to send you email about our work
encrypting the web, EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y
Starting new HTTPS connection (1): supporters.eff.org
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for wp.namdac.xyz
Waiting for verification...
Cleaning up challenges
Deploying Certificate to VirtualHost /etc/nginx/conf.d/wp.namdac.xyz.conf
Redirecting all traffic on port 80 to ssl in /etc/nginx/conf.d/wp.namdac.xyz.conf

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Congratulations! You have successfully enabled https://wp.namdac.xyz

You should test your configuration at:
https://www.ssllabs.com/ssltest/analyze.html?d=wp.namdac.xyz
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/wp.namdac.xyz/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/wp.namdac.xyz/privkey.pem
   Your cert will expire on 2020-09-28. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le

 - We were unable to subscribe you the EFF mailing list because your
   e-mail address appears to be invalid. You can try again later by
   visiting https://act.eff.org.
```

Kiểm tra lại kết quả trong file config site

Sau khi sinh ssl cho site, ta kiểm tra lại file config để thấy sự thay đổi

`cat /etc/nginx/conf.d/wp.namdac.xyz`

```
server {
 server_name wp.namdac.xyz;
     location / {
         proxy_set_header   X-Real-IP             $remote_addr;
         proxy_set_header   X-Forwarded-For     $proxy_add_x_forwarded_for;

         proxy_set_header X-Forwarded-Proto https;
         proxy_pass http://10.10.34.117;
     }


    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/wp.namdac.xyz/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/wp.namdac.xyz/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}
server {
    if ($host = wp.namdac.xyz) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


 server_name wp.namdac.xyz;
    listen 80;
    return 404; # managed by Certbot

```

Khởi động lại nginx

`systemctl restart nginx`

## Cấu hình trên webserver cài WordPress

Sửa file `wp-config.php`

Tiến hành vào file `wp-config.php` tại `/var/www/html/wp-config.php`

`vi /var/ww/html/wp-config.php`

Thêm vào những dòng sau:

```
if ($_SERVER['HTTP_X_FORWARDED_PROTO'] == 'https'){ $_SERVER['HTTPS']='on'; } if (isset($_SERVER['HTTP_X_FORWARDED_HOST'])) { $_SERVER['HTTP_HOST'] = $_SERVER['HTTP_X_FORWARDED_HOST']; } 
```

File `/var/www/html/wp-config.php`

![Imgur](https://i.imgur.com/9L4lGyk.png)

Khởi động lại dịch vụ http

`systemctl restart httpd`

Truy cập trang web bằng địa chỉ  `https://wp.namdac.xyz/` đã có SSL

![Imgur](https://i.imgur.com/bMtfgQd.png)
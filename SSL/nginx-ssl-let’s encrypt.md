## Thiết lập chứng nhận SSL miễn phí từ Let's Encrypt cho các website
Có 2 cách tạo SSL:
 * Nhờ một tổ chức CA(Certification Authority) cấp, là tổ chức có độ tin cậy cao, được quyền cấp và chứng nhận SSL. Sẽ mất phí
 * Self-signed SSL: là server tự cấp, tự ký, tự xác thực (không an toàn và tin tưởng bằng nhờ bên thứ 3). Với cách này bạn sẽ không mất phí

Bài viết này sẽ nhận chứng chỉ SSL miễn phí từ Let's Encrypt và cài đặt SSL trên môi trường Nginx và CentOS 7. Tuy nhiên, chứng chỉ SSL được tạo theo cách này chỉ có tác dụng trong 90 ngày. Sau 90 ngày bạn sẽ cần update lại chứng chỉ

### 1.Chuẩn bị
 * 1 Server chạy hệ điều hành CentOS 7, đã cài đặt LEMP (cách cài Lemp tại [đây](https://github.com/namdachb/linux/blob/master/Wordpress-LEMP/LEMP-centos.md))

### 2. Các bước thực hiện
#### Cài đặt Certbot
Certbot là một công cụ dòng lệnh miễn phí giúp đơn giản hóa quy trình lấy và gia hạn chứng chỉ SSL từ Let's Encrypt và tự động kích hoạt HTTPS trên máy chủ của bạn 
 
 * Cài đặt các gói cần thiết

```
yum install -y install python36
yum -y install gcc mod_ssl python3-virtualenv redhat-rpm-config augeas-libs libffi-devel openssl-devel
```

 * Tải về certbot script

`curl -O https://dl.eff.org/certbot-auto`

 * Sau khi tải xuống hoàn tất, di chuyển file **certbot-auto** tới thư mục `usr/local/bin` và cấp quyền cho file **certbot-auto**

```
mv certbot-auto /usr/local/bin/certbot-auto
chmod 0755 /usr/local/bin/certbot-auto
```

#### Tạo Virtualhost
 * Tạo 1 file cấu hình virtual host(server block) cho tên miền **namdac.xyz**

`vi /etc/nginx/conf.d/namdac.xyz.conf`

Thêm vào nội dung bên dưới:
```
server {
      server_name hi.namdac.xyz;
      root /opt/nginx/namdac.xyz;

      location / {
         index index.html index.htm index.php;
      }

      access_log /var/log/nginx/namdac.access.log;
      error_log /var/log/nginx/namdac.error.log;

      location ~ \.php$ {
        include /etc/nginx/fastcgi_params;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;         }
}
```

 * Tạo 1 document root để đặt các tệp HTML của bạn

`mkdir -p /opt/nginx/namdac.xyz`

 * Thay đổi quyền với thư mục:

`chown -R nginx:nginx /opt/nginx/namdac.xyz`

 * Đặt file HTMl thử nghiệm vào thư mục gốc của tên miền web của bạn

```
echo "hihi @namdac.xyz" > /opt/nginx/namdac.xyz/index.html
```
 
 * Restart nginx service

`systemctl restart nginx`

#### Tạo/Cập nhật bản ghi Dns
 * Truy cập vào công cụ quản lý DNS hoặc trang quản lý tên miền của bạn để tạo bản ghi A tới tên miền 

![Imgur](https://i.imgur.com/tjhskW1.png)

 * Kiểm tra việc truyền DNS với câu lệnh nslookup: `yum install -y bind-utils`

![Imgur](https://i.imgur.com/etFert4.png)

#### Thiết lập nhận chứng chỉ miễn phí Let's Encrypt
 * Sử dụng câu lệnh `certbot` để tạo và cài đặt chứng chỉ Let's Encrypt

`usr/local/bin/certbot-auto --nginx`

**OUTPUT**

```
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator nginx, Installer nginx
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): nguyenthenam1711@gmail.com

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

Which names would you like to activate HTTPS for?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: hi.namdac.xyz
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel): 1
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for hi.namdac.xyz
Waiting for verification...
Cleaning up challenges
Deploying Certificate to VirtualHost /etc/nginx/conf.d/namdac.xyz.conf
Redirecting all traffic on port 80 to ssl in /etc/nginx/conf.d/namdac.xyz.conf

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Congratulations! You have successfully enabled https://hi.namdac.xyz

You should test your configuration at:
https://www.ssllabs.com/ssltest/analyze.html?d=hi.namdac.xyz
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/hi.namdac.xyz/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/hi.namdac.xyz/privkey.pem
   Your cert will expire on 2020-09-27. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot-auto
   again with the "certonly" option. To non-interactively renew *all*
   of your certificates, run "certbot-auto renew"
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

#### Redirect tất cả các truy vấn tới HTTPS
 * Thêm vào file `namdac.xyz.conf` một config server có nội dung như sau:

```
server {
   listen 80;
   server_name hi.namdac.xyz;
   return 301 https://namdac.xyz@request_uri;
 }
```

 * Restart service nginx

`systemctl restart nginx`

#### Cấu hình Firewall
Cấu hình firewall cho phép các yêu cầu HTTPS

```
firewall-cmd --permanent --add-port=443/tcp

firewall-cmd -reload
```

#### Xác nhận chứng nhận Let's Encrypt
Từ trình duyệt ta nhập vào địa chỉ: `http://your_domain` để kiểm tra. Trình duyệt sẽ tự động redirect yêu cầu từ HTTP sang HTTPS

![Imgur](https://i.imgur.com/haQsFBF.png)

#### Kiểm tra chứng nhận SSL
Kiểm tra chứng chỉ SSL của bạn để biết bất kỳ vấn đề nào và xếp hàng bảo mật của nó bằng cách truy cập URL bên dưới https://www.ssllabs.com/ssltest/analyze.html?d=your_domain

![Imgur](https://i.imgur.com/XPjgOmT.png)

#### Thiết lập gia hạn tự động
 * Sử dụng lệnh:

```
echo "0 0,12 * * * root python -c 'import random; import time; time.sleep(random.random() * 3600)' && /usr/local/bin/certbot-auto renew" | sudo tee -a /etc/crontab > /dev/null
```

 * Bạn cũng có thể mô phỏng quá trình gia hạn chứng chỉ bằng lệnh bên dưới để đảm bảo quá trình gia hạn diễn ra suôn sẻ

`/usr/local/bin/certbot-auto renew --dry-run`


Bài viết tham khảo tại [đây](https://www.itzgeek.com/how-tos/linux/centos-how-tos/how-to-setup-lets-encrypt-ssl-certificate-with-nginx-on-rhel-8-centos-7-rhel-7.html)
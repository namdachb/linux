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

![Imgur](https://i.imgur.com/ADKJq6I.png)

 * Kiểm tra việc truyền DNS với câu lệnh nslookup: `yum install -y bind-utils`

![Imgur](https://i.imgur.com/X0bKP3R.png)

#### Thiết lập nhận chứng chỉ miễn phí Let's Encrypt
 * Sử dụng câu lệnh `certbot` để tạo và cài đặt chứng chỉ Let's Encrypt

`usr/local/bin/certbot-auto --nginx`


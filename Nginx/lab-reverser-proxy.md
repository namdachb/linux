# Thực hành Nginx Reverse Proxy
**Mô hình** 

![Imgur](https://i.imgur.com/CILsIYc.png)

Trong mô hình này mình sẽ thực hiện các cấu hình:
 * Cài đặt máy webserver sử dụng Apache, sau đó up các file ảnh hoặc tạo ra một site html
 * Cài đặt máy Nginx làm chức năng reverse proxy
 * Cấu hình file host ở các máy người dùng với domain namdac123.com trỏ về IP của máy chủ Nginx

**IP Planning**

IP Nginx: 192.168.213.174 IP WebServer: 192.168.213.148

**Triển khai**
### 1. Cài đặt Apache
**Cài đặt các gói cơ bản**

 * Thực hiện trên máy `192.168.213.148`
 * Login với quyền root và thực hiện cài đặt các gói hỗ trợ

```
yum update -y
yum install -y epel-release
yum install -t wget byobu
```

**Cài đặt Apache**
 * Cài đặt httpd

`yum install -y httpd`

 * Khởi động httpd và kích hoạt http khi reboot hệ điều hành

```
systemctl start httpd
systemctl enable httpd
```

 * Kiểm tra hoạt động của httpd

`systemctl status httpd`

**Tạo một trang web có chứa ảnh hoặc các file tĩnh**
 
 * Dùng lệnh dưới tải file `index.html` có chứa nội dung ảnh về

`wget -O /var/www/html/index.html https://gist.githubusercontent.com/congto/359e04f735162a987daf58d3f8d44fb6/raw/51ccab89265bff5717084af1212640dae6bbfa92/indext.html`

 * Hoặc các bạn có thể `vi` vào `var/www/html/index.html` để chỉnh sửa

 ![Imgur](https://i.imgur.com/Ph5u2JH.png)

**Truy cập website với địa chỉ IP của máy webserver**

Mở trình duyệt và truy cập địa chỉ IP, nội dung web sẽ hiển thị trên màn hình là thành công
  
  `http://192.168.213.148`

Lưu ý:
 * Do chưa có domain nên ta sẽ truy cập bằng IP
 * Nếu muốn giả lập domain thì có thể sửa file hosts trên windows hoặc trên linux để dùng một tên miền nào đó. Bước này ta sẽ xử lý sau khi cài xong nginx. 

### 2. Cài đặt Nginx
 * Thực hiện trên máy chủ Nginx `192.168.213.174`
 * Đăng nhập với quyền `root` và thực hiện cài đặt các gói bổ trợ

```
yum update -y
yum install -y epel-release
yum install -y wget byobu
```

 * Cài đặt Nginx, chúng ta cài nginx từ package

`yum install -y nginx`

 * Khởi động nginx và kích hoạt chế độ khởi động cùng OS

```
systemctl start nginx
systemctl enable nginx
```

 * Kiểm tra hoạt động của nginx

`systemctl status nginx`

### 3. Cấu hình nginx làm proxy 
Trước khi cấu hình Nginx làm reverse proxy thì việc truy cập vào website của máy 192.168.213.174 sẽ thông qua địa chỉ IP hoặc domain được trỏ ở file host. Trong phần này tôi sẽ khai báo cấu hình cho Nginx làm chức năng reverse proxy cho máy web server. Tức là người dùng sẽ truy cập vào IP hoặc domain được trỏ về IP 192.168.213.148, lúc này Nginx sẽ làm nhiệm vụ điều phối truy cập vào máy 192.168.213.174 để lấy nội dung trang web trả về cho người dùng

Các bước chi tiết như sau
 * Sao lưu file cấu hình mặc định của nginx

`cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bka`

Cấu hình Nginx reverse proxy
 * Di chuyển vào thư mục `/etc/nginx/conf.d` để khai báo file cấu hình làm nhiệm vụ reverse proxy

`cd /etc/nginx/conf.d/`

 * Tạo file với đuôi mở rộng là `.conf`, với một trong 2 cách

Cách 1: Tạo file bằng vi hoặc vim file tên là `namdac123.com.conf`

`vi /etc/nginx/conf.d/namdac123.com.conf`

 * Nội dung của file `namdac123.com.conf` sẽ là:
```
server {
    listen 80;
    server_name namdac123.com;
    access_log /var/log/nginx/namdac.access.log;
    error_log /var/log/nginx/namdac.error.log;

    location / {
        proxy_pass http://192.168.213.148:80/;
        # Input any other settings you may need that are not already contained in the default snippets.
    }
}
```

Cách 2: Tạo file bằng lệnh `cat`
```
cat << EOF> /etc/nginx/conf.d/namdac123.com.conf
server {
    listen 80;
    server_name namdac123.com;
	access_log /var/log/nginx/namdac.access.log;
	error_log /var/log/nginx/namdac.error.log;

    location / {
        proxy_pass http://192.168.213.148:80/;
        # Input any other settings you may need that are not already contained in the default snippets.
    }
}
EOF
```

 * Lưu ý trong dòng `proxy-pass` ta sẽ khai báo về địa chỉ của máy cài httpd
 * Sau khi khai báo xong, kiểm tra lại xem khai báo này đã đúng chưa bằng lệnh 

`nginx -t`

 * Nếu kết quả như ảnh dưới là thành công, tiến hành khởi động lại nginx hoặc nạp lại file cấu hình

 ![Imgur](https://i.imgur.com/22Wyu6K.png)

`nginx -s reload`

hoặc

`systemctl restart nginx`

Đối với môi trường thật thì việc cấu hình này sẽ có IP Public và domain chuẩn, nhưng do đây là môi trường lab nên ta dùng thủ thuật nhỏ để trỏ domain thông qua file host. Cụ thể là sẽ mở file host trê windows hoặc `/etc/hosts` của linux để khai báo file host. Mục tiêu là nói cho client biết địa chỉ `namdac123.com` sẽ nằm ở máy chủ nào

 * Đối với windows: Ta thêm dòng `192.168.213.133 namdac123.com` vào file `C:\Windows\System32\drivers\etc\hosts`

![Imgur](https://i.imgur.com/YT3I9Wr.png)

 * Đối với linux ta khai báo ở file `/etc/hosts`

Lưu ý rằng mặc dù website được đặt trên máy chủ `192.168.213.148` nhưng domain sẽ được trỏ về `192.168.213.174`, đây là việc sử dụng tính năng reverse proxy của nginx để điều hướng các kết nối của người dùng và máy chủ gốc(origin)

 * Kiểm tra nội dung web và xem reverse proxy hoạt động hay chưa bằng cách truy cập vào địa chỉ namdac123.com Trong ảnh này tôi sử dụng thêm mode của phím F12 để có thể show được các kết nối từ client tới webserver. Ta quan sát thấy nội dung của web được hiển thị và địa chỉ IP của Nginx reserver proxy.

![Imgur](https://i.imgur.com/FMJYunP.png)

![Imgur](https://i.imgur.com/Poodtk9.png)

Hoặc chúng ta có thể sử dụng phần mềm Mobaxterm để sử dụng lênh `curl -I` để kiểm chứng, kết quả như trong hính dưới 

![Imgur](https://i.imgur.com/oIhO9tR.png)
## Các khái niệm trong tệp tin cấu hình của NGINX
Bài viết tìm hiểu về cấu trúc và một số khái niệm trong tệp tin cấu hình mặc định của Nginx. Và một số bước cần thiết giúp chúng ta sử dụng config file của Nginx một cách hiệu quả hơn

## 1. Cấu trúc và cách sử dụng tập tin cấu hình
### 1.1 Cấu trúc
 * Tát cả file cấu hình của nginx nằm trong thư mục - `/etc/nginx`
 * File cấu hình chính của nginx là - `/etc/nginx/nginx.conf`
 * Document root directory - `/usr/share/nginx/html`
 * Nginx bao gồm các module được điều khiển bởi các directive trong file cấu hình. "Directive" được định nghĩa như 1 **instruction**(chỉ dẫn) hay **direct**. Directives được chia thành các directive đơn giản và các block directive 
    * Cấu trúc của 1 directive đơn giản gồm tên và tham số được phân tách bởi dấu cách và kết thúc bằng dấu chấm phẩy (;). Ví dụ về 1 directive đơn giản:

        `worker_processses 1;`

    * Một block directive có cấu trúc tương tự như một directive đơn giản nhưng thay vì sử dụng dấu ; nó sẽ sử dụng cặp dấu {} để bắt đầu và kết thúc 1 blcok directive

### 1.2 Cách sử dụng config file hợp lý và hiệu quả
 * Tạo 1 file cấu hình riêng cho mỗi tên miền sẽ giúp server dễ quản lý và hiệu quả hơn
 * NGINX không có Virtual host, thay vào đó là `Server Blocks` sử dụng `server_name` và nghe các chỉ thị để liên kết với các tcp sockets. Tất cả các file server block phải có định dạng là `.conf` và được lưu trong thư mục `/etc/nginx/conf.d` hoặc `/etc/nginx/conf`
 * Nếu bạn là có 1 domain là `mydomain.com` thì bạn nên đặt tên file cấu hình là `mydomain.con.cnf`
 * Các file nhật ký Nginx (`access.log` và `error.log`) được đặt trong thư mục `/var/log/nginx/`. Nên có một tệp nhật ký `access` và `error` khác nhau cho mỗi server block
 * Bạn có thể đặt document root directory của tên miền của bạn đến bất kỳ vị trí nào bạn muốn. Một số vị trí thường được dùng cho webroot bao gồm:
    * /home/<user_name>/<site_name>
    * /var/www/<site_name>
    * /var/www/html/<site_name>
    * /opt/<site_name>
    * /usr/share/nginx/html
    
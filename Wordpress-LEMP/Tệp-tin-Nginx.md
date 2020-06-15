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

### 1.3 Các thao tác cần thực hiện trước và sau khi chỉnh sửa config file
 * Trước khi thay đổi cấu hình, sao lưu lại file cấu hình:

  `cp /etc/nginx/conf/nginx.conf //etc/nginx/conf/nginx.conf.backup`

 * Định kì sao lưu tập tin cấu hình nginx cp `/etc/nginx/conf/nginx.conf`

  `/etc/nginx/conf/nginx.conf.$(date "+%b_%d_%Y_%H.%M.%S")`

 * Sau khi thực hiện thay đổi cấu hình trong file cấu hình, restart lại service
  
  `systemctl restart nginx`

**Chú ý** 
 * Tất cả những dòng có dấu `**#**` phía trước là những dòng chú thích (comment) được sử dụng để giải thích những khối lệnh dùng làm gì hoặc để lại ý kiến làm thế nào chỉnh sửa giá trị
 * Ngoài ra, bạn cũng có thể thêm riêng những comment theo ý của mình. Bạn có thể cho đoạn mã đó được kích hoạt bằng cách loại bỏ các #
 * Cài đặt bắt đầu với những tên biến và sau đó một đối số hay một loại các đối số cách nhau bởi dấu cách
 * Một số thiết lập được đặt trong một cặp dấu ngoặc nhọn ({}). Các dấu ngoặc nhọn có thể được lồng vào nhau cho nhiều khối lệnh, cần nhớ là khi đã mở ngoặc nhọn thì phải nhớ đóng lại nếu không sẽ dẫn tới nginx không chạy được
 * Sử dụng tab hay space để phân cấp đoạn mã sẽ giúp dễ dàng chỉnh sửa hay tìm ra lỗi
 
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

    * 
# Tổng quan Nginx
**Nginx** đọc là (engine-x) là một phần mềm web server mã nguồn mở nổi tiếng. Ban đầu nó dùng để phục vụ wen HTTP. Tuy nhiên, ngày nay nó cũng được dùng làm reverse proxy, HTTP load balancer và email proxy như IMAP, POP3 và SMTP

Nginx cố thể triển khai nội dung của các trang web động bằng cách sử lý 

## NGINX hoạt động như thế nào?
**nginx** được phát triển cho các mục đích tối ưu sử dụng (ram) bộ nhớ thấp nhưng phục vụ được nhiều kết nối đồng thời cao hơn. 

Nginx hoạt động theo kiến trúc không đồng bộ (asynhronous), hướng sự kiện (event driven). Kiến trúc này có thể hiểu là những threads (chủ đề) tương đồng nhau sẽ được quản lý trong một tiến trình (process) và mỗi tiến trình hoạt động chia các thực thể nhỏ hơn gọi là worker connections. Cả bộ đơn bị này chịu trách nhiệm xử lý các threads.

Worker connections sẽ gửi các truy vấn cho một worker processs, worker process sẽ gửi nó tới process cha (master process). Cuối cùng, master process sẽ trả kết quả cho những yêu cầu đó

Điều này có vẻ đơn giản, một worker connection có thể xử lý đến 1024 yêu cầu tương tự. VÌ vậy, nginx có thể xử lý hàng ngàn yêu cầu mà không gặp rắc rối gì. Đây cũng là lý do vì sao phần mềm này tỏ ra hiệu quả hơn khi hoạt động trên môi trường thương mại điện tử, trình tìm kiếm và cloud storage

## Các tính năng của Nginx
 * Có thể xử lý hơn 10.000 kết nối cùng lúc với bộ nhớ thấp
 * Phục vụ tập tin tĩnh (static files) và lập chỉ mục tập tin
 * Tăng tốc proxy ngược bằng bộ nhớ đệm (cache), cân bằng tải đơn giản và khả năng chịu lỗi
 * Hỗ trợ mã hóa SSL và TLS
 * Cấu hình linh hoạt, lưu lại nhật ký truy vấn
 * Hạn chế tỷ lệ đáp ứng truy vấn
 * Giới hạn số kết nối đồng thời hoặc truy vấn từ 1 địa chỉ
 * Khả năng nhúng mã PERL
 * Hỗ trọ và tương thích với IPv6
 * Hỗ trợ WebSockets
 * Hỗ trợ truyền tải file FLV và MP4

## Tìm hiểu cấu hình File-log trên Nginx webserver
Với webserver chúng ta cần quan tâm đến 2 dạng log:
 * Log truy cập (access log) ghi lại các thông tin người dùng truy cập vào website
 * Log lỗi (error log) ghi lại các cảnh báo các lỗi xảy ra với dịch vụ liên quan web server

Log là công cụ giám sát hệ thống rất tốt (monoitoring systems) không thể thiếu với sysadmin quản tri hệ thống

Theo mặc định Nginx Server lưu file log này tại `/var/log/nginx`

### 1. Tìm hiểu Access Log Nginx
Đầu tiên chúng ta tìm hiểu log truy cập (Access Log) tương tụ CustomLog trong **Web Server Apache**. Log này rất cần thiết đặc biệt khi website của chúng ta bị tấn công **DDOS**, nhớ nó chúng ta có thể xác định được nguồn tấn công, tần xuất và quy mô có action phù hợp như chặn theo User Agent hay IP nguồn

Nginx cung cấp cho chúng ta khả năng tùy biến access log để xuất ra file theo định dạng mà chúng ta muốn

Để thay đổi định dạng access log chúng ta chỉ cần làm việc với directive **log_format**, chỉ thị này mặc định nằm trong **block http {...}**. Để xem được log format chúng ta vào `/etc/nginx/nginx.conf` áp dụng cho cả hệ điều hành CentOS và Ubuntu

Mẫu log_format trong nginx:

```
 log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
```

Chỉ thị **log_format** để định dạng access log cho nginx, ý nghĩa các biến như sau:
 * `main` tên gọi của log format, chúng ta có thể tạo nhiều định dạng log khác nhau rồi gán cho một cái tên bất kỳ cũng được
 * `$remote_addr` địa chỉ IP truy cập web site của chúng ta
 * `$remote_user` ghi lại tài khoản truy cập web nếu trang của chúng ta có xác thực người dùng, đa số là không dùng chúng ta có thể bỏ đi
 * `$time_local` thời gian người dùng truy cập
 * `$request` đoạn đàu của request
 * `$status` trạng thái của response
 * `$body_bytes_sent` kích thước body mà server response
 * `$http_referer` URL được tham chiếu
 * `$http_user_agent` thông tin trình duyệt, hệ điều hành mà người dùng truy cập
 * `$http_x_forwarded_for` được ghi vào log nếu webserver detech người dùng truy cập qua proxy server

Để hiểu rõ hơn chúng ta tìm hiểu một mẫu **access log** cụ thể

```
192.168.213.1 - - [12/Jun/2020:05:02:22 -0400] "POST /wp-admin/admin-ajax.php HTTP/1.1" 200 109 "http://192.168.213.148/wp-admin/post-new.php" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) coc_coc_browser/86.0.172 Chrome/80.0.3987.172 Safari/537.36" "-"
```
Ý nghĩa:
 * 192.168.213.1 : Là IP của $remore_addr
 * `-` `-` : là $remote_user, không có gì vì web site không có thực
 * [12/Jun/2020:05:02:22 -0400] : là $time_local
 * "POST /wp-admin/admin-ajax.php HTTP/1.1" : chính là $request
 * 200 : Đây là truy cập thành công webserver phản hồi lại code 200 OK
 * 109 : kích thước body không bao gồm header mà server trả về cho người truy cập
 * "http://192.168.213.148/wp-admin/post-new.php" : là đoạn $http_referer
 * "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) coc_coc_browser/86.0.172 Chrome/80.0.3987.172 Safari/537.36" : thông tin $http_user_agent

Với log format như trên mình thấy khá đầy đủ thông tin cần thiết để theo dõi truy cập của 1 trang web

### 2. Tìm hiểu ERROR Log Nginx
Log này để ghi lại thông tin các lỗi cài đặt cấu hình hay đơn giản chỉ là những cảnh báo giữa **Web Server Nginx** và các dịch vụ của nó

Để sử dụng error log chúng ta thêm chỉ thị error_log vào block http {...}

```
server {
error_log /var/log/nginx/error.log error;
...
}
```

Cú pháp chung của error log:

`error_log log_file log_level`

Cái mà chúng ta cần quan tâm là **log_level**
 * `emerg` : log ở level này mang tính khẩn cấp, dạng như server sắp sập
 * `alert` : cảnh báo các vấn đề cần được xử lý ngay
 * `crit` : các vấn đề quan trọng nhưng không nhất thiết phải xử lý ngay lập tức, để theo dõi thêm 
 * `error` : ghi lại thông tin lỗi như đăng nhập hoặc cấu hình sai, mức độ thấp hơn crit
 * `warn` : ở mức độ cảnh báo không phải lỗi
 * `notice` : để thông báo cái gì đó
 * `info` : ghi thông tin hệ thống, không có gì cả
 * `debug` : ghi lại tất cả mọi thứ, dùng để dò lỗi

Trong tất cả mức độ ưu tiên debug có mức độ cao nhất, nó bao gồm tất cả các thông tin của các level khác

## Thống kê số lần truy cập của các IP vào website
### 1. Hiển thị tất cả địa chỉ IP có trong file access log

`grep -o "[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+" /var/log/nginx/access.log | sort | uniq`

```
[root@localhost ~]# grep -o "[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+" /var/log/nginx/access.log | sort | uniq
192.168.213.1
192.168.213.148
80.0.3987.172
```

### 2. Hiển thị các địa chỉ có trong file access log và số lần xuất hiện của chúng
**Cách 1** :

Đối với cách này sẽ hiển thị tất các các địa chỉ có trong file access log bao gồm cả các địa chỉ có trong User-Agent

**Sắp xếp các địa chỉ theo thứ tự tăng dần**

`grep -o "[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+" /var/log/nginx/access.log | sort | uniq -c | sort -n`

```
[root@localhost ~]# grep -o "[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+" /var/log/nginx/access.log | sort | uniq -c | sort -n
    102 192.168.213.1
    102 80.0.3987.172
    104 192.168.213.148
```

**Sắp xếp các địa chỉ theo thứ tự giảm dần**

`grep -o "[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+" /var/log/nginx/access.log | sort | uniq -c | sort -nr`

```
[root@localhost ~]# grep -o "[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+" /var/log/nginx/access.log | sort | uniq -c | sort -nr
    104 192.168.213.148
    102 80.0.3987.172
    102 192.168.213.1
```

**Cách 2** :

Còn đối với cách này sẽ chỉ hiển thị những địa chỉ IP có ở phần đầu của access log (X-FORWARDED_FOR)

`awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr`

```
[root@localhost ~]# awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr
    102 192.168.213.1
      1 192.168.213.148
```

# Tổng quan Nginx
**Nginx** đọc là (engine-x) là một phần mềm web server mã nguồn mở nổi tiếng. Ban đầu nó dùng để phục vụ wen HTTP. Tuy nhiên, ngày nay nó cũng được dùng làm reverse proxy, HTTP load balancer và email proxy như IMAP, POP3 và SMTP

Nginx cố thể triển khai nội dung của các trang web động bằng cách sử lý 

### NGINX hoạt động như thế nào?
**nginx** được phát triển cho các mục đích tối ưu sử dụng (ram) bộ nhớ thấp nhưng phục vụ được nhiều kết nối đồng thời cao hơn. 

Nginx hoạt động theo kiến trúc không đồng bộ (asynhronous), hướng sự kiện (event driven). Kiến trúc này có thể hiểu là những threads (chủ đề) tương đồng nhau sẽ được quản lý trong một tiến trình (process) và mỗi tiến trình hoạt động chia các thực thể nhỏ hơn gọi là worker connections. Cả bộ đơn bị này chịu trách nhiệm xử lý các threads.

Worker connections sẽ gửi các truy vấn cho một worker processs, worker process sẽ gửi nó tới process cha (master process). Cuối cùng, master process sẽ trả kết quả cho những yêu cầu đó

Điều này có vẻ đơn giản, một worker connection có thể xử lý đến 1024 yêu cầu tương tự. VÌ vậy, nginx có thể xử lý hàng ngàn yêu cầu mà khong gặp rắc rối gì. Đây cũng là lý do vì sao phần mềm này tỏ ra hiệu quả hơn khi hoạt động trên môi trường thương mại điện tử, trình tìm kiếm và cloud storage
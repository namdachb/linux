## Tổng quan HTTP
### 1.HTTP là gì?
HTTP (Hypertext Transfer Protocol) là giao thức truyền tải siêu văn bản, là 1 trong các giao thức chuẩn của mạng internet

HTTP truyền tải thông tin cho dữ liệu giữa Web Server và Web Client trong mô hình dùng cho World Wide Web (WWW). Nơi các tài liệu siêu văn bản (hypertext) bao gồm các siêu liên kết (hyperlinks) đến tài nguyên mà người dùng có thể dễ dàng truy cập

HTTP là giao thức thuộc tầng ứng dụng, trên cơ sở TCP/IP. Cổng mặc định mà HTTP sử dụng là 80

**Lịch sử  và phiên bản**

Phát triển HTTP được Tim Berners-lee bắt đầu tại CERN năm 1989. Từ sự phối hợp của Interner Engineering Task Force (IETF) và World Wide Web Consortium(WWWC) và sau đó là IETF

HTTP/1.1 lần đầu tiên được ghi nhận bởi RFC 2068 năm 1997. Đến năm 2014 được thay thế bằng RFC 7230

HTTP/2 là một phiên bản được ra mắt năm 2015. Sử dụng lớp bảo mật TLS để tăng mức độ bảo mật

HTTP/3 là sự kế thừa từ sản phẩm HTTP/2 và thay đổi bằng cách sử dụng UDP thay cho TCP như trước

|**HTTP/1.0**|**HTTP/1.1**|**HTTP/2**|
|-|-|-|
|Khi truyền tải thông tin dữ liệu qua kết nối TCP thì mỗi một kết nối chỉ truyền tải một tài nguyên độc lập gây nên việc tốn thời gian để truyền tải thông tin dữ liệu một trang web|Khi truyền tải thông tin thì có thể truyền tải nhiều thông tin dữ liệu qua cùng một kết nối điều đó làm giảm thiểu việc phải tạo ra nhiều kết nối điều đó làm giảm thiểu việc phải tạo ra nhiều kết nối TCP nhưng khi truyền tải dữ liệu cần phải đùng thự tự vì khi này không thể nhận biết được thứ tự các yêu cầu. Nên các response sẽ phải tuân thủ quy tắc xử lý lần lượt|Đã khắc phục bằng cách đánh dấu các response khác nhau và thứ tự của chúng nên việc xử lý theo thứ tự đã được giải quyết. Và HTTP/2 cũng đã nén dữ liệu xuống kkhoong còn gửi dưới dạng text như HTTP/1 khiến nó nhẹ hơn|


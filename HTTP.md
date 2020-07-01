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
|dữ liệu truyền đi dưới dạng text|dữ liệu truyền đi dưới dạng text|dữ liệu truyền đi dưới dạng nhị phân|

### 2.Các đặc trưng cơ bản của HTTP
 * **HTTP là giao thức connectionless** (kết nối không liên tục) : ví dụ như HTTP Client khởi tạo 1 request, Client sẽ ngắt kết nối từ Server và đợi cho 1 phản hồi, Server xử lý request và thiết lập lại sự kết nối với Client để gửi phản hồi trở lại
 * **HTTP là một phương tiện độc lập** : bất cứ loại dữ liệu nào cũng có thể được gửi bởi HTTP, miễn là Server và Client biết cách kiểm soát nội dung dữ liệu. Nó được yêu cầu cho Client cũng như Server để xác định kiểu nội dung bởi sử dụng kiểu MIME (Multipurpose Internet Mail Extensions - Giao thức mở rộng thư điện tử Internet đa mục đích) thích hợp
 * **HTTP là stateless** (không trạng thái) : request hiện tại không biết những gì đã hoàn thành trong request trước đó

### 3. Các khái niệm trong HTTP
**Session**

 * Trong giao thức HTTP thì một phiên của nó bao gồm 3 giai đoạn 
   * Client thiết lập kết nối TCP tới Server (hoặc 1 kết nối thích hợp khác nếu tầng vận chuyển không sử dụng TCP)
   * Client gửi request và đợi phản hồi
   * Server nhận được request sẽ phản hồi lại cho Client 
 * Session được lưu trên máy Server. Nó chứa dữ liệu người sử dụng web vào 1 file trên server

**URL(Uniform Resource Locator)**

1 URl được sử dụng để xác định duy nhất một tài nguyên trên web

Cấu trúc URL

 `protocol://hostname:port/path-and-file-name`

Trong đó:
 * `protocol` : giao thức tầng ứng dụng được sử dụng bởi client và server
 * `hostname` : tên DNS domain
 * `port` : cổng TCP để Server lắng nghe REQUEST từ Client
 * `path-and-file-name` : tên và vị trí của tài nguyên 


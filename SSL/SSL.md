## Tổng quan SSL
### 1.SSL là gì?

SSL là viết tắt của Secure Sockets Layer, một công nghệ tiêu chuẩn cho phép thiết lập kết nối được mã hóa an toàn giữa máy chủ web(host) và trình duyệt web(client). Kết nối này đảm bảo rằng dữ liệu được truyền giữa host và client được duy trì một cách riêng tư đáng tin cậy

SSL hiện đã được sử dụng bởi hàng triệu trang web để bảo vệ các giao dịch trực tuyến của họ với khách hàng. Nếu bạn đã từng truy cập một trang web sử dụng **https://** trên thanh địa chỉ nghĩa là bạn đã tạo một kết nối an toàn qua SSL

### 2.SSL làm việc như thế nào?

![Imgur](https://i.imgur.com/J4zE5t4.png)

**Những gì xảy ra khi một máy tính kết nối với một website đã được chứng thực?**


![Imgur](https://i.imgur.com/p29njvn.png)

### 3.Tại sao nên sử dụng SSL?
Ví dụ: chúng ta đăng kí domain để sử dụng các dịch vụ website, email... luôn có những lỗ hổng bảo mật cho hacker tấn công, SSL bảo vệ website và khách hàng của chúng ta
 * **Bảo mật dữ liệu**: dữ liệu được mã hóa và chỉ người nhận đích thực mới có thể giải mã
 * **Toàn vẹn dữ liệu**: dữ liệu không bị thay đổi bởi tin tặc
 * **Chống chối bỏ**: đối tượng thực hiện gửi dữ lieeje không thể phủ nhận dữ liệu của mình

### 4.Lợi ích khi sử dụng SSL
 * Xác thực website, giao dịch
 * Nâng cao hình ảnh, thương hiệu và uy tín doanh nghiệp
 * Bảo mật các giao dịch giữa khách hàng và doanh nghiệp, các dịch vụ truy nhập hệ thống
 * Bảo mật webmail và các ứng dụng như Outlook Web Access, Exchange, và Office Communication Server
 * Bảo mật các ứng dụng ảo hóa như Citrix Delivery Platform hoặc các ứng dụng điện toán đám mây
 * Bảo mật dịch vụ FTP
 * Bảo mật truy cập control panel
 * Bảo mật các dịch vụ truyền dữ liệu trong mạng nội bộ, file sharing, extranet
 * Bảo mật VPN Access Servers, Citrix Access Gateway...

Website không được xác thực và bảo mật sẽ luôn ẩn chứa nguy cơ bị xâm nhập dữ liệu, dẫn đến hậu quả khách hàng không tin tưởng sử dụng dịch vụ

### 5. CA là gì?
Certificate Authority (CA): là tổ chức phát hành các chứng thực các loại chứng thư số cho người dùng, doanh nghiệp, máy chủ(server), mã code, phầm mềm. Nhà cung cấp chứng thực số đóng vai trò là bên thứ ba (được cả hai bên tin tưởng) để hỗ trợ cho quá trình trao đổi thông tin an toàn

### 6. Các định nghĩa, thuật ngữ SSL thường gặp

#### DV-SSL:
Domain Validation (DV): chứng chỉ xác thực tên miền dành cho các khách hàng cá nhân với khả năng mã hóa cơ bản với giá rẻ. DV-SSL chỉ yêu cầu xác minh quyền sở hữu tên miền. Thời gian đăng ký và xác minh rất nhanh

#### OV-SSL
Chứng chỉ xác thực tổ chức (Organization Validation SSL): OV SSL dành cho các tổ chức và doanh nghiệp có độ tin cậy cao. Ngoài việc xác minh quyền sở hữu tên miền còn phải xác minh doanh nghiệp đăng ký đang tồn tại và hoạt động bình thường. Tên doanh nghiệp cũng sẽ được hiển thị chi tiết trên chứng chỉ OV được cấp

#### EV-SSL
Extended Validation(EV): cho khách hàng của bạn thấy Website đang được sử dụng chứng thư SSL có độ bảo mật cao nhất và được rà soát pháp lý kỹ càng. Với thanh địa chỉ sang màu xanh với hiển thị đầy đủ thông tin của công ty, cung cấp một cấp độ cao hơn tin tưởng vào website của bạn

#### Wildcard SSL
Wildcard SSL dành cho các website có nhu cầu sử dụng SSL cho nhiều subdomain khác nhau. Wildcard SSL khác với các loại SSL bình thường là có thể chạy không giới hạn tên miền phụ với một chứng chỉ ssl duy nhất


### 7.SSL có nhược điểm không?
SSL có nhược điểm:
 * Chi phí là điều dễ thấy nhất. Chi phí đến từ việc thiết lập một cơ sở  hạ tầng đáng tin cậy và xác nhận danh tính
 * Hiệu suất : những thông tin được truyền đi sẽ được mã hóa, điều này sẽ tiêu tốn nhiều tài nguyên máy chủ hơn so với thông tin không được mã hóa

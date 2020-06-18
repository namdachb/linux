## Tìm hiểu reverse-proxy-nginx
Một proxy server hoạt động với vai trò trung gian giữa máy khách và máy chủ khác. Proxy server lấy tài nguyên từ máy chủ mà bạn muốn kết nối và gửi nó cho bạn để xem. Một reverse proxy hoạt động theo cùng một cách, ngoại trừ vai trò bị đảo ngược. Khi bạn yêu cầu thông tin từ máy chủ, reverse proxy sẽ giữ yêu cầu và gửi nó đến máy chủ backend thích hợp. Điều này cho phép quản trị viên hệ thống sử dụng máy chủ cho nhiều ứng dụng, cũng như đảm bảo luồng lưu lượng truy cập mượt mà hơn giữa máy khách và máy chủ

Sử dụng phổ biến cho một máy chủ reverse proxy bao gồm:
  * **Load balancing**(cân bằng tải) -Máy chủ proxy đảo ngược có thể hoạt động như người điều hướng, ngồi trước máy chủ phụ trợ của bạn và phân phối các yêu cầu của khách hàng qua một nhóm máy chủ theo cách tối đa hóa tốc độ và sử dụng dung lượng trong khi đảm bảo máy không có máy chủ nào bị quá tải, điều này có thể làm giảm hiệu suất. Nếu một máy chủ ngừng hoạt động, **load balancing** sẽ chuyển hướng lưu lượng đến các máy chủ trực tuyến còn lại
  * **Web acceleration**(tăng tốc web) -Proxy ngược có thể nén dữ liệu không giới hạn, cũng như lưu trữ nội dụng thường được yêu cầu, cả hai đều tăng tốc lưu lượng giữa máy khách và máy chủ. Họ cũng có thể thực hiện các tác vụ bổ sung như mã hóa SSL để tải xuống các máy chủ web của bạn, do đó tăng hiệu suất của chúng
  * **Security and anonymity**(bảo mật và ẩn danh) -Bằng cách chặn các yêu cầu hướng đến máy chủ phụ trợ của bạn , reserve proxy sẽ bảo vệ danh tính của họ và hoạt động như 1 biện pháp bảo về bổ sung chống lại các cuộc tấn công bảo mật. Nó cũng đảm bảo rằng nhiều máy chủ có thể  được truy cập từ một trình định vị bản ghi hoặc URL bất kể cấu trúc của mạng cục bộ của bạn

## Lợi ích của việc sử dụng Reverse Proxy 

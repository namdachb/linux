# Swap Momery
*Swap Memory được sử dụng khi hệ thống của bạn quyết định rằng nó cần thêm bộ nhớ RAM cho quá trình hoạt động và bộ nhớ RAM hiện tại không còn đủ để sử dụng. Nếu điều đó xảy ra, các tài nguyên và dữ liệu tạm thời không hoạt động trên bộ nhớ RAM sẽ được di chuyển để lưu trữ vào không gian Swap để giải phóng bộ nhớ RAM và sử dụng cho việc khác.*

*Lưu ý rằng thời gian truy cập vùng Swap là chậm hơn rất nhiều, do đó bạn không nên coi  việc sử dụng Swap là 1 phương pháp thay thế cho (RAM)*

*Swap là khái niệm bộ nhớ ảo được sử dụng trên Linux. Khi VPS / Server hoạt động, nếu hết RAM hệ thống sẽ tự động dùng 1 phần ở cứng để làm bộ nhớ cho các ứng dụng hoạt động.*

*Với những server không có swap, khi hết RAM hệ thống tự động stop service MYsql do đó hay xuất hiện thông báo lỗi "Establishing a Database Connection"*

*Do sử dụng ổ cứng có tốc độ chậm hơn RAM, nhất là những Server không dùng **SSD**, do đó không nên thường xuyên sử dụng **swap**, sẽ làm giảm hiệu năng hệ thống*

*Với các VPS dùng công nghệ ảo hóa **OpenVZ**. có thể không tạo được **swap** do hệ thống đã tự động kích hoạt sẵn 
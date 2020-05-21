## Network Time Procotol (NTP)
Là một thuật toán phần mềm giữ cho các máy tính và các thiết bị công nghệ khác nhau có thể đồng bộ hóa thời gian với nhau
 
 * **NTP** hoạt động bằng cách sử dụng một nguồn thời gian chính duy nhất (NTP Server), nó sử dụng để đồng bộ tất cả các thiết bị trên mạng
 * **NTP** là một trong những giao thức Internet lâu đời nhất vẫn còn được sử dụng (từ trước năm 1985)
 * **NTP** sử dụng thuật toán Marzullo, và nó cũng hỗ trợ các tính năng như giây thuận. NTPv4 thông thường có thể đảm bảo độ chính xác trong khoảng 10 mili giây (1/100s) trên mạng Internet công cộng, và có thể đạt đến độ chính xác 200 micro giây (1/5000s) hay hơn nữa trong điều kiện lý tưởng của môi trường mạng cục bộ

### NTP server
Máy chủ NTP hay máy chủ thời gian là các thuật ngữ cùng mô tả một khái niệm: một thiết bị được sử dụng để nhận biết yêu cầu đồng bộ thời gian và phân phối tín hiệu thông tin thời gian. Thật ra, một máy chủ **NTP Server** cũng chỉ sử dụng Network Time Protocol (NTP), trong vô vàn các giao thức thời gian khác nhau tồn tại thì NTP được sử dụng phổ biến tới hơn 90%

Các tín hiệu thời gian được sử dụng bởi hầu hết các máy chủ **NTP** là nguồn thời gian **UTC**. UTC (Coordinated Universal Time) là thời gian toàn cầu dựa trên thời gian đồng hồ nguyên tử. Bằng cách sử dụng **UTC**, máy chủ NTP có thể tác động, đồng bộ hóa mạng thời gian với hàng triệu mạng máy tính khắp nơi trên thế giới. Nếu không có **UTC**, nhiều giao dịch trực tuyến sẽ không thể nào thực hiện được

### Cách thức hoạt động của NTP server
 * NTP client gửi đi một gói tin, trong đó có chứa 1 thẻ thời gian tới cho NTP server
 * NTP server nhận được gói tin, và gửi trả lại NTP client một gói tin khác, có thể thời gian là thời điểm nó đã gửi gói tin đó đi
 * NTP client nhận được gói tin đó, rồi tính toán độ trễ, dựa vào thể thời gian mà nó nhận được cùng với độ trễ của đường truyền, NTP client sẽ tự động set lại thời gian của nó

### Lợi ích của NTP server
#### Backup dữ liệu theo lập lịch
 * Backup dữ liệu rất quan trọng với bất kỳ tổ chức nào, nếu hệ thống quá sai về thời gian sẽ khiến việc sao lưu không chính xác

#### Tăng tốc độ mạng truy cập
 * Nhiều thiết bị có sử dụng **cache** và hệ thống tập tin diện rộng có thể dựa vào tem thời gian (timestamp) để xác định phiên bản nào của đoạn dữ liệu ứng với thời điểm hiện tại. Đồng bộ thời gian không chính xác  có thể khiến hệ thống như **cache server** hoạt động không chính xác, sử dụng sai phiên bản dữ liệu

#### Hệ thống quản lý mạng
 * Khi có vấn đề gì đó, việc kiểm tra log hệ thống là một phần chính của debug lỗi. Nhưng nếu thời gian trong những file log này không đồng bộ/ không chính xác thì có thể bạn sẽ mất khoảng thời gian dài hơn để tìm ra nguyên nhân và khắc phục lỗi hệ thống

#### Phân tích xâm nhập
 * Trong trường hợp nếu có sự xâm nhập mạng trái phép, việc tìm hiểu xem mạng của bạn bị xâm nhập như thế nào và dữ liệu nào được truy cập có thể được kiểm tra rõ ràng nếu bạn có log thời gian chính xác việc login trên router hoặc máy chủ. Hacker thường sẽ xóa log nếu có, nhưng nếu hộ không xóa thì bạn cần thời gian chính xác để chuẩn đoán đó 

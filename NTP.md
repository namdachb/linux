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

# Múi giờ và giờ quốc tế GMT, UTC
## UTC
UTC (Coordinated Universal Time). Nghĩa là thời gian phối hợp quốc tế được cơ quan đo lường quốc tế (BIPM) đề xuất làm cơ sở pháp lý để định vị thời gian

UTC được dựa trên tiêu chuẩn múi giờ cũ là giờ trung bình Greenwich hay GMT vào thế kỷ 19, sau đó được đổi tên thành Universal Time có nghĩa là giờ quốc tế

### Cách xác định giờ UTC:
Thực tế, có 2 thành phần chính trong tiêu chuẩn thời gian phối hợp quốc tế (UTC). Đó là giờ quốc tế và giờ nguyên tử quốc tế
 * TAI (International Atomic Time) được xác định bằng cách kết hợp thời gian của hơn 200 đồng hồ nguyên tử trên toàn thế giới. Nói cách khác, nó vô cùng chính xác
 * UT1 hoặc giờ toàn cầu được xác định theo vòng quya của trái đất. Tuy nhiên tốc độ này không cố định, độ dài ngày theo UT không phải lúc nào cũng bằng nhau

## GMT
GMT (Greenwich Mean Time) dựa trên quan sát thiên văn, GMT được xem là tiêu chuẩn thời gian theo quốc gia

### Những điểm khác nhau của UTC và GMT
 * Chênh lệnh bằng các phân số của giây
 * **UTC** là thời gian Internet dữa trên tiêu chuẩn thời gian, còn **GMT** là thời gian theo tiêu chuẩn quốc gia 
 * **GMT** được thông qua trong luật pháp

# Một số câu lệnh thời gian trong linux
Date : đùng để truy cập và thiết lập đồng hồ hệ thống

 * Tùy chọn: + :thiết lập định dạng thời gian
 * -d :hiển thị các ngày khác và ngày hiện thời
 * -r : hiển thị ngày của một file cụ thể

Timedatectl : 
 * Xem lại ngày, thời gian
 * Đổi ngày , giờ
 * Thiếp lập timezone của máy
 * Kích hoạt tự động đồng bộ dựa trên 1 máy chủ khác từ xa

Thay đổi ngày tháng hiện tại, chạy lệnh sau với quyền root:

  `timedatectl set-time YYYY-MM-Đ`
YYYY : năm
MM : tháng
DD : ngày 

Thay đổi thời gian hiện tại, chạy lệnh sau với quyền root:

  `timedatectl set-time HH:MM::SS`

HH : giờ
MM : phút
SS : giây


**hwclock** :  là tiện ích giúp bạn truy cập vào đồng hồ cứng. Nó không phụ thuộc vào hệ điều hành mà bạn sử dụng và vẫn hoạt động ngay cả khi bạn tắt hệ thống.hwclock chỉ lưu thông tin cơ bản năm, tháng, ngày, giờ, phút, giây; nó không thể lưu trữ thời gian chuẩn, thời gian cục bộ hay UTC. Đồng thời cũng không có chế độ Daylight Saving Time

 * Khi bạn cần thay đổi ngày giờ đồng hồ phần cứng, bạn có thể làm như vậy bằng cách nối thêm --setvà --datecác tùy chọn cùng với thông số kỹ thuật của bạn:

 `hwclock --set --date "dd mmm yyyy HH:MM`

 * Đồng bộ hóa phần cứng theo thời gian hệ thống 

 `hwclock --systohc`

 * Hoặc, bạn có thể đặt thời gian hệ thống từ đồng hồ phần cứng bằng cách sử dụng lệnh sau:

 `hwclock --hctosys`

 * Để đặt đồng hồ phần cứng theo thời gian hệ thống hiện tại và giữ đồng hồ phần cứng theo giờ địa phương, hãy chạy lệnh sau như root:

 `hwclocl --systohc --localtime`
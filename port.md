## Tìm hiểu các port
**Port là gì**?

Port là giao thức bit 16 đứng đầu của mỗi tệp tin trong giao thức TCP, UDP hay còn gọi là cánh cổng quy định các tập dữ liệu riêng biệt. Nó là một dạng thuật toán được định sẵn mà mỗi máy tính cần phải đăng ký mới có thể nhận và xuất tập tin được. Port cũng được quy đổi giống như số bit của bất cứ một mã dữ liệu nào đó. Hay nói cách khác nó là lính gác nó có quyền quyết định cho hay không cho những dữ liệu nào có thể ra vào trong hệ thống máy tính

**Công dụng của port**

 * **Là chìa khóa, địa chỉ nhận diện, tệp tin, dữ liệu**: Khi bạn đăng ký các loại port trên hệ thống máy tính của bạn sẽ giúp cho các tập tin được truy cập, được đưa vào đúng với địa chỉ port khớp với đầu bit tập tin đó. 

 * **Có khả năng chọn lọc tin**: Port sẽ quy định chỉ những tập tin nào mới được nhập vào máy và được thông qua xuất nhập trong hệ thống máy. Nếu như tập tin đúng với cổng bit thì sẽ được xâm nhập vào máy nhưng tất nhiên máy tính sẽ từ chối việc nhập dữ liệu nào đó nếu như không đúng cổng port. Điều này sẽ giúp phân loại và lựa chọn luôn những tập tin an toàn

 * **Có khả năng bảo vệ xâm nhập có hại cho máy tính**: Port cũng được xem là một trong những cổng bảo vệ an toàn cho máy tính của bạn. Port có thể phát hiện những tập tin xấu, có chứa virus làm ảnh hưởng đến các tập tin và máy tính. Nó sẽ loại bỏ những tập tin đó đi, loại bỏ virus xâm nhập, giúp máy tính luôn giữ được sự an toàn và đảm bảo tránh được những thông tin nhiễu

### Những loại Port phổ biến hiện nay
Port có tổng cộng là 65535 cổng, được chia làm 3 phần: Well Known Port(WKP) bao gồm cá Port quy định từ 0-1023, quy định cho các ứng dụng như website (Port 80), FTP (Port 21), email (Port 25); Registered Port(RP) bao gồm các Port từ 1024-49151; Dynamic/Private Port(D/PP) bao gồm Port từ 49152-65535

Theo quy định của IANA thì WkP và RP phải được đăng ký với IANA trước khi sử dụng

 * 20-TPC -File Transfer - FTP data: cho phép upload và dowload dữ liệu từ server
 * 21-TPC -File Transfer - FTP control: Khi có máy tính muốn kết nối với dịch vụ FTP của máy bạn, máy đó sẽ tự động phải thêm Port và tìm cách kết nối đến cổng 21 theo mặc định. Khi đầu bit khớp cổng 21 mở cho máy muốn tới FTP để đăng nhập và nối tới server của các bạn
 * 22- TPC /UDP -SSH Remote Login Protocol: Nếu bạn chạy SSH Secure Shell, cổng 22 được yêu cầu cho người dùng SSH để kết nối tới người phục vụ của bạn
 * 23 - TPC - Telnet: Trường hợp bạn chạy một người phục vụ Telnet, cổng này được yêu cầu cho người dùng Telnet kết nối tới người phục vụ của các bạn. Telnet có thể được sử dụng để kiểm tra công tác dịch vụ ở cả các cổng khác
 * 25 - TPC - Simple Mail Transfer Protocol(SMTP): Khi có thư tới server SMTP của bạn, chúng sẽ cố gắng tiến vào server thông qua Cổng 25 theo mặc định
 * 38 - TPC - Route Access Protocol(RAP)
 * 42 - TPC - Host Name Server - Microsoft WINS
 * 45 - TPC - Message Processing Module (receive)
 * 46 - TPC - Message Processing Module(send)
 * 50 - TPC - Remote Mail Checking Protocol(RMCP)
 * 80 - Hyper-Text Transfer Protocol (HTTP):  Khi có người dùng sử dụng địa chỉ IP hay tên miền của bạn, bộ duyệt sẽ giám sát địa chỉ IP trên cổng 80 theo mặc định dành cho website, đồng thời hỗ trợ HTML và các tệp website khác ví dụ như ASP – Active Server Pages
 * 110 - TCP UDP - Post Office Protocol(POP) version 3
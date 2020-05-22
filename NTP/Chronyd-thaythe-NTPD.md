# Chronyd dịch vụ thay thế NTPD trên Unix
Chronyd được đánh giá là lựa chọn tối ưu hơn NTPD để đồng bộ thời gian trên Unix sử dụng **Network Time Protoc (NTP)**

## Chrony 
Là một dịch vụ hỗ trợ một cách đầy đủ, thuận tiện việc quản lý đồng bộ thời gian trên Network Time Protocol

Chrony có thể đồng bộ hóa đồng hồ hệ thống từ các máy chủ NTP. Nó cũng có thể hoạt động như một máy chủ NTP để cung cấp dịch vụ thời gian cho các máy tính khác trong mạng

Chrony hỗ trợ tốt đối với các hệ điều hành như Linux , FreeBSD, NetBSD, macOS and Solaris

## Lựa chọn giữa Chrony và NTP
 * Trong CentOS 7 NTPD đã được thay thế bằng Chronyd làm dịch vụ đồng bộ thời gian mạng mặc định
 * Đối với Ubuntu từ Ubuntu 18.04 thì Chronyd đã được sử dụng làm dịch vụ đồng bộ thời gian mặc định
 * Tương tự đối với Fedora thì Chronyd đã được áp dụng từ bạn Fedora16 từ năm 2011
 * Chronyd có độ chính xác đến mili giây đối với đồng bộ từ internet và nano giây đối với đồng bộ hóa thời gian khi có kết nối lại. Điều này phú hợp với các hệ thống có kết nối đến máy chủ NTP không ổn định
 * Chrony không yêu cầu kiểm tra định kỳ các máy chủ NTP vì vậy các hệ thống có kết nối mạng không liên tục vẫn có thể nhanh chóng đồng bộ hóa thời gian khi có kết nối lại. Điều này phù hợp với các hệ thống có kết nối đến các máy chủ NTP không ổn định
 * Chrony đồng bộ hóa nhanh có thể giảm thiểu tối đa lỗi phù hợp với máy tính để bàn hoặc hệ thống chạy không liên tục
 * Chrony đáp ứng tốt hơn với những thay đổi nhanh chóng về tần số xung nhịp, rất hữu ích cho các máy ảo vốn có đồng hồ không ổn định
 * Sau khi đồng bộ thời gian, Chrony không bao giờ đồng bộ đồng hồ khi thời gian đồng bộ chưa ổn định tránh gây ảnh hưởng đến các ứng dụng hoạt động có sử dụng thời gian hệ thống
 * Chrony được SystemAdmin đánh giá dễ dàng cấu hình và thao tác quản trị hợn về mặt bảo mật cũng như cung cấp trải nghiệm so với NTPD

## Các chương trình con của Chrony
Chrony bao gồm 2 chương trình con:
 * `chronyd` là chrony daemon quản lý service chrony trên `systemd` , `init.d`
 * `chronyc` cung cấp bộ câu lệnh giám sát quá trình hoạt động của chrony trong quá trình vận hành
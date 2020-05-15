## Cấu hình IP tĩnh trên Ubuntu server 20.4 với Netplan

### Giới thiệu netplan
Là một công cụ cấu hình mạng được giới thiệu trong Ubuntu. Nó có thể được sử dụng để viết mô tả YAML(là một chuẩn dữ liệu để tất cả các ngôn ngữ có thể đọc được) đơn giản về giao diện mạng với những thông tin cơ bản và tạo ra cấu hình cần thiết cho một công cụ kết xuất được chọn

### Cấu hình Netplan
Với mặc định một card mạng ens33 ở chế độ NAT. Bây giờ sẽ tiến hành gắn thêm một cấu hình card mạng và cấp ip cho nó

Kiểm tra thiết bị đang có trong mạng:
![Imgur](https://i.imgur.com/K9xprvj.png)

Card mạng ens38 đã tồn tại tuy nhiên nó chưa được cấp địa chỉ IP. Tiếp theo sẽ cấu hình cho nó bằng cách vào thư mục `/etc/netplan/` và chỉnh sửa file 50-cloud-init.yaml.(Lưu ý khi viết vào tệp yaml bạn phải tuân thủ theo đúng cú pháp căn dòng của nó). Ở đây chúng ta muốn cấp IP động nên sẽ ghi vào nội dung file như sau:
  
 * Đặt IP động :

![Imgur](https://i.imgur.com/79dzlQH.png)

Lưu và restart cấu hình bằng câu lệnh **netplan apply**

 * Đặt IP tĩnh:
Ta thấy ens38 đang nhận IP động , ta sẽ tiến hành đặt IP tĩnh theo số liệu sau

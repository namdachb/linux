## Tìm hiểu TCP Wrappers
Đây là một phương pháp chặn truy cập các dịch vụ trên máy chủ Linux của bạn thông qua hạn chế IP. Bài viết này sẽ giúp chúng ta chặn truy cập SSH từ tất cả các IP ngoại trừ danh sách IP "được phép". Cách thức này đạt được là thông qua 2 tệp nằm trong thư mục `/etc`. Một tên là **hosts.allow** và các **host.deny** khác

### 1. TCP Wrappers là gì?
 * Các TCP Wrappers là danh sách điều khiển truy cập (ACL-Access Control List) dựa trên máy và sử dụng để lọc truy cập mạng với các dịch vụ địa phương
 * TCP Wrappers có thể được sử dụng để cấp phép (allow) hoặc từ chối (deny) các mạng khác nhau đến server của bạn
 * Chúng xuất hiện vào những năm 1990 để bảo vệ các máy trạm Unix khỏi các cuộc tấn công
 * Ưu điểm : TCP / Wrappers có một lợi thế lớn so với tường lửa thông thường: chúng hoạt động trong lớp 7 (ứng dụng), do đó chúng có thể lọc các truy vấn ngay cả khi sử dụng mã hóa
 * Nhược điểm : Tất cả các ứng dụng dịch vụ Unix phải được biên dịch để hoạt động với thư viện libwrap. Wrappers không hoạt động với các dịch vụ gọi thủ tục từ xa RPC qua TCP

### 2. Cài đặt TCP Wrappers
TCP Wrappers có sẵn trong kho chính thức của hầu hết các hệ điều hành Linux
  * Trên các hệ thống dựa trên YUM :
   
   `yum -y install tcp_wrappers`

  * Trên các hệ thống dựa trên APT :

   `sudo apt-get install tcp_wrappers`

### 3. Hạn chế quyền truy cập vào máy chủ Linux bằng cách sử dụng TCP Wrappers
TCP Wrappers thực hiện kiểm soát truy cập với sự trợ giúp của hai tệp cấu hình: `/etc/hosts.allow` và `/etc/hosts.deny`. Hai tệp danh sách kiểm soát truy cập này quyết định liệu các máy khách cụ thể có được phép truy cập máy chủ Linux của bạn hay không

 * `/etc/hosts.allow` : Tệp này chứa tên của các máy chủ được phép sử dụng các dịch vụ mạng
 * `/etc/hosts.deny` : Tệp này chứa tên của máy chủ không thể sử dụng dịch vụ mạng
 * Nếu cùng một máy khách, người dùng hoặc ip được liệt kê trong cả hai tệp, hosts.allow có mức độ ưu tiên hơn hosts.deny, hãy cẩn thận với điều này

### 4. Thực hành 
IP Planing

![Imgur](https://i.imgur.com/qj0jKZn.png)

Để bảo mật máy chủ Linux là chặn tất cả các kết nối đến chỉ cho phép một vài máy chủ hoặc mạng cụ thể. Để làm như vậy, chỉnh sửa tệp `/etc/hosts.deny`
  
  `vi /etc/hosts.deny`

Thêm dòng sau. Dòng này từ chối kết nối với tất cả các dịch vụ và tất cả các mạng

 `ALL: ALL`

Sau đó, chỉnh sửa tệp `/etc/hosts.allow`
  
  `vi /etc/hosts.allow`

Thêm địa chỉ mạng cụ thể muốn kết nối

 `ssh : 192.168.213.175`

Theo quy tắc, tất cả các kết nối đến sẽ bị từ chối cho tất cả các máy chủ ngoại trừ máy chủ 192.168.213.175
  * Chúng ta có thể xác minh điều này từ các tệp nhật ký của máy chủ Linux bằng lệnh :

   `cat /var/log/secure`

Chúng ta có thể cho phép các kết nối đến từ tất cả các máy chủ, nhưng không phải từ một máy chủ cụ thể. Để cho phép các kết nối đến từ tất cả các máy chủ trong mạng con **192.168.213**, nhưng không phai từ máy chủ 192.168.213.174, hãy thêm dòng sau vào tệp `/etc/hosts.allow`

 `ALL: 192.168.213. EXCEPT 192.168.213.174`

 

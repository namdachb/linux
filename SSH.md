# SSH - Secure Shell
### 1) Khái Niệm
 * **SSH - Secure Shell** là một giao thức mạng dùng để thiết lập kết nối mạng một cách bảo mật
 * **SSH** có sử dụng cơ chế mã hóa đủ mạnh nhằm ngăn chặn các hiện tượng nghe trộm, đánh cắp thông tin đường truyền. Các giao thức trước đây như **rlogin** , **telent** không hỗ trợ mã hóa
 * **SSH** hoạt động ở tầng **Application** trong mô hình **TCP/IP**
 * Các công cụ **SSH** (như là **OpenSSH**,..) cung cấp cho người dùng cách thức để thiết lập kết nối mạng được mã hóa để tạo một kênh kết nối riêng tư

### 2) Cấu trúc lệnh SSH

![Imgur](https://i.imgur.com/yWcmVvj.png)

#### Server
 * Một chương trình cho phép đi vào kết nối SSH với một bộ máy, trình bày xác thực, cấp phép, ... Trong hầu hết SSH bổ sung của Unix thì server thường là sshd

#### Client
 * Một chương trình kết nối đến SSH server và đưa ra yêu cầu như là "log me in" hoặc "copy this file". Trong SSH1, SSH2 và OpenSSH, client chủ yếu là ssh và scp

#### Session
 * Một phiên kết nối giữa một client và một server. Nó bắt đầu sau khi client xác thực thành công đến một server và kết thúc khi kết nối chấm dứt. Session có thể được tương tác với nhau hoặc có thể là một chuyển riêng

#### Key
 * Một lượng dữ liệu tương đối nhỏ, thông thường từ mười đến 1 hoặc 2 nghìn bit. Tính hữu ích của việc sử dụng thuật toán ràng buộc khóa hoạt động trong vài cách để giữ khóa: trong mã hoá, nó chắc chắn rằng chỉ người nào đó giữ khoá (hoặc một ai có liên quan) có thể giải mã thông điệp, trong xác thực, nó cho phép bạn kiểm tra trễ rằng người giữ khoá thực sự đã kí hiệu vào thông điệp.

### 3) Các kỹ thuật mã hóa
 * Một lợi thế quan trọng của **SSH** so với các giao thức tiền nhiệm trước là nó sử dụng mã hóa để đảm bảo kết nối bảo mật giữa **host** và **client**
   * **Host** là server đầu ra
   * **Client** là máy tính đang thực hiện truy cập từ xa vào **host**
 * Có 3 công nghệ mã hóa khác nhau được sử dụng trong **SSH** là : **symmetric encryption** , **asymmetric encryption** và **hashing**

#### 3.1) Symmetric Encryption (mã hóa đối xứng)
 * **Synmmetric Encryption** là 1 dạng mã hóa sử dụng **secret key** cho cả việc mã hóa và giải mã gói tin bởi **host** và **client**. Do vậy, bất cứ ai có được **secret key** đều có thể giải mã gói tin

 * **Symmetric Encryption** thường được gọi là **shared-key** hoặc **shared-secret** encryption. Thường chỉ có 1 **key** được sử dụng hoặc đôi khi là 1 cặp **key** trong khi từ **key** này có thể tính toán được ra **key** kia 
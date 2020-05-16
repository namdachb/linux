# Cách thiết lập Mount NFS trên Ubuntu 20.04

### Giới thiệu
NFS, hay Network FileSystem, là một giao thức hệ thống tệp phân tán cho phép bạn gắn các thư mục từ xa trên máy chủ của mình. Điều này cho phép bạn quản lý không gian lưu trữ ở một vị trí khác và ghi vào không gian đó từ nhiều khách hàng. NFS cung cấp một cách tương đối chuẩn và hiệu quả để truy cập các hệ thống từ xa qua mạng và hoạt động tốt trong các tình huống mà các tài nguyên được chia sẻ phải được truy cập thường xuyên

Trong bài này, chúng ta sẽ giới thiệu cách cài đặt phần mềm cần thiết cho chức năng NFS trên Ubuntu 20.01, định cấu hình hai giá trị NFS trên máy chủ và máy khách, gắn kết và ngắt kết nối các chia sẻ từ xa

### Điều kiện tiên quyết
Chúng ta sử dụng hai máy chủ trong hướng dẫn này, với một phần chia sẻ hệ thống tập tin của nó với phần còn lại

 * Hai ubuntu server. Mỗi người trong số này nên có một người dùng không phải **root** với các `sudo` đặc quyền, tường lửa được thiết lập với UFW và mạng riêng, nếu nó có sẵn cho bạn

Trong bài này, chúng ta đề cập đến máy chủ chia sẻ các thư mục của nó là **host** và máy chủ gắn kết các thư mục này với tư cách là **client**. Chúng ta cần biết địa chỉ IP cho cả hai. Hãy chắc chắn sử dụng địa chỉ mạng riêng, nếu có sẵn

### Bước 1: Tải xuống và cài đặt các thành phần
Chúng ta sẽ bắt đầu bằng cách cài đặt các thành phần cần thiết trên mỗi máy chủ

#### Trên máy Host
Cài đặt `nfs-kernel-server` gói, cho phép bạn chia sẻ các thư mục của mình. Vì đây là thao tác đầu tiên bạn thực hiện `apt` trong phiên này, hãy làm mới chỉ mục gói cục bộ của bạn trước khi cài đặt:
```
sudo apt update
sudo apt install nfs-kernel-server
```

Khi các gói này được cài đặt, chuyển sang máy chủ client

#### Trên máy Client
Chúng ta cần phải cài đặt 1 gói phần mềm được gọi là `nfs-common`, cung cấp chức năng NFS mà không bao gồm bất kỳ thành phần máy chủ. Làm mới chỉ mục gói cục bộ trước khi cài đặt để đảm bảo rằng bạn có thông tin cập nhật:
```
sudo apt update
sudo apt install nfs-common
```

Bây giờ cả hai máy chủ đề có các gói cần thiết, chúng ta có thể bắt đầu cấu hình chúng

### Bước 2: Tạo thư mục chia sẻ trên máy Host
Chúng tôi sẻ chia sẻ hai thư mục riêng biệt, với cái cài đặt cấu hình khác nhau, để minh họa 2 cách chính mà các gắn kết NFS có thể được cấu hình liên quan đến truy cập siêu người dùng

Superusers có thể làm bất cứ điều gì ở bất cứ đâu trên hệ thống của họ. Tuy nhiên, các thư mục gắn trên NFS không phải là một phần của hệ thống mà chúng được gắn kết, do đó, theo mặc định, máy chủ NFS từ chối thực hiện các hoạt động yêu cầu đặc quyền siêu người dùng. Hạn chế mặc định này có ý nghĩa là các superusers trên máy **client** không thể ghi các tệp dưới dạng **root** , gán lại quyền sở hữu hoặc thực hiện bất kỳ tác vụ superuser nào khác trên NFS

Đôi khi có những người dùng đáng tin cậy trên máy **client** hệ thống cần thực hiện các hành động này trên hệ thống tệp được gắn kết nhưng không cần truy cập superuser trên máy **Host**. Bạn có thể cấu hình máy chủ NFS để cho phép điều này, mặc dù nó giới thiệu 1 phần tử rủi ro, như một người dùng như vậy có thể truy cập root toàn bộ **host** hệ thống




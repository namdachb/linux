## Site manager
Lệnh trên trang web trực tuyến cho phép chúng ta quản lý trang web được lưu trữ trên máy chủ. Tạo bất kỳ loại trang web nào trên máy chủ mới của bạn, thêm chứng chỉ SSL, kích hoạt FastCgi Cache cho cài đặt WordPress của bạn và nhiều tính năng khác sẽ cho phép bạn có toàn quyền kiểm soát các trang web của mình một cách dễ dàng và đáng tin cậy, luôn luôn ở một lệnh và không có biến chứng

**Cú pháp**

`sudo site <domain> <option> <option2>`

**Options**

 * -cache
 * -clone-from
 * -delete
 * -delete-all
 * -force-redirect
 * -forward
 * -html
 * -list
 * -multisite-convert
 * -mysql
 * -on
 * -off
 * -parked
 * -php
 * -proxy
 * -redirection
 * -ssl
 * -wp
 * -yoast-sitemap


 * Tạo một trang HTML cơ bản:

`sudo site example.com -html`

 * Tạo một trang web hỗ trợ PHP:

`sudo site example.com -php`

 * Để tạo trang web php kết hợp với cơ sở dữ liệu

`sudo site example.com -mysql`

### WordPress
Cài đặt WordPress chưa bao giờ dễ dàng đến thế, Webinoly tự động định cấu hình toàn bộ quá trình cài đặt WordPress

`sudo site example.com -wp`

Sử dụng tùy chọn tùy cỉnh của người dùng để sửa đổi một số khía cạnh của cơ sở dữ liệu, theo bất kỳ cách nào bạn có thể chỉ cần nhấn <Enter> vào mỗi câu hỏi để sử dụng dữ liệu được đề xuất

```
# Installation script of a WordPress site using the "custom" configuration
sudo site example.com -wp=custom -cache=on
```

Theo cách tương tự, bạn có thể thực hiện cài đặt bằng dữ liệu tùy chỉnh

```
sudo site example.com -wp=[<setup_db>,<setup_wp>,<host>,<dbname>,<dbuser>,<dbpass>,<wp_prefix>,<external_db_user>,<external_db_pass>]
```

### WordPress multisite(nhiều trang)
Chuyển đổi trang web WordPress của ta sang Miltisite dễ dàng

`sudo site example.com -multisite-convert`

ta nên truy cập vào "Tool-> Network Setup" để chọn loại cài đặt (tên miền phụ hoặc thư mục con) Webinoly sẽ tự động thực hiện tất cả các cấu hình sau khi bạn đã chọn loại Multisite bạn muốn

> Nếu trang web của bạn đã được cài đặt với các `-subfoder` , chỉ có tùy chọn `subdirectory` mới khả dụng khi chuyển đổi sang multisite, WordPress không cho phép cấu hình tên miền phụ trong loại cài đặt này

### Configurable Subfolders(các thư mục con có thể cấu hình)
Một trong những công cụ mạnh nhất để định cấu hình trang web của bạn trên máy chủ là khả năng định cấu hình độc lập từng thư mục con của một trang web cụ thể

```
sudo site example.com -html -subfolder=/one
sudo site example.con -php -subfolder=//two
sudo site example.com -wp -subfolder=/three
sudo site example.com -proxy -subfolder=/four

# delete a subfolder site
sudo site example.com -delete -subfolder=/xxx
```

### Install WordPress in a subdirectory (cài đặt wordpress trong thư mục con)
Ta có thể có nhiều hơn một cài đặt WordPress trong cùng một tên miền

`sudo site example.com -wp -subfolder=/test`


### Cách vô hiệu hóa một trang web
Bất cứ lúc nào bạn có thể kích hoạt hoặc hủy kích hoạt một trang web được lưu trữ trên máy chủ của bạn mà không xóa nó

`sudo site example.com -on`

`sudo site example.com -off`

### Xóa một trang web
Bạn nên sử dụng tùy chọn này một cách thận trọng, vì một khi trang web bị xóa, nó sẽ không thể khôi phục các tệp

`sudo site example.com -delete`

Để xóa tất cả các trang web được lưu trữ trên máy chủ của bạn

`sudo site -delete-all`

Danh sách các trang web của bạn

`sudo site -list`

### Có www hoặc không www trong một trang web
Theo mặc định, Webinoly định cấu hình trang web của bạn để chấp nhận cả hai yêu cầu trong tên miền cả bạn, nghĩa là `example.com` và `www.example.com` sẽ hợp lệ. Bạn có thể buộc sử dung và chuyển hướng các yêu cầu đến bất kỳ tùy chọn nào của bạn

`sudo site example.com -force-redirect=<options>`

**Options**:

 * www
 * root
 * off

Trong một số trường hợp đặc biệt khi chứng chỉ SSL đã được nhập từ nhà cung cấp khác và điều này không bao gồm tên miền www hoặc ngược lại, chúng ta có thể sử dụng `-ignore-ssl` tùy chọn như sau: `sudo site example.com -force-redirect=root -ignore-ssl`
## Tìm hiểu file-log-httpd
## Tổng quan 
Máy chủ HTTP Apache cung cấp nhiều cơ chế khác nhau để ghi nhật ký mọi thứ xảy ra trên máy chủ, từ yêu cầu ban đầu, thông qua trình ánh xạ URL, đến độ phân giải cuối cùng của kết nối, bao gồm mọi lỗi có thể xảy ra trong quy trình

Ngoài ra, các mô-đun của bên thứ ba có thể cung cấp khả năng ghi nhật ký hoặc đưa các mục vào tệp nhật ký hiện có và các ứng dụng như chương trình CGI hoặc tập lệnh PHP hoặc trình xử lý đến nhật ký lỗi máy chủ

## Access and Error Logs
### Log Files
Nhật ký Apache là bản ghi các sự kiện đã xảy ra trên mạng chủ web Apache của chúng ta. Apache lưu trữ 2 loại nhật ký:

### Access Log
Chứa thông tin về các yêu cầu đến máy chủ web. Thông tin này có thể bao gồm những trang mọi người đang xem, trạng thái thành công của các yêu cầu và thời gian yêu cầu được trả lời. Nó sẽ trông giống như sau:

```
192.168.213.1 - - [05/Jun/2020:22:21:16 +0700] "GET / HTTP/1.1" 403 4897 "-" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) coc_coc_browser/86.0.172 Chrome/80.0.3987.172 Safari/537.36"
```

Ý nghĩa của từng lĩnh vực trong hồ sơ :
 * `192.168.213.1`-Tên máy chủ hoặc địa chỉ IP của khách hàng thực hiện yêu cầu 
 * `-`-Tên đăng nhập từ xa. Khi tên người dùng không được đặt, trường này sẽ hiển thị `-`
 * `-`-Nếu yêu cầu được xác thực, tên gnuoiwf dùng từ xa sẽ được hiển thị
 * `[05/Jun/2020:22:21:16 +0700]`- Thời gian máy chủ cục bộ
 * `"GET / HTTP/1.1"`-Dòng yêu cầu đầu tiên.Loại yêu cầu, đường dẫn và giao thức
 * `403`-Mã phản hồi của máy chủ cuối cùng. Nếu `>` biểu tưởng không được sử dụng và yêu cầu đã được chuyển hướng nội bộ, nó sẽ hiển thị trạng thái của yêu cầu ban đầu                   
 * `"-"`-URL của người giới thiệu
 * `Mozilla/5.0 ...`-Tác nhân người dùng của máy khác (trình duyệt web)

File log được lưu trữ tại /var/log/httpd/access_log

Định dạng log (LogFormat) cơ bản như sau là : %h %l %u %t %r %>s %b Refer User_agent

Trong đó:
 * `%h` : địa chỉ của máy client
 * `%l` : nhận dạng người dùng được xác định bởi identd
 * `%u` : tên người dùng được xác định bằng xác thực http
 * `%t` : thời gian yêu cầu xác thực
 * `%r` : là yêu cầu từ người sử dụng (client)
 * `%>s` : mã trạng thái được gửi từ máy chủ đến máy khách
 * `%b` : kích cỡ phản hồi đổi với client
 * `Refer` : tiêu đề refeer của yêu cầu HTTP (chứa URL của trang mà yêu cầu này được khởi tạo)
 * `User_agent` : chuỗi xác định trình duyệt

### Error Log
Chứa thông tin về các lỗi mà máy chủ gặp phải khi xử lý các yêu cầu, chẳng hạn như tệp bị thiếu

Là nơi đầu tiên để xem xét khi xảy ra sử cố khi khởi động máy chủ hoặc với hoạt động của máy chủ vì nó thường chứa thông tin chi tiết về những gì xảy ra và cách khắc phục

Nơi lưu trữ file log là /var/log/httpd/error_log (với centos) và /var/log/apache2/error.log (với ubuntu)

```
[Thu May 21 22:18:52.762993 2020] [lbmethod_heartbeat:notice] [pid 2205] AH02282: No slotmem from mod_heartmonitor
```
 * [lbmethod_heartbeat:notice]-Module tạo ra thông điệp
 * [pid 2205]-process id


### Hiển thị tất cả địa chỉ IP có trong file access log

```
grep -o "[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+" /var/log/httpd/access_log | sort | uniq
```

![Imgur](https://i.imgur.com/Eowygvb.png)

### Hiển thị các địa chỉ có trong file access log và số lần xuất hiện của chúng
**Cách 1**: Đối với cách này sẽ hiển thị tất các các địa chỉ có trong file access log bao gồm cả các địa chỉ có trong User-Agent

Sắp xếp các địa chỉ theo thứ tự tăng dần
```
grep -o "[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+" /var/log/httpd/access_log | sort | uniq -c | sort -n
```

![Imgur](https://i.imgur.com/lXVSZSO.png)

Sắp xếp các địa chỉ theo thứ tự giảm dần
```
grep -o "[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+" /var/log/httpd/access_log | sort | uniq -c | sort -nr
```

![Imgur](https://i.imgur.com/DH1jvDJ.png)

**Cách 2**:Còn đối với cách này sẽ chỉ hiển thị những địa chỉ IP có ở phần đầu của access log (X-FORWARDED_FOR)
```
awk '{print $1}' /var/log/httpd/access_log | sort | uniq -c | sort -nr
```

![Imgur](https://i.imgur.com/wZjkhxS.png)

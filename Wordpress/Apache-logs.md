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
192.168.213.1 - - [05/Jun/2020:22:21:16 +0700] "GET / HTTP/1.1" 403 4897 "-" "Mo                                                                                                        zilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) coc_co                                                                                                        c_browser/86.0.172 Chrome/80.0.3987.172 Safari/537.36"
```

Ý nghĩa của từng lĩnh vực trong hồ sơ :
 * `%h`-`192.168.213.1`-Tên máy chủ hoặc địa chỉ IP của khách hàng thực hiện yêu cầu 
 * `%l`-`-`-Tên đăng nhập từ xa. Khi tên người dùng không được đặt, trường này sẽ hiển thị `-`
 * `%u`-`-`-Nếu yêu cầu được xác thực, tên gnuoiwf dùng từ xa sẽ được hiển thị
 * `%t`-`[]                                                                                                                                            
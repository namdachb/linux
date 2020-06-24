## Giới hạn số lượng request trong một khoảng thời gian
Việc giới hạn số lượng request của một client trong một khoảng thời gian sẽ giảm được rủi ro server bị tấn công DDoS. Bạn tính toán với trang web của mình một người dùng bình thường sẽ không thực hiện quá 1 request trong 2 giây(tương đương 30 request trong 1 phút) còn nếu quá thì chắc chắn người dùng này đang có hành động bất thường. Như vậy ta sẽ giới hạn số request cho một client trong 1 phút chỉ có thể thực hiện tối đa 30 request

## Thực hành
Để làm được điều này trước tiên chúng ta cần một server đã cài nginx

**Cách cài nginx**
```
yum update
yum install epel-release
yum install nginx
```

Khởi động và bắt đầu dịch vụ

`systemctl start nginx`

`systemctl enable nginx`

**Mở file cấu hình `nginx.conf`**

`vi /etc/nginx/nginx.conf`

Thêm dòng sau vào block `http {}`

`limit_req_zone  $binary_remote_addr  zone=one:10m   rate=30r/m;`

File `nginx.conf` sẽ như sau

![Imgur](https://i.imgur.com/jC5Vx5e.png)

Ở đây sẽ tạo ra một vùng nhớ có tên là `one` có dung lượng `10MB` để lưu trạng thái của request theo kiểu key-value (trong trường hợp này là địa chỉ của client). `rate` để chỉ ra số request giới hạn là 30 request trong thời gian 1 phút. Tương đương với 0,5 request mỗi giây nhưng vì không được phép để là 0,5 nên ta để là 30 request mỗi phút

Bây giờ bạn muốn giới hạn request có hiệu lực ở đâu thì bạn đặt `limit_req zone=one;` trong các block đố
  * Nếu bạn muốn nó áp dụng cho tất cả các trang web trên nginx này thì bạn đặt trong block `http {}`
  * Nếu muốn áp dụng cho cả một trang web thì đặt trong block `server {}`
  * Còn muốn giới hạn trên một số màn xác định thì đặt nó trong block `location {}`

Trong bài này mình sẽ đặt nó trong block `location {}`
```
location / {
            limit_req zone=one;
            proxy_pass http://192.168.213.148;
        }
```
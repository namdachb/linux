# httpAuth
Lệnh của HTTP HttpAuth, cho phép chúng tôi quản lý người dùng có quyền truy cập các trang được bảo vệ bởi phương thức HTTP, ngoài việc kiểm soát kích hoạt lớp bảo mật bổ sung này trong các truy cập công cụ như phpMyAdmin và wp-admin hoặc wp-login. Về cơ bản, nó là để bảo vệ một số phần trong trang web của bạn yêu cầu người dùng  và mật khẩu để có thể truy cập nội dung của nó

**Cú pháp**

`sudo httpauth <tùy chọn>`

**Options**

 * -add
 * -delete
 * -list
 * -path
 * -whitelist
 * -wp-admin

**Example**

```
sudo httpauth -add
sudo httpauth -wp-admin=off
```


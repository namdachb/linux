# httpAuth
Lệnh của HTTP HttpAuth, cho phép chúng tôi quản lý người dùng có quyền truy cập các trang được bảo vệ bởi phương thức HTTP, ngoài việc kiểm soát kích hoạt lớp bảo mật bổ sung này trong các truy cập công cụ như phpMyAdmin và wp-admin hoặc wp-login. Về cơ bản, nó là để bảo vệ một số phần trong trang web của bạn yêu cầu người dùng  và mật khẩu để có thể truy cập nội dung của nó

**Cú pháp**

`sudo httpauth <tùy chọn>`

**Options**

 * -add : tạo người dùng
 * -delete : xóa người dùng
 * -list : danh sách
 * -path 
 * -whitelist
 * -wp-admin

**Example**

```
sudo httpauth -add
sudo httpauth -wp-admin=off
```

## Create User (tạo người dùng)
Để tạo người dùng và mật khẩu để truy cập vào các phần được bảo vệ bằng xác thực HTTP, dùng lệnh:

`sudo httpauth -add`

Ta cũng có thể tạo người dùng có quyền hạn chế để chỉ truy cập một tên miền cụ thể

`sudo httpauth example.com -add`

## Xóa người dùng (delete user)
Để xóa người dùng sử dụng lệnh

`sudo httpauth -delete`

## Danh sách  (List)
Hiển thị danh sách tất cả người dùng được tạo với quyền truy cập với quyền truy cập vào xác thực HTTP

`sudo httpauth -list`

Ta có thể sử dụng `-raw` tùy chọn để loại bỏ màu sắc và định dạng

```
# Remove format from list.
sudo httpauth -list -raw

# To list users from an specific domain.
sudo httpauth example.com -list

# To list all the protected paths, areas or folders.
sudo httpauth example.com -list=protected

# To list all the whitelisted IP's.
sudo httpauth -whitelist -list
```



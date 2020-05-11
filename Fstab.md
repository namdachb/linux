# Tìm hiểu Fstab
*`fstab` viết tắt của **f**ile **s**ystem **tab***le, là một tệp cấu hình hệ thống trên Linux và các hệ điều hành tương tự Unix khác có chứa thông tinn về các hệ thống tệp chính trên hệ thống. Nó lấy tên từ tập tin bảng hệ thống và nó nằm trong /etc thư mục*

*Tệp fstab dược đọc bỏi `mount` lệnh, tự động xảy ra khi khởi động để xác định cấu trúc hệ thống tệp tổng thể và sau đó khi người dùng thực thi `mount` lệnh để sửa đổi cấu trúc. Quản trị viên hệ thống có nhiệm vụ tạo và duy trì đúng tệp fstab* 

## Mô tả sơ lược về fstab
*Cấu trúc một file /etc/fstab*
`/dev/sr0 test udf auto 0 0`
|1|2|3|4|5|6
|-|-|-|-|-|-|
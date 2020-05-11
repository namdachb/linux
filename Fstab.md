# Tìm hiểu Fstab
*`fstab` viết tắt của **f**ile **s**ystem **tab***le, là một tệp cấu hình hệ thống trên Linux và các hệ điều hành tương tự Unix khác có chứa thông tinn về các hệ thống tệp chính trên hệ thống. Nó lấy tên từ tập tin bảng hệ thống và nó nằm trong /etc thư mục*

*Tệp fstab dược đọc bỏi `mount` lệnh, tự động xảy ra khi khởi động để xác định cấu trúc hệ thống tệp tổng thể và sau đó khi người dùng thực thi `mount` lệnh để sửa đổi cấu trúc. Quản trị viên hệ thống có nhiệm vụ tạo và duy trì đúng tệp fstab* 

## Mô tả sơ lược về fstab
*Cấu trúc một file /etc/fstab*
`/dev/sr0 test udf auto 0 0`
|1|2|3|4|5|6|
|-|-|-|-|-|-|
|/dev/sro|test|udf|auto|0|0|

### Ý nghĩa cột thứ 1 và 2: Thiết bị cần mount và nơi mặc định để mount
 * Cột thứ nhất là ổ đĩa hoặc thiết bị cần mount
 * Cột thứ 2 chính là nơi mount mặc định 

### Cột thứ 3: loại file system
 * Loại file định dạng "xfs,swap,udf"
### Cột thứ 4: Các tùy chọn mount
 * Đây có lẽ là cột quan trọng nhất, nó quy định xem có mount tự động lúc khởi động không, user có quyền mount không, có phép thực thi các file trên đó không (nhiều trường họp lỗi khi thực thi các script, exec file không được là do phần tùy chọn này)
 * auto và noauto: Với tùy chọn auto, thiết bị sẽ tự động mount lúc khởi động máy tính, đây là tùy chọn mặc định. Nếu bạn không muốn thiết bị của mình được muont tự động hay dùng tùy chọn noauto, khi đó khi nào bạn ra lệnh mount thì hệ thống mới mount cho bạn
 * user và nouser: tùy chọn user cho phép những user bình thường có thể mount thiết bị, ngược lại nouser chỉ cho phép root mới có quyền mount mà thôi. Tùy chọn nouser là mặc định, nếu bạn không thể mount CD, ổ đĩa từ windows... bạn hãy điều chỉnh lại thông số này
 * exec và noexec: exec cho phép bạn thực thi các file thực thi tồn tại trên partition đó, đây là tùy chọn mặc định. Tùy chọn noexec sẽ không cho phép bạn thực thi những file này, tùy chọn này thường được áp dụng đối với những phân vùng không có file thực thi hoặc không muốn cho thực thi
 * ro: mount partition ở chế độ read-only. Với chế độ bày bạn chỉ có thể đọc mà không thể ghi được vào partition đó
 * rw: Mount partition ở chế độ read-write, đôi khi bạn phải đau đầu vì tùy chọn này do không thể ghi vào đĩa mềm mà không hiểu nguyên nhân vì sao
 * sync: thao tác nhập xuất trên filesystem được đồng bộ hóa
 * async: thao tác nhập xuất trên filesystem diễn ra không đồng bộ
 * defaults: sử dụng các tùy chọn mặc định đó là rw, suid, dev, exec, auto, nouser,and async. Các tùy chọn được cách nhau bằng dấu phẩy(,)
### Cột thứ 5 và 6: Các tùy chọn cho lệnh dump và fsck
 * Cột 5 là tùy chọn cho chương trình dump, công cụ sao lưu filesystem. Điền 0: bỏ qua việc sao lưu. 1: thực hiện sao lưu
 * Cột 6 là tùy chọn cho chương trình fsck, công cụ dò lỗi trên filesystem. Điền 0: bỏ qua việc kiểm tra, 1: thực hiện kiểm tra

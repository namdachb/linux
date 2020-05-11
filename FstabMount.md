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

### Một số câu lệnh 
 * Lệnh để kiểm tra giá trị hiện hành
```
[root@localhost ~]# blkid
/dev/sda1: UUID="38dbdb36-c094-41cd-8f97-3cf7f9407975" TYPE="xfs"
/dev/sda2: UUID="yoB2U0-0RDw-Utnk-cEjt-P2A1-2C0V-7bxgC1" TYPE="LVM2_member"
/dev/sr0: UUID="2020-03-11-04-39-23-00" LABEL="ESD_ISO" TYPE="udf"
/dev/mapper/centos-root: UUID="0dc64c5d-a51c-4139-8ed3-24be55629e0d" TYPE="xfs"                                                                                                         
/dev/mapper/centos-swap: UUID="5eafcd7c-ff74-4510-956b-179e7901a139" TYPE="swap                                                                                                         "
```

 * Lệnh lsblk hiển thị thông tin về các thiết bị lưu trữ. Tiện ích này thường được sử dụng để xác định tên thiết bị chính xác được truyền cho lệnh tiếp theo
```
[root@localhost ~]# lsblk
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0   20G  0 disk
├─sda1            8:1    0  512M  0 part /boot
└─sda2            8:2    0 19.5G  0 part
  ├─centos-root 253:0    0 18.5G  0 lvm  /
  └─centos-swap 253:1    0    1G  0 lvm  [SWAP]
sr0              11:0    1    4G  0 rom  /test
```

### Mount thiết bị trong linux
 * Cấu trúc lệnh:
`# mount [options] [device_name] [mount_ponit]`
   * Options:
      * `-v` : chế độ chi tiết, cung cấp thêm thông tin về những gì mount định thực hiện
      * `-w` : mount hệ thống tập tin với quyền đọc và ghi
      * `-r` : mount hệ thống tập tin chỉ có quyền đọc
      * `-t` : xác định lại hệ thống tập tin được mount. Những loại hợp lệ là `ext2`, `ext3`, `ext4`, `vfat`, `iso9600`...
      * `-a` : mount tất cả các hệ thống tập tin được khai báo trong `fstab`
      * `o` : remount (fs) chỉ định việc mount tại 1 file system nào đó
       
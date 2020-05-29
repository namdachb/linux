## Reset root password trên ubuntu 16.04
### Bước 1: Khởi động lại hệ thống vào 'GRUB2' boot loader menu
 - Khởi động lại hệ thống và tinh chỉnh chế độ GRUB2 ở màn hình boot GRUB2
 - Bấm nút **ESC** để màn hình dừng lại, sau đó ấn nút '**e**' ở dòng entry 'Ubuntu' để thực hiện chỉnh sửa boot loader

  ![Imgur](https://i.imgur.com/z8fl0Mc.png)

### Bước 2: chỉnh thông số entry GRUB2
 - Sau khi vào được cấu hình GRUB2 sẽ có thông tin như hình ảnh ở dưới

 ![Imgur](https://i.imgur.com/mn5UETE.png)

 - Tìm đến dòng entry cấu hình bắt đầu bằng "**linux**.." sau đó thêm cấu hình kích hoạt chế độ read-write mode `rw` và `init=/bin/bash vào cuối dòng cấu hình
 
 ![Imgur](https://i.imgur.com/6vyIJrj.png)

 thành

 ![Imgur](https://i.imgur.com/PeLV6D8.png)

 -Ấn **ctrl+X** hoặc **f10** để lưu tự động boot vào môi trường initramfs

### Bước 3: remount filesystem
 - Hệ thống filesystem hiện tại đang ở chế độ "read-write" được mount ở **/**
 - Kiểm tra lại xem filesystem đã được mount quyền đọc-ghi (rw), để thực hiện khôi phục mật khẩu root thì ta cần thêm quyền ghi (write) trên filesystem

 ![Imgur](https://i.imgur.com/JLouP4X.png)

 - Nếu kiểm tra thấy vẫn chưa ở chế độ rw, thì ta sẽ tiến hành remount lại filesystem root/ với quyền đọc-ghi(read-write). Nếu không remount quyền read-write thì khi đổi mật khẩu sẽ gặp lỗi (Authentication tokeb manipulation error)

 `mount -o remount, rw /`

### Bước 4: đổi mật khẩu root và reboot
 - Tiến hành reser password user root

 ![Imgur](https://i.imgur.com/OOnjhgS.png)

 - Khởi động lại hệ thống

 `exec /sbin/init`

Sau khi boot vào hệ thống prompt consolo thành công thì bạn có thể đăng nhập bằng mật khẩu mới 
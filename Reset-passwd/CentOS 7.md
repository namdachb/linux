## Cách khôi phục mật khẩu root trên CentOS 7/RHEL 7
## Cách 1 :
### Bước 1: Khởi động lại hệ  thống, chỉnh sửa 'grub2'
 - Khởi động lại hệ thống và tùy chỉnh chế độ GRUB2 ở màn hình boot GRUB2
 - Bấm nút **ESC** để màn hình dừng lại, sau đó ấn nút '**e**' để thực hiện chỉnh sửa

 ![Imgur](https://i.imgur.com/TZrRhaU.png)

### Bước 2: Chỉnh thôn số entry cần thiết
 - Tìm đến dòng entry cấu hình "**linux16**" hoặc "**linuxefi**" đối với hệ thống UEFI
 - Xóa 2 thông số "**rhgb quiet**" để kích hoạt log message hệ thống khi thực hiện đổi mật khẩu root
 - Thêm vào cuối dòng "**linux16..**" thông số sau
  
  `rd.break`
  
 ![Imgur](https://i.imgur.com/jUtx4uo.png)

 - Ấn **Ctrl+X** để lưu và tự động boot vào môi trường **initramfs**

### Bước 3: remount filesystem và chuyển chế độ chroot
 - Hệ thống filesystem hiện tại đang ở chế độ "**read only**" được mount ở thư mục **/sysroot/**, để thực hiện khôi phục mật khẩu root thì ta cần thêm quyền ghi (**write**) trên filesystem. Ta sẽ tiến hành remount lại filesystem đã được remount quyền đọc-ghi (read-write)
   ```
   mount -o remount, rw /sysroot
   mount | grep -w "/sysroot"
   ```
 
 ![Imgur](https://i.imgur.com/4YqlDzL.png)

 - Lúc này filesystem đã được mount và ta sẽ chuyển đổi sang môi trường filesystem (prompt: **sh-4.2#**)

  `chroot /sysroot`

 - Tiến hành reset password user root

 ![Imgur](https://i.imgur.com/sTMw9Qg.png)

### Bước 4: relabel SELINUX
 - Chạy lệnh sau để update lại các thông số cấu hình SELINUX, nếu bạn có sử dụng SELINUX. Nguyên nhân khi ta update file **/etc/passwd** chứa mật khẩu thì các thống số SELINUX security contex sẽ khác nên cần update lại

 ![Imgur](https://i.imgur.com/m3hZGs0.png)

### Bước 5: remount và reboot
 - Remount filesystem "**/**" ở chế độ **read-only**

 ![Imgur](https://i.imgur.com/Mtaphdx.png)

 - Thoát môi trường **Chroot** và khởi động lại hệ thống

 ![Imgur](https://i.imgur.com/19Z2iXx.png)


## Cách 2:
 * Khởi động lại máy và nhấn phím bất kỳ
 * Nhấn **e** bạn sẽ được chuyển vào màn hình sau:

![Imgur](https://i.imgur.com/vd4atU4.png)

* Tìm đến dòng **linux16** : xóa `rhgb quite` và thay thế nó bằng `init=/bin/bash`
* Sau khi chỉnh sửa xong **Ctrl+x**, nó sẽ bắt đầu khởi động với tham số đã chỉ định, và hiển thị **bash prompt**

![Imgur](https://i.imgur.com/keJvwdl.png)

* Kiểm tra trạng thái của phân vùng gốc bằng lệnh sau:
   * `mount | grep root`

![Imgur](https://i.imgur.com/pjL4op7.png)

 * Bạn có thể nhận thấy, phân vùng gốc hiển thị `ro` (chỉ đọc). Cần chỉnh sửa quyền đọc ghi trên phân vùng gốc để thay đổi mật khẩu gốc

 `mount -o remount, rw /`

 * Kiểm tra lại phân vùng
 * Bây giờ có thể đặt lại mật khẩu cho root

    * `passwd root`
    * `touch /.autorelable`

![Imgur](https://i.imgur.com/7K0rK1D.png)

 * Khởi động lại 
    * `exec /sbin/init`

 * Đăng nhập vào tài khoản root
# Logical Volume Manager(LVM)
## Giới thiệu về LVM
*LVM là một công cụ để quản lý phân cùng logic được tạo và phân bổ từ ổ đĩa vật lí. Với LVM bạn có thể dẽ dàng tạo mới, thay đổi kích thước hoặc xóa bỏ phân vùng đã tạo.*

*LVM được tạo sử dngj cho các mục đích sau*
 * Tạo 1 hoặc nhiều phần vùng logic hoặc phân cung với toàn bộ đĩa cứng(hơi giống với RAID 0, nhưng tương tự như JBOD), cho phép thay đổi kích thước volume
 * Quản lý trang trại đĩa cứng lớn (Large hard Disk Farms) bằng cách cho phép thêm và thay thế đĩa mà không bị ngừng hoạt động hoặc dán đoạn dịch vụ, kết hợp với trao đổi nóng (hot swapping)
 * Trên các hệ thống nhỏ(như máy tính để bàn), thay vì phải ước tính thời gian cài đặt, phân cùng có thể cần lớn hơn đến mức nào, LVM cho phép các hệ thống tệp dễ dàng thay đổi kích thước khi cần.
 * Thực hiện sao lưu nhất quán bằng cách tạo snapshot nhanh các khối một cách hợp lý
 * Mã hóa nhiều phân vùng vật lý bằng một mật khẩu

*Cơ bản, **LVM**(Logical Volume Manager) bao gồm:*
 * **Physical volumes**: là những đĩa cứng vật lý hoặc phân vùng trên nó. (`/dev/fileserver/share, /dev/fileserver/backup,/dev/fileserver/media`)
 * **Volume groups**: là một nhóm bao gồm các Physical volume. Có thể xem Volume group như 1 "ổ đĩa ảo". (`fileserver`)
 * **Logical volumes**: có thể xem như là các "phân vùng ảo" trên "ổ đĩa cứng" bạn có thể thêm vào, gỡ bỏ và thay đổi kích thước một cách nhanh chonggs. (`/dev/sda1, /dev/sdb1, /dev/sdc1, /dev/sdd1`)

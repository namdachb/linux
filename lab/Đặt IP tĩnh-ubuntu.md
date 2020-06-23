## Cấu hình IP tĩnh trên Ubuntu server 20.04 với Netplan

### Giới thiệu netplan
Là một công cụ cấu hình mạng được giới thiệu trong Ubuntu. Nó có thể được sử dụng để viết mô tả YAML(là một chuẩn dữ liệu để tất cả các ngôn ngữ có thể đọc được) đơn giản về giao diện mạng với những thông tin cơ bản và tạo ra cấu hình cần thiết cho một công cụ kết xuất được chọn

### Cấu hình Netplan
Với mặc định một card mạng ens33 ở chế độ NAT. Bây giờ sẽ tiến hành gắn thêm một cấu hình card mạng và cấp ip cho nó

Kiểm tra thiết bị đang có trong mạng:
![Imgur](https://i.imgur.com/K9xprvj.png)

Card mạng ens38 đã tồn tại tuy nhiên nó chưa được cấp địa chỉ IP. Tiếp theo sẽ cấu hình cho nó bằng cách vào thư mục `/etc/netplan/` và chỉnh sửa file 50-cloud-init.yaml.(Lưu ý khi viết vào tệp yaml bạn phải tuân thủ theo đúng cú pháp căn dòng của nó). Ở đây chúng ta muốn cấp IP động nên sẽ ghi vào nội dung file như sau:
  
 * Đặt IP động :

![Imgur](https://i.imgur.com/79dzlQH.png)

Lưu và restart cấu hình bằng câu lệnh **netplan apply**

 * Đặt IP tĩnh:

Ta thấy ens38 đang nhận IP động , ta sẽ tiến hành đặt IP tĩnh theo số liệu sau
   * ip address: 192.168.17.11
   * gateway: 192.168.17.1
   * subnet mask: 255.255.255.0
   * DNS-nameserver: 8.8.8.8

![Imgur](https://i.imgur.com/oX7awxn.png)

Tiếp theo lưu thay đổi file và dùng lệnh `netplan apply` để thiết lập lại cấu hình 

Dùng lệnh kiểm tra kết quả

![Imgur](https://i.imgur.com/NAY5Yo2.png)

Như vậy là chúng ta đã cấp xong IP tĩnh cho ubuntu qua cấu hình **netplan**

## Cấu hình đặt ip tĩnh bằng ifupdown trên ubuntu server 20.04

*Các thao tác sẽ thực hiện dưới quyền root*

### Disable Netplan
Tắt `netplan`:
`echo 'GRUB_CMDLINE_LINUX = "netcfg/do_not_use_netplan = true" >>  /etc/default/grub`

Cập nhật lại `grub`:
 
 `update-grub`

### Cài đặt `ifupdown` thay thế `netplan`
Cài đặt `ifupdown` bằng câu lệnh:
```
apt-get update
apt-get install -y ifupdown
```

### Xóa `netplan` khỏi hệ thống
Xóa bỏ netplan khỏi hệ thống:

`apt-get --purge remove netplan.io`

Nếu bạn chưa cài đặt `ifupdown`, khi xóa bỏ netplan hệ thống sẽ tự cài đặt `ifupdown` thay thế cho bạn

Sau đó ta xóa toàn bộ cấu hình của netplan:
```
rm -rf /usr/share/netplan
rm -rf /etc/netplan
```

### Cấu hình interface
File cấu hình interface: `/etc/network/interfaces`

Kiểm tra IP của máy. Ở đây interface là `ens33`, và đang là IP động

![Imgur](https://i.imgur.com/hFpMDXm.png)

Chỉnh sửa file cấu hình:

`vi/etc/network/interfaces`

Thêm các dòng sau vào file cấu hình:
```
auto lo
iface lo inet loopback

auto ens33
iface ens33 inet static
address 192.168.17.10
netmask 255.255.255.0
gateway 192.168.17.1
broadcast 192.168.17.255
dns-nameservers 8.8.8.8
dns-search lan
```

Chú ý: Cần xác định đúng tên interface và các thông số về IP của bạn.

Reboot hệ thống để IP được nhận.

### Kiểm tra kết nối mạng
Sau khi máy reboot. Ta kiểm tra lại IP đã đặt

![Imgur](https://i.imgur.com/KPdxvlr.png)
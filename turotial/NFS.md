# Network Filesystem
Sử dụng **NFS** (hệ thống tệp mạng) là một trong những phương pháp được sử dụng để chia sẻ dữ liệu trên các hệ thống vật lý. Nhiều quản trị viên hệ thống gắn các thư mục của người dùng từ xa trên một máy chủ để cấp cho họ quyền truy cập vào cùng một tệp và tệp cấu hình trên nhiều hệ thống máy khách. Điều này cho phép người dùng đăng nhập vào các máy khác nhau nhưng vẫn có quyền truy cập vào cùng các tệp và tài nguyên

### Lợi ích của NFS
 * **NFS** cho phép truy cập cục bộ vào các tệp từ xa
 * Nó sử dụng kiến trúc client/server tiêu chuẩn để chia sẻ tệp giữa các máy
 * Với **NFS**, không cần thiết cả hai máy đều chạy trên một hệ điều hành
 * Với sự trợ giúp của **NFS**, có thể định cấu hình các giải pháp lưu trữ tập trung 
 * Người dùng có được dữ liệu của họ bất kể vị trí thực tế
 * Không cần làm mới thủ công cho các tập tin mới
 * Phiên bản mới hơn của NFS cũng hỗ trợ acl, mount root ảo
 * Có thể được bảo mật với tường lủa và **Kerberos**

### Dịch vụ NFS 
Đây là một System V-launched. NfS bao gồm `portmap` và `nfs-utils package` :
 * `portmap` : Nó ánh xạ các cuộc gọi được thực hiện từ các máy khác đến dịch vụ RPC chính xác (không bắt buộc với NFSv4)
 * `nfs` : Nó dịch các yêu cầu chia sẻ từ xa thành các yêu cầu trên hệ thống tệp cục bộ
 * `rpc.mountd` : Dịch vụ này có trách nhiệm lắp và unmount toàn bộ các hệ thống tập tin

### Các tệp quan trọng cho cấu hình NFS
 * `/etc/export` : Đây là tệp cấu hình chính của NFS, tất cả các tệp và thư mục đã xuất được xác định trong tệp này ở cuối máy chủ NFS
 * `/etc/fstab` : Để gắn một thư mục NFS trên hệ thống của bạn trên các lần khởi động lại, chúng ta cần tạo một thư mục trong /etc/fstab
 * `/etc/sysconfig/nfs` : Tệp cấu hình của NFS để kiểm soát cổng rpc và các dịch vụ khác đang nghe

# Network Advanced
### 1. `ping` 
 * Lệnh `ping` gửi các gói `ECHO_REQUEST` tới đỉa chỉ chỉ định
 * Lệnh này nhằm kiểm tra máy tính có thể kết nối với Internet hay một địa chỉ IP cụ thể nào đó hay không
 * Không giống lệnh `ping` trên Windows, câu lệnh `ping` trên Linux sẽ duy trì gửi các gói tin cho đến khi bạn kết thúc nó. Có thể định số lượng gói tối đã gửi đi bằng cách gõ thêm tùy chọn `-c`
  `# ping -c [number] [IP/domain_destination]`

```
[root@localhost ~]# ping -c 4 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=128 time=36.8 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=128 time=80.5 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=128 time=40.1 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=128 time=29.3 ms

--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3034ms
rtt min/avg/max/mdev = 29.379/46.744/80.560/19.911 ms
```

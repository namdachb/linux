## Triển khai NFS
### Mô hình 
2 máy CentOS 7:
 * Client
 * Server
Cài đặt và cấu hình NFS để chia sẻ giữa Client với Server

### Cài đặt NFS trên NFS_CLient và NFS_Server

`sudo yum install -y nfs-utils`

### IP của 2 máy
Cấu hình IP 2 máy client và server:

![Imgur](https://i.imgur.com/7IucKF4.png)

### Thiết lập NFS_Server
Ta tạo 1 thư mục tài nguyên chia sẻ:

`sudo mkdir /shared`

Cấu hình thư mục chia sẻ: `/etc/exports`, mở `/etc/exports` và thêm vào dòng sau:

`/shared 192.168.213.164/24(no_root_squash,no_all_squash,rw,sync)`

Trong đó:
 * `/var/shared` : là đường dẫn thư mục được chia sẻ
 * `192.168.213.164/24` : là dải ip hoặc ip của client
 * `rw` : là quyền truy cập thư mục chia sẻ
 * `sync` : đồng bộ hóa thư mục share
 * `root_squash` : vô hiệu hóa đặc quyền root
 * `no_root_squash` : cho phép đặc quyền root
 * `no_all_squash` : cho phép người dùng có quyền truy cập
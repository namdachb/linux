## rsync để làm gì?
Rsync (remote sync) là công cụ đồng bộ file, thư mục của Linux được dùng rát phổ biến. Với sự giúp đỡ của rsync, chúng ta có thể đồng bộ dữ liệu trên local hoặc giữa các server với nhau 1 cách dễ dàng

Lệnh `rsync` được sử dụng để đồng bộ hóa toàn bộ cây thư mục. Về cơ bản, nó sao chép tập tin như lệnh `cp`. Ngoài ra, `rsync` kiểm tra xem tệp đã được sao chép chưa. Nếu tệp tồn tại và không có thay đổi về kích thước hoặc thời gian sửa đổi `rsync` sẽ tránh một bản sao không cần thiết và tiết kiệm thời gian. Hơn nữa vì `rsync` chỉ sao chép các thành phần của tệp đã thực sự thay đổi, nên nó có thể rất nhanh

### Cài đặt Rsync
Mặc định hầu hết các bản phân phối Linux có sẵn công cụ này, nếu chưa có thì chúng ta sẽ tại về:
  * Trên CENTOS/RED HAT
   ```
   yum install rsync
   ```
  * Trên Ubuntu
   ```
   apt-get install rsync
   ```

### Cú pháp của lệnh Rsync

`rsync option source destination`

Trong đó:
 * **Source** : đường dẫn thư mục chứa dữ liệu gốc muốn đồng bộ, nơi truyền dữ liệu
 * **destination** : đường dẫn nơi chứa dữ liệu đồng bộ đến, nơi nhận dữ liệu
 * **options** : các tham số tùy chọn

Một số tùy chọn thường được sử dụng trong lệnh rsync được liệt kê bên dưới:
 * `-a` : cho phép copy dữ liệu recursively, đồng thời giữ nguyên được tất cả các thông số của thư mục và file
 
 * `-v` : show trạng thái truyền tải file ra màn hình command line để chúng ta theo dõi
 * `-h` : kết hợp với `-v` để định dạng dữ liệu show ra dễ nhìn hơn
 * `-z` : nén dữ liệu trước khi truyền đi giúp tăng tốc quá trình đồng bộ file
 * `-e` : sử dụng giao thức SSH để mã hóa dữ liệu
 * `-P` : dùng khi đường truyền không ổn định, nó sẽ gửi tiếp các file chưa được gửi đi khi có kết nối trở lại
 * `--delete` : xóa dữ liệu ở destination nếu source không tồn tại dữ liệu đó
 * `--exclude` : loại trừ ra dữ liệu không muốn truyền đi, nếu cần loại ra nhiều file hoặc folder ở nhiều đường dẫn khác nhau thì mỗi cái bạn phải thêm `--exclude` tương ứng
 * `--include` : bao gồm những dữ liệu muốn truyền đi

### Làm việc với rsync
### 1. rsync cơ bản

`rsync -a [thư_mục_nguồn] [thư_mục_đích]`

ví dụ máy chúng ta có thư mục **test1** trong nó chứa nhiều file, thư mục con. Giờ muốn đồng bộ toàn bộ thư mục **test1** vào 1 thư mục khác là **test2** (2 thư mục cùng nằm trên 1 máy)

`rsync -anv test1/ test2`

Hoặc chúng ta sao chép sang 1 thư mục đích không tồn tại và `rsync` sẽ tạo ra nó

`rsync -a test1/ /var/www`

Điều đáng nói là `rsync` cung cấp cách xử lý khác nhau cho các thư mục nguồn bằng dấu gạch chép `/`. Nếu bạn thêm 1 dấu gạch chéo vào thư mục nguồn, nó sẽ chỉ sao chép nội dung của thư mục vào thư mục đích. Khi dấu gạch chéo bị bỏ qua `rsync` sẽ sao chép thư mục nguồn bên trong thư mục đích

### 2. Cách sử dụng rsync để đồng bộ hóa dữ liệu từ client đến server

Khi sử dụng `rsync` để truyền từ xa, nó phải được cài đặt trên cả nguồn và máy đích. Các phiên bản mới `rsync` được cấu hình để sử dụng SSH làm vỏ từ xa mặc định

Trong ví dụ dưới đây , chúng ta sẽ chuyển thư mục từ 1 máy client sang 1 máy server (từ xa):

`rsync -zavh /root/test user@IP:/root/demo`
```
[root@localhost ~]# rsync -zavh /root/test root@192.168.213.176:/root/demo
Enter passphrase for key '/root/.ssh/id_rsa':
sending incremental file list
test/
test/file.txt

sent 155 bytes  received 39 bytes  55.43 bytes/sec
total size is 39  speedup is 0.20
```

Nếu  muốn chuyển dữ liệu từ xa sang máy cục bộ thì ta cần sử dụng vị trí từ xa làm nguồn

`rsync -zavh user@IP:/root/demo /root/test`
```
[root@localhost ~]# rsync -zavh root@192.168.213.176:/root/demo /root/data
Enter passphrase for key '/root/.ssh/id_rsa':
receiving incremental file list
demo/
demo/test/
demo/test/file.txt

sent 55 bytes  received 187 bytes  69.14 bytes/sec
total size is 39  speedup is 0.16
```

### 3. Bao gồm và loại trừ các tệp tin khi đồng bộ hóa dữ liệu 
Ví dụ trong 1 thư mục có nhiều file có định dạng khác nhau : **txt**, **conf**, **md**, ... mà chúng ta muốn cóp một loại hoặc lấy hết nhưng không muốn lấy một loại định dạng nào :
  `rsync --include '*.txt* --exclude '*.md' -zavh source user@IP:/destination
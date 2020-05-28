## rsync để làm gì?
Rsync (remote sync) là công cụ đồng bộ file, thư mục của Linux

Lệnh `rsync` được sử dụng để đồng bộ hóa toàn bộ cây thư mục. Về cơ bản, nó sao chép tập tin như lệnh `cp`. Ngoài ra, `rsync` kiểm tra xem tệp đã được sao chép chưa. Nếu tệp tồn tại và không có thay đổi về kích thước hoặc thời gian sửa đổi `rsync` sẽ tránh một bản sao không cần thiết và tiết kiệm thời gian. Hơn nữa vì `rsync` chỉ sao chép các thành phần của tệp đã thực sự thay đổi, nên nó có thể rất nhanh

Mặc định hầu hết các bản phân phối Linux có sẵn công cụ này, nếu chưa có thì chúng ta sẽ tại về:
  * Trên CENTOS/RED HAT
   ```
   yum install rsync
   ```
  * Trên Ubuntu
   ```
   apt-get install rsync
   ```


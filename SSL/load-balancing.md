# Thiết lập cân bằng tải(load balancer) trong hệ thống máy chủ Nginx

## Cân bằng tải (Load Balancing) là gì?
 * `Load Balancing` hay còn gọi là cân bằng tải là một kỹ thuật thường được sử dụng để tối ưu hóa việc sử dụng tài nguyên, băng thông, giảm độ trễ và tăng cường khả năng chịu lỗi
 * Khi chúng ta có nhiều hơn một webserver, cùng với đó là sự gia tăng lưu lượng truy cập thì việc bổ sung thêm một máy chủ để phân phối lưu lượng này một cách hợp lý là cần thiết. Máy chủ được bổ sung này được gọi là Load balancer

### Một số thuật toán được sử dụng trong các hệ thống cân bằng tải là
 * `Round robin` : là thuật toán điều phối vòng tròn, các máy chủ sẽ được xem ngang hàng và sắp xếp theo một vòng quay. Các truy vấn dịch vụ sẽ lần lượt được gửi tới các máy chủ theo thứ tự sắp xếp

 * `Weighted round robin` : tương tự như kỹ thuật Round Robin nhưng WRR còn có khả năng xử lý theo cấu hình của từng server đích. Mỗi máy chủ được đánh giá bằng một số nguyên (giá trị trọng số Weight - mặc định giá trị là 1). Một server có khả năng xử lý gấp đôi server khác sẽ được đánh số lớn hơn và nhận được số request gấp đôi từ bộ cân bằng tải
 * `Least connection` : các request sẽ được chuyển vào server có ít kết nối nhất trong hệ thống. Thuật toán này được coi như thuật toán động, vì nó phải đếm số kết nối đang hoạt động của server
 * `Least response time` : đây là thuật toán dựa trên tính toán thời gian đáp ứng của mỗi server (response time), thuật toán này sẽ chọn server nào có thời gian đáp ứng nhanh nhất. Thời gian đáp ứng được xác định bởi khoảng thời gian giữa thời điểm gửi một gói tín đến server và thời điểm nhận được gói tin trả lời 
 * `IP Hash` : thuật toán xác định kết nối chính xác từ một IP của máy khách sẽ được kết nối trực tiếp đến một server backend

Mô hình

![Imgur](https://i.imgur.com/UnQVVNu.png)

### Thiết lập ban đầu
**[Tại node Load Balancer]**

 * Thiết lập hostname, cập nhật hệ thống

```
hostnamectl set-hostname loadbalancer
yum update
```

 * Tắt Firewall và SELinux

```
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
systemctl stop firewalld
systemctl disable firewalld
```

 * Cấu hình host file

```
echo "192.168.11.147 loadbalancer" >> /etc/hosts
echo "192.168.11.149 web1" >> /etc/hosts
echo "192.168.11.148 web2" >> /etc/hosts
```

 * Khởi động lại hệ thống

`init 6`

**[Tại node web1]**

 * Thiết lập hostname, cập nhật hệ thống

`hostnamectl set-hostname webserver1`

 * Tắt Firewall và SELinux

```
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
systemctl stop firewalld
systemctl disable firewalld
```

 * Cấu hình host file

```
echo "192.168.11.147 loadbalancer" >> /etc/hosts
echo "192.168.11.149 web1" >> /etc/hosts
echo "192.168.11.148 web2" >> /etc/hosts
```

 * Khởi động lại hệ thống

`init 6`

**[Tại node web2]**

 * Thiết lập hostname, cập nhật hệ thống

`hostnamectl set-hostname webserver2`

 * Tắt Firewall vầ SELinux

```
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
systemctl stop firewalld
systemctl disable firewalld
```

 * Cấu hình host file

```
echo "192.168.11.147 loadbalancer" >> /etc/hosts
echo "192.168.11.149 web1" >> /etc/hosts
echo "192.168.11.148 web2" >> /etc/hosts
```

 * Khởi động lại hệ thống

`init 6`

### Cài đặt
**[Trên node Nginx Load Balancer]**

 * Thêm repo của Nginx

`yum install -y epel-release`

 * Cài đặt Nginx

`yum install -y nginx`

 * Khởi động nginx

```
systemctl enable nginx
systemctl start nginx
```

 * Khởi động firewall

```
systemctl enable firewalld
systemctl start firewalld
```

 * Cấu hình firewall và restart lại dịch vụ

```
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --reload
```

 * Truy cập vào địa chỉ IP của server để kiểm tra

 * Sửa file config của Nginx

`vi /etc/nginx/nginx.conf`

 * Thêm vào http block cấu hình sau

```
upstream backends {
    server 192.168.11.149:80 weight=3;
    server 192.168.11.148:80 weight=2;
}
```
Cấu hình trên có nghĩa là cứ 5 request gửi tới server sẽ có 3 request vào web 1 và 2 request vào web2

 * Tại block server thêm hoặc sửa các cấu hình thành như sau

```
server {

    listen      80 default_server;
    listen      [::]:80 default_server;
    server_name _;

    proxy_redirect           off;
    proxy_set_header         X-Real-IP $remote_addr;
    proxy_set_header         X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header         Host $http_host;

    location / {
        proxy_pass http://backends;
    }
```

 * Restart lại nginx

 `nginx -s reload`

**[Trên các node Apache Web server]**

 * Cài đặt Apache

`yum install -y httpd`

 * Khởi động apache

```
systemctl enable httpd
systemctl start httpd
```
 
 * Truy cập thư mục `/var/www/html`
   * Tạo file `index.html`
          
        `vi index.html`
   * Thêm nội dung vào file index.html

        ```
        <h1>
        DAY LA WEBSERVER 1 (2)
        </h1>
        ```

**Kiểm tra**
 
 * Truy cập vào địa chỉ của máy cài Nginx
    * Lần 1

    ![Imgur](https://i.imgur.com/gizKKRf.png)

    * Lần 2

    ![Imgur](https://i.imgur.com/c2HzRNL.png)

## Cấu hình với các thuật toán

`vi /etc/nginx/nginx.conf`

### Round Robin
 * Round Robin là thuật toán mặc định của nginx khi chúng ta không có cấu hình gì thêm trong block `http`
 * Đặc điểm của thuật toán này là các request sẽ được luân chuyển liên tục 1:1 giữa các server, điều này sẽ làm giải tỏa cho các hệ thống có lượng request lớn
 * Cấu hình chi tiết trong file config
  ```
  nginx
  http {

  upstream backends {
      server 192.168.11.149:80;
      server 192.168.11.148:80;
  }
  ```

### Least connection
 * Đây là thuật toán nâng cấp của round robin và weighted load balancing, thuật toán này sẽ giúp tối ưu hóa cân bằng tải cho hệ thống
 * Đặc điểm của thuật toán này là sẽ chuyển request đến cho server đang xử lý ít hơn làm việc, thích hợp đối với các hệ thống mà có các session duy trì trong thời gian dài, tránh được trường họp các session duy trì quá lâu mà các request được chuyển luân phiên theo quy tắc định sẵn, dễ bị down 1 server nào đó do xử lý quá khả năng của nó
 * Cấu hình chi tiết
 ```
 http {

  upstream backends {
      least_conn;
      server 192.168.11.149:80;
      server 192.168.11.148:80;
  }
 ```

### Health check 
 * Thuật toán này xác định máy chủ sẵn sàng xử lý request để gửi request đến server, điều này tránh được việc phải bỏ thủ công một máy chủ không sẵn sàng xử lý
 * Các hoạt động của thuật toán này là nó sẽ gửi một kết nối TCP đến máy chủ, nếu như máy chủ đó lắng nghe trên địa chỉ và port đã cấu hình thì nó mới gửi request đến cho server xử lý
 * Tuy nhiên health check vẫn có lúc kiểm tra xem máy chủ có sẵn sàng hay không, đối với các máy chủ cơ sở dữ liệu thì health check không thể làm điều này
 * Cấu hình chi tiết 
 
 ```
   http {

  upstream backends {
      server 192.168.11.149:80;
      server 192.168.11.148:80 max_fails=3 fail_timeout=5s;
      server 192.168.11.147:80;
  }
 ```

### Load balancing kết hợp thuật toán
 * Các thuận toán không bao giờ có thể hữu dụng trong tất cả các trường hợp, việc lựa chọn thuận toán dựa trên cơ sở hạ tầng chúng ta có cũng như mục đích sử dụng, để có thể tối ưu hóa hơn trong việc cân bằng tải thông thường chúng ta sẽ kết hợp các thuật toán lại với nhau để có thể đưa ra được giải pháp cân bằng tải hợp lý nhất cho hệ thống. Sau đây là một số giải pháp kết hợp

#### Kết hợp least balancing và weight load balancing
 * Thuật toán least load balancing giúp hệ thống có thể lựa chọn server đang xử lý ít hơn để gửi request cho server đó xử lý. Ngoài ra nó còn có thể tự loại bỏ server bị lỗi trong vòng xử lý của nó. Tuy nhiên least load balancing chỉ hữu hiệu khi chúng ta có 2 server nó cùng cấu hình. Giả sử chúng ta có 2 server, server1 có cấu hình mạnh gấp 2 lần server2 thì chúng ta dùng least load balancing thì đến một thời điểm nào đó con server2 rất dễ bị chết. Do đó để trành trường hợp này chúng ta có một giải pháp có thể giảm thiểu khả năng chết của con thứ 2 đó là kết hợp thêm thuật toán weighted load balancing
 * Chi tiết cấu hình trong trường hợp trên như sau

 ```
 http {

    upstream backends {
        least_conn;
        server 192.168.11.149:80 weight=2;
        server 192.168.11.148:80 weight=1;
    }
 ```
 
### Chú ý:
 * `weight` : trọng số ưu tiên của server này
 * `max_fails` : số lần tối đa mà load balancer không liên lạc được với server này (trong khoảng fail_timeout) trước khí server này bị coi là down
 * `fail_timeout` : khoảng thời gian mà một server phải trả lời load balancer, nếu không trả lời thì server này sẽ bị coi là down. Đây cũng là thời gian downtime của server này
 * `backup` : những server nào có thông số này sẽ chỉ nhận request từ load balancer một khi tất cả các server khác đều bị down
 * `down` : chỉ thị này cho biết server này hiện không thể xử lý các request được gửi tới. Load balancer vẫn lưu server này trong danh sách nhưng sẽ không phân tải cho server này cho đến khi chỉ thị này được gỡ bỏ
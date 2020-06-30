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


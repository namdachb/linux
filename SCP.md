## Tìm hiểu lệnh SCP
### 1. SCP là gì?
 * **SCP** (Secure Copy - Sao chép an toàn) là một ứng dụng sử dụng SSH để mã hóa toàn bộ quá trình chuyển tập tin
 * **SCP** là lệnh dùng để di chuyển file dữ liệu giữa các máy tính chạy hệ điều hành Linux từ xa chỉ cần biết địa chỉ IP
 * **SCP** dùng ssh để di chuyển dữ liệu, có chế độ bảo mật giống như ssh

### 2. Cài đặt
 * **SCP** có sẵn trong các bản dis của hệ điều hành Linux. Nếu chưa có , cài đặt như sau:

    ```
    Ubuntu/Debian : sudo apt-get install scp -y
    Centos/Redhat : yum install scp -y
    ```

Cú pháp của SCP:

 `scp [other options] [source username@ip]:/[directory and file name] [destination username@ip]:/[destination directory]`

 
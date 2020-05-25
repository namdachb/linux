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

### 3. Sử dụng SCP
Mô hình :

![Imgur](https://i.imgur.com/ddqOEwd.png)

 * Client: IP 192.168.213.183 máy gửi dữ liệu, chúng ta ngồi trực tiếp trên máy này gõ lệnh gửi file
 * Server: IP 192.168.213.182 User root, pass namdachb , máy nhận dữ liệu

Cú pháp lệnh cơ bản 
 `scp source_file username@destination_host:/destination_folder`

Đẩy file `nam.txt` lên server /root/data
 
 ```
 [root@localhost ~]# scp nam.txt root@192.168.213.182:/root/data
root@192.168.213.182's password:
nam.txt                                                         100%    0     0.0KB/s   00:00
 ```

Muốn truyền nhiều file cùng lúc dùng lệnh

 ```
 [root@localhost ~]# scp nam.txt file.txt root@192.168.213.182:/root/data
root@192.168.213.182's password:
nam.txt                                                         100%    0     0.0KB/s   00:00
file.txt                                                        100%    0     0.0KB/s   00:00
 ```

Muốn copy cả thử mục từ client sang server ta thêm tham số `-r`

Ví dụ: ta muốn copy cả thư mục test ta dùng lệnh
```
[root@localhost test]# ls
demo.txt  file.txt  nam
```

`scp -r /root/test root@192.168.213.182:/root/data`

Kết quả khi sang máy server:
```
[root@localhost ~]# cd data
[root@localhost data]# ls
test
```

Muốn copy dữ liệu ngược lại từ server về client thì gõ lệnh sau:

 ```
 [root@localhost ~]# scp root@192.168.213.182:/root/data/test.txt /root/
root@192.168.213.182's password:
test.txt                                                        100%    0     0.0KB/s   00:00
 ```


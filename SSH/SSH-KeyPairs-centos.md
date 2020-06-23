# LAB : SSH Keypairs

![Imgur](https://i.imgur.com/GxUc1oO.png)

### Bước 1 : Tạo cặp key RSA trên SSH Client:
 * Bước đầu tiên là tạo ra cặp **SSH Key Pair** trên **SSH Client** hay chính máy tính thực hiện **SSH** :

```
[root@centos7-01 ~]# ssh-keygen
```

 * Mặc định, lệnh `ssh-keygen` sẽ tạo ra 1 cặp **RSA key pair `2048-bit`**, gần như đáp ứng đủ mọi trường hợp. Nếu muốn cặp key phức tạp hơn, có thể tạo key với độ dài `**4096-bit**` bằng option `-b 4096`

 * Sau khi thực hiện lệnh, bạn sẽ nhìn thấy output sau:

 ```
 Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
 ```

 * Gõ `enter` để lưu cặp key vào thư mục con `.ssh/` nằm trong thư mục `home` của user hiện hành, hoặc tự chọn 1 đường dẫn khác

 * Nếu trên máy đã có 1 cặp key từ trước đó, bạn sẽ nhìn thấy output sau:

 ```
 [root@centos7-01 ~]# ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
/root/.ssh/id_rsa already exists.
Overwrite (y/n)?
 ```

 * Nếu chọn "`overwrite the key on disk`", bạn sẽ không thể xác thực các key đang sử dụng trước đây nữa

 * Sau khi lựa chọn, sẽ thấy output tiếp theo:

 ```
 Enter passphrase (empty for no passphrase):
 ```

 * Đây là tùy chọn thêm 1 chuỗi mật khẩu, được khuyến nghị để tăng tính bảo mật. Nếu nhập chuổi **passphrase** này bạn sẽ phải gõ thêm chúng bất kỳ lúc nào sử dụng key (chỉ trừ khi sử dụng phần mềm để SSH đã lưu trữ passphrase). Nếu không muốn sử dụng **passphrase**, có thể `enter` để bỏ qua. Nếu nhập **passphrase**, sẽ thấy output sau:

 ```
 Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:CoeZ9xrKe8IQ4voTni02aQzGBKxgrmxbL69g7c5SPsY root@centos7-01
The key's randomart image is:
+---[RSA 2048]----+
|.                |
|oo               |
|*                |
|oo.  +           |
|*. .= o S        |
|o*+o + o         |
|+**O. o .        |
|oo#E+o.o         |
| +=OO=.          |
+----[SHA256]-----+
 ```

### Bước 2 : Copy Public Key vào SSH Server :
 * Cách nahnh nhất để copy **Public Key** trên Centos là sử dụng tiện ích `ssh-copy-id` vì nó khá đơn giản. Nếu không có sẵn `ssh-copy-id`, cần phải copy 1 cách thủ công 

#### Cách 1 : Copy Public Key sử dụng `ssh-copy-id`
 * Công cụ `ssh-copy-id` thường có sẵn trên nhiều hệ điều hành. Nếu dùng cách này, cần có kết nối SSH bằng mật khẩu từ client đến server:

 `ssh-copy-id username@remote_host`

 ```
 [root@centos7-01 ~]# ssh-copy-id root@192.168.213.139
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/root/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@192.168.213.139's password:
```

 * Nhập mật khẩu và gõ `enter`. Công cụ sẽ kết nối tới Server bằng tài khoản được cung cấp. Sau đó nó sẽ copy nội dung file `~/.ssh/id_rsa.pub` vào 1 file tên là `authorized_keys` trong thư mục `~/.shh` của Server

 * Tại bước này, key `id_rsa.pub` đã được upload lên Server

 ```
 [root@centos7-02 ~]# ls -a /root/.ssh
.  ..  authorized_keys
[root@centos7-02 ~]#
```

#### Cách 2 : Copy Public Key sử dụng SSH
 * Nếu không có sẵn tiện ích `ssh-copy-id` , có thể sử dụng phương pháp truyền thống để copy **public key** sang Server.

 * Sử dụng lệnh pipe sau:
 ```
 [root@centos7-01 ~]# cat ~/.ssh/id_rsa.pub | ssh root@192.168.213.139 "mkdir -p  ~/.ssh && touch ~/.ssh/authorized_keys && chmod -R go= ~/.ssh && cat >> ~/.ssh/a uthorized_keys"
```

![Imgur](https://i.imgur.com/UWelx8h.png)

 * Sau khi nhập password, nội dung file `id_rsa.pub` sẽ được copy sang file `~/.ssh/authoized_keys` 

#### Cách 3 : Copy thủ công
 * Nếu không có cách nào để truy cập Server qua SSH, có thể thực hiện copy thủ công qua USB hay bất cứ cách nào khác
 * Xem nội dung file `id_rsa.pub` và copy:
   
```
[root@centos7-01 ~]# cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC9YU8AnF3SAGmAPVp8ZQzgvryJaiyGZnjL0X9PubOYyfoLnoRP0wZ8JkK89p5uYlbS9Nt7KZBPi6Vn9SoiVri39n6wWphSns9UzmCXLa5tu2DIvLaIKOm9eLEdoOpWu8BeXTM10tpC5ZFDdbQEzO4I/p19BB0ZTdcdAAwAdcPV+rMOxwo80ORapeuKLcQWQY3UMTLcByN0fq6XNHFTGfX9/XboSN+IXZnE9CjlC1l8zW4Ugl4W2LtF0vsbuJQugECgzqFPVQFLBUALvosBPLmbuEBsKk01d1ko8u1WfanUOtYzhKABDNeP37woENx69rRd/hz/NVZGLG9SC/m7 root@centos7-01
```

### Bước 3: Xác thực centos server sử dụng SSH Key
 * Sau khi hoàn thành các bước trên, nhập lệnh sau để **SSH** vào server:
   
   ```
   [root@centos7-01 ~]# ssh root@192.168.213.139
   ```

 * Nếu đã tạo **passphrase** thì ở bước này phải nhập thêm **passphrase**, nếu không thì có thể truy cập được luôn

### Bước 4 : Tắt xác thực mật khẩu trên Server
 * Mặc định, tồn tại song song cả 2 chế độ xác thực qua **SSH Key** và xác thực qua mật khẩu. Vì vậy, vẫn có khả năng Server bị tấn công bằng **Brute Force**

 * Trên Server, mở file cấu hình `sshd`:

   ```
   [root@centos7-02 ~]# vi /etc/ssh/sshd_config
   ```

 * Kéo xuống dòng `65`, sửa `yes` thành `no`

     `PasswordAuthentication no`

 * Restart dịch vụ **SSH** 
    ```
    [root@centos7-02 ~]# systemctl restart sshd.service
    ``` 


## SSH window(= privatekey) -> Server
Nếu sử dụng window để SSH đến , tiến hành copy file private key ra máy và load bằng PuTTY hoặc Mobaxterm. Ở đây chúng ta dùng MobaXterm để load private key
 
 * Trên MobaXterm, ta vào **Tools** -> **MobaKeyGen** ![Imgur](https://i.imgur.com/kViGu1O.png)

 * Chọn file private key : chọn load
 ![Imgur](https://i.imgur.com/l6hVhdB.png)

 * Nhập `passphrase`
 ![Imgur](https://i.imgur.com/V1SgL2r.png)

 * Sau đó, lưu lại dưới dạng `ppk` và `Save private key`
 ![Imgur](https://i.imgur.com/Zst9W8w.png)

 * SSH và server :
 ![Imgur](https://i.imgur.com/VULs56K.png)

 * Nếu có `passphrase` thì bạn sẽ phải nhập `passphrase`
 ![Imgur](https://i.imgur.com/H7xunH7.png)


## Genkey bằng mobaXterm
 * Nếu dùng Windows, có thể dùng PuTTY hoặc MobaXterm để gen ssh keys. Ở đây ta dùng MobaXterm. Chọn loại key RSA và click generate

 ![Imgur](https://i.imgur.com/hWdEvYT.png)

 * Copy toàn bộ nội dung trong ô "Public key for pass into OpenSSH authorized_keys file" và lưu lại dưới tên `authorized_keys` rồi gửi lên Server. Đây là Public Key dành riêng cho OpenSSH. Nút "**Save public key**" sẽ cho một Public Key dạng khác, ta không cần quan tâm đến nút này

 * Nhập passphrase và chọn **Save Private key**. Việc tạo bộ khóa hoàn tất

 ![Imgur](https://i.imgur.com/2ihxqDP.png)

 * Đăng nhập, mở session mới, nhập địa chỉ ssh server, chọn **Advanced SSH settings -> Use private key** rồi chọn tới `private key` đã save

 ![Imgur](https://i.imgur.com/OUMEH1U.png)
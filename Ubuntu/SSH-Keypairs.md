## Cài đặt SSH keyparis trên ubuntu 20.04 server

**Client** : 192.168.213.172
**Server** : 192.168.213.170

### 1. Thực hiện trên client
 * Tạo ra cặp key

Bước đầu tiên ta thực hiện là phải tạo ra một cặp khóa trên client thường là máy tính của bạn được sử dụng để ssh tới server

  `ssh-keygen`

Sau khi nhập lệnh thì sẽ thấy hai dòng đầu được hiển thị
  
  ```
  Generating public/private rsa key pair
  Enter file in which to save the key (/root/.ssh/id_rsa):
  ```

Bước này là để xác định thư mục lưu trữ. Nếu bạn nhấn `Enter` thì mặc định nó sẽ lưu trữ thư mục trong ngoặc. Sau khi xong thi nó sẽ hiển thị 2 dòng tiếp sau

  ```
  Created directory '/root/.ssh'
  Enter passphrase (empty for no passphrase):
  Enter same passphrase again:
  ```
  
  * Dòng đầu tiên là để thông báo đã tạo xong thư mục 
  * Dòng thứ 2 được hỏi là có đặt mật khẩu cho key ssh này không. Mật khẩu này chỉ xác thực trên local không được gửi đi trên mạng khi mà ssh tới server
  * Dòng thứ 3 là xác nhận lại mật khẩu. Sau khi xác nhận xong là hoàn thành bước tạo key trên client và sẽ có output như sau

![Imgur](https://i.imgur.com/UGAlehy.png)

### Copy key lên ubuntu 20.04 serer

Sau khi đã tạo được cặp keyparis trên **client** thì việc tiếp theo cần làm đó chính là copy key public lên trên **server** để có thể sử dụng nó

Chúng ta sẽ sử dụng lệnh `ssh-copy-id` để copy public key lên server

Cú pháp của câu lệnh này là

 `ssh-copy-id username@remote_host`

Bạn sẽ nhận được output như sau:

```
namdac@nam:~$ ssh-copy-id namdac@192.168.213.170
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/namdac/.ssh/id_rsa.pub"
The authenticity of host '192.168.213.170 (192.168.213.170)' can't be established.
ECDSA key fingerprint is SHA256:hPmHnnBWRw5N2IfG7i1KLiIWfE4Y19hF3l4enHnn+wk.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

Hỏi xem bạn có thật sự muốn kết nối tới server không. Chắc chắn rồi vì chùng ta đang làm việc đó nên sẽ chọn yees

```
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that ar                                                                                         e already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to                                                                                          install the new keys
namdac@192.168.213.170's password:
```

Tiếp theo bạn cần nhập password để có thể copy được nó vào server này

```
Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'namdac@192.168.213.170'"
and check to make sure that only the key(s) you wanted were added.
```

Và sau khi nhập password xong. Bước này hoàn thành và bạn nhận được thông báo như trên

### Kiểm tra lại việc ssh
Ta sẽ thực hiện lệnh ssh để có thể kiểm tra xem việc ssh bằng key có được thực hiện hay không

 `ssh namdac@192.168.213.170`

Sau khi thực hiện lênh ssj thì ta nhận được output như bên dưới. Và đương nhiên ta sẽ không còn phải nhập password vẫn có thể remote được server

```
namdac@nam:~$ ssh namdac@192.168.213.170
Enter passphrase for key '/home/namdac/.ssh/id_rsa':
Welcome to Ubuntu 20.04 LTS (GNU/Linux 5.4.0-29-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed 20 May 2020 08:54:18 AM UTC

  System load:  0.14               Processes:              229
  Usage of /:   24.2% of 19.56GB   Users logged in:        1
  Memory usage: 20%                IPv4 address for ens33: 192.168.213.170
  Swap usage:   0%

 * MicroK8s passes 9 million downloads. Thank you to all our contributors!

     https://microk8s.io/

23 updates can be installed immediately.
11 of these updates are security updates.
To see these additional updates run: apt list --upgradable


Last login: Wed May 20 07:59:10 2020 from 192.168.213.1
```

### Disable ssh password 
Sau khi có thể sử dụng ssh bằng key để đăng nhập và điều khiển remote được server bằng máy tính của chúng ta thì đương nhiên việc ssh bằng password là đi không cần thiết

Công việc tiếp theo là chúng ta sẽ disable chức năng có thể ssh bằng password của server

  `sudo vi/etc/ssh/sshd_config`

Sau khi đăng nhập xong vào file thì ta tìm đến hai dòng là 

Sử dụng `:set nu` để hiện số dòng

 ```
58 PasswordAuthentication yes
34 PermitRootLogin prohibit-password
 ```

và ta sẽ sửa 2 dòng đó như sau. Sau khi sửa xong thì ta lưu lại và thử ssh lại 1 lần nữa

 ```
 PasswordAuthentication no
 PermitRootLogin no
 ```

Sau đó khởi động lại dịch vụ ssh

 `sudo systemctl restart sshd`


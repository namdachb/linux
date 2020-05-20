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
root@nam:~# ssh-copy-id root@192.168.213.170
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/root/.ssh/id_rsa.pub"
The authenticity of host '192.168.213.170 (192.168.213.170)' can't be established.
ECDSA key fingerprint is SHA256:hPmHnnBWRw5N2IfG7i1KLiIWfE4Y19hF3l4enHnn+wk.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

Hỏi xem bạn có thật sự muốn kết nối tới server không. Chắc chắn rồi vì chùng ta đang làm việc đó nên sẽ chọn yees

```
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@192.168.213.170's password:
```

Tiếp theo bạn cần nhập password để có thể copy được nó vào server này


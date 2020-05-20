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
   
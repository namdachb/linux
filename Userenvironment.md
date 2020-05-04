## Users and Groups
*Linux là một hệ điều hành nhiều người dùng, nơi có nhiều hơn một người dùng có thể đăng nhập cùng một lúc. Lệnh `who` liệt kê những người dùng đang đăng nhập. Để họ xác định người dùng hiện tại, sử dụng lệnh whoami.*
```
[root@localhost ~]# who
namdachb tty1         2020-05-03 17:34
root     pts/0        2020-05-03 23:33 (192.168.213.1)
```
*Linux sử dụng một nhóm để tổ chức người dùng. Nhóm là tập hợp các tài khoản với các quyền được chia sẻ nhất định. Kiểm soát thành viên nhóm được quản lý thông qua tệp `/etc/` nhóm, trong đó hiển thị danh sách các nhóm và thành viên của họ. Theo mặc định, mọi người dùng thuộc về một nhóm mặc định hoặc chính. Khi người dùng đăng nhập, thành viên nhóm được đặt cho nhóm chính của họ và tất cả các thành viên được hưởng cùng mức truy cập và đắc quyền. Quyền trên các tệp và thư mục khác nhau có thể sửa đổi ở cấp độ nhóm.*

*Tất cả người dùng Linux được gán một ID người duy nhất, **uid**,chỉ là một số nguyên, cũng như một hoặc nhiều ID ID nhóm, **gid**, bao gồm một ID mặc định giống với ID người dùng. Trong lịch sử các bản phân phối dựa trên RedHat bắt đầu từ 500. Các bản phân phối khác bắt đầu từ 1000. Các số này được liên kết với tên thông qua các tệp `/etc/passwd` và `/etc/group`. Các nhóm được sử dụng để thiết lập một nhóm người dùng có lợi ích chung cho các mục đích về quyền truy cập, đặc quyền và cân nhắc bảo mật. Quyền truy cập vafp tệp và thiết bị được cấp trên cơ sở người dùng và nhóm mà họ thuộc về.*

*Chỉ người dùng root mới có thể thêm và xóa người dùng và nhóm. Thêm người dùng mới được thức hiện bằng lệnh `useradd` và xóa người dùng hiện tại được thực hiện bằng lệnh `userdel`. Ở dạng đơn giản nhất, một tài khoản cho người dùng mới adriano sẽ được thực hiện với:*
```
[root@localhost ~]# useradd usertest
[root@localhost ~]# cat /etc/passwd | grep usertest
usertest:x:1001:1001::/home/usertest:/bin/bash
```
*Theo mặc định, thư mục chính và một số tệp lệnh cơ bản của `usertest` là `/home/usertest` và đặt shell mặc định thành `/bin/bash`*
```
[root@localhost ~]# ls -la /home/usertest
total 12
drwx------. 2 usertest usertest  62 May  4 00:17 .
drwxr-xr-x. 4 root     root      38 May  4 00:17 ..
-rw-r--r--. 1 usertest usertest  18 Aug  8  2019 .bash_logout
-rw-r--r--. 1 usertest usertest 193 Aug  8  2019 .bash_profile
-rw-r--r--. 1 usertest usertest 231 Aug  8  2019 .bashrc
```
*Xóa tài khoản người dùng bằng cách gõ:*
```
[root@localhost ~]# userdel usertest
[root@localhost ~]# cat /etc/passwd | grep usertest
[root@localhost ~]# ls -la /home/usertest
total 12
drwx------. 2 1001 1001  62 May  4 00:17 .
drwxr-xr-x. 4 root root  38 May  4 00:17 ..
-rw-r--r--. 1 1001 1001  18 Aug  8  2019 .bash_logout
-rw-r--r--. 1 1001 1001 193 Aug  8  2019 .bash_profile
-rw-r--r--. 1 1001 1001 231 Aug  8  2019 .bashrc
```
*Tuy nhiên, điều này sẽ để lại thư mục `/home` nguyên vẹn. Điều này có thể hữu ích nếu nó là tạm thời hoãn hoạt động. Để xóa thư mục chính trong khi xóa tài khoản, người ta cần sử dụng tùy chọn liên quan *
```
[root@localhost ~]# useradd namdachbdmo
[root@localhost ~]# useradd namdachbdemo
[root@localhost ~]# userdel -r namdachbdemo
[root@localhost ~]# cat /etc/passwd | grep namdachbdeomo
[root@localhost ~]# ls -la /home/namdachbdemo
ls: cannot access /home/namdachbdemo: No such file or directory
```
*Lệnh `id` không có đối số cung cấp thông tin về người dùng hiện tại. Nếu được đặt tên của người khác làm đối số, id sẽ báo cáo thông tin về người dùng khác đó. Đối với người dùng root*
```
[root@localhost ~]# id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```
*Đối với người dùng user:*
```
[root@localhost ~]# id nam
uid=1002(nam) gid=1002(nam) groups=1002(nam)
```
*Sử dụng lệnh passwd để thay đổi mật khẩu cho người dùng mới:*
```
[root@localhost ~]# useradd user123
[root@localhost ~]# passwd user123
Changing password for user user123.
New password:
BAD PASSWORD: The password contains the user name in some form
Retype new password:
passwd: all authentication tokens updated successfully.
```
*Thêm một nhóm mới được thực hiện với lệnh `groupadd` và loại bỏ bằng lệnh `groupdel`*
```
[root@localhost ~]# groupadd testgroup
[root@localhost ~]# groupdel testgroup
```
*Thêm người dùng vào một nhóm đã tồn tại được thực hiện bằng lệnh `usermod`.*
```
[root@localhost ~]# groupadd newgroup
[root@localhost ~]# usermod -G newgroup nam
[root@localhost ~]# groups nam
nam : nam newgroup
[root@localhost ~]# usermod -g newgroup nam
[root@localhost ~]# groups nam
nam : newgroup
```
*Cú pháp:*
* `usermod [option] [name_group] [name_user]
  * option:
    * `-g` : Thêm user vào một group
    * `-G` : Thêm user vào nhiều group(mỗi group được phân tách group tiếp theo bằng dấu phẩy, không có sự can thiệp từ khoảng trắng)
*Tất cả các tệp này cập nhật khi cần thiết tại thư mục `/etc/group`. Các lệnh `group` có thể được sử dụng để thay đổi các nhóm thuộc nhóm như Nhóm ID hoặc tên*
```
[root@localhost ~]# groupmod newgroup -n groupnewgroup2
[root@localhost ~]# groups nam
nam : groupnewgroup2
```
## Người dùng root
*Các tài khoản **gốc** có quyền truy cập đầy đủ vào hệ thống. Các hệ điều hành khác thường gọi đây là tài khoản quản trị viên; trong linux, nó thường được gọi là tài khoản **superuser**. Bạn phải cực kỳ thận trọng khi cấp quyền truy cập root đầy đủ cho người dùng; Ví dụ trong các cuộc tấn công bên ngoài thường bao gồm các thủ thuật được sử dụng để nâng lên tài khoản root. Tuy nhiên, bạn có thể sử dụng tính năng sudo để gắn các đặc quyền hạn chế hơn cho các tài khoản người dùng chuẩn:*
1. Chỉ trên cơ sở tạm thời 
2. Chỉ cho một tập con cụ thể của lệnh 
*Khi gán đặc quyền nâng cao, bạn có thể sử dụng lệnh `su`(chuyển đổi người dùng) để khởi chạy shell mới chạy với tư cách người dùng khác(bạn phải nhập mật khẩu của người mà bạn muốn trở thành). Thông thường người dùng khác này là root hoặc shell mới cho phép sử dụng các đặc quyền nâng cao cho đến khi thoát. Nó hầu như luôn luôn là một thực hành xấu(nguy hiểm cho cả bảo mật và ổn định) để sử dụng `su` thành root. Lỗi kết quả có thể bao gồm xóa các tệp tin quan trọng khởi hệ thống và vi phạm bảo mật.*
## Tập tin khởi động
*Trong linux, chương trình shell lệnh, nói chung bash sử dụng một hoặc nhiểu tệp khởi động để cấu hình môi trường. Các tệp trong `/etc` thư mục xác định cài đặt chung cho tất cả người dùng trong khi cái tệp khởi tạo trong thư mục chính của người dùng có thể bao gồm và ghi đè chung. Các tệp khởi động có thể làm bất cứ điều gì mà người dùng muốn làm trong lệnh shell, chẳng hạn như:*
* Tùy chỉnh lời nhắc của người dùng
* Xác định các phím tắ và bí dang của dòng lệnh
* Đặt trình soạn thảo văn bản mặc định
* Đặt đường dẫn cho nơi tìm chương trình thực thi
*Khi bạn đăng nhập lần đầu vào linux, `/etc/profile` tệp sẽ được đọc và đánh giá, sau đó các lệnh sau được tìm kiếm theo thứ tự được liệt kê:*
1. `~/.bash_profile`
2. `~/.bash_login`
3. `~/'profile`
*Shell đăng nhập linux đánh giá bất kỳ tệp khởi động nào mà có xuất hiện đầu tiên và bỏ qua phần còn lại. Điều này có nghĩa là nếu nó tìm thấy `~/.bash_profile`, nó bỏ qua phần còn lại. Các bạn phân phối khác nhau có thể sử dụng các tệp khởi động khác nhau. Tuy nhiên, mỗi khi bạn tạo shell mới hoặc cửa sổ terminal,v.v, bạn không thể đăng nhập toàn hệ thống; chỉ có tệp tin `~/.bashrc` được đọc và đánh giá. Mặc dù tệp này không được đọc và đánh giá cùng login shell. Trong các bản phân phối Ubuntu và CentOS, người dùng phải thực hiện các thay đổi phug hợp trong tệp `~/.bash_profile` để bảo gồm tệp `~/.bashrc`. Các tệp `~/.bash_profilee` sẽ có một số dòng thêm, do đó sẽ thu nhập thông số tùy yêu cầu từ `~/.bashrc`.*
## Biến môi trường
*Các biến môi trường được đặt tên đơn giản là các đại lượng có giá trị cụ thể và được hiểu bởi lệnh shell, chẳng hạn như **bash**. Một số trong số này được hệ thống cài đặt sẵn và một số khác được dùng đặt ở dòng lệnh hoặc khi khởi động và các tệp lệnh khác. Một biến môi trường thực sự không nhiều hơn một chuỗi ký tự thông tin được sử dụng bởi một hoặc nhiều ứng dụng. Có một số các để xem các giá trị của các biến môi trường hiện được đặt. Tất cả các lệnh `set`, `env`và `export` hiện thị các biến môi trường.*

*Theo mặc định, các biến được tạo trong một tệp lệnh chỉ có sẵn cho chương trình bao hiện tại. Tất cả các tiến trình con(shell phụ) sẽ không có quyền truy cập vào các giá trị đã được đặt hoặc sửa đổi. Cho phép các tiến trình con xem các giá trị, yêu cầu sử dụng lệnh export.*
|task|command|
|-|-|
|hiển thị giá trị của một biến cụ thể|`echo $SHELL`|
|Xuất một biến mới|export VAR=value|
|Thêm một biến vĩnh viễn|Add the line export VAR=value to ~/.bashrc
**HOME** *là một biến môi trường đại diện cho HOME hoặc đăng nhập thư mục của người dùng. Các lệnh `cd` có đối số sẽ thay đổi thư mục làm việc hiện tại với giá trị của HOME. Lưu ý ký tự dấu ngã(~). Thường được sử dụng làm chữ viết cho $HOME.*

*Biến môi trường **PATH**, là một danh sách có thứ tự thư mục được quét khi lệnh được đưa ra để tìm các chương trình hay kịch bản thích hợp để chạy. Mỗi thư mục trong đường dẫn được phân tách bằng dấu hai chấm(:). Tên thư mục trống cho biết thư mục hiện tại tại bất kỳ thời điểm nào*

*Các biến môi trường **PS** được sử dụng để tùy chỉnh chuỗi dấu nhắc của bạn trong cửa sở terminal của bạn để hiện thị các thông tin mà bạn muốn. **PS1** là biến dấu nhắc chính điều khiển dấu nhắc dòng lệnh của bạn trông như thế nào. Các ký tự đặc biệt sau đây có thể bao gồm trong **PS1**:*
|Ký tự|Sử dụng|
|-|-|
|\u|Tên tài khoản|
|\h|Tên máy chủ|
|\W|Thư mục làm việc hiện tại|
|\!|Số lịch sử dòng lênh|
|\d|Ngày|

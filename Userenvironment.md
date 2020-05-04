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

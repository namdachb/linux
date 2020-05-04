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
*Tuy nhiên, điều này sẽ để lại thư mục `/home` nguyên vẹn. Điều này có thể hữu ích nếu nó là tạm thời hoãn hoạt động. Để xóa thư mục chính trong khi xóa tài khoản, người ta cần sử dụng tùy chọn liên quan 
```
[root@localhost ~]# useradd namdachbdmo
[root@localhost ~]# useradd namdachbdemo
[root@localhost ~]# userdel -r namdachbdemo
[root@localhost ~]# cat /etc/passwd | grep namdachbdeomo
[root@localhost ~]# ls -la /home/namdachbdemo
ls: cannot access /home/namdachbdemo: No such file or directory
```

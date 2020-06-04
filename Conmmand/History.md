# Tìm hiểu và cách sử dụng lệnh history
## 1. Giới thiệu lệnh history
Lệnh `history` có sẵn trong Bash có thể được dùng để kiểm tra lịch sử lệnh hoặc để có được thông tin về lệnh được thực thi bởi người dùng. Lệnh `history` cho phép chúng ta nhanh chóng xem những gì đã được thực hiện trước đây trên một hệ thống, cho phép người dùng chịu trách nhiệm cho hành động của họ. Nó rất hữu ích khi bạn chạy một lệnh nào đó trước đó và quên lệnh

Theo mặc định ngày và thời gian sẽ không được nhìn thấy trong khi thực hiện lệnh `history`. Tuy nhiên, bash shell cung cấp cho chúng ta tiện ích **CLI** để chỉnh sửa lệnh `history` của người dùng 

## 2. Ứng dụng lệnh history
### Chạy lệnh `history` và nó sẽ in ra lịch sử bash của người dùng hiện tại ra màn hình
 * Cột 1 dùng để hiển thị thứ tự của lệnh
 * Cột 2 hiện thị câu lệnh đã được thực thi

```
[root@client ~]# history
    1  timedatectl set-timezone Asia/Ho_Chi_Minh
    2  timedatectl
    3  firewall-cmd --add-service=ntp --permanent
    4  firewall-cmd --reload
    5  sudo setenforce 0
    6  sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
    7  sed -i 's/SELINUX=permissive/SELINUX=disabled/g' /etc/sysconfig/selinux
    8  yum install -y chrony
    9  systemctl enable --now chronyd
   10  systemctl status chronyd
```
 
### Kiểm tra câu lệnh bằng cách xem lịch sử bằng file
```
[namdachb@client ~]$ cat .bash_history
history
timedatectl
ls -alh
su -
```

### Vị trí tệp lịch sử ~/.bash_history
Theo mặc định, lịch sử bash được ghi vào `~/.bash_history`, điều này được đặt trong biến `$HISTFILE`, để kiểm tra:

 * Vị trí tệp lịch sử của User root 
  
  ```
  [root@client ~]# echo $HISTFILE
/root/.bash_history
  ```

 *  Vị trí tệp lịch sử của User thường

  ```
  [namdachb@client ~]$ echo $HISTFILE
/home/namdachb/.bash_history
  ```

Mặc định file sẽ nằm tại `/user/.bash_history` và chúng ta cũng có thể tùy chỉnh như sau

`export HISTFILE=~/.custom_file`

 * `~/.custom_file` là địa chỉ và tên file mới muốn tùy chỉnh cho file bash_history mới

### Tìm kiếm câu lệnh đã sử dụng trong quá khứ

`history | grep [conmmand]`

Ví dụ : Muốn tìm kiếm câu lệnh `grep` trong quá khứ đã sử dụng :

```
[root@client ~]# history | grep grep
   11  cat /etc/chrony.conf | egrep -v '^$|^#'
   14  cat /etc/chrony.conf | egrep -v '^$|^#'
  206  man grep
  207  grep phoenixNAP sample
```

### Lặp lại câu lệnh gần nhất

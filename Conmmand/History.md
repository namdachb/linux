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
Lệnh gần nhất có thể thực thi đơn gian bằng cách nhập `!!`

```
[root@client ~]# ls
ntnam.txt  song.txt  votinh.txt
[root@client ~]# !!
ls
ntnam.txt  song.txt  votinh.txt
```

Ngoài ra , chúng ta cũng có thể nhấn mũi tên đi lên để hiển thị lệnh cuối cùng sau đó nhấn Enter để thực hiện nó

### Lặp lại lệnh cụ thể 
Conmmand: `![n]`
  * `n` là số dòng của câu lệnh mà chúng ta muốn lặp lệnh

```
   74  ls
   75  pwd
   76  cd
```

```
[root@client ~]# !75
pwd
/root
```
  Như ở ví dụ này, mình lặp lệnh ở số 75

### Ghi vào tệp tin lịch sử
Thông thường tệp lịch sử được ghi vào khi đăng xuất, do đó, nếu bạn có phiên SSH đã hết thời gian, bạn sẽ không có lịch sử của mình từ phiên đó khi bạn đăng nhập lại. Bạn có thể buộc lịch sử hiện tại ghi vào tệp tin lịch sử người dùng `~/. bash_history` với tùy chọn `-w`

`history -w`

### Xóa tệp tin lịch sử

`history -c`

Dòng lệnh sẽ xóa toàn bộ lịch sử bộ nhớ, những thay đổi sẽ được ghi khi người dùng đăng xuất tuy nhiên bạn có thể lưu các thay đổi vào tệp .bash_history khi chạy lệnh history -w

Xóa toàn bộ tệp lịch sử có thể là quá mức cần thiết, thay vào đó bạn có thể xóa một số dòng cụ thể :

 `history -d [n_cmd]`

### Thay đổi format của output history

`HISTTIMEFORMAT="%d/%m/%y %T "`

Option:
 * `%d` : ngày
 * `%m` : tháng
 * `%y` : năm
 * `%T` : thời gian
 * `/` : có thể thay đổi bằng ký tự khác, hoặc dấu cách tùy thuộc vào hiển thị của người sử dụng

### Bỏ qua các lệnh cụ thể

Bạn có thể chỉ định một hoặc nhiều lệnh không bao giờ được ghi vào tệp lịch sử với biến $HISTIGNORE

`export HISTIGNORE="cd"`

Với câu lệnh trên lệnh history sẽ không lưu lại lịch sự với câu lệnh `cd`

![Imgur](https://i.imgur.com/NYVExUX.png)
 Với nhiều câu lệnh `cd` lặp lại nhiều lần nhưng với biến `$HISTIGNORE` thì lệnh sẽ bỏ qua không lưu

### Tăng giảm kích thước lưu trữ history

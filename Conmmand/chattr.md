## Lệnh chattr để bảo vệ sự toàn vẹn của file
Đối với Linux, thông thường chúng ta sử dụng phân quyền bằng lệnh chmod để bảo vệ tập tin. Nhưng bây giờ chúng ta sẽ tìm hiểu một cách khác. Đó là sử dụng lệnh `chattr`

### Khái niệm
`chattr` là viết tắt của Change Attribute. Đây là câu lệnh cho phép bạn thay đổi thuộc tính của file giúp bảo vệ file khỏi bị xóa hoặc ghi đè nội dung, dù cho bạn có đang là user root đi nữa

Cú pháp lệnh:

 `chattr [operator] [flags] [filename]`

Các operator:
 
 * `+`: thêm thuộc tính cho file
 * `-`: gỡ bỏ thuộc tính khỏi file
 * `=`: giữ nguyên thuộc tính của file

Các flag (hay các thuộc tính của file):

Có nhiều flag, ở đây chúng ta sẽ giới thiệu các flag thường dùng là:
  * `i` : flag này khiến file không thể rename, không thể tạo symlink, không thể thực thi, không thể write. Chỉ có user root mới set và unset được thuộc tính này
  * `a` : flag này khiến file không thể rename, không thể tạo symlink, không thể thực thi, chỉ có thể nối thêm nội dung vào file. Chỉ có user root mới set và unset được thuộc tính này
  * `d` : file có thuộc tính này sẽ không được backup khi tiến trình dump chạy
  * `s` : nếu một file có thuộc tính này bị sửa, thay đổi sẽ được cập nhật đồng bộ trên ổ cứng
  * `A` : khi file có thuộc tính được truy cập, giá trị `atime` của file sẽ không thay đổi

**Lưu ý**: Tất cả cách lệnh dưới đây đều chạy dưới quyền user root

Trong bài này, mình đã tạo trước file `demo.txt` trong thư mục `test`. Nội dung file như sau :

```
[root@localhost test]# cat demo.txt
demo.vn
```

### 1. Cách thêm thuộc tính trên file để bảo vệ file khỏi bị xóa
Chúng ta thêm thuộc tính `i` (immutable) cho file

```
[root@localhost test]# chattr +i demo.txt
```

Xem lệnh trên đã có hiệu lực chưa bằng cách dùng lệnh `lsattr` :

```
[root@localhost test]# lsattr
----i----------- ./demo.txt
```

Bây giờ ta thử xóa file trên :

```
[root@localhost test]# rm -rf demo.txt
rm: cannot remove ‘demo.txt’: Operation not permitted
[root@localhost test]# mv demo.txt namdac.txt
mv: cannot move ‘demo.txt’ to ‘namdac.txt’: Operation not permitted
[root@localhost test]# chmod 755 demo.txt
chmod: changing permissions of ‘demo.txt’: Operation not permitted
```

Có thể thấy rằng chúng ta không thể xóa hoặc thay đổi file trên được.

### 2. Cách để unset thuộc tính đã thêm cho file
Ta sử dụng operator `-`

Ví dụ chúng ta sẽ unset thuộc tính `i` trên file `demo.txt`

```
[root@localhost test]# chattr -i demo.txt
```

Sau khi bỏ thuộc tính `i` khỏi file, ta có thể thay đổi file một cách bình thường:

```
[root@localhost test]# mv demo.txt file.txt
[root@localhost test]# lsattr
---------------- ./file.txt
[root@localhost test]# ll
total 4
-rw-r--r--. 1 root root 8 Jun  4 21:46 file.txt
```

### 3. Chỉ cho phép nối thêm nội dung vào file
Chúng ta thêm thuộc tính `a` (append) cho file

```
[root@localhost test]# chattr +a file.txt
[root@localhost test]# lsattr
-----a---------- ./file.txt
```

Thử sửa nội dung file :

```
[root@localhost test]# echo "gicungduoc" > file.txt
-bash: file.txt: Operation not permitted
```
Có thể thấy là không sửa được nội dung

Nhưng chúng ta có thể nối nội dung file:

```
[root@localhost test]# echo "gicungduoc" >> file.txt
[root@localhost test]# cat file.txt
demo.vn
gicungduoc
[root@localhost test]#
```

Ta có thể gỡ bỏ thuộc tính này với lệnh
```
[root@localhost test]# chattr -a file.txt
```

### 4. Cách dùng `chattr` để bảo vệ thư mục 
Để bảo vệ cả thư mục và các file bên trong thư mục đó, ta dùng flag `-R` (recursively) và `+i` với đường dẫn của thư mục đó

 ```
 [root@localhost ~]# chattr -R +i test/
 ```

Thử xóa thư mục này đi
 
 ```
 [root@localhost ~]# rm -rf test/
rm: cannot remove ‘test/file.txt’: Permission denied
 ```
Có thể thấy ta không thể xóa thư mục `test` và các file bên trong nó

Để unset quyền trên, ta sử dụng flag `-R` và `-i` với đường dẫn của thư mục đó

 ```
 [root@localhost ~]# chattr -R -i test/
 ```

Sau khi chạy lệnh này, ta có thể sửa, xóa thư mục và file bên trong như bình thường


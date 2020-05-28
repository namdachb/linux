## Lệnh grep là gì?
**Grep** là (**G**lobal **R**egular **E**xpression **P**rint) là một công vụ dòng lệnh Linux/Unix được sử dung để tìm kiểm một chuỗi ký tự trong một tệp được chỉ định. Khi tìm thấy kết quả khớp, nó sẽ in dòng kết quả. Lệnh `grep` rất tiện lợi khi tìm kiếm thông qua các tệp nhật ký lớn

### Cách sử dụng
**Cú pháp** : `grep [tùy_chọn] <file>`

Sau đây là những ví dụ để chúng ta hiểu dễ hơn về lênh `grep`

Mình có tạo 2 file để ví dụ là `song.txt` và `yeu.txt`

```
[root@localhost ~]# cat song.txt
Du doi va em diu
On ao va lang le
Song khong hieu noi minh
Song tim ra tan be
[root@localhost ~]# cat yeu.txt
yeu la chet trong long mot it
vi may khi yeu ma chac chan duoc yeu
cho rat nhieu nhung nhan chang bao nhieu
nguoi ta phu hoac tho o chang biet
```

### 1. Tìm hiểu một chuỗi trong một file
Ví dụ : Tìm từ **em diu** trên file `song.txt`
  ```
  [root@localhost ~]# grep "em diu" song.txt
Du doi va em diu
  ```
Kết quả sẽ hiện thị dòng chứa từ "em diu"
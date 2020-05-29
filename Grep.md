## Lệnh grep là gì?
**Grep** là (**G**lobal **R**egular **E**xpression **P**rint) là một công vụ dòng lệnh Linux/Unix được sử dung để tìm kiểm một chuỗi ký tự trong một tệp được chỉ định. Khi tìm thấy kết quả khớp, nó sẽ in dòng kết quả. Lệnh `grep` rất tiện lợi khi tìm kiếm thông qua các tệp nhật ký lớn

### Cách sử dụng
**Cú pháp** : `grep [tùy_chọn] <file>`

Sau đây là những ví dụ để chúng ta hiểu dễ hơn về lênh `grep`

Mình có tạo 2 file để ví dụ là `song.txt` và `votinh.txt`

```
[root@localhost ~]# cat song.txt
Du doi va em diu
On ao va lang le
Song khong hieu noi minh
Song tim ra tan be
[root@localhost ~]# cat votinh.txt
vo tinh anh gap em
roi vo tinh thuong nho
doi vo tinh nghiet nga
nen chung minh yeu nhau
```

### 1. Tìm hiểu một chuỗi trong một file
Ví dụ : Tìm từ **em diu** trên file `song.txt`

![Imgur](https://i.imgur.com/CfCrA6D.png)
Kết quả sẽ hiện thị dòng chứa từ "em diu"

### 2. Tìm kiếm chuỗi trên nhiều file
VD: tìm từ "em" trên 2 file `song.txt` và `votinh.txt`

![Imgur](https://i.imgur.com/GiPZWig.png)
Kết quả sẽ hiển thị những dòng chứa từ "gio" trên cả hai file

### 3. Tìm kiếm không phân biệt hoa thường, sử dụng tùy chọn `-i`
VD : trên 2 file text có cả chữ viết hoa và viết thường. Đề tìm kiếm mà không phân biệt hoa thường ta có thể sử dụng tùy chọn `-i`

![Imgur](https://i.imgur.com/vrBSclk.png)
So sánh giữa sử dụng grep bình thường và tùy chọn `-i`

Ở đây, mình tìm từ "du" là chữ thường thì sẽ không ra kết quả, nhưng nếu sử dụng tùy chọn `-i` , thì kết quả sẽ xuất hiện với từ "Du"

### 4. Tìm kiếm ngược, sử dụng tùy chọn  `-v`
VD : để tìm tất cả những dòng không chứa từ
```
[root@localhost ~]# grep -v 'doi' song.txt
On ao va lang le
Song khong hieu noi minh
Song tim ra tan be
```
  Kết quả sẽ hiển thị những dòng  không chứa từ "doi"

### 5. Hiển thị số dòng, số lượng, giới hạn số dòng đầu ra 
 * `-n` : hiển thị số thứ tự của dòng và dòng chứa từ cần tìm
 * `c` : đếm số dòng khớp với kí tự cần tìm
 * `-m[chỉ số cần giới hạn]` : giới hạn số lượng dòng khớp

### 6. Tìm kiếm nhiều chuỗi
Có 3 cú pháp tìm tương đương nhau:
 * Cách 1 : `grep -e "word1" -e "word2" <file>`
 * Cách 2 : `grep "word1\|word2" <file>` `\` để phân biệt word1 với word2
 * Cách 3 : `egrep "word1|word2" <file>`

![Imgur](https://i.imgur.com/wTKPqAN.png)

### 7. Tìm kiếm tên tệp
 * `-l` : để có được tập tin phù hợp với tìm kiếm
 * `-L` : để có được các tập tin không phù hợp với tìm kiếm
 * `-h` : hiển thị không với tên file (khi chỉ định nhiều file)
 * `-H` : hiển thị cùng với tên file

So sánh giữa 2 tùy chọn `-l` và `-L`
```
[root@localhost ~]# grep -l 'em' *
song.txt
votinh.txt
[root@localhost ~]# grep -L 'em' *
```
 * Tùy chọn `-l` sẽ ra kết quả là những tên file có chứa từ **em**
 * Tùy chọn `-L` sẽ ra kết quả là những tên file không chứa từ **em**

So sánh 2 tùy chọn `-h` và `-H`

![Imgur](https://i.imgur.com/3PC74q8.png)
 * Tùy chọn `-h` sẽ cho kết quả những dòng chứa từ **vo** mà không kèm với tên file
 * Còn với tùy chọn `-H` kết quả sẽ bao gồm tên file và dòng chứa từ **vo**
### 8 Hiển thị thêm dòng trước, sau ,xung quanh dòng chứa kết quả cần tìm

`grep -<A,B hoặc C> <n> "chuoi" <file>`

Trong đó :
 * `A` : hiển thị dòng sau dòng khớp với kí tự cần tìm
 * `B` : hiển thị dòng trước dòng khớp với kí tự cần tìm
 * `C` : hiển thị dòng xung quanh dòng khớp với kí tự cần tìm
 * `n` : là số tự nhiên chỉ định xem hiển thị trước, sau hay xung quanh bao nhiêu dòng

VD : 
```
[root@localhost ~]# cat song.txt
Du doi va em diu
On ao va lang le
Song khong hieu noi minh
Song tim ra tan be
```

![Imgur](https://i.imgur.com/djLHEe5.png)
Ở đây với `n=1`, nếu muốn các bạn có thể tìm rộng hơn với `n` là số lớn hơn

### 9. Tìm chính xác với `-w`
```
[root@localhost ~]# cat ntnam.txt
ntnam.txt
ntnam98@gmai.com
ntnam = Nguyen The Nam
123456789
namdachb
```

![Imgur](https://i.imgur.com/1avolgn.png)

 * Khi bạn tìm kiếm bình thường với `grep`, kết quả sẽ hiển thị tất cả những dòng có chứa từ **ntnam** kể cả **ntnam98**
 * Với tùy chọn `-w`, kết quả sẽ tìm chính xác chỉ những dòng chứa từ **ntnam**

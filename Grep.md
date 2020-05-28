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
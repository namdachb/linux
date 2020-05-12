# FileGlobbing (tập tin toàn cầu)
*Tập tin toàn cầu là hoạt động nhận ra các mẫu này và thực hiện công việc mở rộng đường dẫn tệp*
### 1. * asterisk (dấu hoa thị)
*Dấu hoa thị * được hiểu bởi shell là dấu hiệu để tạo tên tệp, khớp dấu hoa thị với bất kỳ tổ hợp ký tự nào (thậm chí không có ký tự nào). Khi không có đường dẫn nào, shell sẽ sử dụng tên tệp trong thư mục hiện tại*

### 2. ? question mark
*Tương tự như dấu hoa thị, dấu hỏi? được trình diễn bởi shell là một dấu hiệu để tạo tên tệp, khớp dấu chấm hỏi với chính xác một ký tự*
```
[root@localhost ~]# ls File?
File4:

FileA:
[root@localhost ~]# ls Fil??
File4:

FileA:
[root@localhost ~]# ls File??
File56:

Fileab:

FileAB:
```
### 3. [] square brackets
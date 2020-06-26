## Thiết lập chứng nhận SSL miễn phí từ Let's Encrypt cho các website
Có 2 cách tạo SSL:
 * Nhờ một tổ chức CA(Certification Authority) cấp, là tổ chức có độ tin cậy cao, được quyền cấp và chứng nhận SSL. Sẽ mất phí
 * Self-signed SSL: là server tự cấp, tự ký, tự xác thực (không an toàn và tin tưởng bằng nhờ bên thứ 3). Với cách này bạn sẽ không mất phí

Bài viết này sẽ nhận chứng chỉ SSL miễn phí từ Let's Encrypt và cài đặt SSL trên môi trường Nginx và CentOS 7. Tuy nhiên, chứng chỉ SSL được tạo theo cách này chỉ có tác dụng trong 90 ngày. Sau 90 ngày bạn sẽ cần update lại chứng chỉ

### 1.Chuẩn bị
 * 1 Server chạy hệ điều hành CentOS 7, đã cài đặt LEMP (cách cài Lemp tại [đây](https://github.com/namdachb/linux/blob/master/Wordpress-LEMP/LEMP-centos.md))
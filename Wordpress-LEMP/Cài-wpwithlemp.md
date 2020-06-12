# Cài đặt Wordpress với LEMP (CentOS 7)
Trước tiến chúng ta tìm hiểu LEMP và Wordpress là gì?

**LEMP** là viết tắt của các khái niệm :
 * **L** : Linux (ở đây chúng ta sử dụng CentOS 7x) là hệ điều hành mã nguồn mở được sử dụng chủ yếu trên các server để phục vụ nhiều mục đích khác nhau
 * **E** : Nginx là phần mềm máy chủ web cũng tương tự Apache nhưng có sức chịu tải lớn hơn rất nhiều so với Apache nên thường được sử dụng trên các ứng dụng web rất đông người truy cập và bitly.com là một trong số đó. Nginx được phát âm là “engine-x” nên được viết tắt thành chữ E trong LEMP.
 * **M** : MySQL là hệ quản trị cơ sở dữ lieuej mã nguồn mở rất phổ biến do tính năng bảo mật, dễ sử dụng và miễn phí. MySQL thường được dùng để lưu trữ dữ liệu cho các ứng dụng web và thường dùng chung với PHP
 * **P** : PHP là ngôn ngữ lập trình kịch bản mã nguồn, chủ yếu được dùng để phát triển các ứng dụng web trên phía máy chủ

**Wordpress**  là một ứng dụng viết mã nguồn mở và miễn phí và một CMS động (Hệ thống Quản lý Nội dung) được phát triển sử dụng MySQL và PHP

Trong bài này, mình sẽ giới thiệu cho các bạn cách cài đặt WordPress bằng cách sử dụng LEMP (Linux, Nginx, MariaDB, PHP) trên RHEL/ CentOS 7

## Cài đặt và cấu hình LEMP
Trước khi tiến hành cài đặt WordPress, chúng ta cần cài LEMP trên máy của chúng ta. Và cách cài LEMP tại [đây](https://github.com/namdachb/linux/blob/master/Wordpress-LEMP/LEMP-centos.md)
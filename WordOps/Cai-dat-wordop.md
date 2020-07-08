# Cài đặt WordOps trên Ubuntu 18.04

## Tài nguyên cần thiết
**Tối thiểu**

WordOps có thể cài đặt trên các thiết bị cấp thấp như Raspberry PI với yêu cầu tối thiểu là:
 * 100 MB dung lượng trống
 * RAM 512MB

Network : Có ít nhất 1 interface với IP public
 * ens3 : 103.101.161.199
 * ens4 : 10.10.35.114

## Cài đặt
### 1. Cấu hình trỏ domain cho IP public
Đầu tiên, khi có IP public chúng ta cần phải trỏ IP cho domain của mình

Mình sẽ trỏ IP 103.101.161.199 cho domain wo.namdac.xyz .Sau đó trỏ IP cho doamin, bằng câu lênh `nslookup <domain>` để kiểm tra

```
thuctap@lab-u18-srv1:~$ nslookup wo.namdac.xyz
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
Name:   wo.namdac.xyz
Address: 103.101.161.199
```

### 2. Cài đặt trang web

### 2.1 Tải về các phụ thuộc
Để làm việc với WordOps rất đơn giản vì đã được cung cấp 1 tập lệnh cài đặt để cài đặt các phụ thuộc cần thiết, chạy câu lệnh sau để cài đặt

`wget -qO wo wops.cc && sudo bash wo`

![Imgur](https://i.imgur.com/FexqIO1.png)

  1. Nhập vào tên của bạn
  2. Nhập địa chỉ email của bạn


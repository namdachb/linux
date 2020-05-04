# Process (quá trình)
## 1) Khái niệm
* Một **process**(tiến trình), hiểu theo cách đơn giản, là một ví dụ của chương trình đang chạy. Bất cứ khi nào bạn thông báo một lệnh trong linux, nó tạo hoặc bắt đầu một **process** mới.
* Mỗi **process** có 1 **PID**(**process ID**) đại diện. **PID** gồm tối đa `5` chữ số và là duy nhất tại một thời điểm. **PID** của **process** A có thể được tận dụng cho **process** B nếu **process** A kết thúc.
* Có 2 loại **process**:
 * **Foregroud Process**
 * **Backgroup Process**
### 1.1) Foregroup Process
* Theo mặc định, mọi **process** mà bạn bắt đầu chạy là **foregroup process**. Nó nhận input từ bàn phím và gửi output tới màn hình.
* Trong khi một chương trình đang chạy trong **foregroupd** và cần một khoảng thời gian dài, chúng ta không thể chạy bất kỳ lệnh khác(bắt đầu **process** khác) bởi vì dòng nhắc lệnh không có sẵn tới khi chương trình đang chạy kết thúc **process** và thoát ra.
```
[root@localhost ~]# ping -c 4 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=128 time=32.7 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=128 time=46.1 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=128 time=76.9 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=128 time=33.1 ms

--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3017ms
rtt min/avg/max/mdev = 32.775/47.243/76.930/17.961 ms
[root@localhost ~]#
```
### 1.2) Background Precess
* **Background process** chạy mà không được kết nối với bàn phím của bạn. Nếu **Backround process** yêu cầu bất cứ đầu vào từ bàn phím, nó đợi.
* Lợi thế của chạy một chương trinh tỏng **background** là có thể chạy các lệnh khác : không phải đợi tới khi nó kết thúc để bắt đầu một **process** mới !
* Để bắt đầu một **background process**, thêm dấu "`&`" tại cuối lệnh.

     `# ping -c 10 8.8.8.8`
```
[root@localhost ~]# PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=128 time=33.0 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=128 time=32.9 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=128 time=37.2 ms
ls
anaconda-ks.cfg  file  namdachb  namdachb.zip  name.zip  nam.txt  newfile  newfile.zip
[root@localhost ~]# 64 bytes from 8.8.8.8: icmp_seq=4 ttl=128 time=37.7 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=128 time=32.3 ms
64 bytes from 8.8.8.8: icmp_seq=6 ttl=128 time=32.8 ms
```
### 1.3) Job ID với Process ID
* **Backgroup process** và **foregroud** thường được thao tác thông qua **job ID**. Số này khác với **Process ID** và được sử dụng bởi vì nó ngắn hơn.
* Ngoài ra, một công việc có thể bao gồm nhiều **process** đang chạy trong seri hoặc tại cùng một thời gian, song song, vì thế sử dụng **job ID** là dễ dàng hơn theo dõi các tiến trình riêng lẻ.
### 1.4) Parent Process (PPID) và Child Process (PID)
* Mỗi một tiến trình Unix có hai ID được gán cho nó: Process ID(pid) và Parent Process ID (ppid).
* Mỗi tiến trình trong hệ thống có một Parent Process (gốc).

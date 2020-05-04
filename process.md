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
### 1.5) Zombie Process và Orphan Process
* Thông thường, khi một **child process** bị `kill`, **parent process** được thông báo qua ký hiệu `SIGCHLD`. Sau đó, **parent process** có thể thực hiện một vài công việc khác hoặc bắt đầu lại **child process** nếu cần thiết.
* Tuy nhiên, đôi khi **parent process** bị `kill` trước khi **child process** của nó bị `kill`. Trong trường hợp này, **parent process** của tất cả các **process**, **"init process"**, trở thành **PPID** mới. Đôi khi những **process** này được là **Orphan Process**.
* Khi một **process** bị `kill`, danh sách liệt kê `ps` có thể vẫn chỉ **process** với trạng thái `z`. Đây là trạng thái `zombie`, hoặc **process** không tồn tại. **Process** này bị `kill` và không được sử dụng. Những **process** này khác với **orphan process**. Nó là những **process** mà đã chạy hoàn thành nhưng vẫn có một cổng vào trong bảng **process**.
 ### 1.6) Deamon Process
 * **Deamon** là các **background process** liên quan tới hệ thống mà thường chạy với quyền hạn truy cập của `root` và các dịch vụ yêu cầu từ **process** khác.
 * Một **deamon** không có terminal điều khiển. Nó không thể mở `/dev/tty`. Nếu thực hiện lệnh `ps-aux` và quan sát vào trường `tty` , tất cả **deamon** sẽ có một dấu `?` cho `tty`.
 ```
 [root@localhost ~]# ps -aux
USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root          1  0.0  0.6 127952  6564 ?        Ss   May03   0:01 /usr/lib/systemd/systemd --switched-root --system --deserialize 22
root          2  0.0  0.0      0     0 ?        S    May03   0:00 [kthreadd]
root          4  0.0  0.0      0     0 ?        S<   May03   0:00 [kworker/0:0H]
root          6  0.0  0.0      0     0 ?        S    May03   0:00 [ksoftirqd/0]
root          7  0.0  0.0      0     0 ?        S    May03   0:00 [migration/0]
root          8  0.0  0.0      0     0 ?        S    May03   0:00 [rcu_bh]
root          9  0.0  0.0      0     0 ?        R    May03   0:01 [rcu_sched]
root         10  0.0  0.0      0     0 ?        S<   May03   0:00 [lru-add-drain]
root         11  0.0  0.0      0     0 ?        S    May03   0:00 [watchdog/0]
```
* **Deamon** chỉ là một **process** mà chạy trong backgroup, thường đợi cho cái gì đó xảy ra mà nó có khả năng làm việc với, giống như máy in deamon đang đợi các lệnh in.
## 2) Các lệnh về Process
### 2.1) `ps` - process status
* Dùng để quan sát các **process** đang chạy.
* Cấu trúc lệnh:
   `# ps [options]`
  * Optinos:
    * `-f` : hiển thị đầy đủ thông tin về các **process**
    * `-e` : hiển thị đầy đủ các **process** (bao gồm cả **system process**)
    * `-aux` = `-ef` : hiển thị đầy đủ thông tin về tất cả các **process**
    * `-u` : hiển thị các **process** liên quan đến user hiện hành 
    * `-p PID` : hiển thị thông tin **process** cụ thể.
* Ý nghĩa output lệnh `ps -f` :
```
[root@localhost ~]# ps -f
UID         PID   PPID  C STIME TTY          TIME CMD
root       2093   2080  0 01:03 pts/2    00:00:00 -bash
root       2596   2093  0 04:27 pts/2    00:00:00 ps -f
```
|Name|Meanings|
|-|-|
|UID|UID mà process này thuộc về (người chạy nó)|
|PID|Process ID|
|PPID|Process ID gốc (ID của process mà bắt đầu nó)|
|C|CPU sử dụng của process|
|STIME|Thời gian bắt đầu process|
|TTY|Kiểu terminal liên kết với process|
|TIME|Thời gian CPU bị sử dụng bởi process|
|CMD|Lệnh mà bắt đầu process này|
### 2.2) `top`
* Ý nghĩa tương tự lệnh `ps`.
* Nội dung hiển thị tương tự lệnh "`ps -aux`"
* Cấu trúc lệnh:
  `# top [options]
   * Options:
     * `-n number` : chỉ định số dòng hiển thị
* Gõ `q` để thoát khỏi quá trình `top`.
* Output :
```
[root@localhost ~]# top
top - 04:51:38 up  7:40,  2 users,  load average: 0.00, 0.01, 0.05
Tasks: 104 total,   2 running, 102 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.3 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :   995748 total,   700180 free,   174972 used,   120596 buff/cache
KiB Swap:  3145720 total,  3145720 free,        0 used.   685152 avail Mem

   PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
  2651 root      20   0  159216   6248   4748 S  0.3  0.6   0:00.35 sshd
  2679 root      20   0  162016   2224   1544 R  0.3  0.2   0:00.09 top
     1 root      20   0  127952   6564   4120 S  0.0  0.7   0:02.05 systemd
     2 root      20   0       0      0 
```
|Name|Meanings|
|-|-|
|top|Cho biết thời gian `uptime`(từ lúc khởi động) cũng như số người dùng thực tế đang hoạt động|
|Tasks|Thống kế về số lượng tiến trình, bao gồm tổng số tiến trình (total), số đang hoạt động(running), số đang ngủ/chờ (sleeing), số đã dừng (stopped) và số không thể dừng hẳn (zombie)|
|%Cpu(s)|Cho biết thông tin về CPU|
|KiB Mem|Cho biết thông tin về RAM|
|KiB Swap|Cho biết thông tin về Swap|
|PID|Process ID|
|User|Người dùng thực thi|
|PR|Độ ưu tiên của process|
|NI|
|VIRT|
|RES|
|SHR|
|S|
|%CPU|Tỉ lệ sử dụng CPU của process|
|%MEM|Tỉ lệ sử dụng RAM của process|
|TIME+|Thời gian sử dụng CPU, giống như TIME nhưng phản ánh mức độ chi tiết hơn qua một phần trăm giây|
|COMMAND|Lệnh mà bắt đầu process này|
### 2.3) `kill`
* Là lệnh tắt **process** đang chạy
* Khi sử dụng lệnh `kill` với một tiến trình con thì chỉ tiến trình đó được tắt nhưng nếu sử dụng `kill` với tiến trình cha thì toàn bộ con của nó cũng được tắt theo.
* Cú pháp:
   `# kill [optins] [pid]
   * Options:
    * `-9` : kill toàn bộ các **process** liên quan
    
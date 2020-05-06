# Logical Volume Manager(LVM)
## Giới thiệu về LVM
*LVM là một công cụ để quản lý phân cùng logic được tạo và phân bổ từ ổ đĩa vật lí. Với LVM bạn có thể dẽ dàng tạo mới, thay đổi kích thước hoặc xóa bỏ phân vùng đã tạo.*

*LVM được tạo sử dngj cho các mục đích sau*
 * Tạo 1 hoặc nhiều phần vùng logic hoặc phân cung với toàn bộ đĩa cứng(hơi giống với RAID 0, nhưng tương tự như JBOD), cho phép thay đổi kích thước volume
 * Quản lý trang trại đĩa cứng lớn (Large hard Disk Farms) bằng cách cho phép thêm và thay thế đĩa mà không bị ngừng hoạt động hoặc dán đoạn dịch vụ, kết hợp với trao đổi nóng (hot swapping)
 * Trên các hệ thống nhỏ(như máy tính để bàn), thay vì phải ước tính thời gian cài đặt, phân cùng có thể cần lớn hơn đến mức nào, LVM cho phép các hệ thống tệp dễ dàng thay đổi kích thước khi cần.
 * Thực hiện sao lưu nhất quán bằng cách tạo snapshot nhanh các khối một cách hợp lý
 * Mã hóa nhiều phân vùng vật lý bằng một mật khẩu

*Cơ bản, **LVM**(Logical Volume Manager) bao gồm:*
 * **Physical volumes**:
   * Là những đĩa cứng vật lý hoặc phân vùng trên nó. (`/dev/fileserver/share, /dev/fileserver/backup,/dev/fileserver/media`)
   * Là 1 tên gọi khác của **partition** trong kỹ thuật **LVM**, nó là những thành phần cơ bản được sử dụng bởi **LVM**
   * Một **Physical Volume** không thể mở rộng ra ngoài phạm vi 1 ổ đĩa.
   * Có thể kết hợp nhiều **Physical Volume** thành một **Volume Group**
 * **Volume groups**:
   * Là một nhóm bao gồm các Physical volume. Có thể xem Volume group như 1 "ổ đĩa ảo". (`fileserver`)
   * Nhiều **Physical Volume** trên những ổ đĩa khác nhau kết hợp lại thành 1 **Volume Group**
   * **Volume Group** được dùng để tạo ra các **Logical Volume**, trong đó người dùng có thể tạo, thay đổi kích thước, gỡ bỏ và sử dụng.
 * **Logical volumes**:
   * Có thể xem như là các "phân vùng ảo" trên "ổ đĩa cứng" bạn có thể thêm vào, gỡ bỏ và thay đổi kích thước một cách nhanh chonggs. (`/dev/sda1, /dev/sdb1, /dev/sdc1, /dev/sdd1`)
## ƯU điểm và nhược điểm của LVM
**Ưu điểm**
 * Có thể gom nhiều đĩa cứng vật lý lại thành 1 đĩa ảo dung lượng lớn
 * Có thể tạo ra các vùng dung lượng lớn nhỏ tùy ý
 * Có thể thay đổi các vùng dung lượng đó dễ dàng, linh hoạt

**Nhược điểm**
 * Các bước thiết lập phức tạp, khó khăn hơn
 * Càng gắn nhiều đĩa cứng và thiết lập càng nhiều LVM thì hệ thống khởi động càng lâu
 * Khả năng mất dữ liệu khi 1 trong các đĩa cứng vật lý bị hỏng
 * Windows không thể nhận ra vùng dữ liệu của LVM. Nếu dual-boot Windows  sẽ không thể truy cập dữ liệu chứa trong LVM
## Các lệnh trong LVM
 * Physical Volume:
   * `pvcreate` : tạo **physical volume**
   * `pvdisplay` , `pvs` : xem **physical volume** đã tạo
   * `pvremove` : xóa **physical volume**
 * Volume Group :
   * `vgcreate` : tạo **volume group**
   * `vgdisplay` , `vgs` : xem **volume group** đã tạo
   * `vgremove` : xóa **volume group**
   * `vgextend` : tăng dung lượng của **volume group**
   * `vgreduce` : giảm dung lượng của **volume group**
 * Logical Volume:
   * `lvcreate` : tạo **logical volume**
   * `lvdisplay` , `lvs` : xem **logical Volume** đã tạo
   * `lvremove` : xóa **logical volume**
   * `lvextend` : tăng dung lượng **logical volume**
   * `lvreduce` : giảm dung lượng **logical volume**

## Thao tác trên LVM
*Liệt kê các phân cùng ổ cứng trong hệ thống. `fdisk -l`. (hoặc để đơn giản. Ta dùng `# ls -la /dev/sd*`)
 * Có 3 ổ cứng: sda, sdb, sdc
 * sda : ổ cài đặt hệ điều hành
 * sdb(2GB) và sdc(4GB): ổ để lưu trữ data
```
brw-rw----. 1 root disk 8,  0 May  5 23:41 /dev/sda
brw-rw----. 1 root disk 8,  1 May  5 23:41 /dev/sda1
brw-rw----. 1 root disk 8,  2 May  5 23:41 /dev/sda2
brw-rw----. 1 root disk 8, 16 May  5 23:41 /dev/sdb
brw-rw----. 1 root disk 8, 32 May  5 23:41 /dev/sdc
```
### Mục tiêu :
 * Sử dụng LVM để phân chia `sdb` và `sdc` thành 2 logical volume 3GB có tên là `Datas` và `Backups`
 * Mount logical volumes
 * Giảm kích thước logical volume `Backups`
 * Tăng kích thước logical volume `Data`
### Tạo Physical Volume
*Tạo 2 Physical Volume từ 2 ổ `sdb` và `sdc`: `# pvcreate /dev/sdb /dev/sdc`*
```
[root@localhost ~]# pvcreate /dev/sdb /dev/sdc
  Physical volume "/dev/sdb" successfully created.
  Physical volume "/dev/sdc" successfully created.
```
### Hiển thị thông tin về Physical Volumes
*Câu lệnh : `# pvdisplay`
```
[root@localhost ~]# pvdisplay
  --- Physical volume ---
  PV Name               /dev/sda2
  VG Name               centos
  PV Size               <19.00 GiB / not usable 3.00 MiB
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              4863
  Free PE               0
  Allocated PE          4863
  PV UUID               nZb4Zl-Polk-kvSL-QqxR-j1pJ-wVEZ-RwZii6

  "/dev/sdb" is a new physical volume of "3.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb
  VG Name
  PV Size               3.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               CmogSY-zmny-UF04-QTi1-8mJW-Y49p-aneOyX

  "/dev/sdc" is a new physical volume of "3.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdc
  VG Name
  PV Size               3.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               sasKRo-h0xa-08QT-Vo4b-90iD-WFqX-74Cy9V
```
### Tạo Volume Groups (VG)
*Lệnh kiểm tra những VG hiện có: `# vgs`*
```
[root@localhost ~]# vgs
  VG     #PV #LV #SN Attr   VSize   VFree
  VG0      2   3   0 wz--n-   6.99g    0
  centos   1   2   0 wz--n- <19.00g    0
```
*Tạo 1 VG có tên VG0 từ 2 Physical Volume vừa tạo ở trên: `# vgcreate VG0 /dev/sdb /dev/sdc`*
```
[root@localhost ~]# vgcreate VG0 /dev/sdb /dev/sdc
  Volume group "VG0" successfully created
```
*Xem lại thông tin về VG0 ta vừa tạo:*
```
[root@localhost ~]# vgdisplay
  --- Volume group ---
  VG Name               centos
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <19.00 GiB
  PE Size               4.00 MiB
  Total PE              4863
  Alloc PE / Size       4863 / <19.00 GiB
  Free  PE / Size       0 / 0
  VG UUID               iGz9Ne-eKZN-UgL8-fcWp-Xve8-HnvO-LJcISS

  --- Volume group ---
  VG Name               VG0
  System ID
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               5.99 GiB
  PE Size               4.00 MiB
  Total PE              1534
  Alloc PE / Size       0 / 0
  Free  PE / Size       1534 / 5.99 GiB
  VG UUID               PFNAZ2-pou2-GZSs-VrBD-2gM3-jmoN-GWOsA5
```
### Tạo Logical Volume
*Lệnh kiểm tra xem có những LV nào: `# lvs`*
```
[root@localhost ~]# lvs
  LV      VG     Attr       LSize    Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  Backups VG0    -wi-a----- 1016.00m
  D       VG0    -wi-a-----    3.00g
  Data    VG0    -wi-a-----    3.00g
  root    centos -wi-ao----  <17.00g
  swap    centos -wi-ao----    2.00g
```
*Ta tạo 2 Logical volume là `Data` và `Backups` như đã nói ở trên.*
```
[root@localhost ~]# lvcreate -n Data -L 3G VG0
  Logical volume "Data" created.
```
```
[root@localhost ~]# lvcreate -n Backups -l 100%FREE VG0
  Logical volume "Backups" created.
```

*Trong đó:*
 * `-n` <ten_logical_volume>
 * `-L` : Sử dụng kích thước cố định
 * `-l` : Sử dụng % của không gian còn lại trong Volume
 ```
 [root@localhost ~]# lvdisplay
  --- Logical volume ---
  LV Path                /dev/centos/swap
  LV Name                swap
  VG Name                centos
  LV UUID                0mMoiO-QaOd-nrEb-XjOS-MX9y-WndX-t70F4h
  LV Write Access        read/write
  LV Creation host, time localhost, 2020-04-08 11:23:30 -0400
  LV Status              available
  # open                 2
  LV Size                2.00 GiB
  Current LE             512
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:1

  --- Logical volume ---
  LV Path                /dev/centos/root
  LV Name                root
  VG Name                centos
  LV UUID                bCoCuP-bsH0-Khup-IHs5-kXdB-fRFV-JFmb31
  LV Write Access        read/write
  LV Creation host, time localhost, 2020-04-08 11:23:31 -0400
  LV Status              available
  # open                 1
  LV Size                <17.00 GiB
  Current LE             4351
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:0

  --- Logical volume ---
  LV Path                /dev/VG0/Data
  LV Name                Data
  VG Name                VG0
  LV UUID                0zSoK3-kXy3-Lkhg-3AeY-xPEc-vnvN-r2Py04
  LV Write Access        read/write
  LV Creation host, time localhost.localdomain, 2020-05-06 03:50:37 -0400
  LV Status              available
  # open                 0
  LV Size                3.00 GiB
  Current LE             768
  Segments               2
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:2

  --- Logical volume ---
  LV Path                /dev/VG0/Backups
  LV Name                Backups
  VG Name                VG0
  LV UUID                MmzQvV-MyTs-URnM-Ivnb-hXhN-R565-NovVP0
  LV Write Access        read/write
  LV Creation host, time localhost.localdomain, 2020-05-06 03:51:03 -0400
  LV Status              available
  # open                 0
  LV Size                2.99 GiB
  Current LE             766
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:3
```
 ### Định dạng Logical Volume
 *`Ext4`: cũng giống như `Ext3`, lưu giữ được những ưu điểm và tính tương thích ngược với phiên bản trước đó. Như vậy, chúng ta có thể dễ dàng kết hợp các phân vùng định dạng `Ext2`, `Ext3` và `Ext4` trong cùng 1 ổ đĩa trong Ubuntu để tăng hiệu suất hoạt động. Trên thực tế, `Ext4` có thể giảm bớt hiện tượng phân mảnh dữ liệu trong ổ cứng, hỗ trợ các file phân vùng và dung lượng lớn... Thích hớp với ở SSD so với `Ext3`, tốc độ hoạt động nhanh hơn so với 2 phiên bản Ext trước đó, cũng khá phù hợp để hoạt động trên server, nhưng lại không bằng `Ext3`*

 *Ta sẽ định dạng Logical Volume ở dạng `ext4`:*
  * `# mkfs.ext4 /dev/VG0/Data`
  * `# mkfs.ext4 /dev/VG0/Backups`
```
[root@localhost ~]# clear
[root@localhost ~]# mkfs.ext4 /dev/VG0/Data
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
196608 inodes, 786432 blocks
39321 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=805306368
24 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done

[root@localhost ~]# mkfs.ext4 /dev/VG0/Backups
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
65024 inodes, 260096 blocks
13004 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=266338304
8 block groups
32768 blocks per group, 32768 fragments per group
8128 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376

Allocating group tables: done
Writing inode tables: done
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done
```
### Mount logical volume 
*Ta cần Mount Logical volume để kiểm tra việc tạo thành công:*
 * ` # mkdir /Data`
 * ` # mount /dev/VG0/Data /Data/`
 * ` # mkdir /Backups`
 * ` # mount /dev/VG0/Backups /Backups/`
```
[root@localhost ~]# ls -la /
total 2097112
dr-xr-xr-x.  19 root root        267 May  6 00:51 .
dr-xr-xr-x.  19 root root        267 May  6 00:51 ..
drwxr-xr-x.   3 root root       4096 May  6 00:45 Backups
lrwxrwxrwx.   1 root root          7 Apr  8 11:23 bin -> usr/bin
dr-xr-xr-x.   5 root root       4096 Apr  8 11:30 boot
drwxr-xr-x.   3 root root       4096 May  6 00:44 Data
drwxr-xr-x.  21 root root       3360 May  6 00:58 dev
drwxr-xr-x.  74 root root       8192 May  6 01:02 etc
drwxr-xr-x.   8 root root         97 May  4 01:11 home
lrwxrwxrwx.   1 root root          7 Apr  8 11:23 lib -> usr/lib
lrwxrwxrwx.   1 root root          9 Apr  8 11:23 lib64 -> usr/lib64
drwxr-xr-x.   2 root root          6 Apr 11  2018 media
drwxr-xr-x.   2 root root          6 Apr 11  2018 mnt
drwxr-xr-x.   2 root root          6 Apr 11  2018 opt
dr-xr-xr-x. 128 root root          0 May  5 23:41 proc
dr-xr-x---.   3 root root        264 Apr 22 03:36 root
drwxr-xr-x.  24 root root        720 May  6 01:02 run
lrwxrwxrwx.   1 root root          8 Apr  8 11:23 sbin -> usr/sbin
drwxr-xr-x.   2 root root          6 Apr 11  2018 srv
-rw-------.   1 root root 1073741824 Apr 20 05:49 swapfile
dr-xr-xr-x.  13 root root          0 May  5 23:41 sys
drwxrwxrwt.   9 root root        251 May  5 23:42 tmp
drwxr-xr-x.  13 root root        155 Apr  8 11:23 usr
drwxr-xr-x.  19 root root        267 Apr  8 11:31 var
```
## Thay đổi kích thước Logical Volume
### Giảm kích thước LV
*Trước khi bắt đầu, cần sao lưu dữu liệu vì vậy sẽ được tránh sự cố xảy ra. Cần thực hiện cẩn thận 5 bước dưới đây:
 * Ngắt kết nối file system
 * Kiểm tra file system sau khi ngắt kết nối
 * Giảm file system
 * Giảm kích thước Logical Volume hơn kích thước hiện tại
 * Kiểm tra lỗi cho file system
 * Mount lại file system và kiểm tra kích thước của nó
*Ta sẽ giảm kích thước của LV `Backups` từ 2.99GB xuống còn 2GB mà không làm mất dữ liệu*
### Kiểm tra dung lượng của Logical Volume và kiểm tra file system trước khi thực hiện giảm
 `# lvdisplay VG0`
 ```
 [root@localhost ~]# lvdisplay VG0
  --- Logical volume ---
  LV Path                /dev/VG0/Data
  LV Name                Data
  VG Name                VG0
  LV UUID                0zSoK3-kXy3-Lkhg-3AeY-xPEc-vnvN-r2Py04
  LV Write Access        read/write
  LV Creation host, time localhost.localdomain, 2020-05-06 03:50:37 -0400
  LV Status              available
  # open                 0
  LV Size                3.00 GiB
  Current LE             768
  Segments               2
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:2

  --- Logical volume ---
  LV Path                /dev/VG0/Backups
  LV Name                Backups
  VG Name                VG0
  LV UUID                MmzQvV-MyTs-URnM-Ivnb-hXhN-R565-NovVP0
  LV Write Access        read/write
  LV Creation host, time localhost.localdomain, 2020-05-06 03:51:03 -0400
  LV Status              available
  # open                 0
  LV Size                2.99 GiB
  Current LE             766
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:3
```
 `# df -TH`
```
[root@localhost ~]# df -TH
Filesystem              Type      Size  Used Avail Use% Mounted on
devtmpfs                devtmpfs  498M     0  498M   0% /dev
tmpfs                   tmpfs     510M     0  510M   0% /dev/shm
tmpfs                   tmpfs     510M  8.1M  502M   2% /run
tmpfs                   tmpfs     510M     0  510M   0% /sys/fs/cgroup
/dev/mapper/centos-root xfs        19G  3.5G   15G  19% /
/dev/sda1               xfs       1.1G  143M  921M  14% /boot
tmpfs                   tmpfs     102M     0  102M   0% /run/user/0
/dev/mapper/VG0-Data    ext4      3.2G  9.5M  3.0G   1% /Data
/dev/mapper/VG0-Backups ext4      3.1G  9.5M  3.0G   1% /Backups
```
### Unmount Logical volume Backups và Kiểm tra trạng thái mount
 `# umount /Backups/`
```
[root@localhost ~]# umount /Backups/
[root@localhost ~]# df -Th
Filesystem              Type      Size  Used Avail Use% Mounted on
devtmpfs                devtmpfs  475M     0  475M   0% /dev
tmpfs                   tmpfs     487M     0  487M   0% /dev/shm
tmpfs                   tmpfs     487M  7.7M  479M   2% /run
tmpfs                   tmpfs     487M     0  487M   0% /sys/fs/cgroup
/dev/mapper/centos-root xfs        17G  3.2G   14G  19% /
/dev/sda1               xfs      1014M  136M  879M  14% /boot
tmpfs                   tmpfs      98M     0   98M   0% /run/user/0
/dev/mapper/VG0-Data    ext4      2.9G  9.0M  2.8G   1% /Data
```
-> Ta thấy không còn `/VG0-Backups` nữa
### Kiểm tra lỗi
*Ta dùng lệnh: ` #e2fsck -f /dev/VG0/Backups`
```
[root@localhost ~]# e2fsck -f /dev/VG0/Backups
e2fsck 1.42.9 (28-Dec-2013)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/VG0/Backups: 11/196224 files (0.0% non-contiguous), 31006/784384 blocks
```
-> Không thấy lỗi


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

  "/dev/sdc" is a new physical volume of "2.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdc
  VG Name
  PV Size               2.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               yne0BJ-phId-tUmT-YWJO-P6aI-PYe5-lZA4DF

  "/dev/sdb" is a new physical volume of "5.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb
  VG Name
  PV Size               5.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               5Ca55i-tNFW-T61a-rK9T-v9c2-4Va2-PQoWFK

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
  VG Size               6.99 GiB
  PE Size               4.00 MiB
  Total PE              1790
  Alloc PE / Size       0 / 0
  Free  PE / Size       1790 / 6.99 GiB
  VG UUID               6rGYwA-XS2k-eIjG-mFcA-dff7-6I3R-GlROlx
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
 ### Định dạng Logical Volume
 *`Ext4`: cũng giống như `Ext3`, lưu giữ được những ưu điểm và tính tương thích ngược với phiên bản trước đó. Như vậy, chúng ta có thể dễ dàng kết hợp các phân vùng định dạng `Ext2`, `Ext3` và `Ext4` trong cùng 1 ổ đĩa trong Ubuntu để tăng hiệu suất hoạt động. Trên thực tế, `Ext4` có thể giảm bớt hiện tượng phân mảnh dữ liệu trong ổ cứng, hỗ trợ các file phân vùng và dung lượng lớn... Thích hớp với ở SSD so với `Ext3`, tốc độ hoạt động nhanh hơn so với 2 phiên bản Ext trước đó, cũng khá phù hợp để hoạt động trên server, nhưng lại không bằng `Ext3`*

 *Ta sẽ định dạng Logical Volume ở dạng `ext4`:*
  * `# mkfs.ext4 /dev/VG0/Data`
  * `# mkfs.ext4 /dev/VG0/Backups`


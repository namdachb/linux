## Các khái niệm trong tệp tin cấu hình của NGINX
Bài viết tìm hiểu về cấu trúc và một số khái niệm trong tệp tin cấu hình mặc định của Nginx. Và một số bước cần thiết giúp chúng ta sử dụng config file của Nginx một cách hiệu quả hơn

## 1. Cấu trúc và cách sử dụng tập tin cấu hình
### 1.1 Cấu trúc
 * Tát cả file cấu hình của nginx nằm trong thư mục - `/etc/nginx`
 * File cấu hình chính của nginx là - `/etc/nginx/nginx.conf`
 * Document root directory - `/usr/share/nginx/html`

**Thư mục nội dung website**

Mặc định có thể nằm tại thư mục `/var/www/html/` hoặc `/usr/share/html/`, nhưng cũng có thể thay đổi đường dẫn thư mục 
### 1.2 Cách sử dụng config file hợp lý và hiệu quả
 * Tạo 1 file cấu hình riêng cho mỗi tên miền sẽ giúp server dễ quản lý và hiệu quả hơn
 * NGINX không có Virtual host, thay vào đó là `Server Blocks` sử dụng `server_name` và nghe các chỉ thị để liên kết với các tcp sockets. Tất cả các file server block phải có định dạng là `.conf` và được lưu trong thư mục `/etc/nginx/conf.d` hoặc `/etc/nginx/conf`
 * Nếu bạn là có 1 domain là `mydomain.com` thì bạn nên đặt tên file cấu hình là `mydomain.con.cnf`
 * Các file nhật ký Nginx (`access.log` và `error.log`) được đặt trong thư mục `/var/log/nginx/`. Nên có một tệp nhật ký `access` và `error` khác nhau cho mỗi server block
 * Bạn có thể đặt document root directory của tên miền của bạn đến bất kỳ vị trí nào bạn muốn. Một số vị trí thường được dùng cho webroot bao gồm:
    * /home/<user_name>/<site_name>
    * /var/www/<site_name>
    * /var/www/html/<site_name>
    * /opt/<site_name>
    * /usr/share/nginx/html

### 1.3 Các thao tác cần thực hiện trước và sau khi chỉnh sửa config file
 * Trước khi thay đổi cấu hình, sao lưu lại file cấu hình:

  `cp /etc/nginx/conf/nginx.conf //etc/nginx/conf/nginx.conf.backup`

 * Định kì sao lưu tập tin cấu hình nginx cp `/etc/nginx/conf/nginx.conf`

  `/etc/nginx/conf/nginx.conf.$(date "+%b_%d_%Y_%H.%M.%S")`

 * Sau khi thực hiện thay đổi cấu hình trong file cấu hình, restart lại service
  
  `systemctl restart nginx`

**Chú ý** 
 * Tất cả những dòng có dấu `**#**` phía trước là những dòng chú thích (comment) được sử dụng để giải thích những khối lệnh dùng làm gì hoặc để lại ý kiến làm thế nào chỉnh sửa giá trị
 * Ngoài ra, bạn cũng có thể thêm riêng những comment theo ý của mình. Bạn có thể cho đoạn mã đó được kích hoạt bằng cách loại bỏ các #
 * Cài đặt bắt đầu với những tên biến và sau đó một đối số hay một loại các đối số cách nhau bởi dấu cách
 * Một số thiết lập được đặt trong một cặp dấu ngoặc nhọn ({}). Các dấu ngoặc nhọn có thể được lồng vào nhau cho nhiều khối lệnh, cần nhớ là khi đã mở ngoặc nhọn thì phải nhớ đóng lại nếu không sẽ dẫn tới nginx không chạy được
 * Sử dụng tab hay space để phân cấp đoạn mã sẽ giúp dễ dàng chỉnh sửa hay tìm ra lỗi

## 2. Tìm hiểu chi tiết về config file

**Core Contexts**

Đây là nhóm đầu tiên của contexts, được nginx sử dụng để tạo ra 1 cây phân cấp và tách biệt các cấu hình giữa các block. Trong đây cũng bao gồm các cấu hình chính của nginx

**Main Context**

Cũng có thể coi là global context. Đây là context chung nhất bao gồm tất cả các directive đơn giản, block directive và các context khác

 * File bắt đầu cùng với 4 directives(chỉ thị): user, worker_processes, error_log và pid. Chúng nằm ngoài bất kỳ block hay context cụ thể nào do đó chúng nằm trong main context(bối cảnh chính)
Các **event** và **http** block là khu vực cho các directives bổ sung do đó chúng cũng nằm trong main context

 * Giải thích cụ thể:
   * **user**: Định nghĩa cho biết người dùng hệ thống Linux nào sẽ có quyền chạy các máy chủ Nginx. Có những trường hợp sử dụng nhất định mà được hưởng lợi từ việc thay đổi người dùng, Ví dụ, chúng ta chạy 2 máy chủ web cùng một lúc, hoặc cần người sử dụng của một chương trình khác để có thể kiểm soát Nginx
   * **worker_process**: Có giá trị mặc định là 1. Nó định nghĩa số lượng worker process Nginx sử dụng. Số lượng worker process nên được set bằng giá trị với số core của CPU. Ví dụ với các web server hay sử dụng về SSL, gzip thì ta nên đặt chỉ số của worker_process này lên cao hơn. Nếu website của chúng ta có số lượng các tệp tin tĩnh nhiều và dung lượng của chúng lớn hơn bộ nhớ RAM thì việc tăng worker_process sẽ tối ưu băng thông địa của hệ thống. Để xác định số cores của CPU của hệ thống ta có thể thực hiện lệnh:

    `cat /proc/cpuinfo | grep processor`

   * **access_log**&**error_log**: những file mà Nginx sẽ sử dụng để log lại toàn bộ error và access request. Phần log này thường được sử dụng để debug

   * **pid**: xác định nơi Nginx sẽ ghi lại master process ID, hoặc PID. PID được sử dụng bởi hệ điều hành để theo dõi và gửi tín hiệu đến quá trình Nginx. Bạn có thể xác định thông tin về PID(master process và worker process) của nginx bằng câu lệnh:
    
    `ps -ax | grep nginx`
   
   * **worker_connections**: cho biết số lượng connection mà mỗi worker_process có thể xử lý. Mặc định, số lượng connection này được thiết lập là 1024. Để xem về mức giới hạn sử dụng của hệ thống bạn có thể dùng lệnh:
    
    `ulimit ulimit -n`

   Con số thiết lập của worker_connections nên nhỏ hơn hoặc bằng giới hạn này?
    `# max clients = worker_connections * worker_processes`

**Events Context**
 ```
  events {
  worker_connections  1024;
  }
 ```

 * NGINX sử dụng mô hình xử lý kết nối dựa trên sự kiện (event) nên các directive được định nghĩa trong context này sẽ ảnh hưởng đến connection processing được chỉ định. Ví dụ ở trên là cấu hình số worker connection mà mỗi worker process có thể xử lý được

**HTTP Context**
 
 * Khi cấu hình Nginx như một web server hoặc reverse proxy, http context sẽ giữ phần lớn cấu hình. Context này sẽ chứa tất cả các directive và những context(block directive) cần thiết khác để xác định cách chương trình sẽ xử lý các kết nối HTTP và HTTPS
 * Giải thích một số directive:
   * **include**: Chỉ thị include (include /etc/nginx/mime.types) của nginx có vai trò trong việc thêm nội dung từ một file khác vào trong cấu hình nginx. Điều này có nghĩa là bất cứ điều gì được viết trong tập tin mime.types sẽ được hiểu là nó được viết bên trong khôi http{}. Điều này cho phép bạn bao gồm một số lượng dài các chỉ thị trong khối http{} mà không gây lộn xộn lên các tập tin cấu hình chính. Và nó giúp tránh quá nhiều dòng mã cho mục đích dễ đọc. Bạn luôn có thể bảo gồm (include) tất cả các tập tin trong một thư mục nhất định với các chỉ thị: include /etc/nginx/conf/*; Bạn cũng có thể bảo gồm tất cả các file theo một định dạng nào đó, như ví dụ sau: include /etc/nginx/conf/**.conf; -> Nó sẽ bao gồm các tập tin có đuôi .conf
   * **gzip**: Chỉ thị gzip sẽ giúp nén các dữ liệu trước khi chuyển chúng tới Client, hạn chế số lượng băng thông sử dụng và tăng tốc độ dịch chuyển dữ liệu. Điều này tương đương với mod_deflate của Apache

**Server Context**
 * Được khai báo trong **http context**. Đây cũng là một ví dụ về context lồng nhau được đặt trong ngoặc. Đây cũng là context đầu tiên cho phép khai báo nhiều lần
 * Định dạng chung của server context có thể trông như sau:
 ```
   # main context
   http {
     # http context
     server {
         # first server context
     }
     server {    
         # second server context
     }        
   }
 ``` 
 * Dựa vào yêu cầu phía client, Nginx sẽ sử dụng thuật toán lựa chọn để quyết định server context nào được sử dụng. Các directive được sử dụng để quyết định server context nào được sử dụng là
   * **listen**: sự kết hợp của IP/port mà server block này được thiết kế để đáp ứng. Nếu một yêu cầu từ phía client phù hợp với giá trị này, block này có thể được lựa chọn để xử lý kết nối
   * **server_name**: nếu có nhiều server block đáp ứng được yêu cầu của listen directive, nginx sau đó sẽ tiến hành phân tích cú pháp tiêu đề "Host" của yêu cầu để lựa chọn block phù hợp

**Location Context**
 * Được đặt trong server context
 * Sau khi đã chọn được server context nào sẽ tiếp nhận request này thì nginx sẽ tiếp tục phân tích URI của request để tìm ra hướng xử lí của request dựa vào các location context có syntax(cú pháp) như sau:
```
  location optinal_modifier location_match {
   ...
  }
```

   * optional_modifier: chúng ta có thể tạm hiểu nó là kiểu so sánh để tìm ra để đối chiếu với `location_match`. Có mấy loại option như sau:
     * (none) : Nếu không khai báo gì thì Nginx sẽ hiểu tất cả các request có URI bắt đầu bằng phần location_match sẽ được chuyển cho location block này xử lý
     * = : Khai báo này chi ra rằng URI phải có chính xác giống như location_match (giống như so sánh string bình thường)
     * ~ : Sử dụng regular expression cho các URI
     * ~* : Sử dụng regular expression cho các URI cho phép pass cả chữ hoa và chữ thường

**Index directive**
index direct nằm bên trong location luôn được nginx trỏ tới đầu tiền khi xử lí điều hướng request. Định nghĩa trang mặc định mà Nginx sẽ phục vụ nếu không có tên tập tin được chỉ rõ trong yêu cầu (nói cách khác, trang chỉ mục). Chúng ta cs thể chỉ rõ nhiều tập tin và tập tin đầu tiên được tìm thấy sẽ được sử dụng. Nếu không có tập tin cụ thể nào được tìm thấy, Nginx sẽ hoặc là cố gắng phát sinh 1 chỉ mục tự động của các tập tin 
 ```
 location = / {
     index index.html;
 }
 ```

**try_files directive**
Cố gắng phục vụ các tập tin được chỉ rõ (các tham số từ 1 đến N-1 trong chỉ thị), nếu không có tập tin nào tồn tại, nhảy đến khối location được khai báo(tham số cối cùng trong chỉ thụ) hoặc phục vụ 1 URI được chỉ định
  ```
  location / {
      try_files $uri $uri.html $uri/ /fallback/index.html;
  }    
  ```

**rewrite directive**
Khác với Apache, Nginx không sử dụng file .htaccess nên khi bạn cần rewrite url sẽ phải convert qua rule của Nginx

**error_page directive**
Chỉ thị khi không tìm thấy file tham chiếu

```
location / {
    error_page 404 = @fallback;
}

location @fallback{
    proxy_pass http:backend;
}
```
 
 * Xử lý trong `location context`
   * Nginx sẽ đọc `root` directive để xác định thư mục chứa trang client yêu cầu. Thứ tự các trang được ưu tiên sẽ được khai báo trong `index` directive
   * Nếu không tìm được nội dung mà client yêu cầu, nginx sẽ điều hướng sang location context khác và thông báo lỗi cho người dùng
## Tổng quan Let's Encrypt 
Trước khi tìm hiểu Let's Encrypt, chúng ta cần phải biết **SSL** là gì, tầm quan trọng của nó đối với website

Khi đã biết về SSL và đang tìm kiếm SSL miễn phí cho website của mình, thì đã đến lúc cần biết về Let's Encrypt

### Let's Encrypt là gì?
**Let's Encrypt** là nhà cung cấp chứng chỉ SSL miễn phí, tự động, hoạt động vì lợi ích của cộng đồng. Nó được quản lý bởi Internet Security Rearch Group (ISRG)

Let's Encrypt cung cấp cho những người quản trị website một chứng nhận số cần thiết để kích hoạt HTTPS (SSL hoặc TLS) cho website của mình, hoàn toàn miễn phí và theo cách thân thiện nhất có thể. Tất cả dựa trên mục tiêu tạo ra môi trường Web an toàn, riêng tư và tôn trọng người dùng hơn

**Let's Encrypt cung cấp chứng chỉ SSL loại Domain Validation**

### Lợi ích khi sử dụng Let's Encrypt
 * **Miễn phí**: Chỉ cần sở hữu một tên miền, bạn có thể sử dụng Let's Encrypt để có được chứng chỉ tin cậy mà không mất bất kỳ chi phí nào
 * **Tự động**: Phần mềm chạy trên máy chủ web có thể tương tác với Let's Encrypt để có được chứng chỉ một cách nhanh chóng, cấu hình an toàn để sẵn sàng sử dụng và tự động thay mới khi cần. Song song đó, chứng chỉ SSL của Let's Encrypt theo chuẩn Domain Validation, do đó bạn chỉ cần tên miền và có thể sử dụng chúng cho bất kỳ máy chủ nào
 * **An toàn**: Let's Encrypt sẽ hoạt động như một nền tảng thúc đẩy những TLS tốt nhất, cả về phía CA (Certificate Authority) và giúp các nhà khai thác trang web đảm bảo an toàn cho máy chủ một cách đúng đắn
 * **Độ rõ ràng, minh bạch**: Tất cả các chứng chỉ được ban hành hoặc thu hồi sẽ được ghi công khai và bất cứ ai cũng có thể kiểm tra
 * **Không hạn chế**: Giao thức phát hành và gia hạn tự động sẽ được công bố như một tiêu chuẩn công khai và người khác có thể áp dụng
 * **Hợp tác**: Giống như những giao thức Interner cơ bản khác, Let's Encrypt nỗ lực để mang lại lợi ích cho cộng đồng và không nằm dưới sự kiểm soát của bất kỳ một tổ chức nào
 
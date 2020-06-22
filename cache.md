## Web Cache
Đứng trên phương diện người dùng, nhiều người sẽ không khỏi nản lòng bởi đôi lúc chúng ta mất nhiều thời gian để tải một tài liệu nào đó. Thậm chí đợi rồi lỗi, không tải được. Điều này gây tốn thời gian và còn tốn công sức. Thay vào đó, nhiều người chọn web cache với ưu điểm giảm độ trễ, tăng tốc độ load, giảm tài nguyên ...

## Web cache là gì?
**web cache** thực chất là một bộ nhớ đệm, thiết bị phần cứng hay ứng dụng phần mềm dùng để lưu trữ tạm thời các nội dung có dạng tĩnh, được truy cập sử dụng thường xuyên

Chúng ta có thể phân tích từ thực tế thói quen người dùng internet. Hầu như mọi người đều thường tự tải xuống các nội dung trên website nhiều lần. Với mỗi người dụng thực hiện request, các reponse sẽ được gửi đi từ **origin** hay còn gọi là máy chủ gốc

Trong trường hợp quá nhiều người tải dữ liệu trong cùng 1 thời gian, thời gian phản hổi (reponse time) sẽ tăng lên, thậm chỉ có thể gây quá tải máy chủ

Khi đó, nếu có sự hỗ trợ của web cache, chúng sẽ giúp xử lý các request cho những nội dung phổ biến. Và chúng sẽ chuyển hướng đến máy chủ gốc. Điều này giúp đặt các nội dung ở vị trí gần hơn với người dùng cuối cùng, giúp cải thiện thời gian phản hồi, tăng tốc độ tải dữ liệu nhanh hơn

## Cách thức hoạt động của web cache
Web cache được coi như một bộ chứa trung gian, lưu trữ dữ liệu trong thời gian ngắn. Bất cứ nội dung nào được tải xuống từ máy chủ, một bản sao đi kèm sẽ được tải vào bộ nhớ đệm web cache và lưu trữ tại đây trong một thời gian nhất định mà bạn cài đặt trước đó

Khi có người dùng tải 1 yêu cầu, nội dung đó web cache sẽ thực hiện công việc thay máy chủ bằng cách gửi nội dung đã được lưu trước đó đi. Khi đó, các requests của người dùng không cần gửi tới máy chủ nữa. Khi đó, bộ nhớ đệm này được gọi là bộ nhớ đệm nội dung (hay **content caching** là một cơ chế tối ưu hóa hiệu suất, trong đó dữ liệu được phân phối từ các máy chủ gần nhất để có hiệu suất ứng dụng tối ưu)

Flow(lưu lượng) của bộ nhớ đệm web cache được phổ biến như:
 * Người dùng có thể truy cập website
 * Ngôn ngữ lập trình HTTP request đến với bộ nhớ đệm
 * Nếu đối tượng được yêu cầu lưu trữ trong web cache thì chúng sẽ phản hồi ngay với đối tượng này mà không cần qua máy chủ 
 * Nếu đối tượng được lưu trữ cache, web cache sẽ giữ lại bản sao và gửi cho request tiếp theo



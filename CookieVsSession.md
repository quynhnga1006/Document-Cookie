# Document about Session and cookie

## Link document : <https://learn.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/storage/cookies>

## Cookie

- Cũng là 1 cách để lưu trữ những thông tin tạm thời. Lưu dưới dạng key-value, có thời gian tồn tại.
- File cookie sẽ được truyền từ server tới browser và được lưu trữ trên máy tính của người dùng khi người dùng truy cập vào ứng dụng.
- Mỗi khi người dùng tải ứng dụng, trình duyệt sẽ gửi cookie để thông báo cho ứng dụng về hoạt động trước đó của bạn.
- Cookie có thể sửa đổi và đánh cắp, có thể lợi dụng để tấn công website của bạn => Đừng bao giờ lưu trữ những thông tin quan trọng, yêu cầu tính bảo mật cao vào cookie.
- Nguyên tắc hoạt động:
  - Người dùng truy cập vaod trang web lần đầu tiên, header mà trình duyệt gửi lên sẽ có dạng :
        GET /index.html HTTP/1.1
        Host: www.example.org
  - Server không tìm thấy cookie trên request người dùng, chính vì vậy nó sẽ response cookie bằng cách gửi lại header như sau:
        HTTP/1.0 200 OK
        Content-type: text/html
        Set-Cookie: theme=light
        Set-Cookie: sessionToken=abc123; Expires=Wed, 09 Jun 2021 10:18:14 GMT
        Set-Cookie: status=active; Max-Age: 300
        Set-Cookie: name=tien; Expires=Wed, 09 Jun 2021 10:18:14 GMT; Max-Age: 300

  - Trình duyệt sẽ lưu lại giá trị của cookie dựa trên header của server trả về

## Session

- Session là 1 phiên làm việc.
- Lưu dữ liệu dưới dạng key-value, lưu các thông tin phiên làm việc của người dùng. Session cho phép lưu trữ dữ liệu trên server, độc lập hoàn toàn với client => Dữ liệu sẽ an toàn và đáng tin cậy hơn.
- Session lưu trữ 1 biến và khiến biến đó tồn tại từ trang này sang trang khác, chỉ mất đi khi người dùng tắt trình duyệt hoặc hết TimeOut
- Nguyên tắc hoạt động: 1 session bắt đầu khi client gửi request đến server, nó tồn tại xuyên suốt từ page này qua page khác và chỉ kết thúc khi hết TimeOut hoặc người dùng tắt ứng dụng.

## Ứng dụng trong bài toán Login

- Khi user login thì server sẽ sinh ra session, lưu ở server( trong memory của ram hoặc save file, tuỳ). Đồng thời server sẽ set header trên response trả về client, mục đích của header này là lưu cookie ở phía client. Trong cooke sẽ có session_id tương ứng với session ở server. Nhờ đó mà server biết được ai thực hiện các request ở lần tiếp theo:
  - Case 1: User không check vào remember me thì khi server set header sẽ không bao gồm expired date cho cookie. Khi browser nhận đc lệnh tạo cookie mà không có expired date thì nó hiểu đây là session cookie - đặc điểm của loại cookie này là khi user tắt browser thì nó xoá cookie này, lúc đó user phải đăng nhập lại.
  - Case 2: User check vào remember me thì server khi set header sẽ bao gồm expired date thì khi tắt browser cookie này vẫn còn. Khi cookie còn hạn thì user còn đăng nhập hoài.

## Note

- Session_id là định danh(tên) được đặt cho file lưu trữ khi session đucojw sinh ra, là một tên dài dòng, khó đoán(y chang cái nết của con Nga) và được tạo ngẫu nhiên.

## Some questions about cookie

-When changing value of cookie by F12, session of user will change
.AspNetCore.Identity.Application

## So sánh Cookie vs Session

- Cookie:
  - Cookie được lưu trữ trên trình duyệt của người dùng
  - Dữ liệu cookie được lưu trữ ở phía máy khách
  - Dữ liệu cookie dễ dàng sửa đổi khi chúng được lưu trữ ở phía khách hàng
  - Dữ liệu cookie có sẵn trong trình duyệt cho đến khi hết hạn

- Session

  - Session không được lưu trữ trong trình duyệt của người dùng
  - Dữ liệu session được lưu trữ ở phía máy chủ
  - Dữ liệu session không dễ dàng sửa đổi vì chúng được lưu trữ ở phía máy chủ
  - Dữ liệu session có sẵn cho trình duyệt chạy, sau khi đóng trình duyệt sẽ mất thông tin session

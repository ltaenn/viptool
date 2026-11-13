# viptool 1.1

Bookmarklet cho trình duyệt điện thoại và máy tính. Hồ sơ mặc định: **HI88**. Tool thao tác trên trang đang mở; tên miền của website không bị gắn cứng trong mã.

**Trang cài đặt:** https://ltaenn.github.io/viptool/

```javascript
javascript:import('https://ltaenn.github.io/viptool/r.js').then(m=>m.o())
```

Mã bookmarklet dài **73 ký tự**. Mã giao diện và bộ điều khiển được nạp từ GitHub Pages bằng ES module. Mở lại bookmarklet sẽ mở bảng đang có; đóng rồi mở lại vẫn hoạt động dù module đã được trình duyệt cache.

## Cài đặt

1. Mở trang cài đặt, bấm **Sao chép mã**.
2. Tạo dấu trang tên **viptool**, sửa URL thành toàn bộ dòng mã, bao gồm `javascript:`.
3. Mở website cần thao tác rồi gọi dấu trang viptool. Trên Chrome Android, nếu menu dấu trang không thực thi, nhập tên dấu trang vào thanh địa chỉ và chọn gợi ý dấu trang.
4. Chọn site **HI88**, nhập data, kiểm tra rồi bấm **Bắt đầu**. Có chế độ **Chạy thử giao diện** để xem thử mà không thao tác vào website.

Trên máy tính có thể kéo nút viptool từ trang cài đặt lên thanh dấu trang. `bookmarklet-offline.txt` chứa bản tự đủ mã, dài hơn và không cần tải module bên ngoài.

## Ba chế độ

- **Đăng ký + thêm bank:** chạy đầy đủ quy trình bên dưới.
- **Điền đăng ký:** chỉ gõ vào form; không bấm đăng ký.
- **Thêm ngân hàng:** bắt đầu từ Tài khoản, cho tài khoản đã đăng nhập.

## Data

```text
username|passwork|numberphone|name|namebank|numberbank|cyti|pinwithdraw
```

Mỗi dòng có đúng 8 cột. Tên cột theo yêu cầu gốc: `passwork` là mật khẩu, `cyti` là thành phố. Ví dụ dữ liệu hư cấu dùng trên trang mẫu:

```text
||0900000000|NGUYEN VAN DEMO|MB|000000000001||
```

Bốn cột username, mật khẩu, thành phố, PIN có thể trống và tự sinh, với công tắc bật/tắt riêng. Giá trị đã nhập được giữ nguyên. Giá trị tự sinh ổn định trong phiên; xem lại và sao chép ở tab Dữ liệu. Số điện thoại và số tài khoản là chuỗi, giữ số 0 đầu.

Các giá trị tự sinh chưa được xác nhận phù hợp mọi quy tắc của website; nếu website yêu cầu độ dài hoặc định dạng khác, điền giá trị phù hợp vào data. PIN tự sinh có 6 chữ số.

## Hai ô mật khẩu rút

Tool xác định đủ hai ô nhập riêng biệt trong cùng form trước khi gõ. Nhận diện tên trường/nhãn xác nhận, đồng thời hỗ trợ form có đúng hai ô mật khẩu mà không có tên trường chuẩn. Hai ô phải khác nhau, được điền cùng PIN từng ký tự và được kiểm tra bằng nhau ngay trước khi bấm Gửi đi. Nếu trỏ cùng phần tử hoặc giá trị không trùng, tool dừng và chưa gửi. Có thể gán lại riêng từng ô trong Cấu hình.

## Thứ tự thao tác

1. Điền form đăng ký, từng ký tự; điền xác nhận mật khẩu nếu có.
2. Bấm Đăng ký ngay; chờ phản hồi thành công hoặc chuyển sang trạng thái đăng nhập.
3. Mở Tài khoản.
4. Mở Rút tiền.
5. Bấm đường dẫn cài mật khẩu rút / Tại đây.
6. Điền PIN và xác nhận PIN, từng ký tự.
7. Bấm Gửi đi; chờ phản hồi đặt PIN.
8. Mở danh sách ngân hàng.
9. Chọn theo mã ngân hàng đã chuẩn hóa.
10. Điền thông tin ngân hàng, từng ký tự. Nếu bật **Lưu ngân hàng sau khi điền**, bấm nút lưu và chờ phản hồi thành công.

**Sau mỗi bước đều nghỉ 5 giây**, kể cả bước cuối. Tốc độ gõ mặc định 70 ms/ký tự, có thể chỉnh 30–150 ms. Tạm dừng sẽ dừng gõ và dừng tính khoảng nghỉ. Dừng sẽ hủy các thao tác đang chờ, nhưng không hoàn tác hành động website đã nhận trước đó.

Nếu website đã có PIN và mở thẳng form ngân hàng, tool giữ PIN hiện tại, không đặt lại PIN. Data nhiều dòng được xử lý theo thứ tự; giữa hai dòng tool dừng để bạn đăng xuất/chuyển đúng tài khoản hoặc mở form cho dòng kế tiếp rồi nhấn Tiếp tục. Tool không tự tạo phiên tài khoản riêng cho từng dòng.

## Tiếp tục sau khi trang tải lại

1. Mở lại dấu trang viptool sau khi đăng ký làm website tải lại.
2. Tool hiển thị tiến trình và data đã giữ trong tab.
3. Nếu chưa nhận được xác nhận của server trước khi tải lại, kiểm tra tài khoản trên website đã đăng nhập đúng rồi nhấn Đã kiểm tra web, tiếp tục.
4. Tool bắt đầu từ Tài khoản, giữ nguyên username/mật khẩu/thành phố/PIN đã tự sinh và không bấm đăng ký lần nữa.

Bản lưu chỉ dùng cho cùng tab và cùng origin, không được đồng bộ lên GitHub hay sang thiết bị khác. Có thể tắt Khôi phục sau tải trang trong Cấu hình. Trang mẫu `sandbox.html?reload=1` hỗ trợ kiểm tra việc tải lại sau đăng ký.

## Tìm phần tử và cấu hình theo site

Đã có bộ nhận diện ban đầu cho HTML Angular Material được cung cấp: nút đăng ký `.submit-btn`, nút rút tiền `a.withdraw`, ô ngân hàng `mat-select[formcontrolname="bankName"]`, mục `.mat-option-text`, cùng các tên trường thường gặp.

HTML các ô đăng ký/PIN/số tài khoản chưa được cung cấp đầy đủ. Vì vậy bộ nhận diện mặc định cần được kiểm tra trên website thật. Khi tool chưa tìm thấy hoặc thấy nhiều phần tử, tool dừng với thông báo thay vì tự chọn tùy ý.

Trong **Cấu hình → Gán ô nhập / nút trên trang**:

- Bấm **Kiểm tra trang** để xem các trường đang nhận diện được.
- Bấm **Chọn** cạnh trường, rồi chạm phần tử tương ứng trên website. Lần chạm này chỉ chọn phần tử, không kích hoạt hành động của nó.
- Có thể nhập CSS selector trực tiếp. Các cấu hình được lưu riêng theo mã site trong trình duyệt đó.
- **Sao chép cấu hình site** và **Nhập cấu hình site từ JSON** giúp chuyển cấu hình sang thiết bị khác. Data tài khoản không nằm trong cấu hình.
- URL trong hồ sơ là tùy chọn. Nếu nhập URL, tool kiểm tra đúng origin trước khi chạy.

Các đường dẫn xác nhận mặc định cho HI88:

```text
/api/1.0/member/register
/api/1.0/member/updateWithdrawPassword
/api/1.0/bank/add
```

Tool chỉ quan sát các phản hồi mà website tạo ra từ thao tác giao diện; không tự gửi các request API này. Hồ sơ site khác có đường dẫn riêng, có thể cung cấp qua cấu hình JSON với mảng `routes` gồm ba đường dẫn theo thứ tự trên.

## Ngân hàng

Danh sách 65 mục lấy từ JSON đã cung cấp, tại `bank-list.json`. Chấp nhận mã ngắn, `Display` hoặc `Name`. Ví dụ:

| Mã | Tên | ID |
|---|---|---:|
| MB | MBBANK | 13 |
| AGRI | AGRIBANK | 1 |
| ACB | ACB BANK | 15 |
| VCB | VIETCOMBANK | 37 |
| TCB | TECHCOMBANK | 31 |
| CTG | VIETINBANK | 38 |
| VP | VPBANK | 39 |

Xem toàn bộ tại tab Mã bank. `VCBNEO` có `IsBindEnabled=false` trong JSON gốc và được đánh dấu chưa hỗ trợ liên kết.

## Kết quả và dữ liệu

Tool quan sát `fetch` và `XMLHttpRequest` trên đúng trang. Phản hồi HTTP lỗi, mã `Code` thất bại hoặc mảng `Error` có nội dung sẽ dừng quy trình. Không nhận được xác nhận lưu ngân hàng cũng sẽ dừng; tool không tự báo đã liên kết thành công.

Khi chạy thật và bật Khôi phục sau tải trang, tiến trình cùng data (gồm các giá trị mật khẩu/PIN đã nhập hoặc tự sinh) được lưu tạm trong `sessionStorage` của chính tab trên cùng origin. Bản lưu có hiệu lực khôi phục 30 phút và bị xóa khi hoàn tất, dừng, đóng tool, bỏ tiến trình hoặc khi phát hiện hết hạn. Cấu hình site được lưu riêng trong `localStorage`. Không có analytics, máy chủ nhận data riêng hoặc lưu data tài khoản lên GitHub. Khi đóng tool, các hàm quan sát request được khôi phục nếu chúng vẫn là bản tool đã gắn.

## Kiểm tra và giới hạn

Có trang mẫu `sandbox.html` để kiểm tra luồng gõ, bấm và xác nhận bằng các phản hồi giả lập ngay trong trình duyệt. Trang này không tạo tài khoản hoặc liên kết ngân hàng thật.

Đã kiểm tra bằng Chromium cục bộ: cả ba chế độ; luồng thật trên DOM mẫu đủ 10 bước; xác nhận ba phản hồi; gõ từng ký tự; giữ số 0 đầu; giữ PIN đã có; tạm dừng/tiếp tục; hủy khi dừng; dừng khi API trả lỗi nghiệp vụ; bookmarklet module mở lại; khung rộng 320, 360, 390, 430 px. Chưa kiểm thử tài khoản thật trên website hoặc trên từng thiết bị điện thoại thực tế.

Bookmarklet cần trình duyệt hỗ trợ JavaScript URL và ES module. CSP của website có thể chặn việc nạp mã. Tool không chạy trong giao diện native của app hoặc iframe khác origin. Các sự kiện từ bookmarklet là sự kiện JavaScript, không phải sự kiện bàn phím được trình duyệt đánh dấu `isTrusted`.

CAPTCHA, OTP, điều khoản và các yêu cầu bổ sung của trang cần người dùng xử lý. Nút bị khóa được chờ để trang tự bật; tool không gỡ `disabled`. Nếu website tải lại toàn bộ tài liệu, giao diện bookmarklet sẽ mất. Gọi lại dấu trang viptool trong cùng tab/cùng tên miền, rồi chọn Tiếp tục. Data và giá trị tự sinh được khôi phục. Nếu trang tải lại trong lúc gửi Đăng ký/Gửi đi/Lưu, cần kiểm tra thao tác đã thành công trên website trước khi xác nhận tiếp tục; tool không tự gửi lại thao tác đó. Bookmarklet không có quyền tự chèn lại mã sau khi tải tài liệu mới; muốn tự mở hoàn toàn cần cơ chế chạy theo trang như userscript/extension.

## Các file

- `r.js`: ES module 1.1 cho bookmarklet ngắn; URL mới để tránh cache bản cũ.
- `v.js`: tên module cũ, được giữ tương thích.
- `viptool.js`: cùng bộ mã ở dạng script tự chạy.
- `index.html`: trang cài đặt.
- `sandbox.html`: trang kiểm tra mẫu.
- `bookmarklet.txt`: bookmarklet ngắn.
- `bookmarklet-offline.txt`: bản chứa toàn bộ mã.
- `bank-list.json`: danh sách ngân hàng.

Để đổi nơi lưu mã, đưa các file lên hosting HTTPS, rồi tạo bookmarklet theo URL mới của `r.js`. Trang cài đặt tự lấy URL module theo địa chỉ HTTPS hiện tại. Hosting cần trả JavaScript với MIME phù hợp và cho phép CORS để module được nạp từ website khác. GitHub Pages đã được chọn cho bản này.

# viptool 2.3

Bookmarklet cho trình duyệt điện thoại và máy tính. Hồ sơ có sẵn: **HI88, NEW88, F8BET, Shbet, MB66, 789BET, QQ88, otpokvip**. Sáu hồ sơ đầu dùng luồng cũ; QQ88 và otpokvip dùng luồng riêng. Cấu hình của các site được lưu độc lập. Tool thao tác trên trang đang mở; tên miền của website không bị gắn cứng trong mã.

**Trang cài đặt:** https://ltaenn.github.io/viptool/

```javascript
javascript:import('https://ltaenn.github.io/viptool/e.js').then(m=>m.o())
```

Mã bookmarklet dài **73 ký tự**. Mã giao diện và bộ điều khiển được nạp từ GitHub Pages bằng ES module. Mở lại bookmarklet sẽ mở bảng đang có; đóng rồi mở lại vẫn hoạt động dù module đã được trình duyệt cache.

## Cài đặt

1. Mở trang cài đặt, bấm **Sao chép mã**.
2. Tạo dấu trang tên **viptool**, sửa URL thành toàn bộ dòng mã, bao gồm `javascript:`.
3. Mở website cần thao tác rồi gọi dấu trang viptool. Trên Chrome Android, nếu menu dấu trang không thực thi, nhập tên dấu trang vào thanh địa chỉ và chọn gợi ý dấu trang.
4. Chọn đúng site, nhập data, kiểm tra rồi bấm **Bắt đầu**. Có chế độ **Chạy thử giao diện** để xem thử mà không thao tác vào website.

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

## Chờ CAPTCHA do người dùng tự giải

Trong bước đăng ký, tool nhận diện một số khung CAPTCHA thông dụng (reCAPTCHA, hCaptcha, Turnstile, Geetest, Yidun, khung trượt hoặc ô checkCode hiện trên trang) và các phản hồi báo lỗi CAPTCHA. Tool không nhập đáp án, không nhấn xác minh và không giải CAPTCHA.

Khi có CAPTCHA, bảng tool thu nhỏ, hiển thị trạng thái Chờ CAPTCHA và không tính thời gian người dùng giải vào giới hạn chờ thao tác. Bạn tự giải trên website; tool tiếp tục khi thấy khung biến mất hoặc token hoàn tất. Nếu thư viện vẫn giữ khung sau khi giải, bấm **Tôi đã giải xong** trên thông báo của tool để tiếp tục kiểm tra. Có thể bấm Dừng ngay khi đang chờ.

Nếu website tự gửi đăng ký sau khi bạn giải, tool đợi kết quả; không gửi lại khi đã nhận thành công. Nếu website từ chối rõ vì CAPTCHA sai và chưa gửi lại, tool chỉ thử lại sau khi người dùng hoàn tất CAPTCHA. CAPTCHA có cấu trúc riêng chưa nhận diện được có thể cần bổ sung mẫu. Không thử giải CAPTCHA tự động.

## Chrome Inspect trên LDPlayer — bản 2.3

Đã kết nối đúng target Chrome của LDPlayer, đọc DOM thực tế ở trang đăng ký và 63 file JavaScript đang tải hoặc được frontend tham chiếu. Trong các file này có 493 đường dẫn API khai báo; danh sách đó khác với API thực sự đã gọi và không chứng minh đã biết mọi endpoint phía máy chủ. Báo cáo chi tiết được cung cấp riêng trong thư mục outputs của phiên làm việc.

Đã xác minh trên LDPlayer thật: chế độ Điền đăng ký điền đủ account, userpass và realName, chưa gửi tạo tài khoản. Sau đó người dùng tự hoàn tất đăng ký và bước PIN; đã đọc màn hình thật sau đăng nhập, hai hàng PIN và hộp PIN xác minh. Đã nạp module 2.3 trong trang thật và dùng chính hàm tool điền số tài khoản, chọn MB thành công, dừng trước xác nhận lưu. Chưa tuyên bố hoàn tất gắn ngân hàng/SĐT khi chưa có xác nhận trên trang thật.

Các phát hiện từ code thật:

- Trang giữ bản sao nút Đăng ký trong .ui-tab__panel-wrapper--inactive. Phần tử ngoài màn hình vẫn có display/visibility bình thường nên lọc :visible không đủ. Bản 2.3 loại mọi ô/nút PIN trong panel inactive.
- Ba ô đăng ký nằm trong form, nút Đăng ký là anh em của form ở phần extra. Bộ chọn tiếp tục tìm trong nhóm đăng ký đang hoạt động.
- Component nút phát tap từ mousedown/touchstart và cha gửi đăng ký qua onTap; click đơn thuần không gửi. Bản 2.3 gửi đúng một mousedown/mouseup, không bấm hai lần và không gỡ disabled/loading.
- Phím số nhận press trên touchstart ở .ui-number-keyboard-key__wrapper; touchend ngăn hành vi mặc định. Sự kiện chạm ở phần tử con lan tới wrapper. Trước khi chuyển hàng PIN, tool phát touchstart vào ô mới để làm mất focus bàn phím cũ, rồi click mở đúng hàng.
- ID addAccountClick bị dùng chung cho Tài khoản ngân hàng và Tiền điện tử; tool lọc đúng nhãn ngân hàng trước khi bấm. Sau hai hàng PIN có nút Xác Nhận lưu PIN riêng; sau nhập PIN để thêm bank có nút Tiếp Theo riêng. Hai bước này đã được thêm vào luồng.
- Mục bank .ui-options__option chỉ nhận touchstart/touchend, không có listener click. Tool dùng chạm để chọn; MB đã được chọn thành công trên trang thật.
- API transport có thể trả text mã hóa. Phản hồi không đọc được JSON được giữ là chưa xác minh; tool chỉ tiếp tục khi UI tương ứng cũng cho thấy đã thành công (chuyển đúng trang, form đóng kèm thông báo thành công, hoặc trạng thái đã liên kết SĐT). Không dùng HTTP 200 làm bằng chứng duy nhất.
- API client báo thành công bằng code: 1. Tool nhận code viết thường theo site otpokvip và không áp quy tắc đó cho site khác.

Các route xác nhận dùng cho otpokvip:

| Thao tác | Route transport |
|---|---|
| Đăng ký | /hall/api/member/register |
| Đặt PIN rút | /hall/api/member/user/security/modifyWithdrawPass |
| Gắn ngân hàng | /hall/api/finance/certify/bindWithdrawAccount |
| Gửi SMS | /hall/api/member/user/security/createSms |
| Kiểm tra OTP | /hall/api/member/user/security/verifyPhoneCode |
| Liên kết/đổi SĐT | /hall/api/member/user/security/modifyPhone |
| Cập nhật SĐT trên profile | /hall/api/member/v2/user/update |

Tool quan sát request do trang tạo từ thao tác giao diện; không tự gọi API tài khoản. Thông tin nguồn đã đọc dùng để chọn endpoint và hợp đồng sự kiện, chưa thay thế kiểm tra kết quả server trên tài khoản thật.

**Kiểm tra từng bước:** bật trong Cấu hình để tool dừng trước từng hành động. Nhấn Tiếp tục để tool thực hiện; hoặc bạn tự làm trên trang rồi nhấn Đã làm thủ công bước này. Bước làm thủ công được ghi rõ trong kết quả, không bị báo như thao tác tool tự xác minh. Dừng sẽ hủy điểm chờ. Có thể mở module mới ngay trong trang; data, chế độ và checkpoint đang lưu được giữ khi cập nhật từ phiên bản cũ.

**Liên kết SĐT:** sau khi điền số, tool chờ bạn xác nhận trên website và nhập OTP nếu trang yêu cầu. Chỉ báo liên kết khi nhận phản hồi thành công của route liên kết. createSms/verifyPhoneCode đơn thuần không tính là đã liên kết. Phản hồi profile update chung cần thêm UI số điện thoại đã liên kết. Tắt Chờ xác nhận liên kết SĐT trong Cấu hình nếu chỉ muốn dừng ở điền số.

Đã kiểm thử bằng trang mẫu mô phỏng đúng hợp đồng mousedown/touchstart, tab inactive, bàn phím teleported nhiều hàng, hai ID Thêm Vào ngân hàng/crypto, nút xác nhận PIN và Tiếp Theo riêng, API code:1 và text mã hóa, xác nhận ngân hàng/SĐT, không coi gửi SMS là liên kết, kiểm tra từng bước và cập nhật module giữ data. Chưa xác minh đầy đủ tạo tài khoản → PIN → ngân hàng → OTP trên tài khoản thật; cần người dùng hoàn tất các bước xác nhận để kiểm tra tiếp.

## Cấu hình otpokvip dùng chung và lượt xác nhận PIN — bản 2.2

Đã lưu cấu hình người dùng cung cấp vào otpokvip-keypad-config.json và nhúng cùng module phát hành. Có cả accountMenu theo selector footer>div>div>div:nth-of-type(5)>div>div, register/pinSlot đã gán và toàn bộ mục cấu hình còn lại. Không có username, mật khẩu hay giá trị PIN trong file.

Lúc mở bản 2.2, tool áp dụng cấu hình này cho trình duyệt mới và cập nhật hồ sơ otpokvip cũ một lần, rồi lưu vào localStorage trên website hiện tại. Các mục gán phím riêng không nằm trong cấu hình chung được giữ. Các lần chỉnh cấu hình tại máy sau lần cập nhật đó được giữ nguyên, không bị ghi đè khi mở lại cùng phiên bản. Nhập cấu hình JSON đánh dấu đã cập nhật để lần mở sau tiếp tục giữ file vừa nhập. Các chỉnh sửa sau này không tự đẩy ngược lên GitHub; bản chung này là cấu hình đã được người dùng yêu cầu lưu vào phát hành.

Lỗi ở cuối lượt PIN đầu đã được tái hiện bằng module 2.1: đủ sáu phím, sau đó tìm hàng xác nhận gây lỗi Chưa xác định được nhóm PIN. Bản 2.2 giữ nguyên cách bấm phím lượt đầu, ghi nhớ nhóm vừa nhập xong, nhận hàng xác nhận trống hoặc hàng cũ đã được trang xóa để nhập lại. Nếu trang hiện hàng thứ hai chậm, tool chờ thay vì dừng ngay. Trước lượt nhập lại, tool chọn/focus hàng xác nhận ngay cả khi bàn phím vẫn đang mở.

Trong mỗi lượt sáu số, nhóm đang nhập được cố định cho đến khi hết lượt. Hàng xác nhận có một vài ký tự sẽ không bị bỏ qua để chọn một hàng khác còn trống. Tool không gõ lại lượt đầu khi chờ hàng xác nhận. Nếu không đủ bằng chứng nhận diện hàng tiếp theo, tool tiếp tục chờ theo giới hạn thao tác và báo đúng bước đang chờ. Khoảng nghỉ vẫn 2 giây.

## Nhập PIN trên bàn phím web — bản 2.1

Cấu hình của otpokvip có thêm các mục sau, được lưu và xuất/nhập cùng hồ sơ site:

- mapping.pinSlot: ô đầu để mở bàn phím; giữ nguyên mục bạn đã gán.
- mapping.pinConfirmSlot: ô đầu hàng xác nhận riêng, nếu trang có hai hàng cùng hiện và cần gán riêng.
- mapping.pinKeyboard: khung bàn phím số, mặc định .ui-number-keyboard.
- mapping.pinKeys: selector chung của các phím số, mặc định .ui-number-keyboard-key. Tool chọn số theo chữ trên phím, bao gồm 0; không chọn phím Xóa hoặc Thu gọn.
- mapping.pinKey0 đến mapping.pinKey9: selector riêng cho từng phím nếu bàn phím có cấu trúc khác. Để trống để dùng pinKeys.
- pinKeyEvent: auto, touch hoặc click. Mặc định auto.

Luồng tự động vẫn bấm PIN lần lượt ở ba bước đặt PIN, nhập lại PIN và nhập PIN để thêm ngân hàng. PIN luôn lấy từ cột pinwithdraw, cần 6 chữ số hoặc tự sinh nếu để trống và bật tự sinh PIN. Không nhập PIN bằng cách dán vào ô mật khẩu native trên site này.

Chế độ auto gửi một cặp touchstart/touchend rồi quan sát số chấm PIN. Khi trang đã nhận ký tự thì không gửi click. Chỉ khi chưa thấy trang nhận ký tự mới dùng click dự phòng, sau đó tiếp tục chờ phản hồi. Nếu không xác nhận được ký tự hoặc trang nhận nhiều ký tự cho một lần bấm, tool dừng trước phím kế tiếp. Có thể chọn touch hoặc click riêng trong Cấu hình để phù hợp bàn phím website.

Có nút độc lập trong tab Dữ liệu: cuộn tới Bấm PIN trên bàn phím web → chọn dòng data → chọn lượt nhập → bấm Nhập PIN trên bàn phím web. Chức năng này chỉ bấm sáu phím, không đăng ký, không bấm nút xác nhận ngân hàng và không gửi OTP. Dừng sẽ hủy cả click dự phòng đang chờ. Nếu có tiến trình đã lưu, cần tiếp tục hoặc bỏ tiến trình đó trước khi chạy chức năng riêng.

Trong Cấu hình, mục Chọn cạnh bàn phím chọn cả khung bàn phím; mục Chọn cạnh Các phím số chọn mẫu phím dùng chung. Các mục số 0–9 cho phép gán riêng từng phím. Chọn chỉ ghi nhận selector, không kích hoạt phím khi đang gán. Các trường native pin/pinConfirm của site khác không hiện trong danh sách gán otpokvip để tránh nhầm lẫn.

File otpokvip-keypad-config.json chứa cấu hình bạn cung cấp được bổ sung bàn phím mặc định. Giữ register/pinSlot bạn đã gán; selector dạng nth-of-type có thể cần gán lại khi website đổi bố cục. File không có data tài khoản hay giá trị PIN. Copy nội dung file vào Cấu hình → Nhập cấu hình site từ JSON để chuyển giữa máy.

## Nhận diện ba ô đăng ký otpokvip

Bản 1.8 nhận trực tiếp ba input trong cùng một nhóm đăng ký đang hiển thị:

- input[data-input-name="account"]
- input[data-input-name="userpass"]
- input[data-input-name="realName"]

Nút đăng ký được tìm theo button#insideRegisterSubmitClick trong đúng nhóm account/userpass/realName vừa điền, mở rộng tới vùng đăng ký nếu nút nằm ở footer bên cạnh hoặc ngoài form. Tool chờ button được bật, rồi chỉ bấm một lần vào div.ui-button__content bên trong; sự kiện lan lên button, hỗ trợ handler gắn ở cả nội dung lẫn button. Không bấm cả hai phần tử nối tiếp nhau để tránh gửi hai lần. Selector đăng ký tự gán được dùng trong đúng nhóm form đăng ký; nếu không khớp thì tìm theo ID nút mặc định trong nhóm đó, không tìm tùy ý trên toàn trang. Tool loại nút opacity 0, aria-hidden, inert hoặc nằm ở nhóm khác; nếu có hai nút cùng hiện thì chỉ chọn khi xác định được duy nhất một nút được bật. Hai nút cùng bật trong cùng form vẫn được báo rõ để tránh bấm nhầm. Các section.ui-input, section.ui-input__input-wrap, section.ui-input__input-container và biểu tượng SVG là thẻ bao ngoài, không phải đích nhập. Việc điền ba ô này không phụ thuộc selector Username/Mật khẩu/Họ tên cũ đã lưu. Tool tự chọn otpokvip khi có đủ bộ ba này trên trang lúc mở. Có thể chọn lại site bằng danh sách hồ sơ.

Tool phân biệt form đăng ký với form đăng nhập đang hiển thị bằng việc tìm bộ ba trong cùng nhóm, bỏ bản sao ẩn qua hidden/aria-hidden/opacity, tìm lại input sau mỗi ký tự để hỗ trợ website dựng lại ô. Nếu thực sự có hai form đăng ký hiện cùng lúc, tool dừng thay vì nhập tùy ý. Cấu hình → Kiểm tra trang cho biết đã tìm đủ ba ô hay chưa; thông tin kiểm tra không chứa giá trị tài khoản/mật khẩu.

Đã kiểm tra các trường hợp trên trang mẫu có cấu trúc section lồng nhau và input tương ứng HTML được cung cấp. Chưa xác nhận nguyên nhân timeout trên phiên website thật; nếu vẫn dừng, gửi thông báo mới trong Tiến trình.

## Luồng otpokvip theo thứ tự file

Có hồ sơ **otpokvip** riêng, dùng nguyên định dạng data và mã bank cũ. City không được dùng vì file đầu vào không có ô city cho site này. Số điện thoại được dùng ở cuối luồng trong Bảo mật. PIN cần đúng **6 chữ số**, hoặc được tự sinh 6 chữ số nếu để trống và bật tự sinh.

Tác vụ Đăng ký + thêm bank chạy 23 bước:

1. Điền tài khoản: input[data-input-name="account"].
2. Điền mật khẩu: input[data-input-name="userpass"].
3. Điền họ tên: input[data-input-name="realName"].
4. Bấm ĐĂNG KÝ bằng mousedown/touchstart của button#insideRegisterSubmitClick trên panel đang hoạt động.
5. Bấm Tài Khoản.
6. Bấm Rút Tiền.
7. Bấm ô PIN đầu tiên.
8. Nhập PIN qua bàn phím web.
9. Nhập lại cùng PIN qua bàn phím web.
10. Bấm Xác Nhận để lưu PIN, chờ API/UI xác nhận.
11. Bấm Thêm Tài Khoản (nếu màn hình chọn loại tài khoản đã mở thì bỏ qua).
12. Bấm Thêm Vào đúng mục Tài khoản ngân hàng, không phải Tiền điện tử.
13. Nhập PIN để thêm ngân hàng.
14. Bấm Tiếp Theo ở hộp PIN để mở form.
15. Điền số tài khoản.
16. Mở chọn ngân hàng phát hành.
17. Chạm chọn tên ngân hàng theo mã bank.
18. Bấm Xác Nhận lưu ngân hàng; chờ API/UI thành công.
19. Bấm thoát / quay lại.
20. Bấm Bảo Mật.
21. Bấm Số Điện Thoại.
22. Điền SĐT từ data.
23. Chờ người dùng xác nhận/OTP và bằng chứng liên kết SĐT thành công.

Các lượt PIN bấm từng chữ số, bao gồm 0; tool kiểm tra chấm PIN đã nhận sau từng phím và chờ chuyển sang bước tiếp theo. Nếu khung PIN đang có ký tự, tool dừng để tránh nhập lặp. Nếu website đã mở Thêm Tài Khoản, ba bước cài PIN ban đầu được bỏ qua; bước nhập PIN để thêm ngân hàng vẫn chạy. Bàn phím/độ dài khác mẫu cần kiểm tra và bổ sung theo HTML thực tế.

Chế độ Điền đăng ký chỉ chạy ba ô tài khoản, mật khẩu, họ tên; không yêu cầu số điện thoại ở chế độ này và không bấm Đăng ký. Chế độ Thêm ngân hàng chạy 19 bước, bắt đầu từ Tài Khoản và kết thúc điền SĐT. Mỗi bước vẫn nghỉ **2 giây**. Có thể khôi phục tiến trình dài qua tải lại trong cùng tab/origin như các site khác.

Tên bank từ HTML cung cấp đã được nối vào mã cũ: MB → MB BANK; VCB → Vietcom Bank; CTG → Vietin Bank(VTB); TCB → TECHCOMBANK(TCB); ACB → ACB; LPB → LIENVIETPOSTBANK(LPB), v.v. Không chọn thay ngân hàng khi không tìm thấy mục trùng.

Bản 2.3 đã bổ sung route xác nhận từ Chrome Inspect như phần đầu tài liệu. Tool vẫn thao tác qua giao diện và chờ request của trang. Phần nhập OTP hoặc xác nhận tài khoản/SĐT cần người dùng thực hiện khi được yêu cầu; tool quan sát kết quả. Nếu chưa tìm được nút do lớp CSS khác, có thể gán lại riêng từng phần tử trong Cấu hình.

## Luồng QQ88

Định dạng data và bảng 65 mã bank giữ nguyên. Ví dụ MB → MB BANK trên QQ88, VCB → VIETCOM BANK, AGRI → AGRIBANK, TCB → TECHCOM BANK. Tool đối chiếu tên và mã riêng của QQ88 từ danh sách đã cung cấp; không lấy ID của site cũ làm mã API QQ88, và không chọn ngân hàng đầu tiên khi thiếu mục phù hợp.

Sau các bước Đăng ký → Tài khoản → Rút tiền:

1. Bấm `.withdraw-bkadd` để mở Thêm ngân hàng.
2. Bấm `div.inputBase[data-label-name="ngân hàng"]` để mở danh sách. Thuộc tính disabled trên div tùy biến không phải trạng thái khóa của input native; tool không xóa thuộc tính này.
3. Chọn tên ngân hàng theo mã bank trong data.
4. Gõ từng ký tự vào `input[name="bankCard"]` từ cột numberbank.
5. Gõ từng ký tự vào `input[name="customBankBranch"]` từ cột cyti.
6. Gõ cùng giá trị pinwithdraw từng ký tự vào `input[type="password"][name="withdraw"]` và `input[type="password"][name="withdrawT"]`.
7. Kiểm tra hai ô PIN trùng nhau và đúng data, rồi mới bấm `span.am-button.btn-success` có chữ Xác nhận. Nếu thiếu một ô, hai ô trỏ cùng phần tử hoặc PIN không trùng thì tool chưa gửi form.

Luồng QQ88 điền PIN ngay trên form thêm ngân hàng; không gọi API đặt PIN của luồng HI88. Cột pinwithdraw bắt buộc có giá trị hoặc được tự sinh nếu để trống và bật tự sinh PIN. Bước điền city bao gồm điền cả hai ô PIN, sau đó nghỉ 2 giây rồi mới tới bước Xác nhận. Tác vụ đầy đủ vẫn có 10 bước, thêm ngân hàng riêng có 8 bước.

Chưa có URL/phản hồi lưu ngân hàng QQ88 trong file đầu vào. Mặc định tool chỉ báo **đã bấm Xác nhận**, không khẳng định đã lưu thành công. Bạn kiểm tra thông báo trên website. Nếu khai báo routes[2] đúng trong JSON cấu hình QQ88, tool sẽ đợi xác nhận từ phản hồi đó. Danh sách lựa chọn QQ88 chưa có outerHTML từng mục, nên tool hỗ trợ các phần tử option, am-list-item và text tương ứng; cần thử trên website thật để xác nhận cấu trúc danh sách.

## Hai ô mật khẩu rút

Tool xác định đủ hai ô nhập riêng biệt trong cùng form trước khi gõ. Nhận diện tên trường/nhãn xác nhận, đồng thời hỗ trợ form có đúng hai ô mật khẩu mà không có tên trường chuẩn. Theo HTML thực tế, ưu tiên `input[formcontrolname="newPassword"]` cho PIN và `input[formcontrolname="confirm"]` cho ô xác nhận trong cùng form. `confirmPassword` là tên trường trong payload của website, khác tên `confirm` trên giao diện. Cấu hình mặc định cũ dùng confirmPassword được tự chuyển sang cấu hình mới; cấu hình tự chọn của người dùng được giữ nguyên. Hai ô phải khác nhau, được điền cùng giá trị `pinwithdraw` từ data từng ký tự và được kiểm tra bằng nhau ngay trước khi bấm Gửi đi. Phản hồi `Code: 200`, `Error: []`, `Result: null` được nhận diện thành công; không yêu cầu Result phải là true. Nếu trỏ cùng phần tử hoặc giá trị không trùng, tool dừng và chưa gửi. Có thể gán lại riêng từng ô trong Cấu hình.

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

**Sau mỗi bước đều nghỉ 2 giây**, kể cả bước cuối. Tốc độ gõ mặc định 70 ms/ký tự, có thể chỉnh 30–150 ms. Tạm dừng sẽ dừng gõ và dừng tính khoảng nghỉ. Dừng sẽ hủy các thao tác đang chờ, nhưng không hoàn tác hành động website đã nhận trước đó.

Nếu website đã có PIN và mở thẳng form ngân hàng, tool giữ PIN hiện tại, không đặt lại PIN. Data nhiều dòng được xử lý theo thứ tự; giữa hai dòng tool dừng để bạn đăng xuất/chuyển đúng tài khoản hoặc mở form cho dòng kế tiếp rồi nhấn Tiếp tục. Tool không tự tạo phiên tài khoản riêng cho từng dòng.

## Tiếp tục sau khi trang tải lại

1. Mở lại dấu trang viptool sau khi đăng ký làm website tải lại.
2. Tool hiển thị tiến trình và data đã giữ trong tab.
3. Nếu chưa nhận được xác nhận của server trước khi tải lại, kiểm tra tài khoản trên website đã đăng nhập đúng rồi nhấn Đã kiểm tra web, tiếp tục.
4. Tool chờ **2 giây** sau khi bạn chọn Tiếp tục, rồi bắt đầu từ Tài khoản; giữ nguyên username/mật khẩu/thành phố/PIN đã tự sinh và không bấm đăng ký lần nữa. Khoảng nghỉ giữa các bước khác vẫn là **2 giây**.

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

Bản 2.2 tái hiện lỗi sau đúng sáu số PIN đầu bằng module 2.1; kiểm tra hàng cũ được xóa để xác nhận, hàng xác nhận riêng không có nhãn, hàng xác nhận hiện chậm, hai hàng trống nhưng có con trỏ ở hàng đang nhận PIN, đúng 18 phím cho ba lượt, cấu hình chung trên máy mới, lưu lại sau tải trang và cập nhật cấu hình cũ một lần. Đã chạy lại kiểm tra chạm/click, gán bàn phím riêng, nhập PIN độc lập và xuất/nhập JSON. Các kiểm tra dùng trang mẫu, chưa xác nhận phiên website thật.

Bản 2.1 đã kiểm tra cấu hình đầu vào được cung cấp, phím chỉ nhận touchend, phím chỉ nhận click, phím nhận cả hai kiểu mà không nhập trùng; ba lượt PIN đủ 18 phím; bàn phím tùy chỉnh và gán riêng số 0–9; nhập PIN độc lập theo dòng data; xuất/nhập JSON giữ kiểu bấm và selector; Dừng hủy click dự phòng đang đợi. Các kiểm tra chạy bằng Chromium với chế độ cảm ứng trên trang mẫu, chưa xác nhận trên phiên website thật.

Bản 2.0 kiểm tra nội dung nút đăng ký có handler riêng, click lan lên button đúng một lần, button ngoài form, các li PIN nằm trong div riêng, hai hàng mới/xác nhận cùng hiện, và bản sao PIN ngoài màn hình. Khoảng nghỉ vẫn 2 giây. Cấu hình → Kiểm tra trang hiển thị trạng thái nút và số nhóm/số ô PIN, không hiển thị giá trị mật khẩu. Đã thử đọc cấu trúc tab LDPlayer thực tế nhưng kênh DevTools không trả lời; kết quả kiểm tra này dựa trên các trang mẫu, chưa xác nhận phiên website thật.

Bản 1.9 tái hiện lỗi Có nhiều phần tử cho Nút đăng ký của bản 1.8 trên trang mẫu rồi xác nhận đã sửa: nút trùng ID ngoài form, bản sao ẩn trong form, selector lưu cũ quá rộng, nút ở footer cùng form và nút trùng bị disabled. Hai nút thực sự cùng bật trong cùng form vẫn bị chặn; đã chạy đủ luồng otpokvip sau các trường hợp chọn nút hợp lệ.

Bản 1.8 kiểm tra nút button#insideRegisterSubmitClick theo HTML cung cấp, chuyển cấu hình nút cũ, chờ disabled, ô PIN li đầu tiên sau nhãn, bỏ nhóm PIN ẩn và dừng khi có nhiều nhóm cùng hiện. Đã chạy lại luồng đầy đủ otpokvip, điền form, bàn phím ba lượt PIN, tạm dừng/dừng và khôi phục tiến trình.

Có trang mẫu `sandbox.html` để kiểm tra luồng gõ, bấm và xác nhận bằng các phản hồi giả lập ngay trong trình duyệt. Trang này không tạo tài khoản hoặc liên kết ngân hàng thật.

Bản 1.6 kiểm tra thêm otpokvip theo thứ tự 20 bước, 16 bước ngân hàng, 3 bước điền đăng ký; ba lượt bàn phím web có số 0, bank MB/VCB/TCB/CTG, dừng/tạm dừng trong lúc bấm phím, chặn gõ trùng PIN đang nhập và khôi phục sau bước 18. Kiểm tra 1.5 gồm hai ô withdraw/withdrawT, ký tự gõ, PIN tự sinh, chặn xác nhận khi thiếu ô hoặc hai giá trị khác nhau; đã kiểm tra bằng Chromium cục bộ: bảy hồ sơ site; QQ88 với MB/VCB/AGRI/TCB; PIN trong form QQ88; CAPTCHA trước và sau bấm đăng ký, chờ hơn hai phút, chỉ thử lại sau khi người dùng hoàn tất khi có phản hồi từ chối CAPTCHA; dừng trong lúc chờ CAPTCHA. Đã chạy lại kiểm tra hồi quy luồng cũ: cả ba chế độ; luồng thật trên DOM mẫu đủ 10 bước; xác nhận ba phản hồi; gõ từng ký tự; giữ số 0 đầu; giữ PIN đã có; tạm dừng/tiếp tục; hủy khi dừng; dừng khi API trả lỗi nghiệp vụ; bookmarklet module mở lại; khung rộng 320, 360, 390, 430 px. Chưa kiểm thử tài khoản thật trên website hoặc trên từng thiết bị điện thoại thực tế.

Bookmarklet cần trình duyệt hỗ trợ JavaScript URL và ES module. CSP của website có thể chặn việc nạp mã. Tool không chạy trong giao diện native của app hoặc iframe khác origin. Các sự kiện từ bookmarklet là sự kiện JavaScript, không phải sự kiện bàn phím được trình duyệt đánh dấu `isTrusted`.

CAPTCHA được chờ theo mô tả ở trên và cần người dùng tự giải. OTP, điều khoản và các yêu cầu bổ sung của trang cần người dùng xử lý. Nút bị khóa được chờ để trang tự bật; tool không gỡ `disabled`. Nếu website tải lại toàn bộ tài liệu, giao diện bookmarklet sẽ mất. Gọi lại dấu trang viptool trong cùng tab/cùng tên miền, rồi chọn Tiếp tục. Data và giá trị tự sinh được khôi phục. Nếu trang tải lại trong lúc gửi Đăng ký/Gửi đi/Lưu, cần kiểm tra thao tác đã thành công trên website trước khi xác nhận tiếp tục; tool không tự gửi lại thao tác đó. Bookmarklet không có quyền tự chèn lại mã sau khi tải tài liệu mới; muốn tự mở hoàn toàn cần cơ chế chạy theo trang như userscript/extension.

## Các file

- `e.js`: ES module 2.3 cho bookmarklet ngắn; URL mới để tránh cache bản cũ.
- `d.js`: tên module 2.2, được giữ tương thích.
- `c.js`: tên module 2.1, được giữ tương thích.
- `b.js`: tên module 2.0, được giữ tương thích.
- `a.js`: tên module 1.9, được giữ tương thích.
- `z.js`: tên module 1.8, được giữ tương thích.
- `y.js`: tên module 1.7, được giữ tương thích.
- `x.js`: tên module 1.6, được giữ tương thích.
- `w.js`: tên module 1.5, được giữ tương thích.
- `u.js`: tên module 1.4, được giữ tương thích.
- `t.js`: tên module 1.3, được giữ tương thích.
- `s.js`: tên module 1.2, được giữ tương thích.
- `r.js`: tên module 1.1, được giữ tương thích.
- `v.js`: tên module cũ, được giữ tương thích.
- `viptool.js`: cùng bộ mã ở dạng script tự chạy.
- `index.html`: trang cài đặt.
- `sandbox.html`: trang kiểm tra luồng cũ.
- `sandbox-qq88.html`: trang kiểm tra luồng QQ88 và CAPTCHA giả lập.
- `sandbox-otpokvip.html`: trang mẫu với ba lượt bàn phím PIN, lựa chọn bank và nhập SĐT.
- `bookmarklet.txt`: bookmarklet ngắn.
- `bookmarklet-offline.txt`: bản chứa toàn bộ mã.
- `bank-list.json`: danh sách ngân hàng.
- `otpokvip-keypad-config.json`: cấu hình đã thêm khung bàn phím, các phím số và kiểu chạm/click.

Để đổi nơi lưu mã, đưa các file lên hosting HTTPS, rồi tạo bookmarklet theo URL mới của `e.js`. Trang cài đặt tự lấy URL module theo địa chỉ HTTPS hiện tại. Hosting cần trả JavaScript với MIME phù hợp và cho phép CORS để module được nạp từ website khác. GitHub Pages đã được chọn cho bản này.

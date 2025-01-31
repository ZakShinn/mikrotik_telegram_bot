
# MikroTik Script: Telegram Bot

_Script_ này được sử dụng để điều khiển MikroTik của bạn chỉ bằng cách sử dụng phương tiện truyền thông xã hội Telegram. Có nhiều lệnh để giám sát, thay đổi _hotspot_, xóa người dùng _hotspot_, thêm tài khoản _hotspot_ mới, thay đổi _mật khẩu người dùng _hotspot_, v.v.

# Danh sách nội dung
- [Tuyên bố miễn trừ trách nhiệm](#Tuyên-bố-miễn-trừ-trách-nhiệm)
- [Lịch sử phiên bản](#Lịch-sử-phiên-bản)
- [Cài đặt](#Cài-đặt)
- [Lệnh, Tham số và Hàm](#Lệnh,-Tham-số-và-Hàm)
- [Nguồn](#Nguồn)

# Tuyên bố miễn trừ trách nhiệm
_Script_ này là _mã nguồn mở_. Bạn có thể sửa đổi, thêm hoặc bớt nội dung của _script_ này miễn là nó không vi phạm các điều khoản trong giấy phép _MIT_. _Script_ này **KHÔNG CÓ BẢO HÀNH** khi bạn sử dụng nó. Nếu bạn gặp bất kỳ sự cố nào khi cài đặt hoặc sử dụng _script_ này, vui lòng thảo luận và giải thích cách sự cố xảy ra thông qua tính năng [**Sự cố**](https://github.com/dwichan0905/telegram_bot/issues).

# Sự đóng góp
Những đóng góp cho kho lưu trữ này chỉ giới hạn ở MikroTik Scripts và Documentation. Bạn có thể đóng góp bằng cách Fork kho lưu trữ này, tạo nhánh mới, thực hiện thay đổi và gửi Yêu cầu kéo tới kho lưu trữ này. Hãy mô tả những gì bạn đã thêm và những gì bạn đã thay đổi trong _kho lưu trữ_ này. Đừng quên viết tập lệnh trợ giúp trong ```tg_cmd_help``` để trợ giúp nếu người dùng tập lệnh này quên lệnh cần viết.

# Lịch sử phiên bản
#### 1.3.1 (27 tháng 10 năm 2020)
- thêm lệnh ``/monitoring`` có chức năng theo dõi tốc độ truyền và tốc độ nhận từ giao diện và cũng có thể được sử dụng để theo dõi mức sử dụng CPU và RAM.
- thêm lệnh ``/stop`` để dừng quá trình giám sát hiện đang chạy trên Telegram
video: [YouTube](https://www.youtube.com/watch?v=0HEuqgMZVp4)

#### 1.3 (ngày 8 tháng 10 năm 2020)
- xuất từng tập lệnh thành phiên bản văn bản để có thể đọc trực tiếp mà không cần phải nhập vào Mikrotik
- thêm hàm viết thường `func_lowercase`
- sửa đổi lệnh hotspot, thêm một hàm viết thường để nếu có tham số sử dụng chữ in hoa thì vẫn có thể đọc được (ví dụ, nếu người dùng nhập `/hotspot SesSion CoUnT` thì có thể đọc được và bot sẽ gửi một câu trả lời)
- xóa `tg_cmd_start` và `tg_cmd_hi` (giải thích bên dưới)
- thêm các lệnh thay thế vào tg_getUpdates, nếu người dùng nhập: `/hi`, `/start`, `/hai`, `/halo`, `/hello`, `/help`. Sau đó, nó sẽ chạy tập lệnh `tg_cmd_help`. Nói cách khác `tg_cmd_help` cũng xử lý các lệnh đó
- cũng áp dụng cho `/hotspot`. Nếu người dùng nhập `/hs` thì nó sẽ được chuyển hướng đến `tg_cmd_hotspot`
- sửa đổi tg_cmd_dhcp để sử dụng các hàm viết thường và loại bỏ các tham số không cần thiết
- thêm lệnh cho thuê /dhcp
- thêm lệnh `/interface show all`
  
#### 1.2 (ngày 11 tháng 8 năm 2019)
 1. Đã sửa lỗi khi nhập tập lệnh (lỗi ``đối số mặc định không hợp lệ``)
 2. Cập nhật lệnh trên điểm phát sóng:
 - Thay thế lệnh ``/hotspot users`` bằng ``/hotspot session count``
 - Thay thế lệnh ``/hotspot showall`` bằng ``/hotspot session showall``
 - Thêm lệnh mới: ``/hotspot session deauth-by-mac``, ``/hotspot session deauth-by-ip``, và ``/hotspot session deauth-by-user``
 3. Sửa lỗi lệnh:
 - ``/reboot`` hiện có thể được sử dụng để khởi động lại bộ định tuyến (trễ 30 giây)
 4. Bổ sung điều kiện mới:
 - Sau khi khởi động lại, Router sẽ gửi báo cáo qua Telegram rằng nó đã khởi động lại và ghi lại tất cả lý do tại sao nó làm như vậy trong "Nhật ký quan trọng" (trễ 30 giây sau khi Router hoàn tất khởi động lại).

#### 1.1 (8 tháng 8, 2019)
 1. Phiên bản đầu tiên

## Cài đặt
Trước khi bắt đầu cài đặt, bạn phải có _Mã thông báo truy cập_ cho Telegram Bot và ChatID của nó. Theo [liên kết này (labkom.co.id)](https://labkom.co.id/mikrotik/mikrotik-netwach-monitoring-status-access-point-hotspot-dengan-menggunakan-telegram) để biết hướng dẫn về cách để tạo một bot telegram
Để cài đặt, vui lòng sao chép hoặc tải xuống kho lưu trữ này, sau đó:

 1. Giải nén tệp ZIP bạn đã tải xuống (bỏ qua nếu bạn đã sao chép kho lưu trữ này)
 2. _Tải_ tệp ``telegram_bot.rsc`` lên MikroTik của bạn (có thể thông qua FileZilla FTP, hoặc cũng có thể thông qua WinBox) và lưu vào thư mục chính (root hoặc /) trên MikroTik của bạn.
 3. Sau đó, mở MikroTik Terminal và nhập lệnh sau:
 ``import file-name=telegram_bot.rsc``
 4. Cấu hình cài đặt _bot_ trong ``System > Scripts > tg_config`` bằng cách thay đổi lệnh sau:
 > Điền mã thông báo truy cập Telegram Bot của bạn:
 ``"botAPI"="xxxxxx:xxxxxxxxx-xxxxxxx"``

 > Điền Telegram ChatID của bạn:
 ``defaultChatID"="xxxxxxxxx"``

> Điền một số ChatID của bạn, có thể là ChatID cá nhân hoặc nhóm. Phân cách bằng dấu phẩy:
 ``"trusted"="xxxxxxxxxx, xxxxxxxxxx, -xxxxxxxxx"``

 Sau đó lưu cấu hình.
  
 5. Xong!

## Lệnh, Tham số và Chức năng của chúng
Nhập lệnh sau vào cột _chatting_ bằng _bot_ Telegram của bạn. Mỗi _tham số_ được nhập vào được phân tách bằng một dấu cách, ví dụ ``/interface show``.

| Câu lệnh | Tham số | Chức năng | Ví dụ |
|-----------|--------------|-------|-----|
| ``/help`` | | Hiển thị danh sách các hàm có thể thực thi. | |
| ``/start`` | | Hiển thị danh sách các hàm có thể thực thi. | |
| ``/cpu`` | | Hiển thị ID bộ định tuyến, tải CPU, thời gian hoạt động và tổng RAM đã sử dụng. | |
| ``/dhcp`` | ``lease`` | hiển thị tất cả các chi tiết trên DHCP Lease (có thể lỗi nếu nhiều thiết bị) | ``/dhcp lease`` |
| ``/dhcp`` | ``client release <interface>`` | Giải phóng máy khách dhcp trên một giao diện cụ thể| ``/dhcp client release ether1`` |
| ``/interface`` | ``show`` | Hiển thị trạng thái kết nối giữa các cổng Ethernet trên MikroTik | ``/interface show`` |
| ``/interface`` | ``show all`` | Hiển thị trạng thái kết nối của tất cả các giao diện trên MikroTik | ``/interface show all`` |
| ``/hotspot`` | ``help`` | Hiển thị trợ giúp chi tiết cho một lệnh `/hotspot` | ``/hotspot help`` |
| ``/hotspot`` | ``session count`` | Hiển thị số lượng người dùng đang hoạt động | ``/hotspot session count`` |
| ``/hotspot`` | ``session showall`` | Hiển thị tất cả thông tin chi tiết của người dùng đang hoạt động bắt đầu từ Tên người dùng đến Thời gian hoạt động (trừ mật khẩu) | ``/hotspot session showall`` |
| ``/hotspot`` | ``session deauth-by-user <username>`` | Thu hồi thiết bị _session_ theo Tên người dùng | ``/hotspot session deauth-by-user telecomadmin`` |
| ``/hotspot`` | ``session deauth-by-ip <ip>`` | Thu hồi thiết bị _session_ theo Địa chỉ IP | ``/hotspot session deauth-by-ip 192.168.1.2`` |
| ``/hotspot`` | ``session deauth-by-mac <mac address>`` | Thu hồi thiết bị _session_ theo Địa chỉ MAC | ``/hotspot session deauth-by-mac AB:CD:EF:01:23:45`` |
| ``/hotspot`` | ``add <username> <password>`` | Thêm người dùng điểm phát sóng mới | ``/hotspot add telecomadmin admintelecom`` |
| ``/hotspot`` | ``delete <username>`` | Xóa vĩnh viễn người dùng điểm phát sóng | ``/hotspot delete telecomadmin`` |
| ``/hotspot`` | ``disable <username>`` | Tắt hoặc vô hiệu hóa người dùng điểm phát sóng | ``/hotspot disable telecomadmin`` |
| ``/hotspot`` | ``enable <username>`` |Bật người dùng điểm phát sóng bị vô hiệu hóa | ``/hotspot enable telecomadmin`` |
| ``/hotspot`` | ``change-password <username> <password baru>`` | Thay đổi mật khẩu người dùng điểm phát sóng | ``/hotspot change-password telecomadmin p4ssw0rdny4`` |
| ``/ping`` | | Ping Google DNS | ``/ping`` |
| ``/monitoring`` | ``interface <interface>`` | Giám sát interface | ``/monitoring interface wlan1`` |
| ``/monitoring`` | ``cpu`` | Theo dõi việc sử dụng CPU trên bộ định tuyến | ``/monitoring cpu`` |
| ``/monitoring`` | ``ram`` | Theo dõi việc sử dụng RAM trên bộ định tuyến | ``/monitoring ram`` |
| ``/monitoring`` | ``memory`` | Theo dõi việc sử dụng bộ nhớ trên bộ định tuyến | ``/monitoring memory`` |
| ``/ping`` | ``to <ip address>`` | Ping một địa chỉ IP cụ thể | ``/ping to 127.0.0.1`` |
| ``/public`` | | Hiển thị DNS động và IP công cộng trên MikroTik của bạn | ``/public`` |
| ``/enablehotspot`` | | Bật tất cả các chức năng điểm phát sóng | ``/enablehotspot`` |
| ``/disablehotspot`` | | Tắt tất cả các chức năng điểm phát sóng | ``/disablehotspot`` |
| ``/forceupdateddns`` | | Buộc cập nhật DNS động | ``/forceupdateddns`` |
| ``/reboot`` | | Khởi động lại MikroTik (tạm dừng 30 giây trước khi khởi động lại) | ``/reboot`` |

> **Lưu ý**: để có thể chạy các lệnh ``/disablehotspot``, ``/enablehotspot`` và ``/interface show``, vui lòng tự cấu hình điểm phát sóng nào sẽ được "tự động hóa" trong tập lệnh ` ``tg_cmd_disablehotspot``, ``tg_cmd_enablehotspot`` và ethernet nào sẽ được hiển thị trong ``tg_cmd_interface``.

# Nguồn

 - [Labkom](https://labkom.co.id)
 - [Diễn đàn MikroTik (EN)](https://forum.mikrotik.com)
 - [API Telegram (EN)](https://api.telegram.org)
 - [Github gốc](https://github.com/dwichan0905/telegram_bot)
   


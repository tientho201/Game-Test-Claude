# 📊 Báo cáo dự án: Bean Rush 3D

[English](REPORT.md) · **Tiếng Việt** · Quay lại [README](README.vi.md)

Báo cáo này trình bày yêu cầu ban đầu, cách xây dựng, cách kiểm thử và chi phí. Toàn bộ dự án do Claude Code thực hiện trong một phiên làm việc trên cloud ngày **14/10/2026**.

## 1. Tóm tắt

| Hạng mục | Kết quả |
|---|---|
| Sản phẩm | Game đua vượt chướng ngại vật 3D phong cách Fall Guys, chơi được trên trình duyệt |
| Công nghệ | HTML + CSS + JavaScript thuần + Three.js r128 (không cần build) |
| Cài đặt | 1 bước: mở `index.html` bằng trình duyệt (chạy được cả khi offline) |
| Dung lượng code | `index.html`: 2.112 dòng, khoảng 118 KB (toàn bộ game) |
| Thư viện | `vendor/three.min.js`: khoảng 603 KB (giấy phép MIT) |
| Lỗi console khi kiểm thử | 0 (cả máy tính và điện thoại) |
| Tổng chi phí (đồng hồ đo của phiên, xem mục 7) | **≈ 5,68 USD**, số liệu chụp lúc game đã hoàn thành |

## 2. Các prompt

Nguyên văn các tin nhắn người dùng gửi cho Claude.

**Prompt 1: yêu cầu chính**

> Hãy xây dựng một [trò chơi thể loại Fall Guys 3D / hoặc một trang dashboard tài chính tương tác] hoàn chỉnh bằng HTML/TailwindCSS/JS thuần (hoặc Three.js chạy mượt 60fps).
> Yêu cầu bắt buộc:
> * Có giao diện người dùng hoàn chỉnh, trực quan, không được dùng placeholder
> * Tự tích hợp sẵn logic điều khiển, hiệu ứng chuyển động và xử lý va chạm.
> * Xuất toàn bộ mã nguồn ra dưới dạng file có thể chạy ngay được kèm hướng dẫn setup trong 1 bước.

**Prompt 2:**

> làm sao tôi thấy game , chạy local được không

**Prompt 3:**

> tạo pull request cho nhánh qua nhánh main . Tạo cho tôi thêm readme report , báo cáo cho tôi về prompt, chi phí , các thứ cho tôi . các readme là tiếng anh nhưng vẫn có bản tiếng việt trỏ qua cho người dùng đọc

## 3. Cách hiểu yêu cầu

| Quyết định | Lựa chọn | Lý do |
|---|---|---|
| Game hay dashboard | **Game 3D kiểu Fall Guys** | Prompt cho chọn một trong hai; game thể hiện trực tiếp cả 3 yêu cầu bắt buộc (điều khiển, chuyển động, va chạm). |
| Đồ hoạ | **Three.js** | Prompt cho phép dùng thay Tailwind; game 3D thật cần WebGL. |
| Giao diện | CSS tự viết thay cho Tailwind | Tailwind qua CDN cần mạng; CSS thuần giúp game chạy offline hoàn toàn. |
| Vật lý | Tự viết từ đầu | Không cần thư viện vật lý; giữ dự án gọn trong một file và mỗi chướng ngại vật hoạt động đúng như thiết kế. |
| Cách đóng gói thư viện | Kèm sẵn `three.min.js` + dự phòng CDN | "Chạy ngay được" thì không nên phụ thuộc mạng. Máy build không truy cập được cdnjs (lỗi 403) nên file được lấy từ gói npm chính thức `three@0.128.0`. |
| Ngôn ngữ trong game | Tiếng Việt | Người dùng viết bằng tiếng Việt. |
| Tài nguyên | Tạo hoàn toàn bằng code | Texture vẽ bằng `<canvas>`, âm thanh tổng hợp bằng Web Audio, nên không có file ảnh hay file âm thanh nào có thể bị thiếu. |

## 4. Những gì đã xây dựng

**Đường đua (7 chặng, dài khoảng 330 m)**: cối xay gió, bệ trượt ngang trên chất nhờn, cửa bí ẩn (mỗi tường 2/5 cửa thật), 136 viên gạch lục giác rơi, cầu búa con lắc, 3 đĩa xoay, dốc bóng lăn và cổng đích. Có 7 điểm hồi sinh.

**Các hệ thống trong `index.html`**

| Hệ thống | Chức năng |
|---|---|
| Va chạm | Capsule (hai khối cầu) với hộp xoay, trụ đứng và khối cầu; nhận biết dốc đi được; giải va chạm 2 vòng mỗi bước |
| Bệ di chuyển | Nhân vật đứng trên bệ trượt/xoay được mang theo dựa vào vị trí trước và sau của bệ |
| Đánh văng | Lực đẩy tính từ vận tốc tại điểm va chạm, kèm trạng thái choáng có hoạt ảnh lắc lư |
| Di chuyển | Gia tốc khác nhau trên mặt đất/trên không, bộ đệm nhảy (0,14 s), coyote time (0,1 s), lao người và thời gian hồi |
| Mô phỏng | Thời gian khung hình được chia thành các bước con ~120 Hz (mượt ở 60/120/144 Hz) |
| Bot AI (11) | Waypoint, phát hiện mép vực, chờ bệ trượt, canh búa, né bóng, nhớ cửa giả, tự gỡ kẹt |
| Hiệu ứng | Hạt hiệu ứng instanced (pháo giấy, bụi, chất nhờn), co giãn nhân vật, biển chất nhờn và bầu trời bằng shader |
| Camera | Bay giới thiệu theo đường cong, camera xoay quanh nhân vật, tự kéo lại gần khi bị tường che |
| Giao diện | Menu tuỳ chỉnh nhân vật, hướng dẫn, cài đặt, đếm ngược, HUD, thông báo, tạm dừng, kết quả, kỷ lục |
| Điều khiển | Bàn phím, chuột có khoá con trỏ (hoặc kéo chuột), joystick và nút cảm ứng |
| Âm thanh | Hiệu ứng tổng hợp và nhạc nền tự sinh |

## 5. Tiến trình (giờ UTC)

| Thời gian | Công việc |
|---|---|
| 16:13 | Bắt đầu phiên; repo đang trống |
| 16:15–16:20 | Đóng gói Three.js (cdnjs bị chặn → lấy từ npm) |
| 16:20–16:30 | Viết toàn bộ game trong `index.html` |
| 16:30–16:39 | Kiểm thử tự động trên trình duyệt, xem ảnh chụp, sửa lỗi (xem mục 6) |
| 16:40 | Commit đầu tiên được đẩy lên nhánh `claude/hopeful-gauss-459e8k` |
| 16:44 | Đăng bản chơi thử dưới dạng artifact riêng tư trên Claude |
| 16:48 | Chụp số liệu sử dụng dùng cho mục 7 |
| sau 16:49 | Viết báo cáo này, README Anh/Việt, tạo pull request |

## 6. Kiểm thử

Kiểm thử dùng Chromium không giao diện (Playwright) với WebGL giả lập bằng phần mềm. Mỗi lần chạy sẽ mở trang, đi qua menu, phần giới thiệu, đếm ngược, chơi thử, rồi chụp màn hình và ghi lại lỗi console. Một cổng kiểm thử (`window.__beanRush.update`) cho phép tua nhanh nguyên một trận 150 giây mà không cần vẽ hình.

**Các lỗi phát hiện và đã sửa**

| # | Lỗi phát hiện | Cách sửa |
|---|---|---|
| 1 | Bot quá chậm: người về đích đầu tiên mất ~115 giây, bị kẹt ở búa và bóng | Bot biết canh nhịp búa, né bóng, nhảy qua thanh quét ổn định hơn. Giờ người về đầu chỉ mất ~55 giây |
| 2 | Menu: bot che mất camera; logo bị cắt ở màn hình 720p | Nhân vật của bạn đứng riêng phía trước; ẩn tên bot ở menu; bố cục menu gọn hơn |
| 3 | Lúc xuất phát: nhân vật bị hàng bot phía sau che khuất | Nhân vật xuất phát ở hàng cuối |
| 4 | Camera lọt ra sau tường cửa, màn hình chỉ thấy bức tường | Camera tự kéo lại gần khi có tường/cửa chắn giữa camera và nhân vật |
| 5 | Điện thoại: đồng hồ và bảng "Đã về đích" đè lên nhau | Bố cục HUD gọn riêng cho màn hình hẹp hơn 820 px |
| 6 | Menu trên điện thoại: hạt đậu bị logo che | Chỉnh góc camera cho màn hình dọc |
| 7 | 136 viên gạch lục giác bị vẽ thêm một lần ở bước tính bóng | Gạch không đổ bóng nữa |

**Số liệu cuối cùng**

| Hạng mục | Kết quả |
|---|---|
| Lỗi console / lỗi trang | 0 |
| Mô phỏng nguyên trận | Bot về đích trong khoảng ~55 đến ~125 giây; trận kết thúc đúng luật |
| Thời gian xử lý logic mỗi khung hình | ~0,8 ms (12 nhân vật, ~200 vật thể va chạm) |
| Số lệnh vẽ mỗi khung hình | ~690, đã tính cả bước tính bóng |
| Tốc độ khung hình | **Chưa đo trên card đồ hoạ thật.** Máy kiểm thử chỉ có dựng hình bằng phần mềm (~5 FPS). Nếu máy yếu, hãy giảm *Chất lượng đồ hoạ* hoặc tắt *Đổ bóng* trong Cài đặt |

## 7. Chi phí và mức sử dụng

Số liệu lấy từ đồng hồ đo của chính phiên làm việc trên cloud (`get_session`), chụp lúc **16:48 UTC**, sau khi game đã được viết, kiểm thử, đẩy lên GitHub và đăng bản chơi thử. Phần viết báo cáo này và tạo pull request diễn ra sau thời điểm đó, nên tổng cuối cùng sẽ cao hơn một chút.

| Chỉ số | Giá trị |
|---|---|
| **Chi phí (USD)** | **5,68 USD** |
| Token đầu ra (code, văn bản, lệnh công cụ) | 149.749 |
| Token đọc từ cache | 4.795.555 |
| Token ghi vào cache | 215.824 |
| Token đầu vào không qua cache | 554 |
| Ngữ cảnh đã dùng | 259.720 / 1.000.000 token |

**Cách hiểu các số này**

- **Token đọc từ cache chiếm phần lớn.** Mỗi bước dùng công cụ, Claude đọc lại toàn bộ cuộc hội thoại tới thời điểm đó; nhờ prompt caching, việc đọc lại rẻ hơn nhiều so với đầu vào mới.
- **Token đầu ra chủ yếu là bản thân game.** File `index.html` 118 KB cùng các lần chỉnh sửa và script kiểm thử chiếm phần lớn 150 nghìn token đầu ra.
- **Con số USD là ước tính.** Đây là con số đồng hồ của phiên báo cáo, tính theo giá API. Nếu bạn dùng gói thuê bao Claude thì mức sử dụng này được trừ vào hạn mức của gói chứ không tính thành hoá đơn riêng. Hãy xem trang sử dụng (usage) trong tài khoản để có con số chính xác.

## 8. Hạn chế đã biết

- Giao diện trong game chỉ có tiếng Việt (tài liệu thì có cả hai thứ tiếng).
- Chỉ có một vòng đấu; không có chơi mạng. 11 đối thủ còn lại là bot AI.
- Chữ dùng font *Baloo 2* từ Google Fonts; khi offline trình duyệt sẽ dùng font hệ thống.
- Chưa đo được 60 FPS trên phần cứng thật (xem mục 6).

## 9. Cách chạy

Mở `index.html` bằng trình duyệt. Hướng dẫn đầy đủ: [README.vi.md](README.vi.md) · [README.md](README.md).

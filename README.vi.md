# 🫘 Bean Rush 3D

[English](README.md) · **Tiếng Việt**

Game đua vượt chướng ngại vật 3D phong cách **Fall Guys**, viết bằng HTML + CSS + JavaScript thuần và **Three.js**. Chạy trực tiếp trên trình duyệt, không cần build, không cần server.

🤖 Được tạo bằng **Claude Code** sử dụng **Claude Opus 5.5** (mức suy luận: **high**).

▶️ **Chơi online:** https://claude.ai/artifact/CDUjFkpVJEqNV5n8Ti5xNq (link riêng tư; chủ sở hữu có thể chia sẻ qua menu Share trên trang)

📊 **Báo cáo dự án** (prompt, quy trình, kiểm thử, chi phí): [REPORT.vi.md](REPORT.vi.md) · [English](REPORT.md)

## ▶️ Cài đặt trong 1 bước

**Mở file `index.html` bằng trình duyệt** (nhấp đúp vào file). Vậy là chơi được!

> Three.js đã được đóng gói sẵn trong `vendor/three.min.js`, nên game chạy được cả khi **không có mạng**. Nếu thiếu file đó, game sẽ tự tải Three.js từ CDN.
> Muốn chạy qua server (không bắt buộc): `npx http-server .` rồi mở `http://localhost:8080`.

## 🎮 Điều khiển

| Phím | Hành động |
|---|---|
| `W A S D` / phím mũi tên | Di chuyển (theo hướng camera) |
| `Space` | Nhảy (có bộ đệm nhảy và "coyote time") |
| `Shift` / `E` | Lao người về phía trước |
| Chuột | Bấm vào màn hình để khoá chuột, rê để xoay camera |
| `Esc` / `P` | Tạm dừng |
| Điện thoại | Joystick ảo + nút **NHẢY** / **LAO**, vuốt màn hình để xoay camera |

## 🏁 Luật chơi

12 hạt đậu (bạn + 11 bot AI) cùng đua. **Chỉ 8 người về đích đầu tiên được đi tiếp**, và có giới hạn 3 phút. Rơi xuống biển chất nhờn hồng thì bạn hồi sinh ở cột mốc gần nhất.

Đường đua gồm 7 chặng:

1. **Cối xay gió**: thanh quét xoay ở tầm thấp, phải nhảy qua.
2. **Bậc thang lơ lửng**: bệ trượt ngang trên biển chất nhờn.
3. **Cửa bí ẩn**: mỗi bức tường có 5 cửa, chỉ 2 cửa vỡ được (ngẫu nhiên mỗi trận).
4. **Gạch rơi**: 136 viên gạch lục giác rung rồi sụp sau khi bị giẫm, hồi lại sau 6 giây.
5. **Búa tạ**: cầu hẹp với 4 búa con lắc đánh văng người chơi.
6. **Đĩa xoay**: 3 đĩa quay kèm thanh chắn, nhân vật bị cuốn theo chiều quay.
7. **Dốc bóng lăn**: leo dốc, né bóng khổng lồ, rồi lao qua cổng đích.

## ✨ Tính năng

- **Vật lý tự viết**: va chạm capsule với hộp xoay (OBB), trụ và cầu; nhân vật đứng trên bệ động (trượt, xoay) sẽ di chuyển theo bệ; bị đánh văng dựa trên vận tốc điểm tiếp xúc; các hạt đậu va chạm và đẩy nhau; mô phỏng chia bước con 120 Hz nên chạy mượt trên mọi tần số màn hình.
- **Bot AI**: đi theo waypoint, tự nhảy qua hố và thanh quét, chờ bệ trượt tới mới nhảy, canh nhịp búa, né bóng, nhớ cửa giả và ưu tiên cửa đã bị phá.
- **Hiệu ứng**: hạt đậu co giãn (squash & stretch), tay chân chuyển động theo trạng thái, pháo giấy, bụi, bắn chất nhờn, cửa vỡ bay, biển chất nhờn và bầu trời dựng bằng shader, camera bay giới thiệu màn chơi, camera tự kéo lại gần khi bị tường che.
- **Giao diện đầy đủ**: menu tuỳ chỉnh nhân vật (tên, 10 màu, 5 phụ kiện), bảng hướng dẫn, đếm ngược, HUD (đồng hồ, thanh tiến độ với vị trí từng người, thứ hạng, số người đã về đích), thông báo từng chặng, menu tạm dừng, bảng kết quả, lưu kỷ lục.
- **Âm thanh tổng hợp bằng Web Audio**: hiệu ứng và nhạc nền tự sinh, không cần file âm thanh.
- **Cài đặt**: độ nhạy chuột, đảo trục, chất lượng đồ hoạ, đổ bóng, âm lượng, nhạc nền, hiện FPS. Cài đặt được lưu lại trên trình duyệt.

## 📁 Cấu trúc

```
index.html          ← toàn bộ game (HTML + CSS + JS)
vendor/three.min.js ← Three.js r128 (MIT), giúp chạy offline
README.md           ← README tiếng Anh
README.vi.md        ← file này (tiếng Việt)
REPORT.md           ← báo cáo dự án (tiếng Anh)
REPORT.vi.md        ← báo cáo dự án (tiếng Việt)
```

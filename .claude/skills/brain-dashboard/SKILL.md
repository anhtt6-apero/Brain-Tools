---
name: brain-dashboard
description: Sinh dashboard HTML trực quan cho bộ não — hiển thị ai đang làm gì, tiến độ cả team, research gần đây, bản đồ đối thủ. Kích hoạt khi user nói "xem tiến độ", "vẽ dashboard", "cập nhật dashboard". Dùng style effective-html.
---

# Skill: brain-dashboard — sinh dashboard HTML

Tạo `brain/dashboard/index.html` trực quan, đẹp, tự chứa (1 file, không cần server).

## Nguồn dữ liệu (quét lúc generate)
- `brain/state/*.json` — gom tiến độ TẤT CẢ thành viên.
- `brain/people/*.md` — tên/vai trò.
- `brain/knowledge/competitors/*.md` — để vẽ bản đồ đối thủ.
- `brain/research/*/report.md` — danh sách report gần đây.
- `brain/knowledge/features/*/questions/*.md` — câu hỏi escalate (lọc `status: open`) — **NGUỒN đếm câu chờ** (quét trực tiếp).
- `brain/ASK-LOG.md` — skim nhanh hỏi-đáp (bổ trợ, KHÔNG phải nguồn đếm).

## Nội dung dashboard
1. **HERO — "Dây chuyền tư duy" (động):** mô phỏng pipeline real-time. Bên trái là **task đang chạy**
   (lấy từ `current_focus` / feature in_progress của user); ở giữa là **bộ não đang nghĩ** (vòng tròn
   pulse + ripple) với 2 dây có **thanh chạy** (gradient động chạy vào & ra); bên phải **kết quả nhả ra
   từng cái một** (5 bước research-competitor reveal so le, lặp vô hạn). Đây là điểm nhấn user yêu cầu.
2. **Header:** tên bộ não + ngày generate.
3. **Summary band:** số tính năng đang làm, đối thủ, report, thành viên, **câu hỏi chờ PO**.
3b. **Band "❓ Câu hỏi đang chờ":** quét MỌI `features/*/questions/*.md` lọc `status: open` (nguồn trực tiếp, KHÔNG dựa ASK-LOG count).
    - User đang xem (git email) là `po` của feature → **card ĐỎ "PO cần trả lời (N)"**: feature · ai hỏi (`asker`) · tiêu đề · trích "AI đã thử gì" · tuổi câu (số ngày từ `opened`) · link file.
    - Vai trò khác → list xám "Câu hỏi đang chờ (N)" read-only (để biết đừng hỏi trùng).
    - Cảnh báo "answered nhưng chưa flow-back" (`status: answered` + `flowed_back: false`) → nhắc PO đưa đáp vào feature.md mục 7.
4. **Tiến độ theo người:** mỗi thành viên 1 card — focus hiện tại + progress bar các tính năng.
5. **Research gần đây:** danh sách report (tiêu đề, ngày, người, link file).
6. **Bản đồ đối thủ:** diagram SVG (style `html-diagram`) — app mình ở giữa, đối thủ xung quanh.
7. **Knowledge stats:** số file theo loại.

> HERO làm bằng CSS animation thuần (keyframes `run` cho thanh chạy, `think`/`ripple` cho bộ não,
> `reveal` so le cho kết quả). Không cần JS ngoài nút theme. Khi có nhiều task đang chạy, có thể
> luân phiên task hiển thị ở ô input.

## Cách làm
- Ưu tiên dùng skill **effective-html** (`html`, `html-diagram`) nếu đã cài
  (`npx skills add plannotator/effective-html` hoặc plugin). Theo style của nó: tự chứa, ít chữ, visual mạnh.
- Nếu chưa cài: tự viết HTML+CSS+SVG inline gọn gàng, hiện đại (dark/light, card, progress bar).
- File HTML **tự chứa** (inline CSS/JS), mở trực tiếp bằng trình duyệt.
- Ghi ra `brain/dashboard/index.html` (thư mục này được gitignore).

## Sau khi generate
- Báo cho user đường dẫn file để mở.
- Không commit (dashboard gitignore).

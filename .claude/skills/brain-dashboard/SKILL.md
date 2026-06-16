---
name: brain-dashboard
description: Sinh dashboard HTML trực quan cho bộ não — hiển thị ai đang làm gì, tiến độ cả team, tài liệu theo version & vai trò, QA chờ PO, bản đồ đối thủ. Kích hoạt khi user nói "xem tiến độ", "vẽ dashboard", "cập nhật dashboard". Dùng style effective-html.
---

# Skill: brain-dashboard — sinh dashboard HTML

Tạo `brain/dashboard/index.html` trực quan, đẹp, tự chứa (1 file, không cần server).
Dự án triển khai theo **VERSION → VAI TRÒ**: dashboard phải phản ánh đúng mô hình này.

## Nguồn dữ liệu (quét lúc generate)
- `brain/project.json` — `current_version` + danh sách `versions` & `roles` (khung để gom tài liệu).
- `brain/state/*.json` — gom focus & tiến độ TẤT CẢ thành viên (`current_focus`, `features[]`).
- `brain/people/*.md` — tên/vai trò (map `<user>` → tên + `role`).
- `docs/*/<role>/*` — tài liệu dự án theo version & vai trò (`po_docs · tester · dev_mobile · be_docs · ai_docs`).
  Quét đệ quy mọi version trong `docs/` (bỏ qua `README.md` khung). Mỗi file = 1 tài liệu; lấy version
  (folder cấp 1) + role (folder cấp 2) + tên file; nếu có frontmatter thì đọc `author`/`updated_by`/`date`.
- `brain/qa/` — QA store (câu hỏi escalate cho PO). **NGUỒN đếm câu chờ** — quét trực tiếp các file QA,
  lọc `status: open` (chờ trả lời) vs `answered` (đã trả lời).
- `brain/ASK-LOG.md` — skim nhanh hỏi-đáp (bổ trợ, **KHÔNG** phải nguồn đếm).
- `brain/knowledge/competitors/*.md` — vẽ bản đồ đối thủ.
- `brain/knowledge/decisions/*.md` & `brain/knowledge/products/*.md` — knowledge stats.
- `brain/research/*/report.md` (nếu có) — danh sách report gần đây.

> ⚠️ Mô hình MỚI: KHÔNG còn `brain/knowledge/features/`. Tuyệt đối không quét/tham chiếu folder đó.
> Tài liệu nằm ở `docs/<version>/<role>/`; QA nằm ở `brain/qa/`.

## Nội dung dashboard
1. **HERO — "Dây chuyền tư duy" (động):** mô phỏng pipeline real-time. Bên trái là **task đang chạy**
   (lấy từ `current_focus` / feature `in_progress` của user); ở giữa là **bộ não đang nghĩ** (vòng tròn
   pulse + ripple) với 2 dây có **thanh chạy** (gradient động chạy vào & ra); bên phải **kết quả nhả ra
   từng cái một** (5 bước research-competitor reveal so le, lặp vô hạn). Đây là điểm nhấn user yêu cầu.
2. **Header:** tên bộ não + **version đang chạy** (`current_version`) + ngày generate.
3. **Summary band:** số tài liệu (theo version hiện tại), đối thủ, report, thành viên, **câu hỏi chờ PO**.
3b. **Band "❓ Câu hỏi đang chờ":** quét `brain/qa/` lọc `status: open` (nguồn trực tiếp, KHÔNG dựa ASK-LOG count).
    - User đang xem (git email) là **PO** → **card ĐỎ "PO cần trả lời (N)"**: feature/chủ đề · ai hỏi
      (`asker`) · tiêu đề · trích "AI đã thử gì" · tuổi câu (số ngày từ `opened`) · link file.
    - Vai trò khác → list xám "Câu hỏi đang chờ (N)" read-only (để biết đừng hỏi trùng).
    - Cảnh báo "answered nhưng chưa flow-back" (`status: answered` + `flowed_back: false`) → nhắc PO đưa đáp
      vào tài liệu nguồn (`docs/<ver>/po_docs/`).
4. **Tiến độ theo người (cả team):** mỗi thành viên 1 card — tên + vai trò + `current_focus` + progress bar
   từng feature trong `state/<user>.json`.
5. **Tài liệu theo VERSION → VAI TRÒ:** nhóm cấp 1 theo version (current_version lên đầu, nổi bật), trong mỗi
   version nhóm tiếp theo 5 vai trò (`po_docs · tester · dev_mobile · be_docs · ai_docs`); mỗi vai trò list
   tài liệu (tên file · `author`/`updated_by` · `date` · link file). Đây là trung tâm của dashboard mô hình mới.
6. **Research gần đây:** danh sách report (tiêu đề, ngày, người, link file) — nếu có.
7. **Bản đồ đối thủ:** diagram SVG (style `html-diagram`) — app mình ở giữa, đối thủ xung quanh.
8. **Knowledge stats:** số file theo loại (`products` · `competitors` · `decisions`).

> HERO làm bằng CSS animation thuần (keyframes `run` cho thanh chạy, `think`/`ripple` cho bộ não,
> `reveal` so le cho kết quả). Không cần JS ngoài nút theme. Khi có nhiều task đang chạy, có thể
> luân phiên task hiển thị ở ô input.

## Cách làm
- Ưu tiên dùng skill **effective-html** (`html`, `html-diagram`) nếu đã cài
  (`npx skills add plannotator/effective-html` hoặc plugin). Theo style của nó: tự chứa, ít chữ, visual mạnh.
- Nếu chưa cài: tự viết HTML+CSS+SVG inline gọn gàng, hiện đại (dark/light, card, progress bar).
- File HTML **tự chứa** (inline CSS/JS), mở trực tiếp bằng trình duyệt.
- Ghi ra `brain/dashboard/index.html` (thư mục này được gitignore).
- ⚠️ Repo có **DẤU CÁCH** trong path → mọi lệnh shell quét file phải **quote** đường dẫn tuyệt đối.

## Sau khi generate
- Báo cho user đường dẫn file để mở.
- Không commit (dashboard gitignore).

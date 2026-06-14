# 🧠 Bộ não tư duy — Apero

Đây là **bộ não thứ hai** local-first, git-tracked, dùng chung cho cả team khi làm product.
Mỗi phiên làm việc, BẮT BUỘC làm theo skill `brain` trước khi làm gì khác.

## Khởi động mỗi phiên (đọc kỹ)

1. **Nạp skill `brain`** trong `.claude/skills/brain/` — đó là sách hướng dẫn vận hành.
2. **Xác định "tôi là ai":** chạy `git config user.email` → map tới `brain/people/<user>.md`.
3. **Đọc focus hiện tại:** `brain/state/<user>.json`.
4. **Quét trí nhớ:** `brain/knowledge/INDEX.md`.

## Khi user nói "muốn thêm tính năng X"

→ Chạy skill `research-competitor` (pipeline 5 bước: nhớ lại → hỏi gọn → nghiên cứu đối thủ
bằng SensorTower + Meta Ads + web → báo cáo & đề xuất → ghi nhớ & cập nhật state).

## Khi muốn xem tiến độ / "vẽ dashboard"

→ Chạy skill `brain-dashboard` → sinh `brain/dashboard/index.html` (effective-html style).

## Quy tắc vàng

- **Đọc trước khi hỏi:** đừng hỏi lại điều đã có trong `knowledge/`.
- **Một file = một thực thể**, có frontmatter `author`/`updated_by`/`date`.
- **Vùng riêng vs chung:** `state/<user>.json` là của riêng từng người (đừng sửa của người khác);
  `knowledge/` là kho chung (ghi nhận `updated_by`).
- **Không tự ý commit git** — chỉ commit khi user yêu cầu rõ.
- `brain/dashboard/` được gitignore — mỗi người tự generate.
- **Tự học:** cuối việc hoặc khi user nói "ghi nhớ" → lưu bài học (dự án → `knowledge/`, cách làm việc → memory). Xem mục "Ghi nhớ / Chốt phiên" trong skill `brain`.

Chi tiết thiết kế: `docs/superpowers/specs/2026-06-14-bo-nao-tu-duy-design.md`.

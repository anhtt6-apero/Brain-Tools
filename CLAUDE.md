# 🧠 Bộ não tư duy — Apero

Đây là **bộ não thứ hai** local-first, git-tracked, dùng chung cho cả team khi làm product.
Mỗi phiên làm việc, BẮT BUỘC làm theo skill `brain` trước khi làm gì khác.

## Khởi động mỗi phiên (đọc kỹ)

1. **Nạp skill `brain`** trong `.claude/skills/brain/` — đó là sách hướng dẫn vận hành.
2. **Xác định "tôi là ai":** chạy `git config user.email` → map tới `brain/people/<user>.md`.
   Lần đầu chưa có hồ sơ → **onboard**: hỏi tên + vai trò (`PO|Tester|Dev|BE|AI`) rồi tạo hồ sơ.
3. **Đọc focus hiện tại:** `brain/state/<user>.json`.
4. **Quét trí nhớ:** `brain/knowledge/INDEX.md`.
5. **Phổ biến quyền theo vai trò:** nói rõ "bạn là ai + làm được gì trong dự án này" theo bảng
   *Vai trò & quyền* trong skill `brain`.

## Vai trò & skill tương ứng

- **PO:** "viết tài liệu tính năng / feature doc / UI preview" → skill `po-feature` (gen `feature.md` + `ui/preview.html`).
- **Tester:** đọc `feature.md` → sinh test case vào `tests/` _(skill riêng của Tester thêm sau)_.
- **Dev:** "gen task" → skill `dev-task` (đọc `feature.md` → `tasks/<user>.md`).
- **BE/AI:** "push logic" → skill `push-logic` (ghi `logic/<user>.md` để bên khác tham khảo).
- **Bí sau khi đọc docs (mọi vai trò):** "không hiểu / thắc mắc về tính năng X" → skill `ask-po`: AI thử trả lời từ docs → **dedup** (mục 7 + `questions/` + `ASK-LOG`) → escalate cho PO (`questions/<id>.md` + `brain/ASK-LOG.md`). PO thấy qua dashboard/đầu phiên; câu trả lời **chảy ngược** vào `feature.md` mục 7 để khỏi ai hỏi lại.

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
- **Không tự ý commit git** — chỉ commit khi user yêu cầu rõ. Khi commit/push: **xác nhận danh tính**
  (`git config user.email` khớp `@apero.vn`) + **ghi `brain/PUSH-LOG.md`** (ai · lúc nào · gì). Xem mục
  "Commit / Push" trong skill `brain`.
- `brain/dashboard/` được gitignore — mỗi người tự generate.
- **Tự học:** cuối việc hoặc khi user nói "ghi nhớ" → lưu bài học (dự án → `knowledge/`, cách làm việc → memory). Xem mục "Ghi nhớ / Chốt phiên" trong skill `brain`.

Chi tiết thiết kế: `docs/superpowers/specs/2026-06-14-bo-nao-tu-duy-design.md`.

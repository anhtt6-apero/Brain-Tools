# 🧠 Bộ não tư duy — Apero

Đây là **bộ não thứ hai** local-first, git-tracked, dùng chung cho cả team khi làm product.
**Dự án triển khai theo VERSION:** mỗi version = 1 folder trong `docs/`, bên trong chia theo 5 vai trò
(`po_docs · tester · dev_mobile · be_docs · ai_docs`). `brain/` giữ luật + phân quyền + version + trí nhớ chung.
Mỗi phiên làm việc, BẮT BUỘC làm theo skill `brain` trước khi làm gì khác.

## Khởi động mỗi phiên (đọc kỹ)

1. **Nạp skill `brain`** trong `.claude/skills/brain/` — đó là bộ luật & phân quyền.
2. **Xác định "tôi là ai":** chạy `git config user.email` → map tới `brain/people/<user>.md`.
   Lần đầu chưa có hồ sơ → **onboard**: hỏi tên + vai trò (`PO|Tester|Dev|BE|AI`) rồi tạo hồ sơ.
3. **Đọc version dự án đang chạy:** `brain/project.json` → `current_version` (tài liệu mới rơi vào version này).
4. **Đọc focus hiện tại:** `brain/state/<user>.json`.
5. **Quét trí nhớ chung:** `brain/knowledge/INDEX.md`.
6. **Phổ biến quyền theo vai trò:** nói rõ "bạn là ai + ghi vào đâu + xem được gì + dùng skill/lệnh nào"
   theo bảng *Vai trò, quyền & định tuyến* trong skill `brain`.

## Vai trò & skill tương ứng

Mỗi vai trò ghi tài liệu vào folder của mình trong version hiện tại: `docs/<current_version>/<role>/`.

- **PO** (`po_docs/`, xem TOÀN dự án): "viết tài liệu tính năng" → skill `po-feature`; quản `current_version`.
- **Tester** (`tester/`): đọc `po_docs/` → sinh test case _(skill riêng của Tester thêm sau)_.
- **Dev mobile** (`dev_mobile/`): "gen task" → skill `dev-task` (đọc `po_docs/` + logic BE/AI).
- **BE** (`be_docs/`) **/ AI** (`ai_docs/`): "push logic" → skill `push-logic` để bên khác tham khảo.
- **Bí sau khi đọc docs (mọi vai trò):** → skill `ask-po`: check docs version hiện tại → `brain/qa/` → docs version cũ → chưa có mới tạo QA (escalate cho PO).

## Khi user nói "muốn thêm tính năng X"

→ Chạy skill `research-competitor` (pipeline 5 bước: nhớ lại → hỏi gọn → nghiên cứu đối thủ
bằng SensorTower + Meta Ads + web → báo cáo & đề xuất → ghi nhớ & cập nhật state).
Lưu lại: research → `brain/research/`; **lý do làm / không làm** → `brain/knowledge/decisions/`;
tính năng chốt làm → `docs/<current_version>/po_docs/`.

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

Chi tiết bộ luật & phân quyền: skill `brain` (`.claude/skills/brain/SKILL.md`). Version dự án: `brain/project.json`.

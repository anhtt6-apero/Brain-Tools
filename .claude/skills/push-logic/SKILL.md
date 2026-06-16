---
name: push-logic
description: BE/AI push logic đang xử lý lên bộ não để PO/Dev/Tester/AI tham khảo. Kích hoạt khi BE/AI nói "push logic", "ghi logic tính năng X", "cập nhật logic BE/AI". Output là 1 file logic trong docs/<current_version>/<be_docs|ai_docs>/ — KHÔNG cần đầy đủ, chỉ đủ để bên khác hiểu hợp đồng input→output.
---

# Skill: push-logic — BE/AI đẩy logic làm tài liệu tham khảo

Mục tiêu **nhẹ**: BE/AI không phải viết tài liệu dài, chỉ push logic/hợp đồng đang xử lý để
Dev mobile / AI / Tester / PO biết khi cần. Logic = **hợp đồng input→output** đủ để bên khác ĐỌC THAM KHẢO.

## Bước 0 — Tôi là ai? (BE hay AI)
- Đọc `brain/people/<user>.md` → lấy `role`.
  - `role: BE` → ghi vào `docs/<current_version>/be_docs/`.
  - `role: AI` → ghi vào `docs/<current_version>/ai_docs/`.
- `<current_version>` đọc từ `brain/project.json` → `current_version` (vd `v1.4.0`). Không tự nhớ version.
- Muốn push cho version khác (vd vá `v1.3.0`) → nói rõ version → ghi vào đúng `docs/<ver>/<role>/`.

## Bước 1 — Đọc bối cảnh (đọc trước khi hỏi)
- `docs/<current_version>/po_docs/` — tài liệu tính năng của PO để biết logic này phục vụ cái gì.
- **AI lưu ý:** khi có QA/việc liên quan, đọc thêm logic các bên trong cùng version
  (`docs/<current_version>/be_docs/` và `ai_docs/`) trước khi push để khỏi mâu thuẫn hợp đồng.
- Chưa có tài liệu PO cho tính năng → hỏi/báo PO viết trước, đừng tự bịa nghiệp vụ.

## Bước 2 — Push logic
- Tạo/cập nhật 1 file trong folder của bạn: `docs/<current_version>/<be_docs|ai_docs>/<slug>.md`
  (`<slug>` = tên tính năng/module; mỗi tính năng/module 1 file → ít đụng git).
- Frontmatter nhẹ:
  ```yaml
  ---
  version: <current_version>
  role: BE        # hoặc AI
  author: <user>
  date: <YYYY-MM-DD>   # lấy giờ thật bằng lệnh `date`
  ---
  ```
- Nội dung tối thiểu: đang xử lý gì · **hợp đồng input→output** (endpoint/hàm, params, response, mã lỗi)
  · điều bên khác cần biết (ràng buộc, case biên).
- Có thể paste/tóm tắt code hiện tại — đủ để Dev mobile ráp, AI dùng lại, Tester biết case biên.

## Bước 3 — Báo & cập nhật
- (Tùy chọn) cập nhật `brain/state/<user>.json` (tính năng + tiến độ) → gợi ý regenerate dashboard.
- **Báo Dev mobile / AI / Tester nếu hợp đồng đổi** (đổi endpoint, params, response).

## Lưu ý
- Đây là **tài liệu sống** — cập nhật khi logic đổi, không cần hoàn hảo. Sửa file người khác → đổi `updated_by`, giữ `author`.
- Thắc mắc nghiệp vụ/hợp đồng chưa rõ → chạy skill **`ask-po`** để escalate cho PO thay vì hỏi miệng rồi quên.
  Nhớ **dedup trước** (quét câu hỏi đang chờ + `grep` `brain/ASK-LOG.md`).
- Không commit git trừ khi user yêu cầu rõ. Khi push → theo mục "Commit / Push" trong skill `brain`
  (xác nhận danh tính `@apero.vn` + ghi `brain/PUSH-LOG.md`).

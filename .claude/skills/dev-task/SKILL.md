---
name: dev-task
description: Dev mobile đọc tài liệu tính năng của PO rồi sinh task/tài liệu triển khai của riêng mình. Kích hoạt khi Dev nói "gen task cho tính năng X", "tạo task doc", "chia task từ feature". Output là docs/<current_version>/dev_mobile/<feature-slug>.md.
---

# Skill: dev-task — Dev mobile sinh task từ tài liệu PO

Dự án chia theo **VERSION**. Trước tiên đọc `brain/project.json` → `current_version` (vd `v1.4.0`).
Mọi đường dẫn dưới đây dùng version đó.

## Bước 1 — Đọc nguồn sự thật (đọc trước khi hỏi)
- `docs/<current_version>/po_docs/` — tài liệu tính năng của PO (luồng/màn hình, acceptance, kỹ thuật, hỏi đáp đã chốt).
- `docs/<current_version>/be_docs/` + `docs/<current_version>/ai_docs/` — logic/hợp đồng API các bên đã push (để biết input→output).
- Chưa có tài liệu của PO cho tính năng → **báo PO viết trước**, đừng tự bịa nghiệp vụ.
- ⚠️ **Trước khi trả lời/escalate bất kỳ thắc mắc nghiệp vụ nào:** BẮT BUỘC đọc hết tài liệu PO (gồm mục hỏi-đáp đã chốt)
  + quét logic BE/AI + `grep` `brain/ASK-LOG.md`. Cả ba trống mới được nêu câu mới → chạy skill `ask-po`.

## Bước 2 — Hỏi gọn nếu thiếu
Chỉ hỏi cái chưa suy ra được: phạm vi task của riêng mình (mảng FE nào), phụ thuộc BE/AI nào còn đang chờ.

## Bước 3 — Viết task doc
- Tạo/cập nhật `docs/<current_version>/dev_mobile/<feature-slug>.md` (hoặc tên gọn theo tính năng).
- **Mỗi dev tự quản file của mình** → không đụng git của người khác.
- Frontmatter nhẹ:
  ```yaml
  version: <current_version>
  role: Dev
  author: <user>
  date: <YYYY-MM-DD>
  status: todo        # todo | doing | done
  ```
- Nội dung: chia đầu việc, ghi rõ phụ thuộc vào logic ở `be_docs/`/`ai_docs/`, câu hỏi ngược cho PO.

## Bước 4 — Cập nhật
- `brain/state/<user>.json` (tính năng + tiến độ).
- `status` trong frontmatter task: `todo` / `doing` / `done`.

## Lưu ý
- **Không sửa tài liệu của PO** trong `po_docs/` (chỉ PO ghi). Dev chỉ ghi vào `dev_mobile/`.
- Thắc mắc đọc docs vẫn chưa rõ → skill **`ask-po`** (escalate có cấu trúc + ghi `brain/ASK-LOG.md`);
  ĐỪNG viết chết câu hỏi trong task (PO không thấy). Link câu đã nêu vào mục "Câu hỏi ngược cho PO" để theo dõi.
- Không commit git trừ khi user yêu cầu rõ.

---
name: dev-task
description: Dev đọc feature doc của PO rồi sinh tài liệu task của riêng mình. Kích hoạt khi Dev nói "gen task cho tính năng X", "tạo task doc", "chia task từ feature". Output là tasks/<user>.md trong folder tính năng.
---

# Skill: dev-task — Dev sinh tài liệu task từ feature doc

## Bước 1 — Đọc nguồn sự thật (đọc trước khi hỏi)
- `brain/knowledge/features/<slug>/feature.md` — mục **4** (luồng/màn hình), **6** (kỹ thuật), **7** (Hỏi đáp đã chốt).
- `brain/knowledge/features/<slug>/logic/*.md` nếu BE/AI đã push logic — để biết hợp đồng API.
- Chưa có feature.md → **báo PO viết trước**, đừng tự bịa nghiệp vụ.
- ⚠️ **Trước khi trả lời/escalate bất kỳ thắc mắc nghiệp vụ nào:** BẮT BUỘC (a) đọc mục 7, (b) quét
  `../questions/*.md`, (c) `grep` `brain/ASK-LOG.md`. Cả 3 trống mới được nêu câu mới → chạy skill `ask-po`.

## Bước 2 — Hỏi gọn nếu thiếu
Phạm vi task của riêng mình (FE / mảng nào), phụ thuộc BE/AI nào còn đang chờ.

## Bước 3 — Viết task doc
- Tạo/cập nhật `brain/knowledge/features/<slug>/tasks/<user>.md` theo `features/_TEMPLATE/tasks/_TEMPLATE.md`.
- **Mỗi dev 1 file riêng** (tên theo `<user>`) → không đụng git.
- Chia đầu việc, ghi rõ phụ thuộc vào `logic/<be-user>.md`, câu hỏi ngược cho PO.

## Bước 4 — Cập nhật
- `brain/state/<user>.json` (tính năng + tiến độ).
- `status` trong frontmatter task: todo / doing / done.

## Lưu ý
- **Không sửa `feature.md`** của PO (chỉ PO ghi).
- Thắc mắc đọc docs vẫn chưa rõ → skill **`ask-po`** (tạo `../questions/<slug>-<bạn>.md` + ghi `brain/ASK-LOG.md`);
  ĐỪNG viết chết câu hỏi trong task (PO không thấy). Link câu đã nêu vào mục "Câu hỏi ngược cho PO" để theo dõi.
- Không commit git trừ khi user yêu cầu rõ.

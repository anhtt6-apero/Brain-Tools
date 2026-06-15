---
name: push-logic
description: BE/AI push logic đang xử lý lên bộ não để PO/Dev/Tester tham khảo. Kích hoạt khi BE/AI nói "push logic", "ghi logic tính năng X", "cập nhật logic BE/AI". Output là logic/<user>.md trong folder tính năng — KHÔNG cần đầy đủ, chỉ đủ để bên khác hiểu hợp đồng.
---

# Skill: push-logic — BE/AI đẩy logic làm tài liệu tham khảo

Mục tiêu **nhẹ**: BE/AI không phải viết tài liệu dài, chỉ push logic/hợp đồng đang xử lý để
PO/Dev/Tester biết khi cần.

## Bước 1 — Xác định tính năng
- Logic này thuộc tính năng nào → `brain/knowledge/features/<slug>/`.
- Chưa có folder → hỏi PO, hoặc tạo stub `feature.md` tối thiểu rồi báo PO bổ sung.

## Bước 2 — Push logic
- Tạo/cập nhật `brain/knowledge/features/<slug>/logic/<user>.md` theo `features/_TEMPLATE/logic/_TEMPLATE.md`.
- **Mỗi người 1 file** (`<user>.md`), set `role: BE` hoặc `AI`.
- Tối thiểu: đang xử lý gì · **hợp đồng input→output** (endpoint/hàm, params, response) · điều bên khác cần biết.
- Có thể paste/tóm tắt code hiện tại — đủ để Dev ráp, Tester biết case biên.

## Bước 3 — Báo & cập nhật
- Link `[[feature-slug]]` ngược về feature.
- (Tùy chọn) cập nhật `brain/state/<user>.json`.
- **Báo Dev/Tester nếu hợp đồng đổi.**

## Lưu ý
- Đây là **tài liệu sống** — cập nhật khi logic đổi, không cần hoàn hảo.
- Thắc mắc nghiệp vụ/hợp đồng chưa rõ → chạy skill **`ask-po`** để escalate cho PO (tạo `../questions/<slug>-<bạn>.md`
  + ghi `brain/ASK-LOG.md`), thay vì hỏi miệng rồi quên. Nhớ **dedup trước** (quét `questions/` + grep ASK-LOG).
- Không commit git trừ khi user yêu cầu rõ.

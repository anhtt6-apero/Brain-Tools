---
feature: [[<feature-slug>]]
role: Dev
author: <user>
updated_by: <user>
date: YYYY-MM-DD
status: todo        # todo | doing | done
---
# Task: <feature> — <user> (Dev)

> 🟨 File của **Dev**, mỗi dev 1 file riêng (`tasks/<user>.md`). Sinh từ `../feature.md`.

## Tóm tắt việc (chắt từ feature.md mục 4–5)
- ...

## Đầu việc
- [ ] ...
- [ ] ...

## Phụ thuộc BE / AI
- Theo hợp đồng trong `../logic/<be-user>.md`: ...

## Câu hỏi ngược cho PO
> Thắc mắc khi đọc feature.md/logic vẫn chưa rõ → ĐỪNG viết chết ở đây (PO không thấy).
> Chạy skill `ask-po`: AI thử trả lời từ docs → nếu bí thì dedup → tạo `../questions/<slug>-<bạn>.md`
> + ghi `brain/ASK-LOG.md`. PO thấy qua dashboard/đầu phiên. Link câu đã nêu vào đây để theo dõi:
- [[../questions/<slug>-<bạn>]] — <tiêu đề> — status: open

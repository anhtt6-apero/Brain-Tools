---
name: <feature-slug>
type: feature
product: [[<product-slug>]]
status: draft        # draft | in_progress | testing | done
po: <user>           # PO sở hữu tính năng này
author: <user>
updated_by: <user>
date: YYYY-MM-DD
research: <[[<date>-<feature>-<user>]] nếu có report đối thủ, để trống nếu chưa>
tags: []
---
# <Tên tính năng>

> 🟦 File của **PO** — **nguồn sự thật duy nhất**. Tester / Dev / BE-AI đều đọc từ đây.

## 1. Vấn đề & mục tiêu
- Vì sao làm, cho ai, giải quyết gì.

## 2. User story
- Là `<vai>`, tôi muốn `<hành động>`, để `<giá trị>`.

## 3. Phạm vi (scope)
- **Trong phạm vi:** ...
- **Ngoài phạm vi:** ...

## 4. Luồng & màn hình  _(đầu vào cho Dev)_
- Bước 1 → ...  · UI: [preview](ui/preview.html)
- Bước 2 → ...

## 5. Acceptance criteria  _(đầu vào cho Tester)_
- [ ] AC1: ...
- [ ] AC2: ...

## 6. Ghi chú kỹ thuật / phụ thuộc
- Liên quan logic BE/AI: xem `logic/` (`[[<feature-slug>]]`).

## 7. Hỏi đáp đã chốt (Q&A)  _(câu trả lời chảy ngược về đây — người đọc sau tự thấy)_

> Mỗi dòng = 1 câu đã chốt, link về file Q gốc trong `questions/`. Hàng đợi câu **ĐANG CHỜ** nằm ở
> `questions/` (status:open) + `brain/ASK-LOG.md`, KHÔNG để ở đây.
> PO chốt: "chốt bởi `<po>` (PO)"; non-PO/AI tự giải: ghi ngay kèm nhãn "**CHỜ PO XÁC NHẬN**".

- _(chưa có — điền khi 1 câu trong `questions/` được trả lời)_

## Liên kết (output các vai trò khác bám về đây)
- 🖼️ UI preview: `ui/preview.html`        — PO
- 🧪 Test cases: `tests/`                   — Tester
- 🛠️ Tasks: `tasks/<user>.md`               — Dev
- ⚙️ Logic: `logic/<user>.md`               — BE/AI
- ❓ Hỏi-đáp: `questions/`                   — Tester/Dev/BE-AI escalate khi đọc docs vẫn bí (skill `ask-po`)

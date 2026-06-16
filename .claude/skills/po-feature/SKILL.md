---
name: po-feature
description: PO viết tài liệu tính năng (feature doc) làm nguồn sự thật cho cả team + gen UI preview HTML. Kích hoạt khi PO nói "viết tài liệu tính năng X", "tạo feature doc", "gen UI preview". Là ĐẦU NGUỒN của luồng bàn giao — Tester/Dev/BE-AI đều đọc file này.
---

# Skill: po-feature — PO tạo tài liệu tính năng + UI preview

Sản phẩm của PO là **nguồn sự thật duy nhất** cho 1 tính năng. Dự án chia theo **VERSION**: tài liệu
tính năng nằm trong `docs/<current_version>/po_docs/`. Tester, Dev, BE/AI đều đọc từ đây.

## Bước 0 — Xác định version đang chạy
- Đọc `brain/project.json` → `current_version` (vd `v1.4.0`). Mọi tài liệu mới rơi vào version này.
- Thiếu/không rõ → hỏi user; mặc định version mới nhất trong `docs/`.
- Muốn viết cho version khác → user phải nói rõ → ghi vào `docs/<version đó>/po_docs/`.

## Bước 1 — Nhớ lại (đọc, không hỏi)
- `brain/knowledge/products/` — app nào, định vị, phân khúc.
- `brain/research/<...>/report.md` — đã soi đối thủ cho tính năng này chưa (có → link vào frontmatter `research`).
- `docs/<current_version>/po_docs/` — tính năng này đã có file chưa (tránh trùng); đọc các feature liên quan.

## Bước 2 — Hỏi gọn phần còn thiếu
Chỉ hỏi cái chưa suy ra được: tên tính năng, thuộc sản phẩm nào, phạm vi in/out, đã có research chưa.

## Bước 3 — Viết feature doc
- Tạo `docs/<current_version>/po_docs/<feature-slug>.md` — **1 tính năng = 1 file**.
- Frontmatter nhẹ:
  ```yaml
  ---
  version: <current_version>
  role: PO
  status: draft        # draft | ready | shipped
  author: <user>
  updated_by: <user>
  date: <YYYY-MM-DD>
  research: <link nếu có>
  ---
  ```
- Bố cục các mục:
  1. **Vấn đề / mục tiêu** — tại sao làm, đo bằng gì.
  2. **User story** — "Là <ai>, tôi muốn <gì>, để <lợi ích>".
  3. **Scope** — in / out (rõ cái KHÔNG làm).
  4. **Luồng & màn hình** — các bước + màn liên quan; trỏ tới UI preview (Bước 4).
  5. **Acceptance criteria** — rõ ràng, đầu vào cho **Tester**.
  6. **Ghi chú kỹ thuật** — ràng buộc, phụ thuộc BE/AI, edge case (đầu vào cho **Dev/BE/AI**).
- Link sản phẩm bằng `[[product-slug]]`; set `status`, `author: <user>`.

## Bước 4 — Gen UI preview (effective-html)
- UI preview (nếu có) đặt **cạnh** feature doc trong cùng `po_docs/`:
  `docs/<current_version>/po_docs/<feature-slug>.html` (hoặc gom vào `po_docs/ui/`).
- HTML **tự chứa**, mô phỏng màn/luồng. Ưu tiên skill **effective-html** (`html`) nếu đã cài; theo style
  của nó (tự chứa, visual mạnh, dark/light). Chưa cài → viết HTML+CSS inline gọn.
- Mục 4 của feature doc trỏ tới file preview này.

## Bước 5 — Ghi nhớ & báo
- Cập nhật `brain/state/<user>.json`: thêm tính năng + tiến độ.
- (Tùy) thêm 1 dòng vào `docs/<current_version>/po_docs/README.md` để dễ tra.
- Báo các bên: "feature doc cho `<X>` (`<current_version>`) đã sẵn sàng — Tester/Dev/BE-AI có thể đọc."

## Lưu ý
- `po_docs/` là **NGUỒN SỰ THẬT** cho cả team — Tester sinh test case, Dev gen task, BE/AI push logic đều bám vào đây.
- Feature doc là tài liệu **iterate**, không vứt đi. Sửa → đổi `updated_by`, bump `date`.
- Câu hỏi từ Tester/Dev/BE/AI về tính năng → trả lời qua skill **`ask-po`**; đáp làm đổi nghiệp vụ thì
  sửa thẳng vào feature doc rồi bump `updated_by`+`date`.
- Không commit git trừ khi user yêu cầu rõ (theo §G skill `brain`).

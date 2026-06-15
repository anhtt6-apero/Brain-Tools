---
name: po-feature
description: PO viết tài liệu tính năng (feature doc) làm nguồn sự thật cho cả team + gen UI preview HTML. Kích hoạt khi PO nói "viết tài liệu tính năng X", "tạo feature doc", "gen UI preview". Là ĐẦU NGUỒN của luồng bàn giao — Tester/Dev/BE-AI đều đọc file này.
---

# Skill: po-feature — PO tạo tài liệu tính năng + UI preview

Sản phẩm của PO là **nguồn sự thật duy nhất** cho 1 tính năng:
`brain/knowledge/features/<slug>/feature.md` + `ui/preview.html`. Tester, Dev, BE/AI đều đọc từ đây.

## Bước 1 — Nhớ lại (đọc, không hỏi)
- `brain/knowledge/products/` — app nào, định vị, phân khúc.
- `brain/research/<...>/report.md` — đã soi đối thủ cho tính năng này chưa (có → link vào frontmatter `research`).
- `brain/knowledge/features/` — tính năng này đã có folder chưa (tránh trùng).

## Bước 2 — Hỏi gọn phần còn thiếu
Chỉ hỏi cái chưa suy ra được: tên tính năng, thuộc sản phẩm nào, phạm vi in/out, đã có research chưa.

## Bước 3 — Viết feature.md
- Tạo `brain/knowledge/features/<feature-slug>/feature.md` theo `features/_TEMPLATE/feature.md`.
- **Mục 5 Acceptance criteria** rõ ràng (đầu vào cho Tester) và **mục 4 Luồng/màn hình** (đầu vào cho Dev).
- Link `[[product-slug]]`, set `status`, `po: <user>`.

## Bước 3b — Giải đáp câu hỏi đang chờ (flow-back)
- Kiểm tra `features/<slug>/questions/*.md` có `status: open` (hoặc dòng OPEN chưa RESOLVED trong `brain/ASK-LOG.md`).
- Trả lời theo skill `ask-po` luồng B: điền `resolver`/`answered`/"Câu trả lời chốt" + đổi `status: answered`, append RESOLVED vào ASK-LOG.
- **Chảy ngược (bắt buộc)** vào feature.md mục `## 7. Hỏi đáp đã chốt`:
  `- (<id>) Hỏi:… · Đáp:… — chốt bởi <user> (PO) <date> · [[questions/<id>]]`, set `flowed_back: true`.
- Câu do AI/non-PO tự giải đã có dòng "**CHỜ PO XÁC NHẬN**" trong mục 7 → bạn chỉ cần đổi nhãn thành "chốt bởi <bạn> (PO)".
- Đáp làm đổi nghiệp vụ → sửa thẳng mục 4/5/6, bump `updated_by`+`date`.

## Bước 4 — Gen UI preview (effective-html)
- Ghi `brain/knowledge/features/<slug>/ui/preview.html` — HTML **tự chứa**, mô phỏng màn/luồng.
- Ưu tiên skill **effective-html** (`html`) nếu đã cài; theo style của nó (tự chứa, visual mạnh, dark/light).
  Chưa cài → viết HTML+CSS inline gọn (xem template starter trong `_TEMPLATE/ui/preview.html`).
- feature.md mục 4 trỏ tới `ui/preview.html`.

## Bước 5 — Ghi nhớ & báo
- Thêm 1 dòng vào `brain/knowledge/INDEX.md` mục **Tính năng (features/)**.
- Cập nhật `brain/state/<user>.json`: thêm tính năng + tiến độ.
- Báo các bên: "feature doc cho `<X>` đã sẵn sàng — Tester/Dev/BE-AI có thể đọc."

## Lưu ý
- feature.md là tài liệu **iterate**, không vứt đi. Sửa → đổi `updated_by`, bump `date`.
- Không commit git trừ khi user yêu cầu rõ.

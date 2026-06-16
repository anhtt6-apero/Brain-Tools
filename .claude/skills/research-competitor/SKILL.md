---
name: research-competitor
description: Nghiên cứu đối thủ khi bắt đầu một tính năng mới cho app. Kích hoạt khi user nói "muốn thêm tính năng X", "nghiên cứu đối thủ", hoặc cần soi quảng cáo/cách làm của đối thủ. Dùng SensorTower + Meta Ads + web rồi ra báo cáo & đề xuất.
---

# Skill: research-competitor — PO nghiên cứu đối thủ/thị trường cho 1 tính năng

Pipeline 5 bước. Chạy một mạch, nhưng dừng lại nếu thiếu thông tin quan trọng.
Đây là việc của **PO**: soi đối thủ/thị trường để quyết định **có làm hay không** một tính năng,
rồi nếu chốt làm thì viết tài liệu cho cả team.

> Dự án chia theo **VERSION** — đọc `brain/project.json` → `current_version`. Tính năng chốt làm sẽ
> được viết vào `docs/<current_version>/po_docs/` (KHÔNG còn folder `features/`).

## Bước 1 — Nhớ lại (đọc, không hỏi)
- Đọc `brain/knowledge/products/` để biết app mình là gì, phân khúc, định vị.
- Đọc `brain/knowledge/competitors/` xem đã có hồ sơ đối thủ nào liên quan tính năng này.
- Đọc `brain/knowledge/decisions/` xem đã từng quyết định gì (làm / bỏ) liên quan — tránh nghiên cứu lại.
- Liếc `docs/<current_version>/po_docs/` xem tính năng này đã có tài liệu chưa.

## Bước 2 — Hỏi gọn 1–2 câu (chỉ phần còn thiếu)
Ví dụ: phân khúc nhắm tới (mới bắt đầu / nâng cao)? Có đối thủ chỉ định hay để tự tìm top?

## Bước 3 — Nghiên cứu dữ liệu thật
Dùng các MCP (tìm schema qua ToolSearch trước khi gọi):
- **SensorTower:** `search_apps`, `get_app_details`, `get_top_apps`, `get_sales_estimates`,
  `get_app_keywords`, `get_ad_creatives`, `get_retention`, `get_demographics`.
  → ai top mảng này, app nào có tính năng tương tự, tải/doanh thu ước tính, keyword ASO,
    **ad creatives họ chạy**.
- **Meta Ads:** quảng cáo đối thủ đang chạy cho tính năng đó (hình ảnh, copy, góc tiếp cận).
- **Web search/fetch:** cách đối thủ làm tính năng (UX, công nghệ, định giá), bài viết, review.

Lưu raw data quan trọng vào `brain/research/<date>-<chủ-đề>-<user>/data/`.

## Bước 4 — Báo cáo + đề xuất
Viết `brain/research/<date>-<chủ-đề>-<user>/report.md`:
- **Bối cảnh:** tính năng gì, vì sao cân nhắc làm.
- **Bảng so sánh đối thủ:** ai có, làm thế nào, điểm mạnh/yếu.
- **Insight quảng cáo:** góc tiếp cận & message đối thủ đang dùng (từ ad creatives + Meta Ads).
- **Đề xuất cho app mình:** nên làm hay không, hướng khác biệt, ưu tiên, rủi ro.
Gợi ý chạy `brain-dashboard` để có bản HTML đẹp của report.

## Bước 5 — Ghi nhớ & cập nhật
- **Lý do làm / KHÔNG làm** → ghi 1 quyết định vào `brain/knowledge/decisions/<date>-<slug>.md`
  (theo template: bối cảnh · quyết định · vì sao · đánh đổi · trạng thái). Đây là chỗ sau này
  tra lại "vì sao mình triển khai" hoặc "vì sao mình đã bỏ" tính năng — luôn ghi, kể cả khi quyết định bỏ.
- Insight đối thủ → tạo/cập nhật file trong `brain/knowledge/competitors/` (đổi `updated_by`).
- Cập nhật `brain/knowledge/INDEX.md` (thêm dòng cho decision/competitor mới).
- Cập nhật `brain/state/<user>.json`: thêm chủ đề nghiên cứu + tiến độ, rồi gợi ý regenerate dashboard.

## Bước 6 — Nếu CHỐT LÀM tính năng
- Bàn giao sang skill **`po-feature`** để viết tài liệu tính năng vào
  `docs/<current_version>/po_docs/<feature-slug>.md` (KHÔNG còn folder `features/`).
- Trong frontmatter feature doc, link tới report ở `brain/research/...` (field `research`) để truy vết nguồn.

## Lưu ý
- Số liệu phải là **thật** từ MCP — không bịa. Nếu MCP lỗi/không có quyền, nói rõ và dùng web search thay.
- `brain/research/` là append-only; `brain/knowledge/` là trí nhớ chung (ghi `updated_by`).
- Không commit git trừ khi user yêu cầu (theo §G skill `brain`).

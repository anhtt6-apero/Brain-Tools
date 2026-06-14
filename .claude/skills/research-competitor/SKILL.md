---
name: research-competitor
description: Nghiên cứu đối thủ khi bắt đầu một tính năng mới cho app. Kích hoạt khi user nói "muốn thêm tính năng X", "nghiên cứu đối thủ", hoặc cần soi quảng cáo/cách làm của đối thủ. Dùng SensorTower + Meta Ads + web rồi ra báo cáo & đề xuất.
---

# Skill: research-competitor — nghiên cứu đối thủ cho 1 tính năng

Pipeline 5 bước. Chạy một mạch, nhưng dừng lại nếu thiếu thông tin quan trọng.

## Bước 1 — Nhớ lại (đọc, không hỏi)
- Đọc `brain/knowledge/products/` để biết app mình là gì, phân khúc, định vị.
- Đọc `brain/knowledge/competitors/` xem đã có hồ sơ đối thủ nào liên quan tính năng này.
- Đọc `brain/knowledge/decisions/` xem đã từng quyết định gì liên quan.

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

Lưu raw data quan trọng vào `brain/research/<date>-<feature>-<user>/data/`.

## Bước 4 — Báo cáo + đề xuất
Viết `brain/research/<date>-<feature>-<user>/report.md`:
- **Bối cảnh:** tính năng gì, vì sao làm.
- **Bảng so sánh đối thủ:** ai có, làm thế nào, điểm mạnh/yếu.
- **Insight quảng cáo:** góc tiếp cận & message đối thủ đang dùng (từ ad creatives + Meta Ads).
- **Đề xuất cho app mình:** hướng làm khác biệt, ưu tiên, rủi ro.
Gợi ý chạy `brain-dashboard` để có bản HTML đẹp của report.

## Bước 5 — Ghi nhớ & cập nhật
- Cập nhật/ tạo file trong `brain/knowledge/competitors/` với insight mới (đổi `updated_by`).
- Ghi 1 quyết định stub vào `brain/knowledge/decisions/<date>-<slug>.md`.
- Cập nhật `brain/knowledge/INDEX.md`.
- Cập nhật `brain/state/<user>.json`: thêm tính năng vào danh sách đang làm, đặt tiến độ.

## Lưu ý
- Số liệu phải là **thật** từ MCP — không bịa. Nếu MCP lỗi/không có quyền, nói rõ và dùng web search thay.
- Không commit git trừ khi user yêu cầu.

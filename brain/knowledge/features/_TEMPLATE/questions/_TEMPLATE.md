---
feature: [[<feature-slug>]]
id: <slug-câu-hỏi>-<asker>      # vd expiry-format-an — KHÔNG dùng seq toàn cục (tránh race khi 2 người hỏi song song)
status: open                    # open | answered | wontfix
asker: <user>                   # ai hỏi
asker_role: Dev                 # Dev | Tester | BE | AI
resolver:                       # ai giải quyết (PO/AI/BE) — điền khi answered
resolver_role:
opened: YYYY-MM-DD HH:MM         # lấy bằng `date "+%Y-%m-%d %H:%M"`
answered:                       # YYYY-MM-DD HH:MM — khi chốt
tags: []                        # [api, ui, data, flow, edge-case, i18n, auth, money] — khoá dedup cross-feature
flowed_back: false              # đã chảy ngược vào feature.md mục 7 chưa
---
# Q: <tiêu đề câu hỏi 1 dòng>

## Bối cảnh (đã đọc gì mà vẫn bí)
- Đã đọc feature.md mục <?>, logic/<be-user>.md → vẫn chưa rõ vì ...

## AI đã thử trả lời  _(BẮT BUỘC — bằng chứng đã đọc trước khi hỏi; bỏ trống = không hợp lệ)_
- Suy luận từ ... nhưng không đủ chắc vì ... → escalate.

## Trao đổi  _(append-only, mỗi lượt ký tên + role + giờ — giống PUSH-LOG)_
- YYYY-MM-DD HH:MM — <user> (<role>) hỏi: ...

## Câu trả lời chốt  _(điền khi answered → chảy ngược feature.md mục 7)_
- <đáp án cuối, ngắn gọn>

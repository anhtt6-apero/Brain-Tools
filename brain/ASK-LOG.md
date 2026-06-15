# ❓ Nhật ký hỏi-đáp (ASK-LOG)

**Append-only.** Song sinh với `PUSH-LOG.md`. Mỗi lần **MỞ** (escalate) hoặc **CHỐT** một câu hỏi,
ghi **1 dòng**: AI hỏi · cho tính năng nào · LÚC NÀO · ai giải quyết · đáp gì.

Vai trò của file này:
1. **Dedup cross-feature:** trước khi escalate, `grep -i "<từ khoá>" brain/ASK-LOG.md` để bắt câu đã trả
   lời ở tính năng khác (auth, định dạng ngày, đơn vị tiền…).
2. **Skim nhanh:** liếc cuối file — dòng `OPEN <id>` chưa có `RESOLVED <id>` tương ứng = còn treo.

> ⚠️ **Nguồn sự thật để biết "còn câu nào chờ" = các file `questions/*.md` có `status: open`**
> (dashboard & skill `brain` quét trực tiếp). ASK-LOG chỉ bổ trợ — lỡ quên 1 dòng ở đây thì câu hỏi
> VẪN hiện trên dashboard, không "tàng hình".

> Quy trình ghi nằm trong skill `ask-po`: lấy giờ thật bằng `date "+%Y-%m-%d %H:%M"`, rồi append. KHÔNG tự commit.

**Format MỞ:**   `- YYYY-MM-DD HH:MM — <user> (<role>) — OPEN <id> [feature: <slug>] "<tiêu đề>" tags:[..]`
**Format CHỐT:** `- YYYY-MM-DD HH:MM — <resolver> (<role>) — RESOLVED <id> → "<đáp 1 dòng>"`
  _(câu bỏ: `RESOLVED-WONTFIX <id> → "<lý do>"`)_

---

<!-- Ví dụ (xoá khi có entry thật):
- 2026-06-15 14:30 — an (Dev) — OPEN expiry-format-an [feature: checkout] "Định dạng expiry MM/YY hay MM/YYYY?" tags:[ui,data]
- 2026-06-15 16:10 — conght (PO) — RESOLVED expiry-format-an → "MM/YY, validate client-side"
-->

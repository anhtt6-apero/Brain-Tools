# 📌 Nhật ký push (PUSH-LOG)

**Append-only.** Mỗi lần commit/push lên repo, ghi **1 dòng** để biết: AI push, push **CÁI GÌ**, **LÚC NÀO**.
Bổ trợ cho `git log` — dành cho người không muốn đọc git history.

> Quy trình ghi nằm trong skill `brain` → mục **"Commit / Push — xác nhận người push & ghi log"**:
> trước khi commit, Claude xác nhận danh tính (`git config user.email` khớp `@apero.vn`), lấy giờ thật
> bằng `date`, rồi append dòng dưới đây.

**Format:** `- YYYY-MM-DD HH:MM — <user> (<role>) — <tóm tắt thay đổi>`

---

- 2026-06-15 19:58 — anhtt6 (PO) — thêm luồng bàn giao theo vai trò (features/ + po-feature/dev-task/push-logic), onboarding + capability map, PUSH-LOG, cơ chế hỏi-đáp ask-po (questions/ + ASK-LOG); onboard anhtt6 (PO)

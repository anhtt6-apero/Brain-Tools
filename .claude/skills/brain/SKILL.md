---
name: brain
description: Operating manual cho bộ não tư duy Apero. Nạp ĐẦU MỖI PHIÊN khi làm việc trong repo này — xác định user, đọc focus hiện tại, quét trí nhớ, và áp quy ước đọc/ghi vùng chung vs riêng.
---

# Skill: brain — vận hành bộ não tư duy

Bộ não là kho markdown local-first, git-tracked, dùng chung cho team. Đây là sách hướng dẫn.

## Bước khởi động (chạy đầu mỗi phiên) — cũng là ONBOARDING lần đầu

1. **Tôi là ai?** — `git config user.email` → lấy phần trước `@` làm `<user>` (vd `ban@apero.vn` → `ban`).
   - ⚠️ **Guard:** nếu email rỗng HOẶC domain ≠ `apero.vn` → DỪNG và nhắc user chạy
     `git config user.email "ban@apero.vn"` trước. KHÔNG tự tạo hồ sơ "user lạ".
   - Đã có `brain/people/<user>.md` → đọc (lấy `role`).
   - **Lần đầu (chưa có hồ sơ):** HỎI tên + **vai trò** (`PO | Tester | Dev | BE | AI`) → tạo
     `people/<user>.md` từ template mục "Template". Đây chính là bước **onboard** người mới.
2. **Focus hiện tại** — đọc `brain/state/<user>.json` (đang làm tính năng gì, tiến độ).
3. **Trí nhớ** — đọc `brain/knowledge/INDEX.md` (sản phẩm, đối thủ, quyết định, **tính năng**).
4. **Tóm tắt + phổ biến quyền** — nói cho user: "Bạn là `<tên>` (`<role>`). Đang làm `<X>`, tiến độ `<Y%>`."
   rồi liệt kê **bạn làm được gì trong dự án này** theo đúng dòng vai trò ở bảng *Vai trò & quyền* bên dưới
   (+ gợi ý skill nên dùng). Xong mới hỏi/làm.
5. **Câu hỏi đang chờ** — 1 lệnh (quote path vì repo có dấu cách):
   `grep -rl 'status: open' "brain/knowledge/features/"*/questions/ 2>/dev/null`.
   - Nếu là **PO**: lọc feature có `po: <me>` → có câu chờ thì NHẮC ngay: "⚠️ Bạn có `<N>` câu hỏi đang
     chờ trả lời ở [[<slug>]] (`<id>`, …)" + gợi ý chạy `ask-po` luồng B.
   - Vai trò khác: nhắc 1 dòng — "Thắc mắc về tính năng → chạy `ask-po` (tự dedup), đừng hỏi trùng."

## Bản đồ thư mục

| Đường dẫn | Là gì | Vùng |
|---|---|---|
| `brain/people/<user>.md` | hồ sơ thành viên | chung (mỗi người 1 file) |
| `brain/state/<user>.json` | focus & tiến độ | **RIÊNG** — không sửa của người khác |
| `brain/knowledge/products/<slug>.md` | bối cảnh sản phẩm | chung |
| `brain/knowledge/competitors/<slug>.md` | hồ sơ đối thủ + insight quảng cáo | chung |
| `brain/knowledge/decisions/<date>-<slug>.md` | nhật ký quyết định | chung (append-only) |
| `brain/research/<date>-<feature>-<user>/` | output nghiên cứu | append-only |
| `brain/knowledge/features/<slug>/feature.md` | tài liệu tính năng (PO) — **nguồn sự thật** | chung |
| `brain/knowledge/features/<slug>/{ui,tests,tasks,logic}/` | output theo vai trò (PO/Tester/Dev/BE-AI) | chung; `tasks/`,`logic/` mỗi người 1 file |
| `brain/knowledge/features/<slug>/questions/<id>.md` | 1 câu hỏi escalate = 1 file (`<slug-câu>-<asker>`) | chung; tên duy nhất → không đụng |
| `brain/ASK-LOG.md` | nhật ký hỏi-đáp (ai hỏi · feature · lúc nào · ai chốt) | chung, append-only |
| `brain/PUSH-LOG.md` | nhật ký push (ai · lúc nào · gì) | chung, append-only |
| `brain/dashboard/` | HTML sinh ra | gitignore, local |

## Vai trò & quyền (capability map) — phổ biến lúc onboard

Không phân quyền cứng (tin team), nhưng mỗi vai trò có **việc & skill mặc định**. Đầu phiên, sau khi
nhận diện user → đọc `role` trong `people/<user>.md` → **phổ biến đúng dòng của họ**:

| Vai trò | Làm gì trong dự án | Skill | Đọc chính | Ghi vào |
|---|---|---|---|---|
| **PO** | Viết feature doc + UI preview; soi đối thủ; **giải đáp câu hỏi chờ** | `po-feature`, `research-competitor`, `ask-po` | `products/`, `research/`, `questions/` | `feature.md`, `ui/`, `questions/` (chốt) |
| **Tester** | Đọc feature doc → sinh test case; **hỏi PO khi bí** | `ask-po` _(+ skill test tự thêm sau)_ | `feature.md`, `questions/` | `tests/`, `questions/` |
| **Dev** | Đọc feature doc → gen task; **hỏi PO khi bí** | `dev-task`, `ask-po` | `feature.md`, `logic/`, `questions/` | `tasks/<user>.md`, `questions/` |
| **BE / AI** | Push logic đang xử lý làm tài liệu; **hỏi PO khi bí** | `push-logic`, `ask-po` | code của mình | `logic/<user>.md`, `questions/` |
| **Mọi vai trò** | Xem tiến độ; tự học | `brain-dashboard`, `brain` | toàn bộ | `state/<user>.json`, memory |

## Quy ước ghi file knowledge

Mọi file trong `knowledge/` mở đầu bằng frontmatter:

```markdown
---
name: <slug>
type: product | competitor | decision
author: <user tạo>
updated_by: <user sửa gần nhất>
date: YYYY-MM-DD
tags: [..]
---
```

- Liên kết các file bằng `[[slug]]`.
- Sau khi tạo/sửa file knowledge → cập nhật 1 dòng trong `brain/knowledge/INDEX.md`.
- Sửa file của người khác trong kho chung: đổi `updated_by` thành mình, giữ `author` gốc.

## Cập nhật state

`brain/state/<user>.json` (xem schema trong file mẫu). Sau mỗi cột mốc → cập nhật tiến độ,
rồi gợi ý user regenerate dashboard (`brain-dashboard`).

## Commit / Push — xác nhận người push & ghi log

**Claude KHÔNG tự commit.** Khi user yêu cầu commit/push, làm đúng thứ tự:

1. **Xác nhận danh tính:** `git config user.email` phải là `<user>@apero.vn` và khớp người đang làm.
   Lệch (vd email cá nhân) → **DỪNG**, báo user sửa `git config user.email`. Không push "nhân danh người khác".
2. **Lấy thời điểm thật:** chạy `date "+%Y-%m-%d %H:%M"`.
3. **Ghi PUSH-LOG:** append 1 dòng vào `brain/PUSH-LOG.md`:
   `- <YYYY-MM-DD HH:MM> — <user> (<role>) — <tóm tắt thay đổi>`
4. **Xác nhận với user 1 câu:** "Commit nhân danh `<user>` (`<role>`) lúc `<time>`: `<tóm tắt>` — ok?" rồi mới commit.
5. Commit message tiếng Việt, ngắn gọn; nêu tên người nếu nhiều người cùng vùng.

→ Mục đích: biết **ai push · lúc nào · cái gì** ngay cả khi không đọc `git log`.

## Quy tắc vàng

- Đọc `knowledge/` TRƯỚC khi hỏi user — đừng hỏi điều đã biết.
- Đầu phiên: nhận diện user → **phổ biến quyền theo vai trò** (bảng *Vai trò & quyền*). Lần đầu thì hỏi vai trò.
- Khi user nói "thêm tính năng X" → `research-competitor`.
- Khi **PO** nói "viết tài liệu tính năng / feature doc / UI preview" → `po-feature`.
- Khi **Dev** nói "gen task" → `dev-task`. Khi **BE/AI** nói "push logic" → `push-logic`.
- Khi user nói "xem tiến độ / vẽ dashboard" → `brain-dashboard`.
- **Đọc trước khi hỏi = dedup:** trước khi escalate thắc mắc cho PO, BẮT BUỘC (a) đọc `feature.md` mục 7 + `logic/`,
  (b) quét `questions/*.md` của tính năng, (c) `grep` `brain/ASK-LOG.md` cross-feature. Trùng & đã `answered` → trả lời lại, KHÔNG escalate.
- Khi Tester/Dev/BE/AI **bí sau khi đọc docs** → skill **`ask-po`** (tạo `questions/<id>.md` + ghi `ASK-LOG`). Câu trả lời **chảy ngược** vào `feature.md` mục 7.
- **Không tự commit git.** Commit/push → theo mục *"Commit / Push"* (xác nhận danh tính + ghi `PUSH-LOG.md`).

## Ghi nhớ / Chốt phiên (tự học) — làm bộ não thông minh hơn

Kích hoạt khi: user nói "ghi nhớ" / "cập nhật bộ não" / "chốt phiên"; HOẶC khi vừa chốt xong một
việc/quyết định đáng kể (chủ động làm, không đợi nhắc).

Rà lại cuộc trò chuyện, rút ra điều **bền vững & không hiển nhiên**, ghi đúng kho:

| Loại bài học | Ghi vào | Phạm vi |
|---|---|---|
| Về DỰ ÁN: sản phẩm, đối thủ, quyết định | `brain/knowledge/` (+ cập nhật `INDEX.md`) | chung, qua git |
| Về CÁCH LÀM VIỆC của user: sở thích, phản hồi, lằn ranh | memory riêng `~/.claude/.../memory/` (+ `MEMORY.md`) | riêng tư |

Nguyên tắc: chỉ lưu thứ phiên sau cần mà không suy ra được từ repo/git; dedup với cái đã có; viết
ngắn gọn. Không lưu vụn vặt hay thứ chỉ đúng trong phiên này.

## Template `people/<user>.md`

```markdown
---
name: <user>
role: <PO | Tester | Dev | BE | AI>
email: <email>
---
# <Tên>
- **Mảng phụ trách:** ...
- **Sản phẩm đang làm:** [[<product-slug>]]
```

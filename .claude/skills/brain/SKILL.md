---
name: brain
description: Operating manual cho bộ não tư duy Apero. Nạp ĐẦU MỖI PHIÊN khi làm việc trong repo này — xác định user, đọc focus hiện tại, quét trí nhớ, và áp quy ước đọc/ghi vùng chung vs riêng.
---

# Skill: brain — vận hành bộ não tư duy

Bộ não là kho markdown local-first, git-tracked, dùng chung cho team. Đây là sách hướng dẫn.

## Bước khởi động (chạy đầu mỗi phiên)

1. **Tôi là ai?** — `git config user.email` → lấy phần trước `@` làm `<user>` (vd `ban@apero.vn` → `ban`).
   - ⚠️ **Guard:** nếu email rỗng HOẶC domain ≠ `apero.vn` → DỪNG và nhắc user chạy
     `git config user.email "ban@apero.vn"` trước. KHÔNG tự tạo hồ sơ "user lạ".
   - Hợp lệ thì đọc `brain/people/<user>.md`. Nếu chưa có, tạo mới từ template ở mục "Template".
2. **Focus hiện tại** — đọc `brain/state/<user>.json` (đang làm tính năng gì, tiến độ).
3. **Trí nhớ** — đọc `brain/knowledge/INDEX.md` để biết đã có kiến thức gì (sản phẩm, đối thủ, quyết định).
4. Tóm tắt 1–2 câu cho user: "Đang làm <X>, tiến độ <Y%>" — rồi mới hỏi/làm.

## Bản đồ thư mục

| Đường dẫn | Là gì | Vùng |
|---|---|---|
| `brain/people/<user>.md` | hồ sơ thành viên | chung (mỗi người 1 file) |
| `brain/state/<user>.json` | focus & tiến độ | **RIÊNG** — không sửa của người khác |
| `brain/knowledge/products/<slug>.md` | bối cảnh sản phẩm | chung |
| `brain/knowledge/competitors/<slug>.md` | hồ sơ đối thủ + insight quảng cáo | chung |
| `brain/knowledge/decisions/<date>-<slug>.md` | nhật ký quyết định | chung (append-only) |
| `brain/research/<date>-<feature>-<user>/` | output nghiên cứu | append-only |
| `brain/dashboard/` | HTML sinh ra | gitignore, local |

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

## Quy tắc vàng

- Đọc `knowledge/` TRƯỚC khi hỏi user — đừng hỏi điều đã biết.
- **Không tự commit git.** Chỉ commit khi user yêu cầu rõ.
- Khi user nói "thêm tính năng X" → dùng skill `research-competitor`.
- Khi user nói "xem tiến độ / vẽ dashboard" → dùng skill `brain-dashboard`.

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
role: <vai trò>
email: <email>
---
# <Tên>
- **Mảng phụ trách:** ...
- **Sản phẩm đang làm:** [[<product-slug>]]
```

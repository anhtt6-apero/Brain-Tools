---
name: brain
description: Bộ luật & phân quyền của bộ não tư duy Apero. Nạp ĐẦU MỖI PHIÊN — nhận diện "bạn là ai", đọc version dự án đang chạy, phổ biến vai trò làm được gì (ghi đâu / xem gì / dùng skill nào), và áp quy ước vùng chung vs riêng.
---

# Skill: brain — bộ luật & phân quyền của bộ não tư duy

Bộ não là kho markdown local-first, git-tracked, dùng chung cho team. **Dự án triển khai theo VERSION**:
mỗi version của app = 1 folder trong `docs/`, bên trong chia theo **5 vai trò**, mỗi vai trò một folder
là nơi vai trò đó bỏ tài liệu/file của mình.

`brain/` định nghĩa phần "luật": **nhận diện người dùng → version đang chạy → phân quyền theo vai trò →
trí nhớ chung**. Đây là sách hướng dẫn brain đọc đầu mỗi phiên.

```
docs/<version>/<role>/   ← tài liệu dự án (po_docs · tester · dev_mobile · be_docs · ai_docs)
brain/                   ← luật + phân quyền + version + trí nhớ chung + state/people/logs
```

## A. Khởi động mỗi phiên ("start") — nhận diện & phổ biến quyền

> Đây chính là trải nghiệm *"clone về → mở phiên → brain hỏi bạn là ai → nói bạn làm được gì"*.
> `CLAUDE.md` ép chạy bước này TRƯỚC khi làm gì khác. Lần đầu chưa có hồ sơ = **onboard**.

1. **Bạn là ai?** — `git config user.email` → lấy phần trước `@` làm `<user>` (vd `ban@apero.vn` → `ban`).
   - ⚠️ **Guard:** email rỗng HOẶC domain ≠ `apero.vn` → DỪNG, nhắc `git config user.email "ban@apero.vn"`.
     KHÔNG tự tạo hồ sơ "user lạ".
   - Có `brain/people/<user>.md` → đọc (lấy `role`).
   - **Lần đầu (chưa có hồ sơ) → HỎI** tên + **vai trò** (`PO | Tester | Dev | BE | AI`) → tạo
     `people/<user>.md` từ template §J. Đây là bước **onboard**.
2. **Dự án đang version nào?** — đọc `brain/project.json` → `current_version` (vd `v1.4.0`). Đây là
   nguồn sự thật dùng chung: mọi tài liệu mới rơi vào version này.
3. **Focus của bạn** — đọc `brain/state/<user>.json` (đang làm gì, tiến độ).
4. **Trí nhớ chung** — đọc `brain/knowledge/INDEX.md` (sản phẩm, đối thủ, quyết định — xuyên version).
5. **Phổ biến quyền** — nói cho user đúng 1 đoạn, theo dòng vai trò của họ ở bảng §C:
   > Bạn là `<tên>` (`<role>`). Dự án đang ở `<current_version>`.
   > • **Ghi vào:** `docs/<current_version>/<role-folder>/`
   > • **Xem được:** `<scope>`
   > • **Dùng được:** `<skill + lệnh tiêu biểu>`

   Xong mới hỏi/làm.
6. **Câu hỏi đang chờ (QA)** — quét `brain/qa/` có `status: open`:
   `grep -rl 'status: open' brain/qa/ 2>/dev/null`.
   - **PO:** có câu chờ → NHẮC ngay ("⚠️ Có `<N>` câu hỏi đang chờ PO trả lời") + gợi ý `ask-po`.
   - Vai trò khác: "Thắc mắc → `ask-po` (tự dedup), đừng hỏi trùng."

## B. Version đang hoạt động — `brain/project.json`

`brain/project.json` là **nguồn sự thật dùng chung** cho "dự án đang ở version nào":

```json
{
  "current_version": "v1.4.0",
  "versions": ["v1.3.0", "v1.4.0"],
  "roles": ["po_docs", "tester", "dev_mobile", "be_docs", "ai_docs"],
  "updated": "YYYY-MM-DD"
}
```

- **Định tuyến tự động:** bất kỳ ai "tạo docs / bỏ file" → ghi vào `docs/<current_version>/<role-folder>/`.
  Không ai phải nhớ version.
- **Muốn đụng version khác** (vd fix `v1.3.0`) → nói rõ version → ghi vào đúng folder đó.
- **Lên version mới** (PO làm): tạo `docs/<vX>/` + 5 folder vai trò, thêm vào `versions`, đổi
  `current_version`, cập nhật `updated` (lấy giờ thật bằng `date`).
- ⚠️ Phân biệt: `project.json` = **chung** (version dự án) · `state/<user>.json` = **riêng** (focus của bạn).

## C. Vai trò, quyền & định tuyến (capability map)

> **Không phân quyền cứng** (markdown + git, không enforce kỹ thuật) — đây là **quy ước mềm + briefing
> đầu phiên + attribution**, tin tưởng team (theo spec gốc: không RBAC). "Quyền" = *được kỳ vọng làm gì*.

`<ver>` = `current_version` trong `project.json`.

| Vai trò | Ghi vào | Xem được (scope) | Skill | Việc / lệnh tiêu biểu |
|---|---|---|---|---|
| **PO** | `docs/<ver>/po_docs/` | **TOÀN dự án** — mọi version, mọi folder vai trò + `brain/knowledge/` | `po-feature`, `research-competitor`, `ask-po` | Viết tài liệu tính năng; xem task Dev / logic BE-AI trong version; **quản `current_version`** (lên version mới) |
| **Tester** | `docs/<ver>/tester/` | `docs/<ver>/po_docs/` (mô tả dự án) | _(skill test — thêm sau)_, `ask-po` | Đọc mô tả → gen file test case |
| **Dev (mobile)** | `docs/<ver>/dev_mobile/` | `po_docs/` + `be_docs/` + `ai_docs/` (cùng version) | `dev-task`, `ask-po` | Đọc docs → gen task / triển khai |
| **BE** | `docs/<ver>/be_docs/` | `docs/<ver>/po_docs/` | `push-logic`, `ask-po` | Push logic backend để các bên tham khảo |
| **AI** | `docs/<ver>/ai_docs/` | `docs/<ver>/po_docs/` | `push-logic`, `ask-po` | Push logic AI để các bên tham khảo |

- **PO là vai trò duy nhất nhìn toàn cảnh** + đổi version dự án.
- Vai trò khác **ghi trong folder của mình**, **đọc `po_docs`** làm nguồn; Dev đọc thêm logic BE/AI.
- Bí sau khi đọc docs → skill `ask-po` (escalate cho PO).

## ❓ Hỏi-đáp (QA), nghiên cứu & trí nhớ dự án

**QA — bí khi đọc docs (skill `ask-po`).** Store dùng chung `brain/qa/` (1 câu = 1 file, lưu vĩnh viễn
để khỏi hỏi lại). Trước khi tạo câu mới, BẮT BUỘC check theo thứ tự:
1. **Docs version HIỆN TẠI** (`docs/<current_version>/`, ưu tiên `po_docs/`) — ưu tiên số 1.
2. **`brain/qa/`** — đã có ai hỏi chưa (answered → trả lời lại).
3. **Docs version CŨ** (`docs/<version cũ>/`).
4. Hết vẫn không có → tạo `brain/qa/<slug>-<asker>.md` (status: open) + ghi `ASK-LOG`.
PO trả lời → ghi đáp vào file QA; chốt chính thức về tính năng → cập nhật `docs/<ver>/po_docs/`.

**Nghiên cứu đối thủ/thị trường (skill `research-competitor`).** Raw + report → `brain/research/`.
**Lý do CHỐT làm / KHÔNG làm tính năng** → `brain/knowledge/decisions/` (để sau biết vì sao). Insight đối
thủ → `brain/knowledge/competitors/`. Tính năng chốt làm → PO viết vào `docs/<current_version>/po_docs/`.

**Trí nhớ dự án xuyên version.** `brain/knowledge/products/` tích lũy hiểu biết về sản phẩm qua nhiều
version — viết để AI/người sau đọc standalone là hiểu, không phải đọc lại toàn bộ docs.

## D. Bản đồ thư mục (vùng chung vs riêng)

| Đường dẫn | Là gì | Vùng |
|---|---|---|
| `brain/project.json` | version dự án đang chạy + danh sách version/role | **chung** (1 nguồn sự thật) |
| `brain/people/<user>.md` | hồ sơ thành viên (tên, vai trò) | chung (mỗi người 1 file) |
| `brain/state/<user>.json` | focus & tiến độ | **RIÊNG** — không sửa của người khác |
| `brain/knowledge/products\|competitors\|decisions/` | trí nhớ **xuyên version** (sản phẩm, đối thủ, quyết định) | chung |
| `brain/knowledge/INDEX.md` | mục lục trí nhớ chung | chung |
| `brain/research/<date>-<chủ-đề>-<user>/` | output nghiên cứu | append-only |
| `brain/qa/<slug>-<asker>.md` | 1 câu hỏi escalate cho PO = 1 file (QA store — chống hỏi lại) | chung; tên duy nhất |
| `docs/<version>/<role>/` | tài liệu dự án theo version & vai trò | chung (mỗi vai trò 1 folder) |
| `brain/ASK-LOG.md` | nhật ký hỏi-đáp | chung, append-only |
| `brain/PUSH-LOG.md` | nhật ký push (ai · lúc nào · gì) | chung, append-only |
| `brain/dashboard/` | HTML sinh ra | gitignore, local |

## E. Quy ước ghi tài liệu

- **`brain/knowledge/`** (trí nhớ chung): mỗi file mở đầu bằng frontmatter
  `name` / `type` (product|competitor|decision) / `author` / `updated_by` / `date` / `tags`.
  Liên kết bằng `[[slug]]`. Tạo/sửa xong → cập nhật 1 dòng trong `brain/knowledge/INDEX.md`.
  Sửa file người khác → đổi `updated_by`, giữ `author` gốc.
- **`docs/<ver>/<role>/`**: nên có frontmatter nhẹ `version` / `role` / `author` / `updated_by` / `date`
  để truy vết. Quy ước chi tiết per-role do skill của từng vai trò định (đang cập nhật cho cấu trúc mới).

## F. Cập nhật state

`brain/state/<user>.json` = focus RIÊNG của bạn. Sau mỗi cột mốc → cập nhật tiến độ, rồi gợi ý
regenerate dashboard (`brain-dashboard`). KHÔNG để version dự án ở đây — version nằm ở `project.json`.

## G. Commit / Push — xác nhận người push & ghi log

**Claude KHÔNG tự commit.** Khi user yêu cầu commit/push, làm đúng thứ tự:

1. **Xác nhận danh tính:** `git config user.email` phải là `<user>@apero.vn` và khớp người đang làm.
   Lệch (vd email cá nhân) → **DỪNG**, báo user sửa. Không push "nhân danh người khác".
2. **Lấy thời điểm thật:** chạy `date "+%Y-%m-%d %H:%M"`.
3. **Ghi PUSH-LOG:** append 1 dòng vào `brain/PUSH-LOG.md`:
   `- <YYYY-MM-DD HH:MM> — <user> (<role>) — <tóm tắt thay đổi>`
4. **Xác nhận với user 1 câu** rồi mới commit. Commit message tiếng Việt, ngắn gọn.

## H. Quy tắc vàng

- Đọc `brain/knowledge/` + `docs/<ver>/po_docs/` TRƯỚC khi hỏi — đừng hỏi điều đã biết.
- Đầu phiên: nhận diện user → đọc `project.json` → **phổ biến quyền theo vai trò** (§C). Lần đầu thì hỏi vai trò.
- Mọi tài liệu mới → định tuyến vào `docs/<current_version>/<role-folder>/` (§B). Version khác phải nói rõ.
- Khi user nói "thêm tính năng X" → `research-competitor`. PO "viết tài liệu tính năng" → `po-feature`.
  Dev "gen task" → `dev-task`. BE/AI "push logic" → `push-logic`. "Xem tiến độ" → `brain-dashboard`.
- Bí sau khi đọc docs → `ask-po`: check docs hiện tại → `brain/qa/` → docs cũ → chưa có mới tạo QA.
- **Không tự commit git.** Commit/push → theo §G (xác nhận danh tính + ghi `PUSH-LOG.md`).

## I. Ghi nhớ / Chốt phiên (tự học)

Kích hoạt khi user nói "ghi nhớ" / "chốt phiên", HOẶC khi vừa chốt một việc/quyết định đáng kể.
Rà cuộc trò chuyện, rút điều **bền vững & không hiển nhiên**, ghi đúng kho:

| Loại bài học | Ghi vào | Phạm vi |
|---|---|---|
| Về DỰ ÁN: sản phẩm, đối thủ, quyết định | `brain/knowledge/` (+ `INDEX.md`) | chung, qua git |
| Về CÁCH LÀM VIỆC của user: sở thích, phản hồi, lằn ranh | memory riêng `~/.claude/.../memory/` | riêng tư |

Chỉ lưu thứ phiên sau cần mà không suy ra được từ repo/git; dedup với cái đã có; viết ngắn gọn.

## J. Template `people/<user>.md`

```markdown
---
name: <user>
role: <PO | Tester | Dev | BE | AI>
email: <email>
---
# <Tên>
- **Mảng phụ trách:** ...
- **Vai trò trong dự án:** xem bảng §C của skill `brain`.
```

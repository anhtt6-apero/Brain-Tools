---
name: ask-po
description: Cơ chế hỏi-đáp (QA) cho Tester/Dev/BE/AI khi đọc docs version hiện tại mà VẪN không hiểu / không rõ / thắc mắc về tính năng X → escalate có cấu trúc cho PO; và khi PO (hoặc bất kỳ ai giải được) trả lời thì chốt vào file QA + chảy ngược vào po_docs. Kích hoạt khi user "không hiểu / không rõ / thắc mắc / hỏi PO / escalate", hoặc "có câu hỏi nào chờ tôi không / trả lời câu hỏi đang chờ". ÉP đọc-trước-khi-hỏi (DEDUP-GATE) trước khi tạo câu mới.
---

# Skill: ask-po — cơ chế hỏi-đáp (QA) & escalate cho PO

Dùng khi **Tester / Dev / BE / AI** đọc tài liệu version đang chạy mà **vẫn bí** về một tính năng.
Mục tiêu: hỏi có cấu trúc, đáp đọng lại vĩnh viễn → **không ai phải hỏi lại câu đã có**.

- **QA store dùng chung:** `brain/qa/` — **1 câu hỏi = 1 file** `brain/qa/<slug-câu>-<asker>.md`.
  Lưu **vĩnh viễn** → trí nhớ chống hỏi trùng (kể cả khi đã `answered`).
- **Version là trục chính:** đọc `brain/project.json` → `current_version` (vd `v1.4.0`). Tài liệu mỗi vai
  trò ở `docs/<current_version>/<role>/` — `po_docs · tester · dev_mobile · be_docs · ai_docs`.
  **`po_docs/` là nguồn sự thật** về tính năng.
- **Nhật ký bổ trợ:** `brain/ASK-LOG.md` (append-only) — chỉ để dedup nhanh, KHÔNG phải nơi đếm.
- **Nguồn sự thật "còn câu nào chờ" = các file `brain/qa/*.md` có `status: open`** (dashboard & skill
  `brain` đầu phiên quét trực tiếp). Lỡ quên 1 dòng ASK-LOG thì câu hỏi vẫn hiện, không "tàng hình".
- ⚠️ Repo có **DẤU CÁCH** trong path → mọi lệnh shell phải **quote** đường dẫn tuyệt đối.

> ❌ Mô hình CŨ đã BỎ: không còn `brain/knowledge/features/<slug>/questions/` và không còn
> "feature.md mục 7". Mọi tham chiếu kiểu đó là sai.

---

## A) LUỒNG HỎI — khi Tester/Dev/BE/AI bí

### Bước 0 — Bối cảnh
Xác định `current_version` (đọc `brain/project.json`) và `asker` + `role` (đọc `brain/people/<user>.md`).

### Bước 1 — DEDUP-GATE (BẮT BUỘC, check ĐÚNG THỨ TỰ — chưa qua thì KHÔNG được tạo QA mới)

Khi có câu hỏi, lần lượt:

1. **Docs version HIỆN TẠI** — `docs/<current_version>/` — **ƯU TIÊN SỐ 1.**
   - Đọc trước `po_docs/` (nguồn sự thật tính năng).
   - Câu kỹ thuật → đọc kèm folder liên quan: `be_docs/` và/hoặc `ai_docs/` (logic), `dev_mobile/` (task).
   - Suy ra được đáp án → **trả lời tại chỗ, DỪNG** (không tạo file). Trích nguồn `(theo <file>)`.

2. **`brain/qa/`** — đã có ai hỏi câu này chưa?
   - `grep -ril "<từ khoá>" "<repo>/brain/qa/"` (quote path).
   - Có file `status: answered` trùng ý → **trả lời lại** từ mục "## Đáp án", trích
     `(theo <slug>, <resolver> chốt <date>)`. **KHÔNG tạo mới.**
   - Có file `status: open` trùng → **KHÔNG tạo trùng**; append vào "## Trao đổi" của file đó
     `- <giờ thật> — <user> (<role>) +1 cũng vướng`; báo "đã có người hỏi, đang chờ trả lời".

3. **Docs version CŨ** — `docs/<version cũ>/` (lấy danh sách từ `versions` trong `project.json`).
   - Có khi tính năng đã được giải thích ở version trước → tái dùng. Trích `(theo <version cũ>/<file>)`.

4. **Check hết 1→3 vẫn không có** → sang **Bước 2** tạo QA mới.

### Bước 2 — Tạo file QA mới + escalate

1. `date "+%Y-%m-%d %H:%M"` → lấy giờ thật.
2. `slug = <slug-câu-hỏi-ngắn-gọn>`; tên file `brain/qa/<slug>-<asker>.md`
   _(gắn `-<asker>` để 2 người hỏi song song không đè/không race)._
3. Tạo file theo template §D: điền frontmatter (`id`, `slug`, `asker`, `role`, `version`,
   `status: open`, `resolver: ""`, `date`) + mục **Bối cảnh** + **Đã tự kiểm tra ở đâu**
   (BẮT BUỘC — bằng chứng đã chạy DEDUP-GATE: đọc po_docs / quét qa / xem version cũ rồi vẫn bí).
4. Append **1 dòng OPEN** vào `brain/ASK-LOG.md` (format trong file đó).
5. Báo người hỏi: "Đã ghi QA `<slug>-<asker>` (version `<current_version>`) — PO sẽ thấy đầu phiên / qua dashboard."

> KHÔNG đếm số câu chờ ở đâu cả — số đó là **DERIVED**: dashboard & `brain` tự đếm file `qa/*.md` `status: open`.

---

## B) LUỒNG TRẢ LỜI & CHẢY NGƯỢC — PO, hoặc bất kỳ ai giải được

1. Mở file QA cần trả lời (`brain/qa/<slug>-<asker>.md`).
2. Điền **`resolver`** + **`role`** (vai trò người giải) + **`answered`** (giờ thật từ `date`) vào
   frontmatter; viết đáp vào mục **"## Đáp án"**; đổi **`status: answered`**.
3. Append **1 dòng RESOLVED** vào `brain/ASK-LOG.md`.
4. **Nếu là chốt chính thức về tính năng → chảy ngược vào docs** để người sau tự thấy, khỏi hỏi lại:
   - PO cập nhật `docs/<current_version>/po_docs/<feature>.md` (mục nghiệp vụ liên quan), bump
     `updated_by` + `date`. Đây là cách đáp án "đọng" vào nguồn sự thật.
   - Đáp làm đổi nghiệp vụ cốt lõi → PO sửa thẳng phần mô tả tính năng trong `po_docs/`, không chỉ ghi phụ lục.
5. **Non-PO tự giải** (Dev/BE/AI/Tester giải được câu kỹ thuật): vẫn ghi đáp + `resolver`/`role` vào
   file QA và đổi `status: answered`, nhưng ghi rõ trong "## Đáp án": **"(CHỜ PO XÁC NHẬN nếu là chốt
   nghiệp vụ)"**. PO duyệt sau → bỏ nhãn chờ + (nếu cần) chảy ngược vào `po_docs/`.

**Attribution trong mỗi file QA:** `asker` · `resolver` · `role` · `opened` (=`date`) · `answered` (giờ chốt).

---

## C) wontfix

Câu không còn áp dụng (scope đổi / tính năng bị loại): resolver ghi lý do vào "## Đáp án", đổi
`status: wontfix`, điền `resolver`/`answered`, append `RESOLVED-WONTFIX <slug>` vào `ASK-LOG.md`.
Không cần chảy ngược po_docs. Giữ file lại làm vết.

---

## D) Template file QA — `brain/qa/<slug>-<asker>.md`

```markdown
---
id: <slug>-<asker>
slug: <slug-câu-hỏi-ngắn-gọn>
asker: <user>
role: <Tester | Dev | BE | AI | PO>
version: <version đang hỏi, vd v1.4.0>
status: open            # open | answered | wontfix
resolver: ""            # điền khi có người trả lời
date: <YYYY-MM-DD HH:MM>   # opened — giờ thật từ `date`
answered: ""            # điền giờ thật khi chốt
---

# <Tiêu đề câu hỏi 1 dòng>

## Bối cảnh
- Tính năng / docs liên quan: `docs/<version>/po_docs/<feature>.md` (+ logic nếu có)
- Đang vướng chỗ nào, cần quyết định gì.

## Đã tự kiểm tra ở đâu  (bằng chứng đã chạy DEDUP-GATE — BẮT BUỘC)
- po_docs hiện tại: <đã đọc gì, vẫn thiếu gì>
- brain/qa/: <không thấy câu trùng>
- docs version cũ: <không thấy / không áp dụng>

## Đáp án
<để trống — người giải điền; ghi (CHỜ PO XÁC NHẬN) nếu non-PO chốt nghiệp vụ>

## Trao đổi
- <giờ> — <user> (<role>) +1 cũng vướng   ← chỉ append khi có người trùng câu open
```

---

## E) PO biết có QA chờ bằng cách nào
- **Đầu phiên** (skill `brain`): quét `brain/qa/*.md` `status: open` → nhắc PO.
- **Dashboard** (skill `brain-dashboard`): liệt kê câu đang chờ.
- **`brain/ASK-LOG.md`:** liếc cuối file, dòng `OPEN <slug>` chưa có `RESOLVED <slug>` = còn treo (bổ trợ dedup).

## F) Lưu ý
- **Không RBAC:** ai cũng tạo / giải được, chỉ cần `asker` / `resolver` đúng người. Tin team.
- File QA giữ **vĩnh viễn** = trí nhớ chống hỏi trùng. `brain/qa/` nhiều quá → dồn câu `answered`/`wontfix`
  cũ sang `brain/qa/archive/` (vẫn grep được khi dedup).
- **Không tự commit git** — chỉ khi user yêu cầu (theo §G skill `brain`).

---
name: ask-po
description: Khi Tester/Dev/BE/AI đọc feature.md + logic mà VẪN không hiểu / không rõ / thắc mắc về tính năng X thì escalate có cấu trúc cho PO; và khi PO (hoặc bất kỳ ai) trả lời thì chốt + chảy ngược vào feature.md. Kích hoạt khi user "không hiểu / không rõ / thắc mắc / hỏi PO / escalate" về một tính năng, hoặc "có câu hỏi nào chờ tôi không / trả lời câu hỏi đang chờ". ÉP đọc-trước-khi-hỏi (dedup) trước khi tạo câu mới.
---

# Skill: ask-po — escalate & giải đáp thắc mắc tính năng

- **1 câu hỏi = 1 file:** `brain/knowledge/features/<slug>/questions/<slug-câu>-<asker>.md`.
- **Nhật ký cross-feature** (dedup + skim): `brain/ASK-LOG.md` (append-only, song sinh PUSH-LOG).
- **Câu trả lời đọng lại:** `feature.md` mục `## 7. Hỏi đáp đã chốt`.
- **Nguồn sự thật để notify = các file `questions/*.md` có `status: open`** (dashboard & `brain` đầu phiên
  quét trực tiếp). ASK-LOG chỉ là tiện ích, KHÔNG phải nơi đếm.
- ⚠️ Repo có **DẤU CÁCH** trong path → mọi lệnh shell phải **quote** đường dẫn tuyệt đối.

## A) LUỒNG HỎI (Tester/Dev/BE/AI bí)

### Bước 1 — Thử trả lời từ docs (đọc trước khi hỏi, cấp 1)
Đọc `features/<slug>/feature.md` (mục 4/5/6 + **mục 7 Hỏi đáp đã chốt**) và `logic/*.md`.
Suy ra được → trả lời tại chỗ, **DỪNG** (không tạo file).

### Bước 2 — Dedup (GATE bắt buộc — không qua thì KHÔNG được tạo câu mới)
1. Quét `features/<slug>/questions/*.md`:
   - Có câu `status: answered` trùng ý → trả lời lại từ "## Câu trả lời chốt", trích nguồn
     `(theo <id>, <resolver> chốt <date>)`. KHÔNG escalate.
   - Có câu `status: open` trùng → KHÔNG tạo trùng; append `- <giờ> — <user> (<role>) +1 cũng vướng`
     vào "## Trao đổi" của file đó; báo "đã có người hỏi, đang chờ PO".
2. Cross-feature: `grep -i "<từ khoá/tag>" "<repo>/brain/ASK-LOG.md"` → câu hệ thống (auth, định dạng
   ngày, tiền tệ…) đã trả lời ở feature khác → tái dùng đáp án.
3. Cả 3 trống → Bước 3.

### Bước 3 — Tạo câu hỏi + escalate
1. `date "+%Y-%m-%d %H:%M"` lấy giờ thật.
2. `id = <slug-câu-hỏi>-<asker>` (KHÔNG dùng seq toàn cục → 2 người hỏi song song không trùng/không race).
3. Copy `_TEMPLATE/questions/_TEMPLATE.md` → `questions/<id>.md`; điền `id`, `asker`, `asker_role`,
   `opened`, `tags`, mục **Bối cảnh** + **AI đã thử trả lời** (BẮT BUỘC — bằng chứng đã đọc docs).
4. Append 1 dòng **OPEN** vào `brain/ASK-LOG.md`.
5. Báo người hỏi: "Đã ghi `<id>` — PO sẽ thấy đầu phiên/qua dashboard."

> KHÔNG bump counter ở feature.md — số câu chờ là **DERIVED**, dashboard & `brain` tự đếm bằng `grep`.

## B) LUỒNG TRẢ LỜI (PO, hoặc AI/bất kỳ ai tự giải được)
1. Mở file Q; điền `resolver` + `resolver_role` + `answered` (giờ thật) + "## Câu trả lời chốt"; đổi `status: answered`.
2. Append 1 dòng **RESOLVED** vào `brain/ASK-LOG.md`.
3. **Chảy ngược NGAY** vào `feature.md` mục 7 (kể cả khi người trả lời KHÔNG phải PO):
   - PO chốt: `- (<id>) Hỏi:… · Đáp:… — chốt bởi <user> (PO) <date> · [[questions/<id>]]`, set `flowed_back: true`.
   - Non-PO/AI tự giải: `- (<id>) Hỏi:… · Đáp:… — trả lời bởi <user> (<role>), **CHỜ PO XÁC NHẬN** · [[questions/<id>]]`,
     set `flowed_back: true`. PO duyệt sau chỉ đổi nhãn "CHỜ PO XÁC NHẬN" → "chốt bởi <po> (PO)".
   → Đáp án + attribution hiện **ngay** cho người đọc; vẫn giữ vết chờ-duyệt.
4. Đáp làm đổi nghiệp vụ cốt lõi → PO sửa thẳng `feature.md` mục 4/5/6, bump `updated_by`+`date`; mục 7 là phụ lục truy vết.

## C) wontfix
Câu không còn áp dụng (scope đổi / đã loại): resolver điền lý do vào "## Câu trả lời chốt", đổi
`status: wontfix`, append `RESOLVED-WONTFIX <id>` vào ASK-LOG để người hỏi truy được. Không cần chảy ngược mục 7.

## Lưu ý
- Không RBAC: ai cũng tạo/giải được, chỉ cần `asker`/`resolver` đúng. Tin team.
- File Q giữ vĩnh viễn = **trí nhớ**; `questions/` nhiều quá có thể dồn `status: answered` cũ sang `questions/archive/`.
- Không tự commit git.

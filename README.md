# 🧠 Brain-Tools — Bộ não tư duy cho Apero

Bộ não thứ hai dùng chung cho cả team, **local-first + git**. Dự án triển khai theo **version**;
mỗi version có tài liệu của 5 vai trò: **PO · Tester · Dev mobile · BE · AI**.

Bạn không cần nhớ quy tắc gì — mở repo bằng **Claude Code**, nó tự đọc `CLAUDE.md`, **hỏi bạn là ai**
rồi **nói cho bạn biết vai trò của bạn làm được gì**.

## ⚡ Bắt đầu (3 bước)

```bash
git clone https://github.com/anhtt6-apero/Brain-Tools.git
cd Brain-Tools
git config user.email "ban@apero.vn"   # bộ não nhận diện bạn qua email — phải @apero.vn
```

Rồi **mở thư mục trong Claude Code** và nói chuyện bình thường. Lần đầu, bộ não sẽ hỏi **vai trò**
của bạn và phổ biến: *bạn xem được gì · ghi vào đâu · dùng skill nào.*

## 🗂️ Cấu trúc (gọn)

```
docs/<version>/<vai-trò>/   # tài liệu dự án theo version & vai trò
                            #   po_docs · tester · dev_mobile · be_docs · ai_docs
brain/                      # bộ luật, phân quyền, version đang chạy, trí nhớ chung, QA
```

## 🚀 Dùng hằng ngày (cứ nói bình thường)

| Bạn nói… | Bộ não làm |
|---|---|
| *"muốn thêm tính năng X"* | Nghiên cứu đối thủ/thị trường → đề xuất (PO) |
| *"viết tài liệu tính năng"* | PO viết docs vào `po_docs/` của version hiện tại |
| *"gen task"* | Dev mobile sinh task từ tài liệu PO |
| *"push logic"* | BE/AI ghi logic để các bên tham khảo |
| *"không hiểu / thắc mắc"* | Hỏi PO (QA) — tự kiểm tra để khỏi hỏi trùng |
| *"vẽ dashboard"* | Xem tiến độ cả team |

> Chi tiết vận hành & phân quyền: Claude tự đọc `CLAUDE.md` + skill `brain` mỗi phiên.
> Không phân quyền cứng — quy ước mềm + tin team + ghi nhận tác giả (attribution).

# 🧠 Brain Tools — Bộ não tư duy cho Apero

Một **"bộ não thứ hai"** dùng chung cho team khi làm product: tích lũy kiến thức, tự **nghiên cứu
đối thủ** (kể cả quảng cáo họ đang chạy) khi bắt đầu một tính năng, và hiển thị tiến độ qua
**dashboard HTML động**. Tất cả là markdown + git, **local-first** — Claude đọc trực tiếp nên càng
dùng càng "nhớ".

> Mở repo này bằng **Claude Code**, nó tự đọc `CLAUDE.md` và biết cách vận hành bộ não. Bạn chỉ cần
> nói chuyện bình thường, ví dụ: *"muốn thêm tính năng luyện phát âm"*.

---

## ⚡ Bắt đầu nhanh (3 bước)

```bash
# 1. Clone
git clone https://github.com/anhtt6-apero/Brain-Tools.git
cd Brain-Tools

# 2. Cài skill vẽ HTML đẹp (effective-html) — dùng cho dashboard
npx skills add plannotator/effective-html

# 3. Khai báo bạn là ai (bộ não nhận diện người qua git email)
git config user.email "ban@apero.vn"
git config user.name  "Tên bạn"
```

> ⚠️ **Set email `@apero.vn` TRƯỚC khi mở Claude Code.** Bộ não nhận diện bạn qua email này; để
> trống hoặc dùng email cá nhân sẽ tạo hồ sơ "người lạ" và làm bộ não phân mảnh.

Sau đó **mở thư mục này trong Claude Code** rồi nói: *"đọc CLAUDE.md đi"* hoặc chỉ cần bắt đầu việc —
nó sẽ tự nạp skill `brain`, biết bạn là ai và đang làm gì.

**Yêu cầu:** [Claude Code](https://claude.com/claude-code) · Node.js (cho `npx`) · git.
**Tùy chọn (để soi đối thủ bằng số liệu thật):** MCP **SensorTower** + **Meta Ads** kết nối trong Claude.

---

## 🚀 Dùng hằng ngày

| Bạn nói… | Bộ não làm gì |
|---|---|
| *"muốn thêm tính năng X"* | Chạy skill **`research-competitor`**: nhớ lại kiến thức cũ → hỏi gọn → soi đối thủ (SensorTower + Meta Ads + web) → ra **report + đề xuất** → ghi nhớ & cập nhật tiến độ |
| **PO:** *"viết tài liệu tính năng X" / "gen UI preview"* | Skill **`po-feature`**: tạo `features/<slug>/feature.md` (nguồn sự thật) + `ui/preview.html` |
| **Dev:** *"gen task cho tính năng X"* | Skill **`dev-task`**: đọc feature doc → `features/<slug>/tasks/<bạn>.md` |
| **BE/AI:** *"push logic"* | Skill **`push-logic`**: ghi `features/<slug>/logic/<bạn>.md` để các bên tham khảo |
| **Tester:** đọc feature doc → sinh test | Đọc `features/<slug>/feature.md` → ghi vào `tests/` _(skill riêng của Tester thêm sau)_ |
| **Mọi vai trò:** đọc docs vẫn *"không hiểu / thắc mắc"* | Skill **`ask-po`**: AI thử trả lời từ docs → dedup → escalate cho PO (`questions/<id>.md` + `ASK-LOG`). PO thấy qua dashboard; đáp án **chảy ngược** vào `feature.md` mục 7 để khỏi ai hỏi lại |
| *"xem tiến độ" / "vẽ dashboard"* | Chạy skill **`brain-dashboard`**: sinh `brain/dashboard/index.html` (mở bằng trình duyệt) |
| *"ghi nhớ đi"* / chốt xong việc | **Tự học**: rút bài học → lưu vào `brain/knowledge/` (về dự án) hoặc memory (về cách bạn làm việc) để phiên sau hiểu hơn |
| (bắt đầu phiên bất kỳ) | Skill **`brain`** xác định bạn qua git email (lần đầu hỏi **vai trò**), đọc focus & trí nhớ, **phổ biến bạn làm được gì**, tóm tắt "đang làm gì" |

---

## ⌨️ Câu lệnh gọi skill (slash command)

Bạn có thể **nói chuyện bình thường** (bộ não tự chọn skill) — hoặc gõ thẳng lệnh trong Claude Code:

| Lệnh | Ai dùng | Làm gì |
|---|---|---|
| `/brain` | mọi người | Nạp operating manual: nhận diện bạn, đọc focus, **nhắc câu hỏi đang chờ** (tự chạy đầu phiên, hiếm khi cần gõ tay) |
| `/research-competitor` | PO | Soi đối thủ cho 1 tính năng (SensorTower + Meta Ads + web) → report + đề xuất |
| `/po-feature` | PO | Viết `feature.md` (nguồn sự thật) + gen `ui/preview.html`; giải đáp câu hỏi chờ |
| `/dev-task` | Dev | Đọc feature doc → sinh `tasks/<bạn>.md` |
| `/push-logic` | BE/AI | Ghi `logic/<bạn>.md` (hợp đồng input→output) cho bên khác tham khảo |
| `/ask-po` | mọi người | Bí khi đọc docs → dedup → escalate câu hỏi cho PO; **PO dùng lệnh này để trả lời** câu chờ |
| `/brain-dashboard` | mọi người | Sinh `brain/dashboard/index.html` |

> Gõ `/` trong Claude Code để xem toàn bộ danh sách. Lần đầu vào repo: set `git config user.email "ban@apero.vn"`
> rồi chỉ cần bắt đầu trò chuyện — skill `brain` tự onboard (hỏi vai trò) và phổ biến bạn làm được gì.

---

## 🗂️ Cấu trúc

```
Brain-Tools/
├── CLAUDE.md                     # Entry point — Claude đọc mỗi phiên
├── brain/
│   ├── people/<user>.md          # Hồ sơ thành viên (tự tạo lần đầu nếu chưa có)
│   ├── state/<user>.json         # Focus & tiến độ RIÊNG mỗi người  ← không sửa của người khác
│   ├── knowledge/                # Trí nhớ CHUNG
│   │   ├── INDEX.md              #   mục lục quét nhanh
│   │   ├── products/             #   bối cảnh sản phẩm (+ _TEMPLATE.md)
│   │   ├── competitors/          #   hồ sơ đối thủ + insight quảng cáo (+ _TEMPLATE.md)
│   │   ├── decisions/            #   nhật ký quyết định (+ _TEMPLATE.md)
│   │   └── features/<slug>/      #   1 folder / tính năng — luồng bàn giao theo vai trò
│   │       ├── feature.md        #     PO: nguồn sự thật (+ mục 7 "Hỏi đáp đã chốt")
│   │       ├── ui/ tests/ tasks/ logic/   # PO / Tester / Dev / BE-AI
│   │       └── questions/<id>.md #     1 câu hỏi escalate = 1 file (skill ask-po)
│   ├── research/                 # Output nghiên cứu: <date>-<feature>-<user>/report.md
│   ├── PUSH-LOG.md               # Nhật ký push: ai · lúc nào · gì (append-only)
│   ├── ASK-LOG.md                # Nhật ký hỏi-đáp: ai hỏi · feature · ai chốt (append-only)
│   └── dashboard/                # HTML sinh ra (GIT BỎ QUA — tự generate)
├── .claude/skills/               # Thư viện skill
│   ├── brain/                    #   operating manual (onboard + đọc/ghi + commit/push + nhắc câu hỏi chờ)
│   ├── research-competitor/      #   pipeline soi đối thủ
│   ├── brain-dashboard/          #   generate dashboard
│   ├── po-feature/               #   PO: feature doc + UI preview + giải đáp câu hỏi
│   ├── dev-task/                 #   Dev: gen task từ feature doc
│   ├── push-logic/               #   BE/AI: push logic tham khảo
│   └── ask-po/                   #   Mọi vai trò: escalate thắc mắc → PO (dedup + ghi nhớ)
└── docs/superpowers/specs/       # Bản thiết kế chi tiết
```

---

## 👥 Làm việc nhiều người (qua git)

Thiết kế để **không xung đột merge**:

| Vùng | Cơ chế | Quy ước |
|---|---|---|
| `brain/state/<user>.json` | mỗi người **1 file riêng** | Chỉ sửa file của mình |
| `brain/knowledge/**` | kho **chung**, 1 file / 1 thực thể | Sửa file người khác → đổi `updated_by`, giữ `author` gốc |
| `brain/research/<...>-<user>/` | append-only, tên gắn người tạo | Không sửa research của người khác |
| `brain/dashboard/` | **gitignore** | Mỗi người tự generate, không commit |

- **Bạn là ai?** Bộ não lấy từ `git config user.email` (phần trước `@`). Lần đầu chưa có hồ sơ →
  skill `brain` tự tạo `brain/people/<user>.md`.
- **Attribution:** mọi file knowledge có frontmatter `author` / `updated_by` / `date` + lịch sử git.

---

## 🎭 Vai trò & luồng bàn giao tính năng

Lần đầu mở bộ não, skill `brain` hỏi **vai trò** của bạn rồi phổ biến bạn làm được gì. Mọi vai trò
xoay quanh **1 folder / tính năng** (`brain/knowledge/features/<slug>/`), trong đó `feature.md` của PO
là **nguồn sự thật**:

| Vai trò | Làm gì | Skill | Output |
|---|---|---|---|
| **PO** | Viết feature doc + UI preview, soi đối thủ | `po-feature`, `research-competitor` | `feature.md`, `ui/` |
| **Tester** | Đọc feature doc → sinh test case | _(skill Tester thêm sau)_ | `tests/` |
| **Dev** | Đọc feature doc → gen task | `dev-task` | `tasks/<bạn>.md` |
| **BE / AI** | Push logic đang xử lý làm tài liệu tham khảo | `push-logic` | `logic/<bạn>.md` |

```
research/  →  PO: feature.md + ui/   →  ┬→ Tester: tests/
                  (nguồn sự thật)        ├→ Dev: tasks/<user>.md
                                         └  BE/AI: push logic/<user>.md  (để mọi người tham khảo)
```

> ❓ **Bí khi đọc docs?** Bất kỳ ai chạy `ask-po` → câu hỏi vào `questions/` + `ASK-LOG`, PO thấy ngay trên dashboard/đầu phiên, đáp án **chảy ngược** vào `feature.md` mục 7 để khỏi ai hỏi lại.

> Không phân quyền cứng — đây là **quy ước mềm + attribution** (tin team), đúng tinh thần MVP.

---

## 📊 Dashboard

```
# Trong Claude Code:  "vẽ dashboard"
# Rồi mở:
open brain/dashboard/index.html      # macOS
```

Hiển thị: **dây chuyền tư duy** động (task → 🧠 → kết quả nhả ra từng bước), tiến độ từng người,
research gần đây, bản đồ đối thủ, thống kê kiến thức. Có dark mode. File này **không commit** (gitignore).

---

## ✍️ Quy ước commit

- **Claude không tự commit** — chỉ commit khi bạn yêu cầu rõ.
- **Khi push:** bộ não xác nhận `git config user.email` khớp `@apero.vn` & đúng người, rồi ghi
  **`brain/PUSH-LOG.md`** (`thời điểm — người — tóm tắt`) để biết *ai push · lúc nào · gì*.
- Commit knowledge/decision/research dùng tiếng Việt, ngắn gọn, nói rõ thêm/sửa gì.
- Không commit `brain/dashboard/`, `.agents/`, secret/API key.

---

## 🛣️ Lộ trình

- **Giai đoạn 1 (hiện tại):** local-first + git, 7 skill (vận hành + soi đối thủ + bàn giao theo vai trò PO/Dev/BE-AI + hỏi-đáp), dashboard động.
- **Giai đoạn 2:** publish dashboard/knowledge sang Notion/Drive cho team xem online (hybrid).
- **Giai đoạn 3:** phân quyền nhẹ + automation (vd nghiên cứu định kỳ đối thủ).

Thiết kế đầy đủ: [`docs/superpowers/specs/2026-06-14-bo-nao-tu-duy-design.md`](docs/superpowers/specs/2026-06-14-bo-nao-tu-duy-design.md).

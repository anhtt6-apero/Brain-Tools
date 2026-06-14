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
| *"xem tiến độ" / "vẽ dashboard"* | Chạy skill **`brain-dashboard`**: sinh `brain/dashboard/index.html` (mở bằng trình duyệt) |
| *"ghi nhớ đi"* / chốt xong việc | **Tự học**: rút bài học → lưu vào `brain/knowledge/` (về dự án) hoặc memory (về cách bạn làm việc) để phiên sau hiểu hơn |
| (bắt đầu phiên bất kỳ) | Skill **`brain`** xác định bạn qua git email, đọc focus & trí nhớ rồi tóm tắt "đang làm gì" |

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
│   │   └── decisions/            #   nhật ký quyết định (+ _TEMPLATE.md)
│   ├── research/                 # Output nghiên cứu: <date>-<feature>-<user>/report.md
│   └── dashboard/                # HTML sinh ra (GIT BỎ QUA — tự generate)
├── .claude/skills/               # Thư viện skill
│   ├── brain/                    #   operating manual
│   ├── research-competitor/      #   pipeline soi đối thủ
│   └── brain-dashboard/          #   generate dashboard
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
- Commit knowledge/decision/research dùng tiếng Việt, ngắn gọn, nói rõ thêm/sửa gì.
- Không commit `brain/dashboard/`, `.agents/`, secret/API key.

---

## 🛣️ Lộ trình

- **Giai đoạn 1 (hiện tại):** local-first + git, 3 skill, dashboard động.
- **Giai đoạn 2:** publish dashboard/knowledge sang Notion/Drive cho team xem online (hybrid).
- **Giai đoạn 3:** phân quyền nhẹ + automation (vd nghiên cứu định kỳ đối thủ).

Thiết kế đầy đủ: [`docs/superpowers/specs/2026-06-14-bo-nao-tu-duy-design.md`](docs/superpowers/specs/2026-06-14-bo-nao-tu-duy-design.md).

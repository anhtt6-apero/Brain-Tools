# Spec — Bộ não tư duy (Thinking Brain) cho Apero

- **Ngày:** 2026-06-14
- **Tác giả:** cong (conght@apero.vn)
- **Trạng thái:** Đã duyệt khung — chờ review spec
- **Loại:** Local-first, git-tracked, multi-user

---

## 1. Mục tiêu

Xây một "bộ não thứ hai" mà Claude đọc mỗi phiên để hỗ trợ làm product: tích lũy kiến thức,
tự nghiên cứu đối thủ (kể cả quảng cáo họ chạy) khi bắt đầu một tính năng, và hiển thị tiến độ
qua dashboard HTML trực quan. Cả team cùng bồi đắp một bộ não chung qua git.

**Năm trụ cột:**
1. 🅰 Trí nhớ tích lũy (knowledge base)
2. 🅱 Workflow nghiên cứu đối thủ
3. 🅲 Dashboard tiến độ (HTML)
4. 🅳 Thư viện skill
5. 🅴 Bàn giao tính năng theo vai trò (feature delivery) — _bổ sung 2026-06-15, xem §13_

**Nguyên tắc nền:** Local-first markdown + git. Claude đọc trực tiếp file (không qua API rời rạc)
nên truy cập nhanh và liền mạch. Multi-user thực hiện qua git, không cần server.

## 2. Phi mục tiêu (YAGNI)

- Không xây server/backend hay UI web động ở MVP.
- Không đồng bộ Notion/cloud ở MVP (để giai đoạn 2 — hybrid).
- Không phân quyền cứng (RBAC); dùng attribution + quy ước, tin tưởng team.

## 3. Cấu trúc thư mục

```
Brain English Apero/
├── CLAUDE.md                       # Entry point — nạp mỗi phiên; trỏ tới skill `brain`
├── brain/
│   ├── people/<user>.md            # Hồ sơ thành viên (tên, vai trò, mảng phụ trách)
│   ├── state/<user>.json           # Focus & tiến độ RIÊNG mỗi người (chống đụng độ git)
│   ├── knowledge/                  # 🅰 Trí nhớ tích lũy (kho CHUNG)
│   │   ├── INDEX.md                #    mục lục để quét nhanh
│   │   ├── products/<slug>.md      #    bối cảnh sản phẩm (app tiếng Anh...)
│   │   ├── competitors/<slug>.md   #    1 file / đối thủ + insight quảng cáo
│   │   ├── decisions/<date>-<slug>.md  # nhật ký quyết định (append-only)
│   │   └── features/<slug>/        # 🅴 1 folder / tính năng (xem §13)
│   │       ├── feature.md          #    PO — nguồn sự thật
│   │       ├── ui/ tests/ tasks/ logic/  # output PO / Tester / Dev / BE-AI
│   ├── research/                   # 🅱 Output nghiên cứu
│   │   └── <date>-<feature>-<user>/    # report.md + data/ (raw SensorTower/Meta)
│   ├── PUSH-LOG.md                 # nhật ký push (ai · lúc nào · gì) — append-only
│   └── dashboard/                  # 🅲 HTML sinh ra (GITIGNORE — regenerate local)
└── .claude/skills/                 # 🅳 Thư viện skill (theo repo, share qua git)
    ├── brain/                      #    operating manual: onboard, đọc/ghi, commit/push
    ├── research-competitor/        #    workflow nghiên cứu đối thủ
    ├── brain-dashboard/            #    generate dashboard
    ├── po-feature/                 # 🅴 PO: feature doc + UI preview
    ├── dev-task/                   # 🅴 Dev: gen task từ feature doc
    └── push-logic/                 # 🅴 BE/AI: push logic tham khảo
```

## 4. Trụ cột 🅰 — Trí nhớ tích lũy

- **Đơn vị:** mỗi file = 1 thực thể (1 đối thủ / 1 sản phẩm / 1 quyết định).
- **Frontmatter bắt buộc:** `name`, `type` (product|competitor|decision), `author`, `updated_by`, `date`, `tags`.
- **Liên kết:** dùng `[[slug]]` để nối các file (vd 1 decision trỏ tới competitor liên quan).
- **INDEX.md:** danh sách 1 dòng/file (tiêu đề + mô tả ngắn) để Claude quét nhanh đầu phiên.
- **Competitor file** chứa: tổng quan app, số liệu (tải/doanh thu ước tính), keyword ASO,
  insight quảng cáo (creative/copy/góc tiếp cận), điểm mạnh/yếu, cập nhật gần nhất.

## 5. Trụ cột 🅱 — Workflow nghiên cứu đối thủ (skill `research-competitor`)

Kích hoạt khi user nói "muốn thêm tính năng X". Pipeline 5 bước (đã duyệt, chạy một mạch):

1. **Nhớ lại** — đọc `knowledge/` xem đã biết gì về sản phẩm & đối thủ liên quan; không hỏi lại điều đã biết.
2. **Hỏi gọn 1–2 câu** — chỉ phần còn thiếu (phân khúc? đối thủ chỉ định hay tự tìm top?).
3. **Nghiên cứu (dữ liệu thật):**
   - **SensorTower:** top app cùng mảng, app có tính năng tương tự, lượt tải/doanh thu ước tính,
     ad creatives, keyword ASO, retention, nhân khẩu.
   - **Meta Ads:** quảng cáo đối thủ đang chạy cho tính năng đó (hình ảnh, copy, góc tiếp cận).
   - **Web search:** cách đối thủ làm tính năng (UX, công nghệ, định giá).
4. **Báo cáo + đề xuất** — `research/<date>-<feature>-<user>/report.md`: bảng so sánh đối thủ,
   insight quảng cáo, **đề xuất hướng làm khác biệt cho app mình**. Kèm bản HTML tóm tắt.
5. **Ghi nhớ & cập nhật** — lưu insight mới vào `knowledge/competitors/`, ghi decision vào
   `decisions/`, cập nhật `state/<user>.json` → dashboard cập nhật tiến độ.

## 6. Trụ cột 🅲 — Dashboard (skill `brain-dashboard` + effective-html)

- Sinh `dashboard/index.html` theo phong cách effective-html (`html` + `html-diagram`).
- **Nguồn dữ liệu:** gom tất cả `state/*.json` + quét `knowledge/`, `research/`.
- **Hiển thị:** focus & tiến độ TỪNG người (cả team), report gần đây, **bản đồ đối thủ** (SVG),
  thống kê kiến thức. Có chế độ xem riêng từng người.
- **Regenerate** theo lệnh; file HTML được gitignore (mỗi người tự sinh, tránh xung đột merge).

## 7. Trụ cột 🅳 — Thư viện skill

- **`brain`** (meta-skill): operating manual. Đầu phiên: xác định user qua `git config user.email`,
  nạp `people/<user>.md` + `state/<user>.json`, đọc `knowledge/INDEX.md`. Quy ước đọc/ghi, vùng
  chung vs riêng, attribution.
- **`research-competitor`**: pipeline mục 5.
- **`brain-dashboard`**: generate dashboard mục 6.
- Mở rộng sau dễ dàng (vd `aso-research`, `feature-spec`).

## 8. Multi-user (qua git)

| Vùng | Cơ chế | Rủi ro đụng độ |
|---|---|---|
| `state/<user>.json` | mỗi người 1 file | Không |
| `knowledge/*` | kho chung, 1 file/thực thể, frontmatter `updated_by` | Hiếm |
| `decisions/`, `research/<...>-<user>/` | append-only, tên gắn người | Gần như không |
| `dashboard/` | gitignore, regenerate local | Không commit |

- **Nhận diện:** `git config user.email` → map tới `people/<user>.md`. Không đăng nhập.
- **Attribution:** frontmatter (`author`, `updated_by`) + lịch sử git.
- **Không RBAC** ở MVP: tin tưởng team + quy ước.

## 9. Vòng lặp một phiên

1. Mở phiên → đọc `CLAUDE.md` → nạp `brain` → biết user + focus hiện tại.
2. User: "làm tính năng X" → chạy `research-competitor`.
3. Đề xuất hướng → ghi decision.
4. Cập nhật `state/<user>.json` → regenerate dashboard.
5. Commit git.

## 10. Phụ thuộc

- MCP: **SensorTower**, **Meta Ads** (nghiên cứu đối thủ + quảng cáo).
- Skill ngoài: **effective-html** (`html`, `html-diagram`, `html-plan`) — cần cài.
- Web search/fetch cho nghiên cứu tính năng.

## 11. Lộ trình

- **MVP (giai đoạn 1):** cấu trúc thư mục + skill `brain` + `research-competitor` + `brain-dashboard`
  + CLAUDE.md + người dùng đầu tiên (cong). Tất cả local + git.
- **Giai đoạn 2 (hybrid):** publish dashboard/knowledge sang Notion/Drive cho team xem online.
- **Giai đoạn 3:** phân quyền nhẹ, automation thêm (vd nghiên cứu định kỳ đối thủ).

## 12. Tiêu chí thành công

- Mở phiên mới, Claude tự biết "đang làm gì" mà không cần nhắc lại.
- Nói "thêm tính năng X" → ra được report đối thủ + đề xuất, có số liệu thật từ SensorTower/Meta.
- Dashboard HTML mở được, thấy tiến độ cả team.
- Hai người cùng push trong tuần mà không xung đột vùng `state`.

---

## 13. Bổ sung 2026-06-15 — Trụ cột 🅴: Bàn giao tính năng theo vai trò

Mở rộng bộ não từ "1 PO nghiên cứu" sang **luồng bàn giao đa vai trò** (PO · Tester · Dev · BE · AI),
lấy **tính năng làm trung tâm**: mỗi tính năng = 1 folder, các vai trò gắn artifact của mình vào.

### 13.1 Cấu trúc feature (hub)
```
brain/knowledge/features/<slug>/
├── feature.md   # 🟦 PO — NGUỒN SỰ THẬT (mô tả, user story, scope, luồng, acceptance criteria)
├── ui/          # 🟦 PO — UI preview HTML (effective-html), preview.html
├── tests/       # 🟩 Tester — test case (đọc mục 5 acceptance criteria)
├── tasks/       # 🟨 Dev — tasks/<user>.md (mỗi dev 1 file)
└── logic/       # 🟧 BE/AI — logic/<user>.md (push hợp đồng input→output)
```
- "1 folder = 1 tính năng" — mở rộng tự nhiên từ "1 file = 1 thực thể".
- **Luồng:** `research/` → PO viết `feature.md` + `ui/` → Tester/Dev đọc feature.md sinh output;
  BE/AI **push** `logic/` để các bên tham khảo (BE/AI không bắt buộc đọc feature.md).
- File theo người (`tasks/<user>.md`, `logic/<user>.md`) → multi-user không đụng git, như `state/`.

### 13.2 Vai trò & skill
| Vai trò | Skill | Đọc | Ghi |
|---|---|---|---|
| PO | `po-feature` | products, research | `feature.md` + `ui/` |
| Tester | _(tự thêm sau)_ | `feature.md` (acceptance criteria) | `tests/` |
| Dev | `dev-task` | `feature.md`, `logic/` | `tasks/<user>.md` |
| BE/AI | `push-logic` | code của mình | `logic/<user>.md` |

Quyết định UI preview = **effective-html** (đồng bộ dashboard, git-friendly). Format test case để
**skill riêng của Tester** quyết định sau — folder `tests/` + hợp đồng (đọc `feature.md`) đã giữ sẵn.

### 13.3 Onboarding & phổ biến quyền
Đầu phiên skill `brain` nhận diện user qua git email; **lần đầu** thì hỏi tên + vai trò rồi tạo hồ sơ
(`role: PO|Tester|Dev|BE|AI`), sau đó **phổ biến "bạn làm được gì"** theo bảng *Vai trò & quyền*.

### 13.4 Xác nhận push & nhật ký
Vẫn "không tự commit". Khi commit/push: (1) xác nhận `git config user.email` khớp `@apero.vn` & đúng
người; (2) lấy giờ thật bằng `date`; (3) append `brain/PUSH-LOG.md`: `YYYY-MM-DD HH:MM — user (role)
— tóm tắt`; (4) xác nhận 1 câu với user rồi commit. Mục đích: biết **ai · lúc nào · gì** không cần đọc git log.

> Vẫn theo §2 (không RBAC cứng) — capability map là **quy ước mềm + attribution**, không chặn quyền.

### 13.5 Vòng hỏi-đáp khi docs chưa giải đáp (skill `ask-po`)
Khi Tester/Dev/BE/AI đọc `feature.md` + `logic/` vẫn không hiểu → **không hỏi miệng rồi quên** mà chạy `ask-po`:

- **1 câu hỏi = 1 file** `features/<slug>/questions/<slug-câu>-<asker>.md` (id không seq → không race; tên duy nhất → không đụng git).
- **Dedup-gate bắt buộc** trước khi tạo câu mới: đọc `feature.md` mục 7 + `logic/` → quét `questions/*.md` cùng tính năng → `grep brain/ASK-LOG.md` cross-feature. Trùng & đã `answered` → trả lời lại, không escalate. Gate này được nhúng vào chính `dev-task`/`push-logic` (điểm vào tự nhiên), không chỉ ở `ask-po`.
- **PO biết** qua 2 surface đọc-thẳng-nguồn (`questions/*.md` status:open): nhắc đầu phiên (skill `brain`) + band "❓ Câu hỏi đang chờ" trên dashboard. `brain/ASK-LOG.md` (append-only, song sinh PUSH-LOG) chỉ bổ trợ dedup/skim — **không** phải nguồn đếm (tránh "tàng hình" nếu quên ghi).
- **Ghi nhớ + attribution:** file Q lưu `asker`/`resolver` + role + `opened`/`answered` → biết *ai hỏi · ai giải · khi nào*. Giữ vĩnh viễn = trí nhớ chống hỏi lại.
- **Chảy ngược:** đáp án vào `feature.md` mục `## 7. Hỏi đáp đã chốt` (người đọc sau tự thấy). Non-PO/AI tự giải được ghi ngay kèm nhãn "CHỜ PO XÁC NHẬN"; PO duyệt sau chỉ đổi nhãn. `wontfix` cho câu hết áp dụng.
- **Không counter trong feature.md:** số câu chờ là DERIVED (grep), tránh git-conflict + giữ `feature.md` 1-chủ (chỉ PO ghi nội dung).

Thiết kế này ra từ workflow design-panel (4 hướng → tổng hợp → phản biện), 2026-06-15.

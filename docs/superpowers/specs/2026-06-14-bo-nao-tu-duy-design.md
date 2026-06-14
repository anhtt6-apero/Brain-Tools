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

**Bốn trụ cột:**
1. 🅰 Trí nhớ tích lũy (knowledge base)
2. 🅱 Workflow nghiên cứu đối thủ
3. 🅲 Dashboard tiến độ (HTML)
4. 🅳 Thư viện skill

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
│   │   └── decisions/<date>-<slug>.md  # nhật ký quyết định (append-only)
│   ├── research/                   # 🅱 Output nghiên cứu
│   │   └── <date>-<feature>-<user>/    # report.md + data/ (raw SensorTower/Meta)
│   └── dashboard/                  # 🅲 HTML sinh ra (GITIGNORE — regenerate local)
└── .claude/skills/                 # 🅳 Thư viện skill (theo repo, share qua git)
    ├── brain/                      #    operating manual: cách đọc/ghi bộ não
    ├── research-competitor/        #    workflow nghiên cứu đối thủ
    └── brain-dashboard/            #    generate dashboard
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

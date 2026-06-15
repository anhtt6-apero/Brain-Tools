# 🧪 tests/ — Test cases (Tester)

Vùng **output của Tester** cho tính năng này.

## Hợp đồng (cho skill Tester tương lai ráp vào)
- **ĐỌC:** `../feature.md` — đặc biệt **mục 5. Acceptance criteria** và **mục 4. Luồng & màn hình**.
- **SINH:** file test case vào thư mục này. Format do **skill riêng của Tester** quyết định
  (vd bảng Markdown `testcases.md`, Gherkin `*.feature`, hoặc CSV để import TestRail/Qase…).
- **Mỗi tester 1 file** nếu cần chia: `testcases-<user>.md` → tránh đụng git.
- **Frontmatter nên có:** `feature: [[<feature-slug>]]`, `role: Tester`, `author`, `date`.

> Tester sẽ tự thêm skill sinh test sau. Folder + hợp đồng này giữ sẵn để skill đó cắm vào không phải nghĩ lại cấu trúc.

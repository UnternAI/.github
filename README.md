# `unternai/.github` — repo cấu hình chung của org

Repo **public** đặc biệt của GitHub: nội dung ở đây được GitHub tự áp cho cả org. Không đặt bất cứ thứ gì
bí mật hay nội bộ ở đây — mọi người trên internet đều đọc được. Nội dung chỉ dành cho thành viên nằm ở
`unternai/.github-private`.

## Repo này làm gì

| Đường dẫn | Tác dụng |
|---|---|
| `profile/README.md` | Org profile công khai, hiện ở `github.com/unternai` cho mọi người |
| `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`, `SECURITY.md`, `SUPPORT.md` | Community health files **mặc định** cho mọi repo của org chưa có file cùng loại |
| `.github/ISSUE_TEMPLATE/` (`bug_report.yml`, `feature_request.yml`, `config.yml`) | Issue forms mặc định; tắt blank issue, thêm contact links (security, support, conduct) |
| `.github/pull_request_template.md` | PR template mặc định |
| `workflow-templates/*.yml` + `*.properties.json` | Workflow templates hiện trong tab **Actions → New workflow** của mọi repo trong org |
| `.github/workflows/reusable-python-ci.yml` | Reusable workflow (`on: workflow_call`) cho CI Python: `uv sync --locked`, ruff check, ruff format --check, pytest |
| `.github/workflows/lint-workflows.yml` | CI của chính repo này: actionlint (+ shellcheck) trên workflows và workflow templates; job tên `ci` |

## Hành vi GitHub cần biết (đã kiểm tra trên docs.github.com, 2026-11)

- **Community health files mặc định áp cho mọi repo của org, kể cả private**, nếu repo đó chưa có file cùng
  loại (trong `.github/`, root hoặc `docs/`). Repo `.github` **bắt buộc public**.
  Nguồn: [Creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file).
- **Issue templates là "tất cả hoặc không"**: nếu một repo có `.github/ISSUE_TEMPLATE/` hợp lệ của riêng nó thì
  toàn bộ thư mục mặc định ở đây bị bỏ qua (kể cả `config.yml`).
- **Label trong issue form phải tồn tại** ở cả repo `.github` và repo dùng template. Hai form ở đây dùng
  `type/bug` và `type/feature` từ `templates/labels.json` (bootstrap phase 4 tạo label cho mọi repo, kể cả
  `.github`); nếu đổi bộ label thì giữ hai label này hoặc sửa `labels:` trong form. Repo tạo sau bootstrap
  (kể cả từ template) chỉ có label mặc định của GitHub — chạy lại bootstrap hoặc
  `gh label clone unternai/.github --repo unternai/<repo>`.
- **Không thể** tạo `LICENSE` mặc định cho org — mỗi repo public tự thêm `LICENSE`.
- **Org profile**: public README ở `profile/README.md` của repo public `.github`; README chỉ thành viên thấy ở
  `profile/README.md` của repo **private** `.github-private`. Thành viên đăng nhập mặc định thấy bản member.
  Nguồn: [Customizing your organization's profile](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/customizing-your-organizations-profile).
- **Workflow templates** trong repo `.github` public dùng được cho mọi loại repo (public và private). Mỗi template
  cần file `<tên>.properties.json` cùng tên (`name`, `description` bắt buộc; `iconName`, `categories`,
  `filePatterns` tùy chọn). `$default-branch` được thay bằng nhánh mặc định khi tạo workflow.
  Nguồn: [Creating workflow templates](https://docs.github.com/en/actions/how-tos/reuse-automations/create-workflow-templates),
  [Reusing workflow configurations](https://docs.github.com/en/actions/reference/workflows-and-actions/reusing-workflow-configurations).
- **Repo private gọi được reusable workflow trong repo public** của org (caller private dùng được workflow private
  và public; caller public chỉ dùng được workflow public). Điều kiện: Actions policy của repo caller phải cho phép
  actions và reusable workflows của `unternai`.
- **Secrets không tự chuyển vào reusable workflow** — caller phải truyền rõ (`secrets:`), template ở đây truyền
  đúng một secret `PRIVATE_DEPS_APP_PRIVATE_KEY`. `GITHUB_TOKEN` trong workflow được gọi chỉ có thể giảm quyền.
- **Org secrets/variables không dùng được cho private repo trên gói GitHub Free** — cần Team trở lên, hoặc đặt
  secret ở từng repo.

## Required status check `ci`

Rulesets của org yêu cầu đúng **một** check tên `ci`. Job trong reusable workflow được báo cáo dưới dạng
`<job caller> / <job được gọi>` (ví dụ `python / python-ci`), nên caller luôn có thêm một **job gate tên `ci`**
(`needs: [...]`, `if: always()`, fail nếu job con không `success`). Xem `workflow-templates/python-ci.yml`.

## Thay đổi reusable workflow

Caller tham chiếu `@main` (không pin SHA) vì repo này do chính org quản lý và chưa tồn tại lúc viết template.
Vì vậy mọi thay đổi ở `main` có hiệu lực ngay với **mọi** repo:

1. Thay đổi tương thích ngược (thêm input có default, sửa bug): merge bình thường qua PR.
2. Thay đổi phá vỡ: tạo file mới (`reusable-python-ci-v2.yml`) hoặc tag (`v1`, `v2`) và để repo caller tự chuyển.
3. Pin action bằng full commit SHA kèm comment `# vX.Y.Z`; nâng version bằng Renovate/Dependabot.

Lint cục bộ trước khi push:

```sh
uvx --with shellcheck-py --from actionlint-py actionlint .github/workflows/*.yml workflow-templates/*.yml
```

## Sau khi đổi placeholder

Thay câu mô tả trong `profile/README.md`, kiểm tra các địa chỉ `security@`, `conduct@`, `support@` đã tồn tại
(xem `docs/03-email-identity/`), và bật **Private vulnerability reporting** cho các repo public.

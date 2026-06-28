---
title: "Chuẩn bị"
date: 2026-04-20
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### Tổng quan

Module này chuẩn bị máy local và tài khoản AWS để build và deploy DocuFlow AI. Kết thúc module, bạn sẽ có đủ tool cần thiết, một AWS profile cấu hình cho `ap-southeast-1`, đã bật model access của Amazon Bedrock, và một SAM template đã validate.

Phần setup này chỉ làm một lần. Mọi module sau đều giả định các bước này đã hoàn tất.

#### Tool cần cài

Cài các tool sau trên máy. Version là mức tối thiểu; bản patch mới hơn vẫn dùng được.

| Tool | Version | Mục đích |
|---|---|---|
| Node.js | 20.x LTS | Runtime cho frontend build và Lambda functions |
| pnpm | 9.x | Package manager cho monorepo |
| Git | 2.40+ | Quản lý version |
| AWS CLI | v2 (mới nhất) | Xác thực và gọi AWS API |
| AWS SAM CLI | bản 1.x mới nhất | Build, validate và deploy serverless stack |
| Docker | 24+ (tùy chọn) | Chỉ cần cho `sam local` và `sam build --use-container` |

Kiểm tra từng tool:

```bash
node --version      # v20.x
pnpm --version      # 9.x
git --version       # 2.40+
aws --version       # aws-cli/2.x
sam --version       # SAM CLI, version 1.x
docker --version    # tùy chọn: Docker version 24+
```

{{% notice tip %}}
Nếu chưa có `pnpm`, bật qua Corepack đi kèm Node.js 20: `corepack enable && corepack prepare pnpm@9 --activate`.
{{% /notice %}}

{{% notice info %}}
Docker **không** bắt buộc cho phát triển bình thường. Các Lambda function của chúng ta là TypeScript bundle bằng esbuild, nên `sam build` chạy native không cần container. Chỉ cài Docker nếu bạn muốn giả lập Lambda local bằng `sam local invoke` hoặc `sam local start-api`.
{{% /notice %}}

#### Bước 1 - Cấu hình truy cập AWS

DocuFlow AI deploy lên **`ap-southeast-1` (Singapore)**, region gần Việt Nam nhất. DocuFlow AI coi access key dài hạn là một rủi ro bảo mật (xem risk R-06 và R-08 trong proposal), nên cách xác thực được khuyến nghị là **AWS IAM Identity Center** (credential ngắn hạn, tự refresh và không nằm trên đĩa).

**Phương án A - IAM Identity Center (khuyến nghị)**

```bash
aws configure sso --profile docuflow
```

Làm theo hướng dẫn để nhập SSO start URL và region, sau đó chọn account và permission set. Đặt CLI default region là `ap-southeast-1` và output là `json`. Đăng nhập lại mỗi khi session hết hạn:

```bash
aws sso login --profile docuflow
```

**Phương án B - Access key (chỉ khi không dùng được Identity Center)**

Nếu account không dùng được Identity Center, dùng tạm access key và xoá sau khi xong workshop:

```bash
aws configure --profile docuflow
# Default region name: ap-southeast-1
# Default output format: json
```

Xác nhận identity và region với cả hai phương án:

```bash
aws sts get-caller-identity --profile docuflow
aws configure get region --profile docuflow   # ap-southeast-1
```

{{% notice warning %}}
Không bao giờ commit credential vào Git. Repository đã có sẵn `gitleaks` pre-commit hook (xem module 5.7) và `.gitignore` loại trừ các file `.env`. Với CI/CD, workshop dùng GitHub OIDC thay cho mọi key lưu trữ. Nếu bạn dùng Phương án B, xoá access key trong IAM console khi workshop kết thúc.
{{% /notice %}}

#### Bước 2 - Quyền IAM

Tài khoản hoặc role dùng cho workshop cần quyền tạo và xoá các dịch vụ trong kiến trúc DocuFlow. Với tài khoản workshop cá nhân, policy tương đương administrator là đơn giản nhất. Với tài khoản dùng chung hoặc bị giới hạn, gắn policy scope theo các dịch vụ sau:

```text
cloudformation, s3, cognito-idp, apigateway, lambda, states,
dynamodb, events, sqs, sns, textract, bedrock, cloudfront,
logs, cloudwatch, iam, kms, budgets, ssm
```

Vì một SAM stack provision toàn bộ resource, principal deploy cũng cần `iam:CreateRole`, `iam:PassRole` và `cloudformation:*` trên stack.

#### Bước 3 - Bật Model Access của Amazon Bedrock

Model Bedrock phải opt-in theo từng region. Bật access trước khi chạy module AI extraction (5.5).

1. Mở console **Amazon Bedrock** ở `ap-southeast-1`.
2. Vào **Model access** ở thanh điều hướng bên trái.
3. Chọn **Manage model access** (hoặc **Enable specific models**).
4. Bật **Amazon Nova Lite** (model chính) và **Anthropic Claude 3 Haiku** (fallback).
5. Submit và chờ tới khi trạng thái hiển thị **Access granted**.

Kiểm tra từ CLI rằng các model đã có:

```bash
aws bedrock list-foundation-models \
  --profile docuflow \
  --region ap-southeast-1 \
  --query "modelSummaries[?contains(modelId, 'nova-lite') || contains(modelId, 'haiku')].modelId"
```

{{% notice info %}}
Nếu Nova Lite chưa GA ở `ap-southeast-1` tại thời điểm workshop, đặt parameter `BedrockRegion` thành `us-east-1`. Kiến trúc cô lập việc này sau một environment variable duy nhất (`BEDROCK_REGION`), như mô tả ở risk R-07 trong proposal.
{{% /notice %}}

#### Bước 4 - Clone Repository

```bash
git clone https://github.com/<your-org>/docuflow-ai.git
cd docuflow-ai
pnpm install
```

`pnpm install` đọc `pnpm-workspace.yaml` và link các workspace `apps/*`, `packages/*` và `services/functions/*`. Ở giai đoạn này các workspace mới là skeleton nên install hoàn tất nhanh, chưa có application dependency.

#### Bước 5 - Validate SAM Template

Repository đã có sẵn SAM template baseline tại `infrastructure/template.yaml`. Validate trước khi chỉnh sửa gì:

```bash
sam validate --lint --template infrastructure/template.yaml --region ap-southeast-1
```

Output mong đợi:

```text
infrastructure/template.yaml is a valid SAM Template
```

Bước này xác nhận credential AWS CLI, region và bản cài SAM CLI hoạt động ăn khớp với nhau.

#### Bước 6 - (Tùy chọn) GitHub OIDC cho CI/CD

Để GitHub Actions deploy mà không lưu AWS key, tạo một IAM OIDC identity provider cho `token.actions.githubusercontent.com` và một role tin tưởng repository của bạn. Bước này không bắt buộc cho phát triển local nhưng cần cho pipeline `deploy-dev` và `deploy-prod` ở module 5.7.

Bạn sẽ cấu hình chi tiết trong module observability và security. Hiện tại, lưu ý CI workflow ở `.github/workflows/ci.yml` chỉ chạy lint, typecheck, test và `sam validate` — đều không cần credential AWS.

#### Checklist

Trước khi sang module 5.3, xác nhận:

- [ ] Đã cài Node 20, pnpm 9, Git, AWS CLI v2 và SAM CLI (Docker tùy chọn).
- [ ] `aws sts get-caller-identity --profile docuflow` trả về tài khoản của bạn (qua SSO hoặc access key).
- [ ] Region mặc định của profile là `ap-southeast-1`.
- [ ] Đã được cấp Bedrock model access cho Nova Lite và Claude 3 Haiku.
- [ ] Đã clone repository và `pnpm install` hoàn tất.
- [ ] `sam validate --lint` báo template hợp lệ.

Khi đã đủ các mục trên, tiếp tục sang **5.3 Frontend, Auth và Upload**.

---
title: "Prerequisites"
date: 2026-04-20
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### Overview

This module prepares your local machine and AWS account to build and deploy DocuFlow AI. By the end you will have the required tools installed, an AWS profile configured for `ap-southeast-1`, Amazon Bedrock model access enabled, and a validated AWS SAM template.

You only need to do this setup once. Every later module assumes these prerequisites are in place.

#### Required Tools

Install the following on your workstation. Versions are minimums; newer patch releases are fine.

| Tool | Version | Purpose |
|---|---|---|
| Node.js | 20.x LTS | Runtime for frontend build and Lambda functions |
| pnpm | 9.x | Monorepo package manager |
| Git | 2.40+ | Version control |
| AWS CLI | v2 (latest) | Authenticate and call AWS APIs |
| AWS SAM CLI | latest 1.x | Build, validate, and deploy the serverless stack |
| Docker | 24+ (optional) | Only for `sam local` emulation and `sam build --use-container` |

Verify each tool:

```bash
node --version      # v20.x
pnpm --version      # 9.x
git --version       # 2.40+
aws --version       # aws-cli/2.x
sam --version       # SAM CLI, version 1.x
docker --version    # optional: Docker version 24+
```

{{% notice tip %}}
If `pnpm` is not installed, enable it through Corepack which ships with Node.js 20: `corepack enable && corepack prepare pnpm@9 --activate`.
{{% /notice %}}

{{% notice info %}}
Docker is **not** required for normal development. Our Lambda functions are TypeScript bundled with esbuild, so `sam build` runs natively without containers. Install Docker only if you want to emulate Lambda locally with `sam local invoke` or `sam local start-api`.
{{% /notice %}}

#### Step 1 - Configure AWS Access

DocuFlow AI deploys to **`ap-southeast-1` (Singapore)**, the closest region to Vietnam. DocuFlow AI treats long-lived access keys as a security risk (see proposal risks R-06 and R-08), so the recommended way to authenticate is **AWS IAM Identity Center** (short-lived credentials that refresh automatically and never sit on disk).

**Option A - IAM Identity Center (recommended)**

```bash
aws configure sso --profile docuflow
```

Follow the prompts to enter your SSO start URL and region, then select the account and permission set. Set the CLI default region to `ap-southeast-1` and output to `json`. Sign in whenever the session expires:

```bash
aws sso login --profile docuflow
```

**Option B - Access keys (only if Identity Center is unavailable)**

If your account cannot use Identity Center, fall back to a temporary access key and delete it after the workshop:

```bash
aws configure --profile docuflow
# Default region name: ap-southeast-1
# Default output format: json
```

Confirm the identity and region with either option:

```bash
aws sts get-caller-identity --profile docuflow
aws configure get region --profile docuflow   # ap-southeast-1
```

{{% notice warning %}}
Never commit credentials to Git. The repository ships with a `gitleaks` pre-commit hook (see module 5.7) and a `.gitignore` that excludes `.env` files. For CI/CD, this workshop uses GitHub OIDC instead of any stored keys. If you used Option B, delete the access key in the IAM console once the workshop is complete.
{{% /notice %}}

#### Step 2 - IAM Permissions

The account or role used for the workshop needs permission to create and delete the services in the DocuFlow architecture. For a personal workshop account, an administrator-equivalent policy is the simplest option. For a shared or restricted account, attach a policy scoped to these services:

```text
cloudformation, s3, cognito-idp, apigateway, lambda, states,
dynamodb, events, sqs, sns, textract, bedrock, cloudfront,
logs, cloudwatch, iam, kms, budgets, ssm
```

The single SAM stack provisions every resource, so the deploying principal also needs `iam:CreateRole`, `iam:PassRole`, and `cloudformation:*` on the stack.

#### Step 3 - Enable Amazon Bedrock Model Access

Bedrock models are opt-in per region. Enable access before running the AI extraction module (5.5).

1. Open the **Amazon Bedrock** console in `ap-southeast-1`.
2. Go to **Model access** in the left navigation.
3. Choose **Manage model access** (or **Enable specific models**).
4. Enable **Amazon Nova Lite** (primary model) and **Anthropic Claude 3 Haiku** (fallback).
5. Submit and wait until status shows **Access granted**.

Verify from the CLI that the models are listed:

```bash
aws bedrock list-foundation-models \
  --profile docuflow \
  --region ap-southeast-1 \
  --query "modelSummaries[?contains(modelId, 'nova-lite') || contains(modelId, 'haiku')].modelId"
```

{{% notice info %}}
If Nova Lite is not yet generally available in `ap-southeast-1` at the time of the workshop, set the `BedrockRegion` parameter to `us-east-1`. The architecture isolates this behind a single environment variable (`BEDROCK_REGION`), as described in the proposal risk R-07.
{{% /notice %}}

#### Step 4 - Clone the Repository

```bash
git clone https://github.com/<your-org>/docuflow-ai.git
cd docuflow-ai
pnpm install
```

`pnpm install` reads `pnpm-workspace.yaml` and links the `apps/*`, `packages/*`, and `services/functions/*` workspaces. At this stage the workspaces are skeletons, so the install completes quickly with no application dependencies.

#### Step 5 - Validate the SAM Template

The repository ships with a baseline SAM template at `infrastructure/template.yaml`. Validate it before making any changes:

```bash
sam validate --lint --template infrastructure/template.yaml --region ap-southeast-1
```

Expected output:

```text
infrastructure/template.yaml is a valid SAM Template
```

This confirms your AWS CLI credentials, region, and SAM CLI installation all work together.

#### Step 6 - (Optional) GitHub OIDC for CI/CD

To let GitHub Actions deploy without storing AWS keys, create an IAM OIDC identity provider for `token.actions.githubusercontent.com` and a role that trusts your repository. This is optional for local development but required for the `deploy-dev` and `deploy-prod` pipelines in module 5.7.

You will configure this in detail during the observability and security module. For now, note that the CI workflow at `.github/workflows/ci.yml` only runs lint, typecheck, test, and `sam validate` — none of which need AWS credentials.

#### Checklist

Before moving to module 5.3, confirm:

- [ ] Node 20, pnpm 9, Git, AWS CLI v2, and SAM CLI are installed (Docker optional).
- [ ] `aws sts get-caller-identity --profile docuflow` returns your account (via SSO or access key).
- [ ] Default region for the profile is `ap-southeast-1`.
- [ ] Bedrock model access granted for Nova Lite and Claude 3 Haiku.
- [ ] Repository cloned and `pnpm install` completed.
- [ ] `sam validate --lint` reports a valid template.

With these in place, continue to **5.3 Frontend, Auth, and Upload**.

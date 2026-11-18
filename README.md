# AWS FinOps Cost Auditor (GitHub Action)

A zero-agent, read-only GitHub Action powered by [`aws-finops-auditor`](https://pypi.org/project/aws-finops-auditor/) to detect orphaned EBS volumes, idle EC2 instances, and unattached Elastic IPs directly in CI/CD workflows.

## Quickstart

```yaml
name: AWS FinOps Cost Audit

on:
  pull_request:
  schedule:
    - cron: '0 8 * * 1'

permissions:
  id-token: write
  contents: read

jobs:
  finops-audit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::<YOUR_AWS_ACCOUNT_ID>:role/GitHubActionsFinOpsRole
          aws-region: us-east-1

      - name: Run AWS FinOps Cost Audit
        id: finops
        uses: Ace-FinOps-Enterprise/aws-finops-auditor-action@v1
        with:
          fail-on-waste: '100'
```

## Inputs & Outputs

| Input | Description | Default |
| :--- | :--- | :--- |
| `file` | Optional CSV path for offline/demo audits | `''` |
| `fail-on-waste` | Fail workflow if monthly USD waste exceeds threshold | `''` |
| `package-version` | PyPI version constraint for `aws-finops-auditor` | `>=1.0.0` |

| Output | Description |
| :--- | :--- |
| `monthly-waste` | Identified monthly AWS waste in USD |
| `annual-waste` | Identified annualized AWS waste in USD |

## Enterprise & Multi-Account Upgrades

Need automated **AWS Organizations multi-account scanning**, **executive PDF showback reports**, and **idempotent bash remediation scripts**?

- **[AWS Cloud Cost Auditor (Practitioner & Agency Tiers)](https://aisser.gumroad.com/l/krgac)**

# blazop-ec2-config-requests (CONFIG repo, simulation)

Simulates the client's **request/config repository**. BlaZop (GitHub adapter) writes here.

## How it is used
| Who | What |
|---|---|
| BlaZop `var_file` step | Creates branch per request from `main`, commits `terraform.tfvars.json` |
| BlaZop `deployment_file` step | Commits `deployment.json` (`{"terraform_code_version": "v0.1", "template": "ec2"}`) on the same branch |
| BlaZop `trigger_workflow` step | Runs `submit_terraform_request.yml` on that branch (`action`, `dry_run`) |
| `submit_terraform_request.yml` | Validates branch `req/RITMxxxxxxx`, sends `repository_dispatch` to the CODE repo |

## Files on `main`
- `.github/workflows/submit_terraform_request.yml`: client workflow (simulation copy)
- `schema.tfvars.json`: schema for `terraform.tfvars.json`

## Secrets
- `CODE_REPO_DISPATCH_TOKEN`: fine-grained PAT with **Contents: Read & Write** on `blazop-aws-standalone-ec2-instances`

## Example request branch
```
req/RITM0000001
├── terraform.tfvars.json
└── deployment.json
```

# 08 — HCP Terraform

!!! abstract "Exam objective"
    **Use HCP Terraform capabilities.** Remote operations, CLI and VCS workflows, workspaces, projects, variable sets, teams and permissions, dynamic credentials, drift detection, policies, and governance features.

**Status:** <span class="coverage-badge coverage-empty">Empty</span>

---

## What HCP Terraform is

*HashiCorp's hosted service. Previously called Terraform Cloud. Free tier, Team, Business tiers. What's included at each.*

## Remote operations

*Running `plan` and `apply` on HashiCorp's infrastructure instead of your laptop. Why.*

## CLI-driven workflow

*`terraform login`, the `cloud` block, running commands locally with remote execution.*

## VCS-driven workflow

*Connecting a workspace to GitHub/GitLab. Auto-plan on PR, apply on merge.*

## Workspaces

*Unit of isolation in HCP Terraform. Different from CLI workspaces. One state per workspace.*

## Projects

*Grouping workspaces. Permission boundaries.*

## Variable sets

*Shared variables across workspaces. Precedence rules.*

## Teams and permissions

*Team-based access control. Workspace-level permissions.*

## Dynamic provider credentials

*OIDC-based short-lived credentials. Why this beats long-lived static keys.*

## Drift detection and health assessments

*Continuous validation that real infra still matches config. Alerts.*

## Policies (Sentinel, OPA)

*Policy-as-code enforcement before apply. Soft-mandatory vs hard-mandatory.*

## Explorer and change requests

*Cross-workspace visibility. Approval workflows.*

## Gotchas and nuances

*Notes go here. Especially around VCS vs CLI workflow tradeoffs.*

## References

- [HCP Terraform docs](https://developer.hashicorp.com/terraform/cloud-docs)
- [Get Started with HCP Terraform](https://developer.hashicorp.com/terraform/tutorials/cloud-get-started)
- [Workspaces](https://developer.hashicorp.com/terraform/cloud-docs/workspaces)
- [Dynamic provider credentials](https://developer.hashicorp.com/terraform/cloud-docs/workspaces/dynamic-provider-credentials)

## Commands cheat sheet

| Command | What it does |
|---------|-------------|
| `terraform login` | Authenticate the CLI to HCP Terraform |
| `terraform logout` | Clear stored credentials |
| `terraform workspace list` | List CLI workspaces (not HCP workspaces) |
| `terraform plan` (with `cloud` block) | Runs remotely in HCP Terraform |
| `terraform apply` (with `cloud` block) | Runs remotely in HCP Terraform |

!!! warning "CLI workspaces vs HCP workspaces"
    These are different things with the same name. CLI workspaces (`terraform workspace new`) are a lightweight local feature for switching state files. HCP Terraform workspaces are a full isolation boundary with their own state, variables, and permissions. The exam will try to trick you on this.

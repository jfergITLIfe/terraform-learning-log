# 06 — State Management

!!! abstract "Exam objective"
    **Manage Terraform state.** Understand what state is, local vs remote backends, locking, drift, and the `moved` block for refactoring.

**Status:** <span class="coverage-badge coverage-empty">Empty</span>

**Runnable code:** [`code/06-state-management/`](https://github.com/jfergITLife/terraform-learning-log/tree/main/code/06-state-management) *(add when ready)*

---

## What state actually is

*Terraform's memory. The mapping between config and real resources. Why losing it is catastrophic.*

## Local state

*Default behavior. `terraform.tfstate` on disk. Why this doesn't work for teams.*

## Remote backends

*S3 + DynamoDB is the classic AWS pattern. Also Azure Storage, GCS, Consul, HCP Terraform. State locking and why.*

## Migrating state

*`terraform init -migrate-state`. Moving from local to remote. Moving between backends.*

## State locking

*Preventing concurrent applies. DynamoDB lock table for S3 backend. What happens when locks fail.*

## Drift

*Infrastructure that changed outside Terraform. Detection, `terraform refresh`, refresh-only mode.*

## The `moved` block

*Refactoring without destroy/recreate. Renaming resources safely.*

## `terraform state` subcommands

*`list`, `show`, `mv`, `rm`, `pull`, `push`. When to reach for each.*

## Sensitive data in state

*State files contain secrets in plaintext. Access control. Encrypted backends.*

## Gotchas and nuances

*Notes go here. The "never edit tfstate by hand" rule, and what to do instead.*

## References

- [Terraform state](https://developer.hashicorp.com/terraform/language/v1.12.x/state)
- [Backend configuration](https://developer.hashicorp.com/terraform/language/v1.12.x/backend)
- [`moved` block](https://developer.hashicorp.com/terraform/language/v1.12.x/block/moved)
- [Manage resource drift](https://developer.hashicorp.com/terraform/tutorials/state/resource-drift)

## Commands cheat sheet

| Command | What it does |
|---------|-------------|
| `terraform state list` | List all resources in state |
| `terraform state show <addr>` | Show one resource's state |
| `terraform state mv <src> <dst>` | Rename/move a resource in state |
| `terraform state rm <addr>` | Remove resource from state (doesn't destroy infra) |
| `terraform state pull` | Download remote state to stdout |
| `terraform state push <file>` | Upload state (dangerous) |
| `terraform refresh` | Reconcile state with real infra (legacy) |
| `terraform apply -refresh-only` | Modern way to do the above |
| `terraform init -migrate-state` | Move state when changing backends |

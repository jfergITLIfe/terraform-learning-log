# 03 — Core Workflow

!!! abstract "Exam objective"
    **Use the core Terraform workflow.** Write → init → plan → apply. Understand `terraform fmt`, `terraform validate`, dependency graphs, and resource lifecycle.

**Status:** <span class="coverage-badge coverage-empty">Empty</span>

**Runnable code:** [`code/03-core-workflow/`](https://github.com/jfergITLife/terraform-learning-log/tree/main/code/03-core-workflow) *(add when ready)*

---

## The four commands you'll run ten thousand times

*Notes go here.*

## `terraform init`

*What it actually does. Providers, backend, lock file.*

## `terraform plan`

*Reading a plan output. The diff format. Why you read it every time.*

## `terraform apply`

*Behavior, `-auto-approve`, and why that flag is dangerous outside automation.*

## `terraform destroy`

*Safe usage, targeting, and cleanup habits.*

## `fmt` and `validate`

*Notes go here.*

## The dependency graph

*How Terraform figures out order. `depends_on`. When to reach for it.*

## Resource lifecycle

*`create_before_destroy`, `prevent_destroy`, `ignore_changes`, `replace_triggered_by`.*

## Gotchas and nuances

*Notes go here.*

## References

- [Core Terraform workflow](https://developer.hashicorp.com/terraform/intro/v1.12.x/core-workflow)
- [Terraform CLI reference](https://developer.hashicorp.com/terraform/cli/v1.12.x)
- [Dependency graph](https://developer.hashicorp.com/terraform/internals/v1.12.x/graph)

## Commands cheat sheet

| Command | What it does |
|---------|-------------|
| `terraform init` | Downloads providers, initializes backend |
| `terraform fmt` | Reformats `.tf` files to canonical style |
| `terraform validate` | Syntactic / type check without contacting APIs |
| `terraform plan` | Shows diff between config and real infra |
| `terraform apply` | Applies the plan, mutates real infra |
| `terraform destroy` | Removes everything managed by this config |
| `terraform plan -out=<file>` | Save plan to file for guaranteed apply |
| `terraform apply <planfile>` | Apply a previously saved plan |

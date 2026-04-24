# 07 — Maintain Infrastructure

!!! abstract "Exam objective"
    **Maintain existing Terraform infrastructure.** Import resources, troubleshoot issues, enable debug logging, and inspect state for ongoing maintenance.

**Status:** <span class="coverage-badge coverage-empty">Empty</span>

**Runnable code:** [`code/07-maintain-infrastructure/`](https://github.com/jfergITLife/terraform-learning-log/tree/main/code/07-maintain-infrastructure) *(add when ready)*

---

## Importing existing resources

*Two paths: the classic `terraform import` command and the newer `import` block. When to use each.*

## `import` blocks (newer, config-driven)

*Declarative import. Can be reviewed in a plan. Removed in a future apply.*

## `terraform import` command (legacy but still on the exam)

*Imperative. Updates state directly. You still write the config by hand.*

## Troubleshooting workflow

*Four-step process: reproduce, narrow, inspect, fix. Reading errors. When to suspect provider bugs vs config bugs.*

## Debug logging

*`TF_LOG` levels: TRACE, DEBUG, INFO, WARN, ERROR. `TF_LOG_PATH` to write to file.*

## Inspecting state

*`terraform state list`, `terraform show`, `terraform console`.*

## Gotchas and nuances

*Notes go here.*

## References

- [Import existing resources](https://developer.hashicorp.com/terraform/language/v1.12.x/import)
- [Troubleshoot Terraform](https://developer.hashicorp.com/terraform/tutorials/configuration-language/troubleshooting-workflow)
- [Debugging Terraform](https://developer.hashicorp.com/terraform/internals/v1.12.x/debugging)

## Commands cheat sheet

| Command | What it does |
|---------|-------------|
| `terraform import <addr> <id>` | Import existing resource into state |
| `terraform show` | Show current state in human-readable form |
| `terraform show -json` | Machine-readable state |
| `terraform console` | REPL for testing expressions against current state |
| `TF_LOG=DEBUG terraform plan` | Verbose logging |
| `TF_LOG_PATH=tf.log terraform plan` | Log to file |
| `terraform state list` | List resources in state |

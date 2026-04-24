# 04 — Configuration & HCL

!!! abstract "Exam objective"
    **Read and write Terraform configuration.** Resources, data sources, variables, outputs, locals, functions, dynamic expressions, sensitive data, resource lifecycle, and custom conditions.

**Status:** <span class="coverage-badge coverage-empty">Empty</span>

**Runnable code:** [`code/04-configuration-hcl/`](https://github.com/jfergITLife/terraform-learning-log/tree/main/code/04-configuration-hcl) *(add when ready)*

---

## Resources

*The basic building block. `resource` blocks, addressing, arguments.*

## Data sources

*Read-only references to things already existing. When to use instead of `resource`.*

## Variables (inputs)

*`variable` blocks, types, defaults, validation, `.tfvars` files, precedence.*

## Outputs

*Exposing values. Sensitive outputs. Using outputs between configs.*

## Locals

*Local values for repeated expressions. When locals beat variables.*

## Functions

*The ones you'll actually use: `concat`, `merge`, `lookup`, `for`, `format`, `jsonencode`, `templatefile`.*

## Dynamic expressions

*`for_each`, `count`, `dynamic` blocks, conditional expressions.*

## Type constraints

*string, number, bool, list, set, map, object, tuple, any.*

## Sensitive data

*Marking variables sensitive, what that does and doesn't protect.*

## Resource lifecycle meta-argument

*`create_before_destroy`, `prevent_destroy`, `ignore_changes`, `replace_triggered_by`.*

## Custom conditions and checks

*`precondition`, `postcondition`, `check` blocks.*

## Gotchas and nuances

*Notes go here.*

## References

- [Resources](https://developer.hashicorp.com/terraform/language/v1.12.x/resources)
- [Variables](https://developer.hashicorp.com/terraform/language/v1.12.x/values/variables)
- [Built-in functions](https://developer.hashicorp.com/terraform/language/v1.12.x/functions)
- [Type constraints](https://developer.hashicorp.com/terraform/language/v1.12.x/expressions/type-constraints)

## Commands cheat sheet

| Command | What it does |
|---------|-------------|
| `terraform console` | REPL for testing expressions and functions |
| `terraform plan -var="name=value"` | Set a variable at plan time |
| `terraform plan -var-file="prod.tfvars"` | Use a specific tfvars file |
| `TF_VAR_name=value terraform plan` | Set variable via env var |
| `terraform output` | Print all outputs |
| `terraform output <name>` | Print one output |
| `terraform output -json` | Machine-readable outputs |

# 05 — Modules

!!! abstract "Exam objective"
    **Use and create modules.** Use registry modules, build local modules, understand the standard module structure, and compose modules effectively.

**Status:** <span class="coverage-badge coverage-empty">Empty</span>

**Runnable code:** [`code/05-modules/`](https://github.com/jfergITLife/terraform-learning-log/tree/main/code/05-modules) *(add when ready)*

---

## What a module is

*Any directory with `.tf` files is a module. Root module vs child modules.*

## Using registry modules

*Pulling modules from the Terraform Registry. Version constraints. Source syntax.*

## Building a local module

*Inputs, outputs, file structure, when to extract to a module.*

## Standard module structure

*`main.tf`, `variables.tf`, `outputs.tf`, `versions.tf`, `README.md`, `examples/`.*

## Module composition

*Composing small modules vs monolithic ones. Dependencies between modules.*

## Providers within modules

*Implicit vs explicit provider passing. When you need `configuration_aliases`.*

## Gotchas and nuances

*Notes go here.*

## References

- [Modules overview](https://developer.hashicorp.com/terraform/language/modules)
- [Standard module structure](https://developer.hashicorp.com/terraform/language/v1.12.x/modules/develop/structure)
- [Providers within modules](https://developer.hashicorp.com/terraform/language/v1.12.x/modules/develop/providers)
- [Terraform Registry](https://registry.terraform.io/)

## Commands cheat sheet

| Command | What it does |
|---------|-------------|
| `terraform init` | Downloads modules into `.terraform/modules/` |
| `terraform init -upgrade` | Pull latest versions within constraints |
| `terraform get` | Download/update modules only (rarely needed) |

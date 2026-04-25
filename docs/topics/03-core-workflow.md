# 03 — Core Workflow

!!! abstract "Exam objective"
    **Use the core Terraform workflow.** Write → init → validate → plan → apply → destroy. Understand `terraform fmt`, dependency graphs, and resource lifecycle.

**Status:** <span class="coverage-badge coverage-partial">Partial</span>

**Runnable code:** [`code/03-core-workflow/`](https://github.com/jfergITLife/terraform-learning-log/tree/main/code/03-core-workflow) *(coming)*

---

## The two framings of "the workflow"

Terraform's workflow gets described two ways, and both show up on the exam. It helps to know which framing a question is asking about.

**The official conceptual workflow — three stages:**

`Write → Plan → Apply`

This is the philosophical loop. It describes what you're doing as a human: you write code, you review what it'll do, you commit to the changes.
 
**The practical CLI commands you actually run — five (or six):**

`init → validate → plan → apply → destroy`

This is what you type into the terminal. `init` and `validate` are prep work that support the Write stage. `destroy` is a teardown command, not part of the ongoing loop.

So init isn't its own *stage* — it's prep work you do as part of the Write stage. If a question asks for the three stages of the core workflow, the answer is Write/Plan/Apply. If it asks which command comes first, the answer is `terraform init`.

## Write: developing configuration

The Write stage is where you create one or more `.tf` files that define your **desired state** — what you want the infrastructure to look like. Terraform's job is then to make reality match that description.

Terraform configurations are iterative. You don't have to get it right the first time. Write a little, plan, apply, see what breaks, refine, repeat.

## `terraform init`

*Notes coming next session.*

### The `.terraform/` directory

*Notes coming.*

### The lock file (`.terraform.lock.hcl`)

*Notes coming.*

## `terraform validate`

*Notes coming.*

## `terraform plan`

*Notes coming.*

### Saving a plan (`-out`)

*Notes coming.*

### The resource graph

*Notes coming.*

## Dependencies

*Notes coming — implicit vs explicit, `depends_on`.*

## terraform init

`terraform init` is the first command you run in any Terraform project. It downloads the providers and modules your configuration declares, and sets up the working directory so the rest of the workflow can function. If you skip it, every other command will fail.

### what init actually does

Three things, in order:

1. **Downloads providers.** Terraform reads your configuration, finds every `provider` block (aws, azurerm, google, etc.), and downloads the matching binaries from the Terraform Registry.
2. **Downloads modules.** If your configuration uses `module` blocks pointing at external sources (Git, the registry, S3), init pulls those down too.
3. **Initializes the backend.** If you've configured remote state (S3, Terraform Cloud, etc.), init wires that up. If you haven't, it sets up local state.

### what init creates on disk

Two artifacts. They look similar. They are not the same.

| artifact | what it is | commit to git? |
|---|---|---|
| `.terraform/` | a directory holding the actual downloaded provider binaries and module code | no — gitignore it |
| `.terraform.lock.hcl` | a file recording the exact versions and cryptographic hashes that got downloaded | yes — commit it |

The mental model:

- `.terraform/` is the stuff. Like `node_modules/` or `venv/`. Machine-specific, big, never committed.
- `.terraform.lock.hcl` is the receipt. Tiny, text-based, version-controlled. It's what guarantees your teammate downloads the same provider versions you did when they run `init` on their own machine.

### init is not just for new projects

This is where the exam tries to trip you up. You run `terraform init` whenever the configuration's dependencies change, not just on day one. That includes:

- starting a new project
- cloning an existing repo for the first time
- a teammate adds, removes, or upgrades a provider
- a teammate adds, removes, or changes a module source
- you change your backend configuration

If `.terraform/` and your config files are out of sync, Terraform refuses to run `plan` or `apply` — it errors out and tells you to run `init`. The fix is always the same.

!!! note "the one people miss on the exam"
    `terraform init` is **idempotent and safe to re-run.** It will not destroy your state, your resources, or your lock file's pinned versions. If you're ever in doubt about whether to run it, just run it. The only time it changes anything is when your configuration's dependencies have actually changed.

### useful flags

| flag | what it does |
|---|---|
| `-upgrade` | bypasses the lock file and pulls the newest allowed provider versions, then rewrites the lock file |
| `-reconfigure` | throws away the existing backend config and re-initializes from scratch (used when migrating backends) |
| `-backend=false` | skips backend initialization (rarely needed, mostly for CI) |

`-upgrade` is the one to remember. Default `init` respects the lock file and downloads exactly what's pinned. `init -upgrade` is how you intentionally bump versions.

## `terraform apply`

Apply is where Terraform actually changes your infrastructure. Everything up to this point has been prep — writing config, downloading providers, generating a plan to see what would happen. Apply is the one that does the thing.

### What it actually does

When you run `terraform apply` on its own, Terraform does three things in order:

1. Generates a plan (implicitly — same as running `terraform plan`)
2. Shows you the plan and prompts `Confirm (Y/N):`
3. If you type yes, executes the plan against your cloud provider

So you don't *have* to run `terraform plan` before `terraform apply` — apply includes a plan step by default. Running plan separately is a review habit, not a technical requirement.

### Three ways to run it

| Command | Prompts? | Uses saved plan? | Re-plans? |
|---------|----------|------------------|-----------|
| `terraform apply` | Yes | No | Yes (implicit) |
| `terraform apply -auto-approve` | No | No | Yes (implicit) |
| `terraform apply myplan.tfplan` | No | Yes | No |

The third row is the one people miss on the exam. When you apply a saved plan file, Terraform executes exactly what's in that file — no re-planning, no confirmation prompt. It's the safest option for automation because there are no surprises between review and execution.

### Saved plans (the pattern worth knowing)

```bash
terraform plan -out=myplan.tfplan
# review the plan output
terraform apply myplan.tfplan
```

This is the pattern used in real pipelines: one job generates a plan, a human reviews it in a PR comment or approval workflow, then a second job applies the exact plan that was reviewed. No chance of drift between review and execution.

### `-auto-approve`: when it's appropriate

Never use `-auto-approve` at the terminal. It skips the confirmation prompt entirely — if your plan says "destroy 47 resources," they're gone before you can react.

The only legitimate use case is **automation where approval happened somewhere else**:

- CI/CD pipelines running Terraform on merge to main (PR review was the approval)
- HCP Terraform or Atlantis (policy checks were the approval)
- Scheduled jobs where the config is pre-vetted

The rule: if a human is at the terminal, don't use `-auto-approve`. If a machine is running it, you probably need both `-auto-approve` and `-input=false` (which also disables prompts for missing variable values).

### Best practices checklist

**Before you apply:**
- Always review the plan
- Confirm you're in the right directory
- Test in non-production first

**During apply:**
- Read the plan carefully before typing yes
- Watch for unexpected changes
- Be patient with large applies — some resources take minutes to provision

**After apply:**
- Verify the changes in the console or via CLI
- Commit your configuration
- Document significant changes

## `terraform destroy`

*Notes coming.*

## Gotchas and nuances

- Apply refreshes state before it runs — it compares your desired state to what actually exists, not just to what was in state last time. This is why apply can sometimes show unexpected changes: someone modified infrastructure outside of Terraform (called "drift").
- `terraform apply` with no plan file ≠ `terraform apply saved.tfplan`. The first re-plans and can surprise you; the second executes exactly what was saved.
- A saved plan file is only valid for a short window. If too much time passes or state changes, Terraform will reject it with a "stale plan" error.
- You MUST run `terraform init` before `terraform validate` — validate needs the providers to be downloaded to know what's a valid resource attribute.

## References

- [Core Terraform workflow](https://developer.hashicorp.com/terraform/intro/v1.12.x/core-workflow)
- [Terraform CLI reference](https://developer.hashicorp.com/terraform/cli/v1.12.x)
- [Dependency graph internals](https://developer.hashicorp.com/terraform/internals/v1.12.x/graph)

## Commands cheat sheet

| Command | What it does |
|---------|-------------|
| `terraform init` | Downloads providers, initializes backend |
| `terraform fmt` | Reformats `.tf` files to canonical style |
| `terraform validate` | Syntactic / type check (requires init first) |
| `terraform plan` | Shows diff between config and real infra |
| `terraform plan -out=<file>` | Save plan to file for guaranteed apply |
| `terraform apply` | Re-plans, prompts, then applies |
| `terraform apply <planfile>` | Apply a saved plan exactly, no prompt |
| `terraform apply -auto-approve` | Skip prompt (automation only) |
| `terraform destroy` | Remove everything managed by this config |

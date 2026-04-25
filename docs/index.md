# the core terraform workflow

The 99% of what you actually use day to day. One page. Grep it when you need it.

The workflow gets described two ways:

- **Conceptual:** Write → Plan → Apply
- **CLI commands you actually type:** `init` → `fmt` → `validate` → `plan` → `apply` → `destroy`

---

## terraform init

The first command you run in any Terraform project. It downloads the providers and modules your configuration declares and sets up the working directory so the rest of the workflow can function. Skip it and every other command fails.

### what init actually does

Three things, in order:

1. **Downloads providers.** Reads your config, finds every `provider` block (aws, azurerm, google, etc.), and pulls the matching binaries from the Terraform Registry.
2. **Downloads modules.** If your config uses `module` blocks pointing at external sources (Git, registry, S3), init pulls those down too.
3. **Initializes the backend.** If you've configured remote state (S3, Terraform Cloud), init wires it up. If you haven't, it sets up local state.

### what init creates on disk

Two artifacts. They look similar. They are not the same.

| artifact | what it is | commit to git? |
|---|---|---|
| `.terraform/` | directory holding the actual downloaded provider binaries and module code | no, gitignore it |
| `.terraform.lock.hcl` | file recording exact versions and cryptographic hashes that got downloaded | yes, commit it |

Mental model:

- `.terraform/` is the stuff. Like `node_modules/` or `venv/`. Machine-specific, big, never committed.
- `.terraform.lock.hcl` is the receipt. Tiny, text-based, version-controlled. It guarantees your teammate downloads the same provider versions you did.

### init is not just for new projects

You run init whenever the configuration's dependencies change, not just on day one:

- starting a new project
- cloning an existing repo for the first time
- a teammate adds, removes, or upgrades a provider
- a teammate adds, removes, or changes a module source
- you change your backend configuration

If `.terraform/` and your config are out of sync, Terraform refuses to plan or apply. It errors and tells you to run init. The fix is always the same.

!!! note
    `terraform init` is idempotent and safe to re-run. It will not destroy your state, your resources, or your lock file's pinned versions. If in doubt, run it.

### useful flags

| flag | what it does |
|---|---|
| `-upgrade` | bypasses the lock file, pulls newest allowed provider versions, rewrites the lock file |
| `-reconfigure` | throws away existing backend config and re-initializes from scratch (used when migrating backends) |
| `-backend=false` | skips backend initialization (rare, mostly for CI) |

`-upgrade` is the important one. Default init respects the lock file. `init -upgrade` is how you intentionally bump versions.

---

## terraform fmt

Rewrites your `.tf` files to match the canonical Terraform style. Indentation, alignment, spacing. Cosmetic, not functional.

By default it only touches files in the current directory.

### useful flags

| flag | what it does |
|---|---|
| `-recursive` | also format files in subdirectories |
| `-check` | exit non-zero if files are not formatted, but do not rewrite |
| `-diff` | show what would change without applying it |

`-check` is how you enforce formatting in CI without letting the pipeline rewrite files behind your back.

---

## terraform validate

Static check that your configuration is syntactically valid and internally consistent. Does not talk to any provider, does not touch state, does not look at the real world.

### what validate checks

1. **Syntax.** Did you close every brace? Are your strings quoted?
2. **Internal references.** If you reference `aws_instance.web.id`, does `aws_instance.web` actually exist in the config?
3. **Required arguments.** If a resource requires an argument, did you provide it?

### what validate does not check

- whether your AWS credentials work
- whether resources would actually deploy
- whether state matches reality
- whether a hardcoded AMI ID exists in AWS

Validate is fast, offline, and catches dumb mistakes before you waste time on plan.

!!! note
    `terraform validate` requires `init` to have been run first. It needs the provider schemas to know what arguments are valid for each resource type. No init, no validate.

---

## terraform plan

Compares three things and tells you what Terraform is about to do:

1. your configuration (what you want)
2. your state file (what Terraform thinks exists)
3. the real world (what actually exists, via the provider API)

The output is a list of actions: create, update, destroy, replace. Nothing happens to your infrastructure when you run plan. It is read-only.

### the three-way comparison

This is the mental model that makes everything else click:

- **Config vs state:** "you wrote this in main.tf, but state does not have it. I will create it."
- **State vs real world:** "state says this bucket exists, but the API says it is gone. I will recreate it."
- **Config vs real world:** "you changed the instance type. I will update the running instance."

When all three agree, plan shows "no changes."

### saved plan files

Plan can save its output to a file with `-out`:

```bash
terraform plan -out=tfplan
terraform apply tfplan
```

This is the paranoid-but-correct way to do production changes. You review the plan, save it, then apply exactly what you reviewed. No drift between what you saw and what ran.

A saved plan is only valid for a short window. If state or real-world conditions change, Terraform rejects it as stale.

### useful flags

| flag | what it does |
|---|---|
| `-out=FILE` | save the plan to a file for later apply |
| `-destroy` | show what a destroy would do without actually destroying |
| `-refresh-only` | only update state from real-world, do not propose config changes |
| `-target=RESOURCE` | limit the plan to a specific resource (emergency use only) |

!!! warning "do not rely on -target"
    `-target` exists for surgical recovery from a broken state. It is not a workflow tool. Using it regularly means your configuration is structured wrong.

---

## terraform apply

Where Terraform actually changes your infrastructure. Everything up to this point has been prep. Apply is the one that does the thing.

### what it actually does

When you run `terraform apply` on its own, three things happen in order:

1. Generates a plan (implicit, same as running `terraform plan`)
2. Shows you the plan and prompts `Confirm (Y/N):`
3. If you type yes, executes the plan against your cloud provider

You do not have to run `plan` before `apply`. Apply includes a plan step by default. Running plan separately is a review habit, not a technical requirement.

### three ways to run it

| command | prompts? | uses saved plan? | re-plans? |
|---|---|---|---|
| `terraform apply` | yes | no | yes (implicit) |
| `terraform apply -auto-approve` | no | no | yes (implicit) |
| `terraform apply myplan.tfplan` | no | yes | no |

When you apply a saved plan file, Terraform executes exactly what is in that file. No re-planning, no confirmation prompt. Safest option for automation because there are no surprises between review and execution.

### the saved plan pattern

```bash
terraform plan -out=myplan.tfplan
# review the plan output
terraform apply myplan.tfplan
```

This is the pattern used in real pipelines. One job generates a plan, a human reviews it in a PR comment or approval workflow, then a second job applies the exact plan that was reviewed. No chance of drift between review and execution.

### -auto-approve: when it is appropriate

Never use `-auto-approve` at the terminal. It skips the confirmation prompt entirely. If your plan says "destroy 47 resources," they are gone before you can react.

The only legitimate use case is automation where approval happened somewhere else:

- CI/CD pipelines running Terraform on merge to main (PR review was the approval)
- HCP Terraform or Atlantis (policy checks were the approval)
- Scheduled jobs where the config is pre-vetted

The rule: if a human is at the terminal, do not use `-auto-approve`. If a machine is running it, you probably need both `-auto-approve` and `-input=false` (which also disables prompts for missing variable values).

---

## terraform destroy

Tears down every resource in the current state. The reverse of apply.

### what destroy actually does

Runs a plan with `-destroy` implied, shows you everything that will be destroyed, prompts for confirmation, then deletes the resources in reverse dependency order.

It does not delete your `.tf` files. It does not delete your state file (state is updated to reflect that the resources are gone). It only deletes the real infrastructure.

### the destroy workflow most people miss

You can also destroy by commenting out or deleting resources from your config and running `apply`. Terraform sees the resource in state but not in config, so it destroys it. Same result, different path.

| command | what gets destroyed |
|---|---|
| `terraform destroy` | everything in state |
| `terraform apply` after removing a resource from config | just that one resource |
| `terraform destroy -target=RESOURCE` | just that one resource (same caveat as plan -target) |

### prevent_destroy

You can tell Terraform to refuse to destroy specific resources:

```hcl
resource "aws_s3_bucket" "critical" {
  bucket = "production-logs"

  lifecycle {
    prevent_destroy = true
  }
}
```

With this set, `terraform destroy` errors out instead of deleting. You have to remove the lifecycle block first, then re-run. Safety net for production data, not a real access control.

!!! note
    `terraform destroy` is equivalent to `terraform apply -destroy`. Same operation, two names.

---

## gotchas worth remembering

- Apply refreshes state before it runs. It compares your desired state to what actually exists, not just to what was in state last time. This is why apply sometimes shows unexpected changes: someone modified infrastructure outside of Terraform. That is called drift.
- `terraform apply` with no plan file is not the same as `terraform apply saved.tfplan`. The first re-plans and can surprise you. The second executes exactly what was saved.
- A saved plan file is only valid for a short window. If too much time passes or state changes, Terraform rejects it with a "stale plan" error.
- You must run `terraform init` before `terraform validate`. Validate needs the providers downloaded to know what is a valid resource attribute.
- `-auto-approve` and `-input=false` go together in automation. One disables the confirmation prompt, the other disables prompts for missing variable values.

---

## the full workflow at a glance

```bash
terraform init       # set up the working directory
terraform fmt        # clean up formatting
terraform validate   # static check the config
terraform plan       # preview changes
terraform apply      # make the changes
terraform destroy    # tear it all down when you are done
```

In CI this collapses to `init` then `fmt -check` then `validate` then `plan` as gates, with `apply` only on approved merges to main. In local dev you run whatever you need.

---

## commands cheat sheet

| command | what it does |
|---|---|
| `terraform init` | downloads providers, initializes backend |
| `terraform init -upgrade` | bumps providers to newest allowed versions |
| `terraform fmt` | reformats .tf files to canonical style |
| `terraform fmt -check` | non-destructive format check for CI |
| `terraform validate` | syntactic and type check (requires init first) |
| `terraform plan` | shows diff between config and real infra |
| `terraform plan -out=FILE` | save plan to file for guaranteed apply |
| `terraform apply` | re-plans, prompts, then applies |
| `terraform apply FILE` | apply a saved plan exactly, no prompt |
| `terraform apply -auto-approve` | skip prompt (automation only) |
| `terraform destroy` | remove everything managed by this config |
| `terraform destroy -target=RESOURCE` | remove one specific resource (emergency only) |

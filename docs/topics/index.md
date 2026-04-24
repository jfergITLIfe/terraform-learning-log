# Topics

The eight objectives on the HashiCorp Certified: Terraform Associate (004) exam. Pages grow as I work through concepts — `Empty` means I haven't started, `Partial` means there's real content but the topic isn't fully covered, `Full` means I'm confident it covers the objective.

<div class="topic-grid">

<div class="topic-card" markdown>
### [01 — IaC Concepts](01-iac-concepts.md)
<span class="coverage-badge coverage-empty">Empty</span>

What Infrastructure as Code is, why it beats click-ops, and where Terraform fits among the alternatives.
</div>

<div class="topic-card" markdown>
### [02 — Terraform's Purpose](02-terraform-purpose.md)
<span class="coverage-badge coverage-empty">Empty</span>

Multi-cloud and provider-agnostic positioning, benefits of state, and Community Edition vs HCP Terraform.
</div>

<div class="topic-card" markdown>
### [03 — Core Workflow](03-core-workflow.md)
<span class="coverage-badge coverage-empty">Empty</span>

`init`, `fmt`, `validate`, `plan`, `apply`, `destroy`. The CLI lifecycle you'll run ten thousand times.
</div>

<div class="topic-card" markdown>
### [04 — Configuration & HCL](04-configuration-hcl.md)
<span class="coverage-badge coverage-empty">Empty</span>

Resources, data sources, variables, outputs, locals, functions, expressions, sensitive data, custom conditions.
</div>

<div class="topic-card" markdown>
### [05 — Modules](05-modules.md)
<span class="coverage-badge coverage-empty">Empty</span>

Using registry modules, building your own, module composition, and the standard module structure.
</div>

<div class="topic-card" markdown>
### [06 — State Management](06-state-management.md)
<span class="coverage-badge coverage-empty">Empty</span>

What state is, local vs remote backends, state locking, `moved` blocks, drift, and the `terraform state` subcommands.
</div>

<div class="topic-card" markdown>
### [07 — Maintain Infrastructure](07-maintain-infrastructure.md)
<span class="coverage-badge coverage-empty">Empty</span>

Importing existing resources, troubleshooting, debug logs, and ongoing workspace care.
</div>

<div class="topic-card" markdown>
### [08 — HCP Terraform](08-hcp-terraform.md)
<span class="coverage-badge coverage-empty">Empty</span>

HCP Terraform features, remote operations, CLI and VCS workflows, workspaces, variable sets, teams, dynamic credentials, drift detection, policies.
</div>

</div>

!!! note "How this grows"
    Topics are seeded with the official 004 objective and an outline. I fill them in as I work through each concept and paste notes. Screenshots for tooling and AWS console views live in `docs/assets/screenshots/` — anything from the KodeKloud course itself stays referenced by name rather than reproduced.

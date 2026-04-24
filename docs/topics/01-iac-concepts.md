# 01 — IaC Concepts

!!! abstract "Exam objective"
    **Understand Infrastructure as Code (IaC) concepts.** Explain what IaC is, the advantages of IaC patterns, and why Terraform is a good fit for IaC.

**Status:** <span class="coverage-badge coverage-partial">Partial</span>

---

## What is Infrastructure as Code

Infrastructure as Code is exactly what it sounds like: you describe your infrastructure — servers, networks, storage, databases, DNS records, IAM policies — in text files, and a tool reads those files and makes the real world match. The text files live in version control. Changes happen through code review. Infrastructure becomes a software artifact, not a pile of manually clicked console buttons.

The alternative is "click-ops" — logging into a cloud console and clicking your way through creating resources. That works for a single admin setting up a single server, but it falls apart the moment you need to do it twice, document it, review it, or hand it off.

## Why IaC beats click-ops

The shift from manual provisioning to IaC isn't just a different way to do the same thing. It changes what's possible:

- **Reproducibility.** Spin up an identical environment in minutes, not days. Dev, staging, and prod all start from the same definition.
- **Version control.** Every change is a commit. You can see who changed what, when, and why. You can revert a bad change. You can review changes before they happen.
- **Peer review.** Infrastructure changes go through a pull request like any other code change. A second set of eyes catches mistakes before they hit production.
- **Self-documenting.** The code is the documentation. There's no Confluence page that drifted out of sync with reality six months ago.
- **Speed at scale.** Provisioning 50 identical servers takes the same time as 1.
- **Disaster recovery.** If something gets nuked, you re-run the code. No heroic reconstruction from memory.

The hidden one that matters for GRC: **evidence collection.** When your infrastructure is defined in code, compliance evidence becomes API-provable. You don't screenshot a console setting to prove a control is in place — you point to the line of code, the commit history, and the audit log of the apply. That's the whole foundation of GRC engineering.

## Where Terraform fits

Terraform isn't the only IaC tool, but it sits in a specific spot:

- **Declarative** (you describe the end state, not the steps to get there), not imperative
- **Provider-agnostic** — one language for AWS, Azure, GCP, Kubernetes, GitHub, Cloudflare, Datadog, and hundreds more
- **Stateful** — it tracks what it's managing so it can make smart decisions about changes
- **Open source** core, with HashiCorp's hosted version (HCP Terraform) layered on top

Compared to the usual suspects:

| Tool | Best at | Not ideal for |
|------|---------|---------------|
| Terraform | Provisioning cloud infrastructure across providers | OS-level config management |
| Ansible | Configuring existing servers, orchestration | Declarative cloud resource management |
| CloudFormation | Deep AWS integration, AWS-only shops | Multi-cloud |
| Pulumi | Teams who want real programming languages | Shops that want HCL's constrained DSL |

The exam doesn't ask you to pick favorites, but it does expect you to know Terraform's niche: **declarative, multi-cloud infrastructure provisioning**.

## Gotchas and nuances

- IaC is not the same as configuration management. Terraform provisions the server; Ansible configures what runs on it. Most real stacks use both.
- "Infrastructure as Code" and "GitOps" aren't synonyms. GitOps is a specific pattern where a Git repo is the source of truth and an agent continuously reconciles reality with it. IaC is the broader concept — GitOps is one way to operationalize IaC.

## References

- [HashiCorp: What is Infrastructure as Code?](https://www.hashicorp.com/resources/what-is-infrastructure-as-code)
- [Terraform intro docs](https://developer.hashicorp.com/terraform/intro)

## Commands cheat sheet

No commands for this topic — pure concepts.

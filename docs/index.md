# Terraform Learning Log

A build-first reference for the **HashiCorp Certified: Terraform Associate (004)** exam.

I am working through the Terraform Associate path on KodeKloud, cross-referenced with Brikman's *Terraform: Up & Running* (4th ed.) and Bryan Krausen's 004 practice exams. Instead of linear daily logs, this site is organized by the **eight official exam objectives**. Each page grows as I learn more and builds into a reference I can grep before the exam — and that my GRC engineering study group can use too.

[Start with the topics →](topics/index.md){ .md-button .md-button--primary }
[Set up your environment →](setup.md){ .md-button }

---

## Why topic-based, not daily

A daily log is a journal. A topic reference is a tool. When you're two weeks from the exam and need to nail down the difference between `terraform import` and `import` blocks, you don't want to dig through day 17's notes. You want a single page titled "Maintain Infrastructure" that you can search, skim, and come back to.

## Why this exists at all

I'm a cybersecurity analyst moving deeper into GRC engineering — the practice of treating compliance controls as software artifacts. Infrastructure as Code is the foundation. You cannot auto-collect evidence for controls whose infra lives as clicked console buttons. Terraform is the tool. This log is me learning it honestly, in public.

## How to follow along

1. Read [Setup](setup.md) once. You need the Terraform CLI and an AWS account (about 15 minutes).
2. Pick a topic from the [Topics index](topics/index.md).
3. Clone the repo. Runnable code for each topic lives under `code/`.
4. Open a thread in [Discussions](https://github.com/jfergITLife/terraform-learning-log/discussions) if something doesn't click.

!!! warning "Cost discipline"
    Exercises that provision real AWS resources end with a `terraform destroy` step. Actually run it. Set a $5 billing alarm before you start.

## About

Jacob Ferguson — Navy veteran, cybersecurity analyst, Rice MBA candidate. Building a [GRC engineering portfolio](https://github.com/jfergITLife) under `JFergITLife`.

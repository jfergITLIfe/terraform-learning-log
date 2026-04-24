# Terraform Learning Log

A build-first reference for the **HashiCorp Certified: Terraform Associate (004)** exam, organized by the eight official exam objectives.

**Site:** [jfergITLife.github.io/terraform-learning-log](https://jfergITLife.github.io/terraform-learning-log/)

## What this is

I'm working through the Terraform Associate path on KodeKloud, Brikman's *Terraform: Up & Running* (4th ed.), and Bryan Krausen's 004 practice exams. Rather than a linear daily log, this site is organized as a reference mapped to the eight exam objectives. Pages grow as I learn — by the time I sit the exam, this is the doc I'll grep before I walk in.

If you're studying along, clone the repo and run the code in each topic's folder. Open a [Discussion](../../discussions) if you get stuck.

## Repo layout

```
terraform-learning-log/
├── docs/
│   ├── topics/              # One page per exam objective (01-08)
│   ├── assets/screenshots/  # My own screenshots (no course material)
│   ├── index.md
│   ├── setup.md
│   └── resources.md
├── code/                    # Runnable Terraform per topic
├── mkdocs.yml
└── .github/workflows/       # Auto-deploy to GitHub Pages
```

## Running the site locally

```bash
pip install mkdocs-material
mkdocs serve
```

Site at `http://127.0.0.1:8000`.

## License

MIT. Do what you want with the code and notes.

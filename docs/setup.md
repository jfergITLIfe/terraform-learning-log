# Setup

One-time setup. About fifteen minutes.

## 1. AWS account with a billing alarm

You need an AWS account with programmatic access. If you don't have one, [sign up for the free tier](https://aws.amazon.com/free/).

!!! warning "Set a billing alarm FIRST"
    In the AWS console: **Billing → Budgets → Create budget → Cost budget → $5/month** with email alerts at 80% and 100%. Do this before you provision anything.

## 2. Install Terraform

=== "Linux (Debian / Ubuntu / Mint)"

    ```bash
    wget -O- https://apt.releases.hashicorp.com/gpg | \
      sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

    echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
      https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
      sudo tee /etc/apt/sources.list.d/hashicorp.list

    sudo apt update && sudo apt install terraform
    ```

=== "macOS"

    ```bash
    brew tap hashicorp/tap
    brew install hashicorp/tap/terraform
    ```

=== "Windows"

    ```powershell
    choco install terraform
    ```

Verify:

```bash
terraform -version
```

Target version 1.12 or higher — that's what the 004 exam tests on.

## 3. AWS CLI and credentials

Follow the [official AWS CLI install guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html), then:

```bash
aws configure
```

Enter an access key, secret key, region (`us-east-1` is cheap and reliable), and `json` as the output format.

!!! tip "Use an IAM user, not root"
    Create a dedicated IAM user with programmatic access for Terraform. Never use root credentials.

## 4. Clone the repo

```bash
git clone https://github.com/jfergITLife/terraform-learning-log.git
cd terraform-learning-log
```

Runnable code for each topic lives in `code/<topic-name>/`.

## 5. Editor

Any editor works. The HashiCorp Terraform extension exists for VS Code, Windsurf, Vim, Emacs, and JetBrains tools — install whichever matches your setup for syntax highlighting and `terraform fmt` on save.

Always run `terraform fmt` before committing.

---

Start with [Topic 01 — IaC Concepts →](topics/01-iac-concepts.md).

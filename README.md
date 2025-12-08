<div align="center">

<img src="assets/jules-logo.png" alt="Jules" width="400">

## Jules Invoke
Invoke a powerful remote coding agent using GitHub Actions

[Get Started](#-quick-start) • [Examples](./examples) • [API Docs](https://developers.google.com/jules/api)

[![Built with Jules](https://img.shields.io/badge/Built%20with-Jules-715cd7?link=https://jules.google)](https://jules.google)


</div>


## What is Jules?

**[Jules](https://jules.google)** is a remote AI coding agent from [Google Labs](https://labs.google) powered by Gemini 3 Pro. It works autonomously in a cloud VM—analyzing your codebase, implementing features, fixing bugs, and creating pull requests while you focus on what matters.

**This action** lets you trigger Jules from any GitHub event: issues, pull requests, schedules, or workflow dispatches.

## ✨ Quick Start

### 1. Get a Jules API Key

1. Authenticate with GitHub at [jules.google.com](https://jules.google.com)
2. Generate an API key from your account settings
3. See [API docs](https://developers.google.com/jules/api) for details

### 2. Add to GitHub Secrets

1. Go to **Settings** → **Secrets and variables** → **Actions**
2. Click **New repository secret**
3. Name: `JULES_API_KEY`, Value: your key

### 3. Create a Workflow

Create `.github/workflows/jules.yml`:

```yaml
name: Invoke Jules

on:
  workflow_dispatch:
    inputs:
      prompt:
        description: 'Task for Jules'
        required: true

jobs:
  jules:
    runs-on: ubuntu-latest
    permissions:
      contents: read
    steps:
      - uses: google-labs-code/jules-invoke@v1
        with:
          prompt: ${{ github.event.inputs.prompt }}
          jules_api_key: ${{ secrets.JULES_API_KEY }}
```

That's it! Go to **Actions** → **Invoke Jules** → **Run workflow** to try it.

---

## 📋 Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `prompt` | **Required.** The task for Jules to perform. | — |
| `jules_api_key` | **Required.** Your Jules API key (use secrets). | — |
| `starting_branch` | Branch for Jules to start from. | `main` |
| `include_last_commit` | Include the last commit's diff in context. | `false` |
| `include_commit_log` | Include recent commit history in context. | `false` |

---

## 📂 Example Workflows

Copy these into `.github/workflows/` and customize:

| Example | Trigger | Description |
|---------|---------|-------------|
| [feature-from-issue](./examples/feature-from-issue.yml) | Issue labeled `feature` | Auto-implement features from issue descriptions |
| [bug-fixer](./examples/bug-fixer.yml) | Issue labeled `bug` | Diagnose and fix bugs with structured prompts |
| [weekly-cleanup](./examples/weekly-cleanup.yml) | Scheduled (cron) | Automated code maintenance and refactoring |
| [ci-failure-fix](./examples/ci-failure-fix.yml) | CI workflow fails | Automatically fix failed builds |
| [unblocked-issues](./examples/unblocked-issues.yml) | Issue closed | Work on issues that were blocked by the closed issue |

See [examples/README.md](./examples/README.md) for detailed setup instructions.

---

## 💡 Writing Good Prompts

Jules works best with specific, detailed prompts:

**❌ Vague:**
```
Add login
```

**✅ Specific:**
```
Add JWT authentication with:
- POST /api/login endpoint (email/password)
- Access token (1h) and refresh token (7d)
- Bcrypt password hashing
- Auth middleware for protected routes
- Integration tests for the auth flow
```

---

## 🔐 Security

> **⚠️ Important:** Issue-triggered workflows can be exploited. Always restrict who can trigger Jules.

**Use allowlists:**

```yaml
- name: Check authorization
  if: ${{ !contains(fromJSON('["trusted-user"]'), github.event.issue.user.login) }}
  run: exit 1
```

**Best practices:**
- Never commit `JULES_API_KEY`—always use secrets
- Review Jules' plan before approving changes
- Use branch protection to require PR reviews
- Treat Jules like any team member: audit its work

---

## 📚 Resources

- **Jules Homepage:** [jules.google](https://jules.google)
- **Jules Web App:** [jules.google.com](https://jules.google.com)
- **API Documentation:** [developers.google.com/jules/api](https://developers.google.com/jules/api)
- **GitHub Actions:** [docs.github.com/actions](https://docs.github.com/actions)

---

## Attribution

If you find this useful, add the badge to your README:

```markdown
[![Built with Jules](https://img.shields.io/badge/Built%20with-Jules-715cd7?link=https://jules.google)](https://jules.google)
```

---

<div align="center">

**Built with ❤️ by Google**

[Jules](https://jules.google) • [API Docs](https://developers.google.com/jules/api) • [Report Issues](https://github.com/google-labs-code/jules-invoke/issues)

</div>

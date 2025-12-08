# Example Workflows

Ready-to-use GitHub Actions workflows for common Jules use cases. Copy these into your `.github/workflows/` directory and customize as needed.

## 🔐 Security First

> **⚠️ Important:** All issue-triggered workflows include user allowlists. Always restrict who can trigger Jules to prevent abuse.

Replace `["your-username", "trusted-collaborator"]` with your actual trusted usernames.

---

## Workflows

### [feature-from-issue.yml](./feature-from-issue.yml)
**Trigger:** Issue labeled with `feature`

Automatically implement features when you label issues. Great for delegating well-specified features to Jules while you focus on architecture.

**Tips for best results:**
- Write detailed issue descriptions with clear requirements
- Include acceptance criteria when possible
- Mention specific files or patterns to follow

---

### [bug-fixer.yml](./bug-fixer.yml)
**Trigger:** Issue labeled with `bug`

Diagnose and fix bugs with a structured debugging prompt. Jules will analyze, trace, fix, and add regression tests.

**Tips for best results:**
- Include error messages and stack traces in the issue
- Describe expected vs actual behavior
- Mention steps to reproduce

---

### [weekly-cleanup.yml](./weekly-cleanup.yml)
**Trigger:** Scheduled (Mondays at 2 AM UTC) or manual

Keep your codebase clean with automated maintenance. Removes dead code, reduces duplication, and improves naming.

**Customize by:**
- Adjusting the cron schedule
- Adding/removing focus areas in the prompt
- Targeting specific directories

---

### [ci-failure-fix.yml](./ci-failure-fix.yml)
**Trigger:** When your CI workflow fails

Automatically attempt to fix CI failures. Jules analyzes the failure and proposes a fix on the failing branch.

**Setup:**
- Change `workflows: ["CI"]` to match your actual CI workflow name
- Works best for test failures and lint errors

---

### [unblocked-issues.yml](./unblocked-issues.yml)
**Trigger:** When an issue is closed

Uses the [on-unblocked](https://github.com/google-labs-code/on-unblocked) action to find issues that were blocked by the closed issue, then starts working on them.

**Setup:**
1. Use `blocked by #123` syntax in your issues to create dependencies
2. When you close issue #123, any issues blocked by it will trigger Jules

---

## Combining Workflows

You can combine multiple approaches:

```yaml
# Feature + Bug in one workflow
name: Jules Issue Handler
on:
  issues:
    types: [labeled]

jobs:
  handle:
    if: contains(fromJSON('["feature", "bug"]'), github.event.label.name)
    # ... use different prompts based on label
```

## Need Help?

- [Jules Documentation](https://jules.google/docs)
- [Jules API Reference](https://developers.google.com/jules/api)
- [GitHub Actions Docs](https://docs.github.com/actions)

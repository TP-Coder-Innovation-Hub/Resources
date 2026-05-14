# Git & PR Conventions

Consistent git practices across all challenge projects.

## Branch Naming

```
<type>/<short-description>

Examples:
feature/user-registration
feature/add-payment-flow
bugfix/login-redirect-loop
hotfix/fix-null-pointer-on-checkout
chore/update-dependencies
```

| Type | When to use |
|---|---|
| `feature/` | New functionality |
| `bugfix/` | Fixing a bug found during development |
| `hotfix/` | Urgent fix for production |
| `chore/` | Maintenance tasks (deps, config, refactoring) |
| `docs/` | Documentation changes only |

## Commit Messages

Format: `type: short description`

```
feat: add user registration endpoint
fix: handle null email in login flow
chore: update express to v4.19
docs: add setup instructions to README
refactor: extract validation logic to shared module
test: add unit tests for order calculation
```

**Rules:**
- Use lowercase, no period at the end
- Keep under 72 characters
- Use imperative mood ("add" not "added" or "adds")

## Pull Request Template

```markdown
## What changed
<!-- Brief description of changes -->

## Why
<!-- Motivation / what problem this solves -->

## How to test
<!-- Steps to verify the changes -->
1.
2.
3.

## Checklist
- [ ] Code compiles / no type errors
- [ ] Tests pass
- [ ] No secrets or credentials committed
- [ ] README updated (if needed)
```

## Code Review Checklist

When reviewing someone's PR (or your own):

- [ ] Does the code do what the PR says?
- [ ] Are there obvious bugs or edge cases?
- [ ] Is the code readable? Would a new team member understand it?
- [ ] Are functions/methods doing one thing?
- [ ] Are there tests for the new/changed logic?
- [ ] Are there any hardcoded values that should be config?
- [ ] No secrets, API keys, or credentials in the code?

## Workflow

```
main (stable)
  └── feature/my-feature
        1. Code + commit
        2. Push branch
        3. Open PR
        4. Review + address feedback
        5. Merge to main
        6. Delete branch
```

**Rules:**
- Never commit directly to `main`
- Keep PRs small and focused (one feature/fix per PR)
- Rebase or merge `main` into your branch if it falls behind
- Delete branches after merge

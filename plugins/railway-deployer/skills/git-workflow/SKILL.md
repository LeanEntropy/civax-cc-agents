---
name: git-workflow
description: Git workflow patterns for multi-environment deployments with GitHub. Applied automatically for branch management and release workflows.
---

# Git Workflow Knowledge Base

## Branch Strategy: GitFlow Simplified

This workflow is optimized for projects with staging and production environments.

```
                    Production Deploys
                          ↑
main ─────●───────────────●───────────────●──────►
          ↑               ↑               ↑
          │ merge         │ merge         │ merge
          │               │               │
develop ──●───●───●───●───●───●───●───●───●──────►
              ↑       ↑           ↑
              │       │           │
feature/a ────┘       │           │
feature/b ────────────┘           │
hotfix/x ─────────────────────────┘
                    Staging Deploys
```

## Branch Naming Conventions

| Pattern | Purpose | Example |
|---------|---------|---------|
| `main` | Production code | - |
| `develop` | Staging/integration | - |
| `feature/*` | New features | `feature/user-auth` |
| `bugfix/*` | Bug fixes | `bugfix/login-error` |
| `hotfix/*` | Production emergencies | `hotfix/security-patch` |
| `release/*` | Release preparation | `release/v1.2.0` |
| `docs/*` | Documentation | `docs/api-reference` |

## Commit Message Convention

### Format
```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types
| Type | When to Use |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting (no code change) |
| `refactor` | Code restructuring |
| `perf` | Performance improvement |
| `test` | Adding/fixing tests |
| `build` | Build system changes |
| `ci` | CI configuration |
| `chore` | Maintenance tasks |
| `revert` | Reverting changes |

### Examples

```
feat(auth): implement JWT refresh tokens

Added automatic token refresh when JWT expires.
Tokens are refreshed 5 minutes before expiration.

Closes #123
```

```
fix(api): handle null response from payment gateway

The gateway occasionally returns null instead of error object.
Added defensive null check to prevent TypeError.

Fixes #456
```

```
chore(deps): update dependencies to latest versions

- Updated React from 18.2 to 18.3
- Updated FastAPI from 0.109 to 0.115
- Ran security audit, no vulnerabilities
```

## Pull Request Best Practices

### PR Title Format
```
<type>: <brief description>
```

Examples:
- `feat: Add user authentication system`
- `fix: Resolve memory leak in image processing`
- `docs: Update API documentation`

### PR Description Template

```markdown
## Summary
Brief description of what this PR does.

## Changes
- Change 1
- Change 2
- Change 3

## Testing
How to test these changes:
1. Step 1
2. Step 2
3. Expected result

## Screenshots (if UI changes)
Before | After
--- | ---
![before](url) | ![after](url)

## Checklist
- [ ] Tests pass
- [ ] No breaking changes
- [ ] Documentation updated
- [ ] Reviewed my own code

Closes #ISSUE_NUMBER
```

## Release Process

### Version Numbering (SemVer)
```
MAJOR.MINOR.PATCH

1.0.0 → 1.0.1  (patch: bug fix)
1.0.1 → 1.1.0  (minor: new feature, backward compatible)
1.1.0 → 2.0.0  (major: breaking change)
```

### Creating a Release

```bash
# 1. Ensure develop is stable
git checkout develop
git pull origin develop

# 2. Update version (if needed)
# Edit package.json, pyproject.toml, etc.

# 3. Create release PR
gh pr create --base main --head develop \
  --title "Release v1.2.0" \
  --body "## Release v1.2.0

### New Features
- Feature A
- Feature B

### Bug Fixes
- Fix X
- Fix Y

### Breaking Changes
None"

# 4. After merge, tag
git checkout main
git pull origin main
git tag -a v1.2.0 -m "Release v1.2.0"
git push origin v1.2.0
```

## Conflict Resolution

### When Conflicts Occur
```bash
# During merge
git merge develop
# CONFLICT in file.js

# 1. Open conflicted files
# Look for conflict markers:
<<<<<<< HEAD
current branch code
=======
incoming branch code
>>>>>>> develop

# 2. Edit to resolve (keep what you need)
# 3. Mark as resolved
git add file.js

# 4. Complete merge
git commit -m "Merge develop: resolve conflicts in file.js"
```

### Avoiding Conflicts
- Pull frequently from base branch
- Keep PRs small and focused
- Communicate with team about overlapping work
- Use feature flags for long-running features

## Useful Git Commands

### Daily Commands
```bash
git status                    # Current state
git log --oneline -10         # Recent history
git diff                      # Unstaged changes
git diff --staged             # Staged changes
git branch -vv                # Branches with tracking
```

### Branch Operations
```bash
git checkout -b feature/x     # Create and switch
git branch -d feature/x       # Delete local (safe)
git branch -D feature/x       # Delete local (force)
git push origin --delete feature/x  # Delete remote
```

### History Operations
```bash
git log --graph --oneline     # Visual history
git blame file.js             # Who changed what
git show abc123               # View specific commit
git log --follow file.js      # History of renamed file
```

### Undo Operations
```bash
git checkout -- file.js       # Discard unstaged changes
git reset HEAD file.js        # Unstage file
git reset --soft HEAD~1       # Undo commit, keep changes
git reset --hard HEAD~1       # Undo commit, discard changes
git revert abc123             # Create undo commit
```

### Stashing
```bash
git stash                     # Save work temporarily
git stash list                # View stashes
git stash pop                 # Restore and delete stash
git stash apply               # Restore, keep stash
git stash drop                # Delete stash
```

## GitHub CLI (gh) Commands

```bash
# PRs
gh pr create                  # Create PR interactively
gh pr list                    # List open PRs
gh pr view 123                # View PR details
gh pr checkout 123            # Checkout PR locally
gh pr merge 123 --squash      # Merge with squash

# Issues
gh issue create               # Create issue
gh issue list                 # List issues
gh issue close 123            # Close issue

# Repository
gh repo view                  # View repo info
gh repo clone owner/repo      # Clone repository
```

## Branch Protection (Recommended)

### For `main` branch:
- Require pull request before merging
- Require at least 1 approval
- Require status checks to pass
- Require branches to be up to date
- Do not allow force pushes
- Do not allow deletions

### For `develop` branch:
- Require status checks to pass
- Allow administrators to bypass (for hotfixes)

## Git Hooks (Optional)

### pre-commit
```bash
#!/bin/sh
# Run linting before commit
npm run lint
```

### commit-msg
```bash
#!/bin/sh
# Validate commit message format
if ! grep -qE "^(feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert)(\(.+\))?: .+" "$1"; then
    echo "Invalid commit message format"
    exit 1
fi
```

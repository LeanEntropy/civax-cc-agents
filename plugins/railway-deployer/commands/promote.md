---
description: Promote staging to production (merge develop → main)
allowed-tools: Bash, Read, Grep, mcp__Railway__*
---

# Promote Command

Promote verified staging changes to production environment.

## Usage
```
/railway-deployer:promote [--tag v1.2.3]
```

## What This Does

1. **Verify staging is healthy**
   - Check Railway staging deployment status
   - Review recent staging logs

2. **Compare branches**
   - Show commits in develop not in main
   - List changed files

3. **Create promotion**
   - Option A: Create PR (develop → main)
   - Option B: Direct merge (if configured)

4. **Tag release** (if --tag provided)
   - Create annotated git tag
   - Push tag to origin

5. **Monitor production deployment**
   - Watch Railway logs
   - Verify health check

## Pre-flight Checks
- develop branch has changes not in main
- Staging deployment is healthy
- No pending PRs blocking merge
- User confirms promotion

## Workflow
```
develop (staging)
    │
    ├── Verify staging healthy
    │
    ├── Create PR or merge
    │         │
    │         ▼
    └──────► main (production)
              │
              └── Tag release (optional)
```

## Example Output
```
Promoting staging → production...

Staging Health Check:
✓ Deployment: healthy
✓ Last deploy: 2 hours ago
✓ No errors in recent logs

Changes to promote:
- abc123 feat: add user dashboard
- def456 fix: resolve login timeout
- ghi789 docs: update API docs

Create PR for promotion? [Y/n] Y

✓ PR #42 created: develop → main
  Title: "Release v1.2.3"

After PR is merged, run:
  /railway-deployer:promote --tag v1.2.3
```

---
description: Check deployment status across all environments
allowed-tools: Bash, Read, Grep, mcp__Railway__*
---

# Status Command

Check current deployment status for all environments.

## Usage
```
/railway-deployer:status [staging|production|all]
```

## What This Shows

### Git Status
- Current branch
- Uncommitted changes
- Commits ahead/behind remote

### Environment Status
For each environment (staging, production):
- Deployment health
- Last deployment time
- Current commit hash
- Recent errors (if any)

### Branch Comparison
- Commits in develop not in main
- Pending PRs

## Example Output
```
╔══════════════════════════════════════════════════╗
║           DEPLOYMENT STATUS                       ║
╚══════════════════════════════════════════════════╝

Git Status:
  Branch: develop
  Status: clean
  Remote: up-to-date

┌─────────────────────────────────────────────────┐
│ STAGING (develop)                               │
├─────────────────────────────────────────────────┤
│ Status:     ✓ healthy                           │
│ Deployed:   10 minutes ago                      │
│ Commit:     abc1234 (feat: add dashboard)       │
│ Services:   backend ✓, frontend ✓               │
└─────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────┐
│ PRODUCTION (main)                               │
├─────────────────────────────────────────────────┤
│ Status:     ✓ healthy                           │
│ Deployed:   2 days ago                          │
│ Commit:     xyz7890 (Release v1.2.0)            │
│ Services:   backend ✓, frontend ✓               │
│ Version:    v1.2.0                              │
└─────────────────────────────────────────────────┘

Pending Changes:
  3 commits in develop not in main
  - abc1234 feat: add dashboard
  - def5678 fix: login timeout
  - ghi9012 chore: update deps
```

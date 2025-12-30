---
description: Rollback a deployment to previous version
allowed-tools: Bash, Read, Grep, mcp__Railway__*
---

# Rollback Command

Rollback a deployment to a previous version.

## Usage
```
/railway-deployer:rollback [staging|production] [--to <commit|tag>]
```

## Rollback Methods

### Method 1: Git Revert (Recommended)
Creates a new commit that undoes changes. Preserves history.

```bash
git revert <commit-hash>
git push origin main
# Railway auto-deploys the revert
```

### Method 2: Railway Dashboard
Rollback to specific deployment in Railway UI:
1. Go to Railway Dashboard
2. Select Service → Deployments
3. Click "Rollback" on previous healthy deployment

### Method 3: Hard Reset (Emergency Only)
**Warning**: Rewrites history, requires force push.

```bash
git reset --hard <commit>
git push --force origin main  # DANGEROUS
```

## Interactive Workflow
```
Rollback Production
===================

Current deployment:
  Commit: abc1234 (feat: new feature)
  Deployed: 30 minutes ago
  Status: unhealthy ⚠️

Recent deployments:
  1. abc1234 - feat: new feature (30m ago) ⚠️
  2. def5678 - fix: bug fix (2h ago) ✓
  3. ghi9012 - Release v1.2.0 (1d ago) ✓

? Rollback to which deployment?
  > def5678 - fix: bug fix
    ghi9012 - Release v1.2.0
    Enter specific commit/tag

? Rollback method:
  > Git revert (recommended)
    Railway rollback
    Git reset (dangerous)

Creating revert commit...
✓ Reverted abc1234
✓ Pushed to origin/main
✓ Railway deployment triggered

Monitoring rollback...
✓ Build successful
✓ Deployment healthy

Rollback complete!
Production is now running: def5678
```

## When to Rollback

### Immediate Rollback Needed
- Application crashes on startup
- Critical functionality broken
- Security vulnerability discovered
- Data corruption risk

### Can Wait for Fix
- Minor UI bugs
- Non-critical feature broken
- Performance degradation (if tolerable)

## Post-Rollback Actions
1. Verify production is healthy
2. Investigate root cause
3. Fix issue on develop branch
4. Test thoroughly in staging
5. Re-deploy when ready

## Emergency Contacts
For critical production issues:
1. Check Railway status: https://status.railway.app
2. Railway Discord: https://discord.gg/railway
3. Supabase status: https://status.supabase.com

---
description: Deploy to staging or production environment
allowed-tools: Bash, Read, Grep, mcp__Railway__*
---

# Deploy Command

Deploy the current codebase to Railway environment.

## Usage
```
/railway-deployer:deploy [staging|production]
```

## What This Does

### For Staging (develop branch)
1. Verify you're on develop branch
2. Check for uncommitted changes
3. Pull latest from origin
4. Push to trigger Railway auto-deploy
5. Show deployment status

### For Production (main branch)
1. **Confirm with user** - production deployments require confirmation
2. Verify staging is stable
3. Merge develop → main (or confirm merge is done)
4. Push to trigger Railway auto-deploy
5. Monitor deployment logs

## Pre-flight Checks
- Branch is correct (develop for staging, main for production)
- No uncommitted changes
- Branch is up-to-date with remote
- Previous deployment was healthy

## After Deployment
- Check Railway logs for errors
- Verify health check passes
- Test critical functionality

## Example Output
```
Deploying to staging...
✓ On develop branch
✓ No uncommitted changes
✓ Branch up-to-date
✓ Pushing to origin/develop
✓ Railway deployment triggered
✓ Monitoring logs...

Deployment successful!
- Service: backend
- Environment: staging
- Commit: abc123
- Status: healthy
```

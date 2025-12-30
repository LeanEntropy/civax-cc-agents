---
description: Sync environment variables between local and Railway
allowed-tools: Bash, Read, Grep, Write, mcp__Railway__*
---

# Sync Environment Variables Command

Synchronize environment variables between local .env and Railway environments.

## Usage
```
/railway-deployer:sync-env [compare|push|pull] [staging|production]
```

## Operations

### compare (default)
Show differences between local .env and Railway environment.

### push
Push local .env variables to Railway (with confirmation).
**Warning**: This overwrites Railway variables!

### pull
Pull Railway variables to a local file for reference.
Creates `.env.staging` or `.env.production`.

## Example: Compare
```
/railway-deployer:sync-env compare staging

Comparing local .env with staging environment...

Variables in local but NOT in staging:
  - NEW_API_KEY
  - DEBUG_MODE

Variables in staging but NOT in local:
  - RAILWAY_ENVIRONMENT
  - DATABASE_URL

Variables with different values:
  - CORS_ORIGINS
    Local:   http://localhost:3000
    Staging: https://staging.example.com

Variables matching: 15
```

## Example: Pull
```
/railway-deployer:sync-env pull staging

Pulling staging variables...
✓ Saved to .env.staging (23 variables)

Note: Review before copying to .env
      Secrets are masked for security
```

## Example: Push
```
/railway-deployer:sync-env push staging

⚠️  WARNING: This will update staging variables!

Variables to update:
  - NEW_API_KEY (add)
  - FEATURE_FLAG (update)

Proceed? [y/N] y

✓ Updated 2 variables in staging
✓ Redeployment triggered
```

## Safety Rules
- Never push to production without explicit confirmation
- Secrets are masked in output
- Creates backup before overwriting
- Shows diff before any changes

---
description: Set up Railway deployment structure for a project
allowed-tools: Bash, Read, Grep, Write, mcp__Railway__*
---

# Setup Command

Initialize Railway + Git deployment structure for a project.

## Usage
```
/railway-deployer:setup
```

## What This Does

### 1. Verify Prerequisites
- Git repository exists
- Railway CLI installed and authenticated
- GitHub remote configured

### 2. Git Branch Setup
- Ensure `main` branch exists
- Ensure `develop` branch exists
- Set up tracking for both branches

### 3. Railway Project Setup
- Link to existing Railway project OR create new one
- Create staging environment (linked to develop)
- Create production environment (linked to main)

### 4. Environment Variables
- Create `.env.example` template if missing
- Prompt for required variables
- Set variables in Railway environments

### 5. Configuration Files
- Create/update `.gitignore` for .env files
- Create `railway.json` if needed

## Interactive Workflow
```
Railway Deployment Setup
========================

Checking prerequisites...
✓ Git repository found
✓ Railway CLI installed (v3.x.x)
✓ Railway authenticated as user@example.com
✓ GitHub remote: origin → github.com/user/project

Git Branches:
✓ main branch exists
✓ develop branch exists

Railway Project:
? Select or create Railway project:
  > Use existing project
    Create new project

? Select existing project:
  > my-app-production
    other-project

Environments:
? Environment for staging (develop branch):
  > Create new environment "staging"
    Use existing environment

? Environment for production (main branch):
  > Use existing environment "production"
    Create new environment

Environment Variables:
Found .env.example with 15 variables.
? Set up staging variables now? [Y/n]

Setting staging variables...
? ANTHROPIC_API_KEY: ********
? DATABASE_URL: [use Railway Postgres]
...

Setup Complete!
===============
✓ Git branches configured
✓ Railway project linked
✓ Staging environment ready (develop → staging)
✓ Production environment ready (main → production)

Next steps:
1. Push to develop to deploy staging
2. Use /railway-deployer:status to check deployments
3. Use /railway-deployer:promote when ready for production
```

## Files Created/Modified

| File | Purpose |
|------|---------|
| `.env.example` | Template (if missing) |
| `.gitignore` | Ensure .env ignored |
| `railway.json` | Railway config (if needed) |

## Post-Setup Verification
- Run `/railway-deployer:status` to verify
- Push a test commit to develop
- Check Railway dashboard for deployment

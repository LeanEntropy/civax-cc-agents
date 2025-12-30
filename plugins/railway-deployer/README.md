# Railway Deployer Plugin for Claude Code

A comprehensive deployment management plugin for Railway + Git workflows with staging/production environments.

## Features

- **Multi-environment deployment**: Staging (develop) and Production (main)
- **Git workflow automation**: Branch management, PRs, releases
- **Environment variable sync**: Local ↔ Railway synchronization
- **Deployment monitoring**: Status checks, logs, health monitoring
- **Rollback support**: Safe rollback procedures

## Installation

### Option 1: Local Plugin (Recommended for Development)
```bash
# The plugin is located at:
~/.claude/plugins/railway-deployer/

# Claude Code will automatically detect plugins in ~/.claude/plugins/
```

### Option 2: Project-Specific
Copy the plugin to your project:
```bash
cp -r ~/.claude/plugins/railway-deployer ./.claude/plugins/
```

## Prerequisites

1. **Railway CLI** installed and authenticated:
   ```bash
   npm install -g @railway/cli
   railway login
   ```

2. **Git** with GitHub remote configured

3. **Branch structure**:
   - `main` branch for production
   - `develop` branch for staging

## Agents

| Agent | Description |
|-------|-------------|
| `railway-devops` | **Complete setup specialist** - Use for initial Railway setup, creating Dockerfiles, configuring environments |
| `railway-ops` | Day-to-day deployment orchestrator |
| `git-workflow` | Git branch and PR management |
| `env-sync` | Environment variable synchronization |

## Slash Commands

| Command | Description |
|---------|-------------|
| `/railway-deployer:setup` | Initialize Railway structure for a project |
| `/railway-deployer:status` | Check deployment status |
| `/railway-deployer:deploy` | Deploy to staging or production |
| `/railway-deployer:promote` | Promote staging to production |
| `/railway-deployer:sync-env` | Sync environment variables |
| `/railway-deployer:rollback` | Rollback a deployment |

## Quick Start

### 1. Set up a new project
```
/railway-deployer:setup
```

### 2. Check current status
```
/railway-deployer:status
```

### 3. Deploy to staging
```
/railway-deployer:deploy staging
```

### 4. Promote to production
```
/railway-deployer:promote --tag v1.0.0
```

## Environment Architecture

```
┌─────────────────────────────────────────┐
│           PRODUCTION                     │
│    Railway ← main branch                 │
└─────────────────────────────────────────┘
                    ↑
              merge/promote
                    │
┌─────────────────────────────────────────┐
│            STAGING                       │
│    Railway ← develop branch              │
└─────────────────────────────────────────┘
                    ↑
             push changes
                    │
┌─────────────────────────────────────────┐
│             LOCAL                        │
│    .env files for development            │
└─────────────────────────────────────────┘
```

## Usage Examples

### Deploy workflow
```
# 1. Make changes on develop
git checkout develop
# ... make changes ...
git add . && git commit -m "feat: new feature"
git push origin develop
# Railway auto-deploys to staging

# 2. Verify staging works
/railway-deployer:status staging

# 3. Promote to production
/railway-deployer:promote --tag v1.1.0
```

### Environment variable management
```
# Compare local vs staging
/railway-deployer:sync-env compare staging

# Pull staging vars for reference
/railway-deployer:sync-env pull staging

# Push local vars to staging (careful!)
/railway-deployer:sync-env push staging
```

### Emergency rollback
```
/railway-deployer:rollback production
```

## Skills (Knowledge Base)

The plugin includes knowledge bases that Claude uses automatically:

- **railway-deployment**: Railway platform patterns and best practices
- **git-workflow**: Git branching strategies and conventions

## File Structure

```
railway-deployer/
├── .claude-plugin/
│   └── plugin.json           # Plugin manifest
├── agents/
│   ├── railway-devops.md     # Complete setup specialist
│   ├── railway-ops.md        # Day-to-day orchestrator
│   ├── git-workflow.md       # Git specialist
│   └── env-sync.md           # Env var management
├── skills/
│   ├── railway-deployment/
│   │   └── SKILL.md
│   └── git-workflow/
│       └── SKILL.md
├── commands/
│   ├── setup.md
│   ├── status.md
│   ├── deploy.md
│   ├── promote.md
│   ├── sync-env.md
│   └── rollback.md
└── README.md
```

## Best Practices

1. **Always test in staging first** before promoting to production
2. **Use semantic versioning** for releases (v1.2.3)
3. **Keep secrets out of git** - use Railway environment variables
4. **Monitor deployments** - check logs after every deploy
5. **Document breaking changes** in release notes

## Troubleshooting

### Railway CLI not found
```bash
npm install -g @railway/cli
railway login
```

### Branch not tracking remote
```bash
git branch --set-upstream-to=origin/develop develop
git branch --set-upstream-to=origin/main main
```

### Deployment stuck
1. Check Railway dashboard for build logs
2. Verify Dockerfile/build configuration
3. Check resource limits

## Contributing

Feel free to extend this plugin with additional agents, commands, or skills for your specific workflows.

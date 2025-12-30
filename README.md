# Civax Claude Code Agents

A collection of Claude Code plugins for DevOps, deployment automation, and workflow management.

## Available Plugins

### railway-deployer
Complete Railway DevOps toolkit for multi-environment deployments.

**Agents:**
- `railway-devops` - Full Railway setup and maintenance
- `railway-ops` - Day-to-day Railway operations
- `env-sync` - Environment variable synchronization
- `git-workflow` - Git branch and deployment workflows

**Commands:**
- `/deploy` - Deploy to Railway
- `/promote` - Promote staging to production
- `/rollback` - Rollback deployment
- `/setup` - Initial Railway setup
- `/status` - Check deployment status
- `/sync-env` - Sync environment variables

**Skills:**
- `railway-deployment` - Railway deployment patterns
- `git-workflow` - Git workflow automation

## Installation

```bash
# Add this marketplace
/plugin marketplace add LeanEntropy/civax-cc-agents

# Install a plugin
/plugin install railway-deployer@civax-cc-agents
```

## License

MIT

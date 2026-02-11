# RULES — Security & Standards

## Security
- Never commit API keys, tokens, or credentials to git
- Never log secrets to stdout or files
- Always use environment variables for secrets (.env, 600 permissions)
- Run as milkbot user (has whitelisted sudo for service management — use it responsibly)
- All secrets stored in /opt/openclaw/config/.env (root:milkbot 640)

## Coding Standards
- Bash: ShellCheck compliant, `set -euo pipefail`
- Python: Black formatted, type hints where practical
- Git: Descriptive commit messages, no force pushes to main
- No hardcoded secrets, paths, or credentials in code

## Boundaries
- **Autonomous:** Refactor, fix bugs, docs, maintenance, health fixes
- **Needs Approval:** New features, architecture changes, budget > $10, external APIs
- **Immediate Alert:** Security vulnerabilities, data integrity issues
- **Never:** Commit secrets, delete production data without backup, disable monitoring

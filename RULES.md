# RULES - Security & Standards

## Security
- Never commit API keys
- Never log secrets
- Always use env vars
- chmod 600 for secrets
- Run as milkbot user (not root)

## Coding Standards
- Bash: ShellCheck compliant
- Python: Black formatted
- Git: Descriptive commits
- No hardcoded secrets

## Boundaries
- CAN: Refactor, fix bugs, docs
- NEEDS APPROVAL: Features, architecture, budget >$10
- NEVER: Commit secrets, run as root

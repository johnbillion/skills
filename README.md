# Skills

My agent skills for Claude Code and other compatible AI agents.

## Available skills

### [security-vulnerability-triage](skills/security-vulnerability-triage/SKILL.md)

Autonomously assess the validity of a WordPress security vulnerability report.

Assumptions:

- The WordPress core local development environment is in use at http://localhost:8889.
- Playwright MCP is available.
- WordPress-Trac MCP is available.
- [wp-cli.local.yml](wp-cli.local.yml) is in place in the environment

Usage:

- Save the report description to a file then run the skill with `/security-vulnerability-triage report.md`.

## Installation

### Claude Code

Add this marketplace to Claude Code:

```
/plugin marketplace add johnbillion/skills
```

Install the skills:

```
/plugin install wordpress-skills@johnbillion-skills
```

## License

MIT

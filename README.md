# Skills

My agent skills for Claude Code and other compatible AI agents.

## Available skills

### [security-vulnerability-triage](skills/security-vulnerability-triage/SKILL.md)

Autonomously assess the validity of a WordPress security vulnerability report.

Assumptions:

- The WordPress core local development environment is in use at http://localhost:8889.
- [Playwright MCP](https://github.com/microsoft/playwright-mcp) is available.
- [WordPress-Trac MCP](https://github.com/jameswlepage/trac-mcp) is available.
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

Skills installed via a plugin marketplace don't auto-update by default. Change this by running `/plugin`, then go to Marketplaces -> johnbillion/skills -> Enable auto-update.

## Important

If you're using skills to process sensitive information, ensure that you've configured the privacy settings of your AI agent so your data is not used for model training.

- [Claude Code model improvement privacy settings](https://privacy.claude.com/en/articles/12109829-how-do-i-change-my-model-improvement-privacy-settings).

## License

MIT

# Skills Collection

Personal collection of AI agent skills for OpenCode, Claude Code, and other coding agents.

## Available Skills

### log-driven-debugging

Systematic verification-based debugging for JavaScript/TypeScript (Node.js, Bun, browser). Guides agents to add console.log statements and verify hypotheses with actual logs rather than assuming root causes.

**Install:**
```bash
npx add-skill bakrrrrr/skills --skill log-driven-debugging
```

## Adding Skills

Add new skills as directories with `SKILL.md` files:

```
skill-name/
├── SKILL.md
└── references/
    └── patterns.md
```

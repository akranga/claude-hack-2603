---
name: make-skill
description: 'Create new Agent Skills for GitHub Copilot from prompts or by duplicating this template. Use when asked to "create a skill", "make a new skill", "scaffold a skill", or when building specialized AI capabilities with bundled resources. Generates SKILL.md files with proper frontmatter, directory structure, and optional scripts/references/assets folders.'
---

# Make Skill

Scaffold new Agent Skills — create the folder, generate `SKILL.md` with frontmatter, and wire up optional resource directories.

**Triggers:** "create a skill", "make a new skill", "scaffold a skill", "add a capability"

## Workflow

### 1. Create skill directory

```
.github/skills/<skill-name>/
└── SKILL.md
```

Name: lowercase, hyphens only, 1-64 chars.

### 2. Generate SKILL.md frontmatter

```yaml
---
name: <skill-name>
description: '<WHAT it does>. Use when <triggers/keywords>.'
---
```

| Field | Req | Constraints |
|-------|-----|-------------|
| `name` | Yes | Must match folder name |
| `description` | Yes | 10-1024 chars; include WHAT + WHEN + keywords |
| `license` | No | License name or `LICENSE.txt` reference |
| `compatibility` | No | Environment requirements |
| `metadata` | No | Key-value pairs |
| `allowed-tools` | No | Space-delimited pre-approved tools |

**Description is the discovery mechanism.** Always include capabilities, triggers, and keywords users might say.

Good: `'Toolkit for testing web apps with Playwright. Use when asked to verify UI, capture screenshots, or view console logs.'`
Bad: `'Web testing helpers'`

### 3. Write the body

Recommended sections after frontmatter:

`# Title` · `## When to Use` · `## Prerequisites` · `## Workflows` · `## Troubleshooting` · `## References`

### 4. Add resource directories (optional)

| Folder | Purpose |
|--------|---------|
| `scripts/` | Executable automation (Python, Bash, JS) |
| `references/` | Docs the agent reads (API refs, schemas) |
| `assets/` | Static files used as-is (images, fonts) |
| `templates/` | Starter code the agent modifies |

## Quick Start

1. Copy an existing skill folder → rename to `<skill-name>`
2. Update `name:` to match folder, write keyword-rich `description:`
3. Replace body with your instructions
4. Add resource directories as needed

## Validation

- [ ] Folder name = lowercase + hyphens, matches `name:` field
- [ ] Description: 10-1024 chars, explains WHAT and WHEN, single-quoted
- [ ] Body under 500 lines; bundled assets under 5MB each

## References

- [Agent Skills spec](https://agentskills.io/specification)

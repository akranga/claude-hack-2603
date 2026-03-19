# Claude Code Best Practices (5-min Talk)

---

## 1. Introduce Yourself to Claude (~30s)

Tell Claude about yourself, your role, your project, your preferences.

- Claude stores it in **local auto memory** — a `MEMORY.md` file
- Next session: Claude **already knows** your habits, tooling, and conventions
- It learns from your **corrections** over time — improving with every session

**How it works:** You tell Claude about your context → Saved to `~/.claude/projects/<project>/memory/` → Tailored behavior in every future session.

> Auto memory adapts Claude to your habits and project specifics without you repeating yourself.

---

## 2. Context Engineering: CLAUDE.md First (~60s)

The #1 thing: give Claude persistent context it can't infer from code.

- Run `/init` — generates a starter CLAUDE.md from your project
- Keep it **under 200 lines** — bloated = ignored
- Only put what Claude gets wrong without it: build commands, style rules, workflow conventions
- Prune ruthlessly. If Claude already does it right — delete the rule

**Hierarchy:** `~/.claude/CLAUDE.md` (personal) → `./CLAUDE.md` (project, shared via git) → `.claude/rules/` (topic files, path-scoped)

> CLAUDE.md is loaded every session. It's your "always-on" context.

---

## 3. Skills + CLI Tools over MCPs (~45s)

**Skills** (`.claude/skills/SKILL.md`) = domain knowledge + reusable workflows.
**MCPs** = live tool connections (Figma, databases, Playwright).
**CLI tools** (`gh`, `aws`, `gcloud`) = most context-efficient way to talk to external services.

They're **complementary**, but skills have advantages:
- Loaded on demand — **zero context cost** when not invoked
- Faster to create than spinning up an MCP server
- Portable — install across teams with **Skillfish**: `npx skillfish add owner/repo`
- Wrap skills into a **plugin** for org-wide distribution and versioning

**Meta-skill:** use a skill-builder skill to create new skills. Bootstrap a shared library that compounds across your whole team.

Claude can learn unfamiliar CLIs from `--help`.

> Skills for knowledge. MCPs for live connections. CLI for everything else.

---

## 4. Subagents: Specialized Workers (~45s)

When a task is specific — don't stuff it into CLAUDE.md. Create a **subagent**.

- Lives in `.claude/agents/code-reviewer.md`
- Runs in its **own context window** — doesn't pollute your main session
- Has its own tools, model, and permissions
- Claude delegates automatically based on description, or you invoke with `@agent-name`

| Skills | Subagents |
|--------|-----------|
| Knowledge & workflows | Specialized workers |
| Runs in your context | Separate context window |
| Reusable across projects | Scoped to project or user |
| Distill repeatable patterns here | When you need isolated execution |

**Examples:** code-reviewer (read-only, Sonnet), debugger (with Edit, Opus), security-scanner (Haiku, fast)

**Code Review with Claude:**
- Create a review subagent that checks style, bugs, and security
- Use in CI/CD for automated PR review
- Pair human review with Claude review — Claude catches mechanical issues, you focus on design

> When subagents repeat the same patterns → distill into a Skill. That's where compounding happens.

---

## 5. /review — Code Review with Claude (~30s)

Claude Code has a built-in `/review` command. Use it.

- `/review` — analyzes staged/unstaged changes or a specific PR instantly
- Create a **review subagent**: `.claude/agents/code-reviewer.md` — read-only, Sonnet, own context
- Claude catches **mechanical issues**: style violations, bugs, security gaps
- You focus on **design and intent** — what Claude can't judge
- Run in **CI/CD**: `claude --print -p "Review this PR"` in GitHub Actions

**Review agent setup:**
1. Create `.claude/agents/code-reviewer.md` with read-only permissions
2. Use structured output: critical / major / minor severity
3. Iterate on instructions — when it misses things, update the agent

See [REVIEW.md](../REVIEW.md) for detailed review guidelines and checklists.

> Pair Claude review with human review. **Iterate on your review agent** — when it misses things, update the instructions.

---

## 6. Not Everyone Is Ready for Spec-Driven Dev (~30s)

The full workflow: **Interview → Spec → Fresh Session → Implement → Iterate**

```
"Interview me about [feature]. Dig into edge cases."
→ Claude writes SPEC.md
→ Start fresh: claude --new
→ "Implement @SPEC.md. Write tests first."
```

| Skills + Subagents | Spec-Driven |
|--------------------|-------------|
| Tactical: review, debug, test | Strategic: new features, major refactors |
| Low adoption barrier | Requires discipline to write specs first |
| Any developer, immediately | Teams with complex features & review process |

> Identify who on your team would benefit. Not everyone needs the full lifecycle.

---

## 7. Explore → Plan → Code → Verify (~45s)

Claude Code performs well with XP-style practices. The canonical loop:

1. **Plan Mode** (`Shift+Tab`): explore the codebase, read files, no changes
2. **Plan**: ask Claude for implementation plan (`Ctrl+G` to edit it)
3. **Implement**: switch to Normal Mode, code against the plan
4. **Verify**: run tests, take screenshots, compare output

> **#1 leverage point from Anthropic:** give Claude a way to verify its own work.
> Tests, screenshots, expected outputs. Without verification — you're the only feedback loop.

**Make Claude self-verifiable:** set up conditions where Claude can check its own work — run on an emulator, inspect logs, compare screenshots, validate API responses. The more it can verify autonomously, the less you become the bottleneck.

**Hooks — guaranteed checks:**
- Pre/post command hooks run automatically before or after Claude's actions
- Guarantee linting, tests, or security checks happen on every change
- Configure in `.claude/hooks/` — Claude can't skip what's automated

Use `/rewind` (or `Esc+Esc`) for checkpoints. After 2 failed corrections → `/clear` and start fresh with a better prompt.

---

## 8. Manage Context Aggressively (~30s)

**Guiding principle:** give Claude the **minimum context sufficient** for the task. Don't overload — every extra file or instruction dilutes focus.

Context window = your most important resource. It fills fast, performance degrades.

- `/clear` between unrelated tasks
- `/compact` to summarize and free space
- Subagents for investigation (they explore in separate context)
- `/btw` for side questions that don't enter history
- `--continue` / `--resume` to pick up where you left off

**Multi-project setup:** use `/add-dir` to bring in additional project directories, or launch Claude from a parent folder containing your subprojects. Reason across repos without polluting context permanently.

> A clean session with a better prompt always beats a long session with accumulated corrections.

---

## 9. Tune the Model IQ (~30s)

Match model to task complexity:

| Model | Use for |
|-------|---------|
| **Haiku** | Fast exploration, simple edits, subagent workers |
| **Sonnet** | Daily driver — balanced speed & capability |
| **Opus** | Complex architecture, multi-file refactors, hard bugs |

**Reasoning budget:** "think", "think hard", "ultrathink" — controls how deeply Claude reasons before responding. Use `/effort` to adjust.

Use `/model` to switch mid-session as task complexity changes.

> Right model + right reasoning depth = optimal cost & quality.

---

## 10. Claude Remote Control (~20s)

Start a task on your laptop, **continue from anywhere** — phone, tablet, web.

- Kick off long-running tasks and **walk away**
- Review results & approve permissions remotely
- Pair with **agent teams** for multi-session orchestration

**Workflow:** Start task on laptop → Monitor & approve from mobile → Review PR from anywhere.

> Requires **Enterprise license**. Enables asynchronous agentic workflows at scale.

---

## Recap (15s)

1. **Introduce yourself** — auto memory tailors Claude to your habits
2. **CLAUDE.md** — persistent context, keep it lean
3. **Skills + CLI** — share as plugins, zero context cost
4. **Subagents** — specialized workers → distill into skills
5. **/review** — pair Claude + human review, automate in CI
6. **Spec-driven** — for teams ready for full lifecycle
7. **Explore → Plan → Code → Verify** — hooks guarantee checks
8. **Manage context** — minimal sufficient, `/clear`, `/add-dir`
9. **Tune IQ** — right model + right reasoning for the task
10. **Remote Control** — start on laptop, continue from anywhere (Enterprise)

# Code Review Guidelines

## Purpose
Perform thorough, constructive code reviews without making changes to the codebase.
Use Claude Code's built-in `/review` command or dedicated review subagents.

## Claude Code Review Modes

### Quick Review: `/review`
- Built-in command — run in any Claude Code session
- Analyzes staged/unstaged changes or a specific PR
- Zero setup — works out of the box

### Review Subagent
- Create `.claude/agents/code-reviewer.md` — read-only, uses Sonnet
- Runs in its own context window — doesn't pollute your main session
- Invoke with `@code-reviewer` or let Claude auto-delegate
- Restrict permissions: no file edits, no terminal — review only

### CI/CD Integration
- Run `claude --print -p "Review this PR"` in GitHub Actions / GitLab CI
- Pair with human review — Claude catches mechanical issues, humans focus on design
- Use hooks to trigger review automatically on every push

## Core Principles

### What to Do
- **Analyze** changes thoroughly
- **Identify** issues and potential improvements
- **Explain** the reasoning behind suggestions
- **Provide** specific examples of what could be improved
- **Highlight** both strengths and weaknesses

### What NOT to Do
- **DO NOT** create commits or fix issues
- **DO NOT** modify any files
- **DO NOT** create pull requests
- **DO NOT** push changes
- **DO NOT** run automated fixes (linters, formatters, etc.)

## Review Process

### 1. Understand the Context
- Read PR description and linked issues
- Check diff to understand scope of changes
- Review commit history for context
- Identify the type of change (feature, bugfix, refactor, etc.)

### 2. Analyze Changes

#### Code Quality
- **Readability**: Is the code easy to understand?
- **Maintainability**: Will this be easy to maintain long-term?
- **Simplicity**: Is the solution as simple as it can be?
- **Consistency**: Does it follow project conventions?
- **Documentation**: Are complex parts well-documented?

#### Correctness
- **Logic errors**: Are there any bugs or edge cases?
- **Type safety**: Are types used correctly?
- **Error handling**: Are errors handled appropriately?
- **Null safety**: Are null/undefined cases handled?

#### Performance
- **Efficiency**: Are there obvious performance issues?
- **Resource usage**: Memory leaks, unnecessary allocations?
- **Scalability**: Will this work at scale?

#### Security
- **Vulnerabilities**: SQL injection, XSS, command injection, etc.
- **Data validation**: Is input properly validated?
- **Authentication/Authorization**: Are security checks in place?
- **Secrets**: No hardcoded credentials or sensitive data?

#### Testing
- **Coverage**: Are there appropriate tests?
- **Test quality**: Do tests actually validate behavior?
- **Edge cases**: Are boundary conditions tested?

#### Architecture
- **Design patterns**: Are patterns used appropriately?
- **Separation of concerns**: Is code well-organized?
- **Dependencies**: Are dependencies appropriate?
- **Coupling**: Is coupling minimized?

### 3. Provide Feedback

#### Structure Your Review
```markdown
## Overview
[Brief summary of what the PR does]

## Strengths
- [Highlight good practices]
- [Note clever solutions]
- [Acknowledge improvements]

## Issues

### Critical
- **[Issue]**: [Description]
  - Location: [file:line]
  - Impact: [Why this matters]
  - Suggestion: [How to fix]

### Major
- **[Issue]**: [Description]
  - Location: [file:line]
  - Rationale: [Why this is a problem]
  - Alternative: [Better approach]

### Minor
- **[Issue]**: [Description]
  - Location: [file:line]
  - Improvement: [How to improve]

## Questions
- [Clarifications needed]
- [Unclear design decisions]
```

#### Feedback Guidelines
- **Be specific**: Reference exact files and line numbers
- **Be constructive**: Focus on improvement, not criticism
- **Be clear**: Explain the "why" behind suggestions
- **Prioritize**: Distinguish critical from nice-to-have
- **Provide examples**: Show what better code looks like
- **Ask questions**: When intent is unclear

#### Severity Levels
- **Critical**: Must fix (security, data loss, crashes)
- **Major**: Should fix (bugs, poor design, performance)
- **Minor**: Nice to have (style, optimization, refactoring)
- **Nitpick**: Optional (subjective preferences)

## Review Checklist

- [ ] PR description matches changes
- [ ] Code follows project style guide
- [ ] No security vulnerabilities
- [ ] Error handling is appropriate
- [ ] No obvious bugs or logic errors
- [ ] Performance considerations addressed
- [ ] Tests exist and are meaningful
- [ ] No unnecessary complexity
- [ ] Documentation updated if needed
- [ ] No hardcoded secrets or config
- [ ] Dependencies are justified
- [ ] Breaking changes are documented
- [ ] Backward compatibility considered

## Common Issues to Look For

### Code Smells
- God classes/functions doing too much
- Duplicate code that should be abstracted
- Magic numbers without explanation
- Overly complex conditionals
- Deep nesting (>3 levels)
- Long parameter lists (>4 params)
- Inconsistent naming

### Anti-Patterns
- Premature optimization
- Over-engineering
- Not using platform/framework features
- Reinventing the wheel
- Tight coupling
- Hidden dependencies
- Mutable global state

### Security Red Flags
- Unsanitized user input
- Hardcoded credentials
- Weak cryptography
- Missing authentication checks
- SQL concatenation instead of parameterized queries
- Disabled security features
- Overly permissive access controls

## Language-Specific Considerations

### Python
- PEP 8 compliance
- Type hints usage
- Context manager usage (with statements)
- Exception handling specificity
- List comprehensions vs loops
- Virtual environment dependencies

### JavaScript/TypeScript
- Type safety (TypeScript)
- Async/await vs promises
- Memory leaks (event listeners, timers)
- Bundle size impact
- Browser compatibility
- Null/undefined handling

### Java
- Exception hierarchy
- Resource management (try-with-resources)
- Concurrency issues
- Memory usage
- API design
- Package organization

## Final Review

Before submitting review:
1. Verify all concerns are clearly explained
2. Ensure tone is professional and constructive
3. Double-check file references are accurate
4. Confirm no requests for automated fixes
5. Review categorization (critical/major/minor)

## Remember
- You are **reviewing**, not **fixing**
- You are **advising**, not **implementing**
- You are **analyzing**, not **modifying**
- The goal is to **help the author improve**, not to take over

## Best Practices for Claude Code Review

- **Use Sonnet** for reviews — fast, accurate, cost-effective
- **Read-only permissions** — the reviewer should never edit files
- **Separate context** — run review in a subagent so it doesn't pollute your coding session
- **Structured output** — train Claude to use the feedback template above (put it in CLAUDE.md or the agent file)
- **Pre-commit hook** — run `claude --print -p "Review staged changes"` before every commit
- **Pair with human review** — Claude catches style, bugs, security; you catch design and intent
- **Iterate on the agent** — when the reviewer misses things, update the agent instructions

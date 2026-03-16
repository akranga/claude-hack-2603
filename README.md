# Claude Code Hackathon

Personal experiments and proofs-of-concept for AI automation and assistant orchestration using Claude and related tools.

## Overview

This repository contains:
- **AI Agent Integration** - Experiments with Anthropic Claude and Codemie code generation
- **Automation Utilities** - Reusable orchestration patterns and helpers
- **TypeScript/JavaScript Stack** - Modern tooling with npm package management
- **Python Support** - Project scaffolding with uv package manager
- **Development Tools** - Pre-commit hooks, testing, and linting infrastructure

## Project Structure

```
.
├── .editorconfig                    # Editor configuration (indentation, charset)
├── .gitignore                       # Git ignore rules (node_modules, .env, etc)
├── .pre-commit-config.yaml          # Pre-commit hook configuration
├── .tool-versions                   # Tool version management (asdf)
├── LICENSE                          # MIT License
├── README.md                         # Project documentation (this file)
├── package.json                      # npm dependencies and scripts
├── package-lock.json                # Locked npm package versions
└── pyproject.toml                   # Python project configuration (uv)
```

## Prerequisites

### Required Tools

- **asdf** - Version manager for multiple languages and tools
- **Node.js** 23.10+ (via asdf or direct installation)
- **Python** 3.12+ (for project configuration)
- **UV** package manager (for Python dependency management)
- **npm** 10.9+ (for JavaScript dependency management)

### Environment Setup
### 1. Clone and Install

```bash
# Clone repository
git clone https://github.com/akranga/claude-hack-2603.git
cd claude-hack-2603

# Install npm dependencies
npm install

# Install Python dependencies (if using Python)
uv sync

# Install Claude Code via Codemie (recommended)
npm run codemie:install:claude
```

### 2. Install Claude Code Agent

```bash
# Install Claude Code via Codemie (version-managed)
npm run codemie:install:claude
```

### 3. Use AI Agents

```bash
# Run the built-in CodeMie agent (no install needed)
npm run codemie:analyze
# or interactively:
npx codemie-code

# Run Claude Code (after installing)
npm run claude
# or with a task:
npx codemie-claude "Review my code"
```

### 4. Available Commands

```bash
# Run tests
npm test

# Health check
npm run doctor

# Linting (Python)
uv run ruff check .
uv run ruff format .

# Pytest (Python)
uv run pytest
```

## Project Configuration

### Node.js / npm

See `package.json` for:
- Project metadata and scripts
- Dependency specifications

### Python

See `pyproject.toml` for:
- Python version constraints (3.12+, <3.15)
- Development dependencies (pytest, ruff)
- Project metadata

Tool versions managed in `.tool-versions`:
```
python 3.14.3
nodejs 23.10.0
uv 0.10.10
```

## Contributing

1. Ensure `.envrc` is allowed: `direnv allow`
2. Install dependencies: `npm install && uv sync`
3. Make changes and test locally
4. Commit with clear message following conventional commits
5. Push to GitHub

## License

MIT - See [LICENSE](LICENSE) for details

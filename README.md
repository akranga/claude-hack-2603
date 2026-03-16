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
├── .envrc                           # direnv configuration (auto-loads environment)
├── .gitignore                       # Git ignore rules (node_modules, .env, etc)
├── .pre-commit-config.yaml          # Pre-commit hook configuration
├── .github/                         # GitHub workflows and configuration
│   ├── copilot-instructions.md      # Copilot-specific instructions
│   └── skills/                      # Copilot skills directory
├── .tool-versions                   # Tool version management (asdf)
├── LICENSE                          # MIT License
├── README.md                         # Project documentation (this file)
├── package.json                      # npm dependencies and scripts
├── package-lock.json                # Locked npm package versions
├── pyproject.toml                   # Python project configuration (uv)
└── node_modules/                    # npm dependencies (generated)
```

## Prerequisites

### Required Tools

- **Node.js** 23.10+ (via asdf or direct installation)
- **Python** 3.12+ (for project configuration)
- **UV** package manager (for Python dependency management)
- **npm** 10.9+ (for JavaScript dependency management)

### Environment Setup

`.envrc` provides automatic environment configuration via direnv:

```bash
# Load environment
direnv allow

# Verify setup
uv --version
npm --version
```

## Getting Started

### 1. Clone and Install

```bash
# Clone repository
git clone https://github.com/akranga/claude-hack-2603.git
cd claude-hack-2603

# Install npm dependencies
npm install

# Install Python dependencies (if using Python)
uv sync
```

### 2. Available Commands

```bash
# Run tests
npm test

# Linting (Python)
uv run ruff check .
uv run ruff format .

# Pytest (Python)
uv run pytest
```

## Dependencies

### JavaScript/TypeScript
- **@anthropic-ai/claude-code** - Anthropic Claude code generation
- **@codemieai/code** - Codemie code integration (from GitHub)

### Python (Development)
- **pytest** 8.2.0+ - Testing framework
- **ruff** 0.5.0+ - Fast Python linter and formatter

## Development Workflow

### Pre-commit Hooks

Pre-commit hooks enforce code quality standards:

```bash
# Install hooks
cd .github && git config core.hooksPath .githooks

# Run manually on all files
npm run lint
uv run pre-commit run --all-files
```

### Environment Variables

`.envrc` automatically exports:
- `GITHUB_TOKEN` - GitHub authentication
- `ANTHROPIC_MODEL` - Claude 3.7 Sonnet (Bedrock)
- `ANTHROPIC_SMALL_FAST_MODEL` - Claude 3.5 Haiku
- `CLAUDE_CODE_USE_BEDROCK` - Enable Bedrock integration
- `DISABLE_PROMPT_CACHING` - Prompt cache control

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

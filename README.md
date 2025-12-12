<p align="center">
  <img src="https://img.shields.io/badge/version-1.0.0-blue.svg" alt="Version">
  <img src="https://img.shields.io/badge/license-MIT-green.svg" alt="License">
  <img src="https://img.shields.io/badge/bash-5.0%2B-orange.svg" alt="Bash">
  <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs Welcome">
</p>

<h1 align="center">🤖 AI Code Review</h1>

<p align="center">
  <strong>Provider-agnostic code review using AI</strong><br>
  Use Claude, Gemini, Codex, Ollama, or any AI to enforce your coding standards.<br>
  Zero dependencies. Pure Bash. Works everywhere.
</p>

<p align="center">
  <a href="#-quick-start">Quick Start</a> •
  <a href="#-providers">Providers</a> •
  <a href="#%EF%B8%8F-configuration">Configuration</a> •
  <a href="#-commands">Commands</a> •
  <a href="#-examples">Examples</a>
</p>

---

## 🎯 Why?

You have coding standards. Your team ignores them. Code reviews catch issues too late.

**AI Code Review** runs on every commit, validating your staged files against your project's `AGENTS.md` (or any rules file). It's like having a senior developer review every line before it hits the repo.

```
┌─────────────────┐     ┌──────────────┐     ┌─────────────────┐
│   git commit    │ ──▶ │  AI Review   │ ──▶ │  ✅ Pass/Fail   │
│  (staged files) │     │  (any LLM)   │     │  (with details) │
└─────────────────┘     └──────────────┘     └─────────────────┘
```

**Key features:**
- 🔌 **Provider agnostic** - Use whatever AI you have installed
- 📦 **Zero dependencies** - Pure Bash, no Node/Python/Go required
- 🪝 **Git native** - Installs as a standard pre-commit hook
- ⚙️ **Highly configurable** - File patterns, exclusions, custom rules
- 🚨 **Strict mode** - Fail CI on ambiguous responses

---

## 📦 Installation

### Homebrew (Recommended)

```bash
brew tap gentleman-programming/tap
brew install ai-code-review
```

### Manual Installation

```bash
git clone https://github.com/Gentleman-Programming/ai-code-review.git
cd ai-code-review
./install.sh
```

---

## 🚀 Quick Start

```bash
# 1. Install (see above)

# 2. Go to your project
cd ~/your-project

# 3. Initialize config
ai-code-review init

# 4. Create your rules file (AGENTS.md)
cat > AGENTS.md << 'EOF'
# Code Review Rules

## General
- No console.log statements in production code
- All functions must have JSDoc comments
- Maximum file length: 300 lines

## TypeScript
- No `any` types
- Use `const` over `let` when possible
- Prefer interfaces over type aliases

## React
- Use functional components only
- No inline styles - use Tailwind classes
- Components must be accessible (aria labels, semantic HTML)
EOF

# 5. Install the git hook
ai-code-review install

# 6. Done! Now every commit gets reviewed 🎉
```

---

## 🔌 Providers

Use whichever AI CLI you have installed:

| Provider | Config Value | CLI Used | Install |
|----------|-------------|----------|---------|
| **Claude** | `claude` | `claude --print` | [claude.ai/code](https://claude.ai/code) |
| **Gemini** | `gemini` | `gemini` | [github.com/google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli) |
| **Codex** | `codex` | `codex exec` | `npm i -g @openai/codex` |
| **Ollama** | `ollama:<model>` | `ollama run <model>` | [ollama.ai](https://ollama.ai) |

### Examples

```bash
# Use Claude (default)
PROVIDER="claude"

# Use Gemini
PROVIDER="gemini"

# Use OpenAI Codex
PROVIDER="codex"

# Use Ollama with Llama 3.2
PROVIDER="ollama:llama3.2"

# Use Ollama with CodeLlama
PROVIDER="ollama:codellama"

# Use Ollama with Qwen
PROVIDER="ollama:qwen2.5-coder"
```

---

## ⚙️ Configuration

Create `.ai-code-review` in your project root:

```bash
# AI Provider (required)
PROVIDER="claude"

# File patterns to review (comma-separated globs)
# Default: * (all files)
FILE_PATTERNS="*.ts,*.tsx,*.js,*.jsx"

# Patterns to exclude (comma-separated globs)
# Default: none
EXCLUDE_PATTERNS="*.test.ts,*.spec.ts,*.d.ts"

# File containing your coding standards
# Default: AGENTS.md
RULES_FILE="AGENTS.md"

# Fail if AI response is ambiguous (recommended for CI)
# Default: true
STRICT_MODE="true"
```

### Config Hierarchy

1. **Environment variable** `AI_CODE_REVIEW_PROVIDER` (highest priority)
2. **Project config** `.ai-code-review`
3. **Global config** `~/.config/ai-code-review/config`

```bash
# Override provider for one run
AI_CODE_REVIEW_PROVIDER="gemini" ai-code-review run
```

---

## 📋 Commands

```bash
ai-code-review <command>
```

| Command | Description |
|---------|-------------|
| `run` | Run code review on staged files |
| `install` | Install git pre-commit hook |
| `uninstall` | Remove git pre-commit hook |
| `config` | Show current configuration |
| `init` | Create sample `.ai-code-review` file |
| `help` | Show help message |
| `version` | Show version |

---

## 📝 Rules File

The AI needs to know your standards. Create an `AGENTS.md` (or any file you configure):

```markdown
# Code Review Rules

## TypeScript
- Use `const` and `let`, never `var`
- No `any` types - always use proper typing
- Prefer interfaces over type aliases for objects
- Use optional chaining (`?.`) and nullish coalescing (`??`)

## React
- Functional components only, no class components
- No `import * as React` - use named imports
- Use semantic HTML elements
- All images need alt text
- Interactive elements need aria labels

## Styling
- Use Tailwind CSS utilities
- No inline styles
- No hex colors - use design tokens

## Testing
- All new features need tests
- Test files must be co-located with source files
- Use descriptive test names that explain the behavior
```

> 💡 **Pro tip**: Your `AGENTS.md` can also serve as documentation for human reviewers!

---

## 🎨 Examples

### TypeScript/React Project

```bash
# .ai-code-review
PROVIDER="claude"
FILE_PATTERNS="*.ts,*.tsx"
EXCLUDE_PATTERNS="*.test.ts,*.test.tsx,*.spec.ts,*.d.ts,*.stories.tsx"
RULES_FILE="AGENTS.md"
```

### Python Project

```bash
# .ai-code-review
PROVIDER="ollama:codellama"
FILE_PATTERNS="*.py"
EXCLUDE_PATTERNS="*_test.py,test_*.py,conftest.py"
RULES_FILE=".coding-standards.md"
```

### Go Project

```bash
# .ai-code-review
PROVIDER="gemini"
FILE_PATTERNS="*.go"
EXCLUDE_PATTERNS="*_test.go,mock_*.go"
```

### Full-Stack Monorepo

```bash
# .ai-code-review
PROVIDER="claude"
FILE_PATTERNS="*.ts,*.tsx,*.py,*.go"
EXCLUDE_PATTERNS="*.test.*,*_test.*,*.mock.*,*.d.ts"
```

---

## 🔄 How It Works

```
git commit
    │
    ▼
┌───────────────────────────────────────┐
│  Pre-commit Hook (ai-code-review run) │
└───────────────────────────────────────┘
    │
    ├──▶ Load config (.ai-code-review)
    │
    ├──▶ Get staged files matching FILE_PATTERNS
    │    (excluding EXCLUDE_PATTERNS)
    │
    ├──▶ Read rules from AGENTS.md
    │
    ├──▶ Build prompt with files + rules
    │
    ├──▶ Send to AI provider
    │
    └──▶ Parse response
         │
         ├── STATUS: PASSED ──▶ ✅ Commit proceeds
         │
         └── STATUS: FAILED ──▶ ❌ Commit blocked
                                  (with violation details)
```

---

## 🚫 Bypass Review

Sometimes you need to commit without review (emergencies, WIP commits):

```bash
# Skip pre-commit hook
git commit --no-verify -m "wip: work in progress"

# Or disable temporarily
CODE_REVIEW_ENABLED=false git commit -m "skip review"
```

---

## 🐛 Troubleshooting

### "Provider not found"

```bash
# Check if your provider CLI is installed
which claude   # Should show path
which gemini
which codex
which ollama

# Check if it's working
echo "Hello" | claude --print
```

### "Rules file not found"

Create your rules file:
```bash
touch AGENTS.md
# Add your coding standards
```

### "Ambiguous response" in Strict Mode

The AI must respond with `STATUS: PASSED` or `STATUS: FAILED` as the first line. If it doesn't:

1. Try a different provider (Claude is most reliable)
2. Check your rules file for confusing instructions
3. Set `STRICT_MODE="false"` to allow through (not recommended)

### Slow on large files

The tool sends full file contents to the AI. For large files:
- Add them to `EXCLUDE_PATTERNS`
- Or split them into smaller files (which is better anyway!)

---

## 🤝 Contributing

Contributions are welcome! Some ideas:

- [ ] Add more providers (Copilot, Codeium, etc.)
- [ ] Support for `.ai-code-review.yaml` format
- [ ] Caching to avoid re-reviewing unchanged files
- [ ] GitHub Action version
- [ ] Output formats (JSON, SARIF for IDE integration)

```bash
# Fork, clone, and submit PRs!
git clone https://github.com/YOUR_USER/ai-code-review.git
```

---

## 📄 License

MIT © 2024

---

<p align="center">
  <sub>Built with 🧉 by developers who got tired of repeating code review comments</sub>
</p>

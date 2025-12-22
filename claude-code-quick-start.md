# Claude Code Quick Start - December 2025

> One-page reference for Claude Code essentials

**Updated**: December 2025 | **Version**: 2.0.70+

---

## Installation

```bash
npm install -g @anthropic-ai/claude-code
claude --version
```

---

## Essential Commands

### Session Management 🆕
```bash
/rename my-feature          # Name current session
/resume my-feature          # Resume by name
claude --resume my-feature  # Resume from command line
```

### Core Commands
```bash
/clear                      # Clear conversation
/help                       # Show help
/context                    # Show current context
/config                     # Open configuration
/stats                      # Usage statistics 🆕
```

### Model Selection
```bash
Alt+P (Option+P on Mac)     # Switch models while typing 🆕
/model                      # Change model
```

### Planning
```bash
Shift+Tab (twice)           # Enter plan mode
Shift+Tab                   # Exit plan mode
```

### Thinking Mode 🆕
```bash
Alt+T (Option+T on Mac)     # Toggle thinking (changed from Tab)
"ultrathink: [question]"    # Max thinking budget
```

### Background Tasks 🆕
```bash
& run tests                 # Run in background
& deploy to staging         # Background deployment
```

---

## Best Models (December 2025)

### Claude Opus 4.5 🆕
- **Best for**: Complex refactors, architecture, planning
- **Performance**: 80.9% SWE-bench (world's best)
- **Cost**: $5 input / $25 output per million tokens
- **Efficiency**: 65% fewer tokens than previous versions

### Claude Sonnet 4.5
- **Best for**: Regular development, iterations, testing
- **Speed**: Fast and efficient
- **Cost**: Lower than Opus

### Pro Tip
Plan with Opus (Shift+Tab), then switch to Sonnet for execution (Alt+P)

---

## Project Setup

### 1. Create CLAUDE.md
```markdown
# Project Context

## Overview
[Project description]

## Tech Stack
- Language: [e.g., TypeScript]
- Framework: [e.g., React]
- Testing: [e.g., Jest]

## Key Files
- Main entry: src/index.ts
- Config: config/app.json

## Development
```bash
npm run dev    # Start dev server
npm test       # Run tests
```

## Rules 🆕
See .claude/rules/ for coding standards
```

### 2. Optional: Create .claude/rules/ 🆕
```bash
mkdir -p .claude/rules
```

Create files like:
- `coding-style.md` - Style guidelines
- `testing.md` - Test requirements  
- `security.md` - Security rules

---

## Common Workflows

### Feature Development
```bash
# 1. Start named session
claude --session-id feature-auth

# 2. Plan with Opus (Shift+Tab twice)
"Ultrathink: Design OAuth2 authentication"

# 3. Review and approve plan

# 4. Switch to Sonnet (Alt+P)

# 5. Implement
"Implement the OAuth flow from the plan"

# 6. Test in background
& run integration tests
```

### Bug Fixing with LSP 🆕
```
"Find all references to authenticateUser"
"Show me where this error is thrown"
"Update all calls to use the new signature"
```

### Code Review
```
"Review the changes in this PR for security issues"
"Check for performance problems"
"Suggest improvements"
```

---

## LSP Integration 🆕

Claude now has IDE-level code intelligence:

**Automatic Actions:**
- Go to definition
- Find all references
- Show documentation
- Navigate symbols

**Just ask naturally:**
```
"Find where UserService is defined"
"Show all files that import this module"
"What does this function do?"
```

**Supported Languages:**
TypeScript, Python, Rust, Go, Java, Kotlin, C/C++, PHP, Ruby, C#, PowerShell, HTML/CSS

---

## Quick Tips

### Context Management
- Keep context <80% (check with `/context`)
- Use `/clear` when switching tasks
- Remove unused files from context

### Named Sessions 🆕
- Always name important sessions: `/rename feature-name`
- Easy to resume later: `claude --resume feature-name`
- Fork sessions to try alternatives

### Background Work 🆕
- Long tests: `& run full test suite`
- Builds: `& npm run build`
- Deployments: `& deploy to staging`

### Permissions
- Review: `/permissions`
- Search: Press `/` in permissions menu
- Wildcard MCP: `mcp__server__*` to allow/deny all

### Cost Optimization
- Opus 4.5 for complex work (efficient now!)
- Sonnet for regular tasks
- Monitor with `/stats`

---

## Keyboard Shortcuts

### New in December 2025
- **Alt+T** / **Option+T**: Toggle thinking (was Tab)
- **Alt+P** / **Option+P**: Switch models while typing

### Standard
- **Shift+Tab**: Toggle plan mode
- **Ctrl+C**: Cancel operation
- **Escape**: Stop Claude
- **Up/Down**: Navigate history
- **Ctrl+R**: Search history

---

## Troubleshooting

### LSP Not Working 🆕
```bash
# Check language server installed
which typescript-language-server
which rust-analyzer
which pylsp

# Try /doctor
/doctor
```

### Named Sessions Not Saving 🆕
```bash
# Check directory
ls -la ~/.claude

# Try explicit session ID
claude --session-id test-session
```

### High Context Usage
```bash
/context              # See what's using tokens
/clear                # Reset context
```

### Slow Responses
- Clear context: `/clear`
- Use Sonnet instead of Opus
- Remove large files from context

---

## Resources

- **Main Guide**: [claude-code-best-practices-2025.md](claude-code-best-practices-2025.md)
- **Changelog**: [CHANGELOG.md](CHANGELOG.md)
- **Official Docs**: https://docs.claude.com
- **Community**: r/ClaudeAI on Reddit

---

## Quick Reference Card

```
┌─────────────────────────────────────────────┐
│ ESSENTIAL SHORTCUTS                         │
├─────────────────────────────────────────────┤
│ Alt+T        Toggle thinking (NEW)          │
│ Alt+P        Switch models (NEW)            │
│ Shift+Tab    Plan mode                      │
│ /rename      Name session (NEW)             │
│ /resume      Continue session (NEW)         │
│ /clear       Reset context                  │
│ /context     Check usage                    │
│ /stats       View statistics (NEW)          │
│ & command    Background task (NEW)          │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│ MODEL SELECTION                             │
├─────────────────────────────────────────────┤
│ Opus 4.5     Complex, planning, architecture│
│ Sonnet 4.5   Regular dev, fast iteration   │
│ Haiku 4.5    Simple, repetitive tasks      │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│ PRO TIPS                                    │
├─────────────────────────────────────────────┤
│ 1. Name every session (/rename)            │
│ 2. Plan with Opus, code with Sonnet        │
│ 3. Use LSP for code navigation              │
│ 4. Background tasks for tests/builds        │
│ 5. Monitor usage with /stats                │
└─────────────────────────────────────────────┘
```

---

**Updated**: December 2025 with Opus 4.5, LSP, Named Sessions, Background Agents

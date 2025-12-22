# Claude Code Best Practices - December 2025 Update

> **Latest Features from November-December 2025**  
> Based on Claude Code v2.0.75+ and Claude Opus 4.5 release

**Last Updated**: December 22, 2025  
**Compatible with**: Claude Code v2.0.70 - v2.0.75+

---

## 📋 Table of Contents

1. [What's New: November-December 2025](#whats-new-november-december-2025)
2. [Claude Opus 4.5 - Game Changer](#claude-opus-4-5-game-changer)
3. [LSP Integration (NEW!)](#lsp-integration-new)
4. [Named Sessions & Resume](#named-sessions--resume)
5. [Background Agents & Async Operations](#background-agents--async-operations)
6. [Claude in Chrome Beta](#claude-in-chrome-beta)
7. [Improved Thinking Mode](#improved-thinking-mode)
8. [Enhanced Status Line & Context](#enhanced-status-line--context)
9. [Plugin System & Marketplaces](#plugin-system--marketplaces)
10. [Terminal Setup Improvements](#terminal-setup-improvements)
11. [Permission System Updates](#permission-system-updates)
12. [.claude/rules/ Directory](#clauderules-directory)
13. [Stats & Usage Tracking](#stats--usage-tracking)
14. [Updated Workflows](#updated-workflows)
15. [Community Best Practices](#community-best-practices)
16. [Performance & Cost Optimization](#performance--cost-optimization)
17. [Troubleshooting New Features](#troubleshooting-new-features)

---

## What's New: November-December 2025

### 🎯 Major Updates Summary

**November 24, 2025**: Claude Opus 4.5 Launch
- Best coding model in the world (80.9% SWE-bench Verified)
- Up to 65% fewer tokens for same tasks
- Priced at $5/$25 per million tokens (much more affordable)
- State-of-the-art for agents and computer use

**December 2025 Feature Releases**:
- **LSP Integration**: Native Language Server Protocol support
- **Named Sessions**: Name, resume, and manage sessions easily
- **Background Agents**: Run agents while you continue working
- **Claude in Chrome**: Control browser directly from Claude Code
- **Improved Thinking**: Default enabled for Opus 4.5
- **Plugin System**: Extend Claude Code with community plugins
- **Enhanced Terminal Support**: Kitty, Alacritty, Zed, and Warp
- **Better IME Support**: Improved CJK language input

---

## Claude Opus 4.5 - Game Changer

### Why It Matters

Claude Opus 4.5 is the most significant update since Claude Code launched:

**Performance Breakthrough**:
- **80.9% on SWE-bench Verified** (vs. 77.4% for competitors)
- **65% fewer tokens** for complex tasks
- **Higher pass rates** on held-out tests
- **Fewer dead-ends** in multi-step workflows

**Cost-Effectiveness**:
- **$5 input / $25 output** per million tokens
- Making Opus-level capabilities accessible
- Better ROI than previous Opus pricing

**Practical Impact**:
```
Before: Complex refactor = 50k tokens @ $15/M input
After:  Same refactor = 17k tokens @ $5/M input
Savings: 66% fewer tokens + 67% lower cost = 89% total cost reduction
```

### When to Use Opus 4.5

**Use Opus 4.5 for**:
- Complex refactors spanning multiple files
- Architectural decisions and planning
- Security-critical code reviews
- Long-horizon autonomous tasks
- Code migration projects
- Debugging complex issues

**Use Sonnet 4.5 for**:
- Quick fixes and updates
- Test writing
- Documentation
- Routine refactors
- Iterative development

**Pro Tip**: Plan with Opus, execute with Sonnet
```bash
# Start in plan mode with Opus
# Shift+Tab twice to enter plan mode
# Review and approve plan
# Switch to Sonnet for implementation: alt+p (or option+p on Mac)
```

### Enabling Thinking Mode for Opus 4.5

Thinking mode is now **enabled by default** for Opus 4.5!

Check your config:
```bash
/config
# Look for "Thinking Mode" setting
```

Manual toggle:
- **Alt+T** (Linux/Windows) or **Option+T** (Mac) to toggle thinking
- No longer Tab (changed to avoid accidental triggers)

---

## LSP Integration (NEW!)

### What is LSP?

Language Server Protocol gives Claude **IDE-level code intelligence**:
- Go-to-definition
- Find references
- Hover documentation  
- Symbol navigation
- Intelligent code understanding

**Added in v2.0.74 (December 20, 2025)**

### How to Use LSP

LSP tools are built-in and automatic. Claude can now:

```
You: "Find all places where processRequest is used"
Claude: Using LSP findReferences...
Found 5 references:
- src/handlers/request.ts:127:1 (definition)
- src/index.ts:45:15
- src/utils/loader.ts:23:8
- tests/request.test.ts:15:10
- tests/request.test.ts:89:12
```

### LSP Commands Claude Can Use

1. **goToDefinition**: Jump to symbol definitions
2. **findReferences**: Find all usages of a symbol
3. **documentSymbol**: Get overview of symbols in a file
4. **hover**: Get documentation and type information

### Supported Languages

Out of the box, LSP works with:
- TypeScript/JavaScript
- Python
- Rust
- Go
- Java
- Kotlin
- C/C++
- PHP
- Ruby
- C#
- PowerShell
- HTML/CSS

### Installation (If Needed)

Most language servers are auto-detected if already installed:

**TypeScript/JavaScript**:
```bash
npm install -g @vtsls/language-server typescript
```

**Python**:
```bash
pip install python-lsp-server --break-system-packages
```

**Rust**:
```bash
rustup component add rust-analyzer
```

**Go**:
```bash
go install golang.org/x/tools/gopls@latest
```

### Best Practices with LSP

**1. Let Claude use LSP naturally**
```
✅ "Find all functions that call authenticate()"
✅ "Show me where UserConfig is defined"
✅ "What are all the exports from this module?"

❌ Don't manually specify LSP commands
```

**2. Combine with code editing**
```
"Find all references to OLD_API_URL and update them to NEW_API_URL"
```

**3. Use for refactoring**
```
"Rename getUserData to fetchUserProfile across the entire codebase"
```

**4. Debugging workflows**
```
"Find the definition of this error message and trace back to where it's thrown"
```

### Troubleshooting LSP

**Check if LSP is working**:
```bash
# Claude should automatically use LSP tools
# Watch for "Using LSP..." in responses
```

**Manual LSP server restart** (if needed):
```
"Restart the TypeScript LSP server"
```

**If LSP isn't working**:
1. Check language server is installed and in PATH
2. Verify file extensions match language server config
3. Try `/doctor` for diagnostics
4. Check terminal has proper environment variables

---

## Named Sessions & Resume

### What Changed

**Before**: Sessions identified by UUIDs only
**Now**: Name sessions for easy identification and resumption

### Naming Sessions

**During a session**:
```bash
/rename my-feature-branch-work
```

**From command line**:
```bash
claude --session-id feature-auth-refactor
```

### Resuming Sessions

**By name**:
```bash
/resume feature-auth-refactor
# or
claude --resume feature-auth-refactor
```

**By number** (from resume screen):
```bash
/resume 3  # Resume session #3 from list
```

**Interactive resume screen**:
```bash
/resume
# Shows all sessions with:
# - Session names
# - Last activity
# - Token usage
# - Grouped forked sessions
# Keyboard shortcuts: P (preview), R (rename)
```

### Forking Sessions

Create a new session from an existing one:
```bash
claude --resume feature-auth --fork-session --session-id feature-auth-v2
```

### Best Practices

**1. Name by feature/task**
```bash
/rename bug-1234-fix
/rename api-migration-phase-2
/rename security-review-q4
```

**2. Use descriptive names**
```
✅ oauth-implementation-debugging
✅ react-component-refactor
❌ test
❌ debug
❌ fix
```

**3. Resume for context continuity**
```bash
# Yesterday's work
claude --resume oauth-implementation

# Continue where you left off
"Let's continue implementing the refresh token flow"
```

**4. Fork for experiments**
```bash
# Try different approach without losing main thread
claude --resume main-feature --fork-session --session-id experiment-alt-approach
```

---

## Background Agents & Async Operations

### What Are Background Agents?

**New in December 2025**: Agents can now run in the background while you continue working.

### Starting Background Agents

**From terminal**:
```bash
# Start message with &
claude "& run full test suite"
```

**From web**:
```
& Deploy to staging and run smoke tests
```

### How It Works

```
You: & run integration tests

Claude: Starting background agent for integration tests...
You can continue working while this runs.

[Background agent runs...]
[You continue with other work...]

Background Agent (30 seconds later): 
Integration tests complete. 45/45 passed. 
Found 2 deprecation warnings in auth module.
```

### Async Operations

Agents and bash commands can run **asynchronously** and message the main agent when complete.

**Example workflow**:
```
You: Run tests in background while I work on the frontend

Claude: Running tests asynchronously...
Starting frontend work...

[Claude works on frontend]

Background Test Agent: Tests complete - 3 failures in UserService
Main Agent: I see the test failures. Let me fix those issues...
```

### Best Use Cases

**1. Long-running tasks**:
- Full test suites
- Large builds/compiles
- Database migrations
- Deployment pipelines

**2. Parallel work**:
```
& run backend tests
# While continuing with frontend development
```

**3. Monitoring**:
```
& watch logs and alert on errors
# While you work on fixes
```

### Managing Background Agents

**Check status**:
```bash
/agents
# Shows running background agents
```

**View background agent logs**:
```bash
/stats
# Includes background agent activity
```

---

## Claude in Chrome Beta

### What It Does

Control your browser directly from Claude Code using the Claude for Chrome extension.

**Released**: December 2025 (Beta)

### Setup

1. **Install Chrome extension**:
   - Visit https://claude.ai/chrome
   - Install extension
   - Sign in with your Claude account

2. **Enable in Claude Code**:
   - Already built-in for v2.0.72+
   - Claude will automatically detect the extension

### Use Cases

**1. Web Testing**:
```
"Open localhost:3000, click the login button, and take a screenshot"
```

**2. Research**:
```
"Search for TypeScript best practices and summarize the top 3 articles"
```

**3. Form Filling**:
```
"Fill out the contact form with test data and submit"
```

**4. Automated Workflows**:
```
"Log into the admin panel, navigate to users, and export the CSV"
```

**5. Visual QA**:
```
"Open the staging site, click through each page, and capture screenshots for review"
```

### Example Workflow

```
You: Test the new checkout flow

Claude: I'll use Claude in Chrome to test the checkout flow.

1. Opening https://staging.yoursite.com
2. Adding item to cart
[Screenshot #1]
3. Clicking checkout
[Screenshot #2]
4. Filling test payment info
5. Submitting order
[Screenshot #3]

Result: Checkout completed successfully. Order #12345 created.
Issue found: "Continue Shopping" button has broken styling.
```

### Best Practices

**1. Combine with Puppeteer MCP for advanced scenarios**:
```
# Claude in Chrome for manual testing
# Puppeteer for automated test suites
```

**2. Use for visual verification**:
```
"Take screenshots at each step so I can review the UI"
```

**3. Debugging web issues**:
```
"Navigate to the error page and check the console for errors"
```

**4. Competitive research**:
```
"Visit competitor sites and note their key features"
```

---

## Improved Thinking Mode

### What Changed

**November-December 2025 updates**:
- **Default enabled** for Opus 4.5
- **Alt+T toggle** (changed from Tab to avoid accidents)
- **Moved to /config** for persistence
- **Better UI indicators** when thinking is active

### Using Extended Thinking

**Toggle during prompt**:
- **Alt+T** (Linux/Windows) or **Option+T** (Mac)
- Press before submitting your message

**Check config**:
```bash
/config
# Look for "Thinking Mode" setting
# Toggle for current session or set default
```

### Thinking Levels (Still Work!)

These magic words still trigger different thinking budgets:

```
"think" → 4,000 tokens
"think hard" or "think deeply" → 10,000 tokens  
"ultrathink" → 31,999 tokens (max)
```

**Example**:
```
"Ultrathink: Design a scalable microservices architecture for our monolith"
```

### When to Use Thinking

**Use extended thinking for**:
- Architectural decisions
- Complex refactors
- Security analysis
- Performance optimization
- Algorithm design
- Debugging gnarly issues

**Don't use for**:
- Simple fixes
- Routine updates
- Quick questions
- Test writing

### Monitoring Thinking

Watch the status line for thinking indicators:
```
[Thinking...] or [Extended Thinking Active]
```

---

## Enhanced Status Line & Context

### New /context Visualization

**Improved in December 2025**:
```bash
/context
```

Now shows:
- **Grouped skills and agents** by source
- **Slash commands** with namespacing
- **Sorted by token count** (largest first)
- **Visual hierarchy** for better readability

### Context Window Information

Status line now shows **accurate context window percentage**:
```
Opus 4.5 | 📁my-project | 🔀main | █████░░░░░ 45% of 200k tokens
```

### Managing Context

**Quick context check**:
```bash
/context
```

**Clear conversation** (resets context):
```bash
/clear
```

**Remove specific files**:
```
"Remove utils/oldHelper.ts from context"
```

### Best Practices

**1. Monitor context usage**:
- Keep below 80% for best performance
- Use /clear when switching tasks
- Remove unused files from context

**2. Use named sessions for context management**:
```bash
# Heavy context for feature work
/rename complex-refactor-session

# Start fresh for new feature
/clear
claude --session-id new-feature
```

**3. Leverage status line info**:
- Check token% before large operations
- Switch to Sonnet if context is high
- Use /context to identify large files

---

## Plugin System & Marketplaces

### Overview

**Released**: November 2025

Extend Claude Code with:
- Custom commands
- Specialized agents  
- Hooks for automation
- MCP servers
- Community tools

### Installing Plugins

**Add a marketplace**:
```bash
/plugin marketplace add https://example.com/marketplace.json
```

**Discover plugins**:
```bash
/plugin discover
# Type to filter by name, description, or marketplace
# Use spacebar to select, Enter to install
```

**Install specific plugin**:
```bash
/plugin install plugin-name
```

### Managing Plugins

**List installed**:
```bash
/plugin list
```

**Enable/disable**:
```bash
/plugin enable plugin-name
/plugin disable plugin-name
```

**Auto-update control**:
```bash
/config
# Look for "Plugin Auto-Update" per marketplace
```

### Popular Plugin Categories

1. **LSP Servers** (TypeScript, Rust, Python, etc.)
2. **Language-specific tools** (formatters, linters)
3. **Framework integrations** (React, Vue, Django)
4. **DevOps tools** (Kubernetes, Docker, Terraform)
5. **Testing frameworks** (Jest, Pytest, JUnit)
6. **Code quality** (ESLint, Prettier, Black)

### Creating Custom Plugins

See the [Plugin Development Guide](https://docs.claude.com) for:
- Plugin structure
- Manifest format
- Tool definitions
- Publishing to marketplaces

---

## Terminal Setup Improvements

### /terminal-setup Command

**New in December 2025**: Expanded terminal support

```bash
/terminal-setup
```

Now supports:
- **Kitty**
- **Alacritty**
- **Zed Terminal**
- **Warp**
- iTerm2 (existing)
- VS Code Terminal (existing)

### What It Fixes

**Common issues resolved**:
- Shift+Enter for newlines
- Image pasting (Ctrl+V vs Cmd+V)
- Alt/Option shortcuts
- Clipboard integration
- Color rendering

### Manual Setup (if needed)

**For Kitty**:
```bash
# Add to ~/.config/kitty/kitty.conf
map shift+enter send_text all \x1b[13;2u
```

**For Alacritty**:
```yaml
# Add to ~/.config/alacritty/alacritty.yml
key_bindings:
  - { key: Return, mods: Shift, chars: "\x1b[13;2u" }
```

### Mac Alt Shortcuts

If Alt shortcuts don't work on Mac:

```bash
/terminal-setup
# Follow the Mac-specific guidance shown
```

Typically need to enable "Use Option as Meta key" in terminal settings.

---

## Permission System Updates

### MCP Wildcard Permissions

**New**: Allow/deny all tools from an MCP server at once

```bash
# Allow all tools from a server
/permissions add allow mcp__github__*

# Deny all tools from a server  
/permissions add deny mcp__postgres__*
```

### Search Permissions

**New shortcut**: Press `/` in /permissions to search

```bash
/permissions
# Press / to search
# Type to filter rules
```

### Shell Glob Patterns

**Fixed**: Shell glob patterns now work correctly

```bash
# These now work as expected:
ls *.txt
for f in *.png; do echo $f; done
rm **/*.log
```

### Permission Modes

**Configure default mode for new conversations**:
```bash
/config
# Look for "VSCode Permission Mode"
# Options: ask, allow, deny
```

---

## .claude/rules/ Directory

### What It Is

**New in December 2025**: Store memory rules and guidelines separately from CLAUDE.md

### Structure

```
.claude/
├── CLAUDE.md              # Project context
├── rules/                 # Memory rules (NEW)
│   ├── coding-style.md
│   ├── testing.md
│   ├── security.md
│   └── ...
├── commands/              # Slash commands
└── agents/                # Custom agents
```

### Use Cases

**1. Team Standards**:
```markdown
<!-- .claude/rules/coding-style.md -->
# Coding Style Rules

- Always use TypeScript strict mode
- Prefer functional components in React
- Use const over let unless reassignment needed
- Maximum line length: 100 characters
```

**2. Security Guidelines**:
```markdown
<!-- .claude/rules/security.md -->
# Security Rules

- Never log sensitive data
- Always validate user input
- Use parameterized queries for SQL
- Sanitize HTML output
```

**3. Testing Requirements**:
```markdown
<!-- .claude/rules/testing.md -->
# Testing Rules

- Write tests before implementation (TDD)
- Minimum 80% code coverage
- Test edge cases and error conditions
- Use descriptive test names
```

### How Claude Uses Rules

Rules are loaded hierarchically:
1. Enterprise rules (if managed)
2. User rules (~/.claude/rules/)
3. Project rules (.claude/rules/)

Claude automatically applies relevant rules when:
- Writing code
- Reviewing PRs
- Making architectural decisions
- Implementing features

---

## Stats & Usage Tracking

### New /stats Command

**Enhanced in December 2025**:

```bash
/stats
```

Shows:
- **Favorite model** (most used)
- **Usage graph** (visual timeline)
- **Usage streak** (consecutive days)
- **Token consumption** by model
- **Session statistics**
- **Background agent activity**

### Visual Features

**Usage Graph**:
```
Token Usage (Last 30 Days)
█████████░░░░░░░░░░░░░░░░░░░
Nov 22  Nov 29  Dec 6  Dec 13  Dec 20
```

**Model Breakdown**:
```
Most Used Models:
1. Opus 4.5     - 45% (1.2M tokens)
2. Sonnet 4.5   - 40% (900k tokens)
3. Haiku 4.5    - 15% (200k tokens)
```

### Use Cases

**1. Cost tracking**:
```bash
/stats
# Review token usage before month-end
```

**2. Optimization**:
```
# Identify if using Opus too much for simple tasks
# Consider switching to Sonnet for routine work
```

**3. Trend analysis**:
```
# See usage patterns over time
# Identify peak usage days
```

**4. Team reporting**:
```bash
# Share stats with team for ROI analysis
/stats | pbcopy  # Mac
/stats | xclip   # Linux
```

---

## Updated Workflows

### Workflow 1: Feature Development with New Tools

```
1. Start Named Session
   claude --session-id feature-user-dashboard

2. Plan with Opus in Plan Mode
   Shift+Tab twice → Enter plan mode
   "Ultrathink: Design a real-time user dashboard with WebSockets"
   
3. Review & Approve Plan
   Shift+Tab to exit → Review → Approve

4. Switch to Sonnet for Implementation
   Alt+P → Select Sonnet 4.5
   
5. Background Tests
   "& run test suite in background"
   
6. Use LSP for Navigation
   "Find all WebSocket connection handlers"
   
7. Test with Claude in Chrome
   "Open localhost:3000 and test the dashboard updates"
   
8. Resume Tomorrow
   /rename dashboard-feature-wip
   (Next day: claude --resume dashboard-feature-wip)
```

### Workflow 2: Bug Fixing with LSP

```
1. Investigate with LSP
   "Find all references to handleUserLogin"
   
2. Use Thinking for Complex Bugs
   "Think deeply: Why is the session timing out after 5 minutes?"
   
3. Fix with Context
   "Update the session timeout logic, update all references"
   
4. Verify with Background Tests
   "& run authentication tests"
   
5. Check with Browser
   "Test the login flow in Chrome and verify session persists"
```

### Workflow 3: Code Review Automation

```
1. Install GitHub Review Plugin
   /plugin install github-pr-review
   
2. Configure Review Rules
   Create .claude/rules/code-review.md
   
3. Review PR
   "Review PR #123 focusing on security and performance"
   
4. Use LSP for Deep Analysis
   Claude automatically uses LSP to trace dependencies
   
5. Generate Review Comments
   "Create GitHub review comments for the issues found"
```

### Workflow 4: Multi-Project Work

```
1. Project A - Backend API
   cd project-a
   claude --session-id backend-api
   "& run backend tests"
   
2. Project B - Frontend (parallel)
   cd project-b
   claude --session-id frontend-app
   "Implement the new dashboard component"
   
3. Monitor Both
   # Terminal 1: Background tests running
   # Terminal 2: Active development
   
4. Switch Between Projects
   claude --resume backend-api
   claude --resume frontend-app
```

---

## Community Best Practices

### From Reddit & Discord (December 2025)

**1. Use Named Sessions Religiously**
```
"Best habit I formed: naming every session. 
Makes resuming 10x easier." - u/devtools2025
```

**2. Opus for Planning, Sonnet for Doing**
```
"The Opus 4.5 price drop changed the game.
I use Opus for all planning now, then Sonnet for execution.
Token savings are real." - @coder_mike
```

**3. LSP is a Game-Changer**
```
"LSP integration is underrated. Claude now navigates 
codebases like I do in my IDE." - u/rust_dev
```

**4. Background Agents for CI/CD**
```
"Running tests in background while I work on the 
next feature is amazing. No more context switching." - @jess_codes
```

**5. /clear Liberally**
```
"Don't be precious with context. Clear often.
Named sessions let you resume if needed." - u/productivity_hacker
```

**6. Rules Directory for Team Consistency**
```
"We put all our team standards in .claude/rules/.
Every dev gets the same guidelines automatically." - @teamlead_sarah
```

**7. Use /stats for Cost Management**
```
"/stats every Friday. Helps me stay under budget
and optimize model usage." - u/freelance_dev
```

**8. Claude in Chrome for E2E Testing**
```
"Replaced half our Playwright tests with Claude in Chrome.
Faster to write, easier to maintain." - @qa_automation
```

### Anti-Patterns to Avoid

**1. Don't rely solely on Claude's code**
```
❌ Accept all code without review
✅ Use LSP + manual review for critical code
```

**2. Don't overuse Opus**
```
❌ Use Opus for everything
✅ Opus for complex tasks, Sonnet for routine work
```

**3. Don't ignore context limits**
```
❌ Let context grow to 95%
✅ Monitor with /context, /clear when needed
```

**4. Don't skip the plan phase**
```
❌ "Just start coding"
✅ Use Plan Mode (Shift+Tab) for complex features
```

**5. Don't forget to name sessions**
```
❌ UUID-only sessions
✅ /rename meaningful-task-name
```

---

## Performance & Cost Optimization

### Opus 4.5 Cost Savings

**Real-world example**:
```
Task: Refactor authentication system (200+ files)

Before (Sonnet 4.5):
- 50,000 tokens input  @ $3/M  = $0.15
- 20,000 tokens output @ $15/M = $0.30
- Total: $0.45

After (Opus 4.5):
- 17,500 tokens input  @ $5/M  = $0.09
- 7,000 tokens output  @ $25/M = $0.18
- Total: $0.27

Savings: 40% cost + faster completion
```

### Model Selection Strategy

**Decision Tree**:
```
Is it complex/critical? → Opus 4.5
├─ Yes: Use Opus
└─ No: Is it routine? → Sonnet 4.5
   ├─ Yes: Use Sonnet  
   └─ No: Is it simple? → Haiku 4.5
      └─ Yes: Use Haiku
```

**Examples by task type**:

| Task Type | Model | Why |
|-----------|-------|-----|
| Architectural planning | Opus | Needs deep reasoning |
| Feature implementation | Sonnet | Balanced performance |
| Test generation | Sonnet/Haiku | Repetitive patterns |
| Documentation | Haiku | Straightforward |
| Code review (critical) | Opus | Security/correctness |
| Code review (routine) | Sonnet | Fast enough |
| Bug investigation | Opus | Complex reasoning |
| Bug fix implementation | Sonnet | Clear direction |
| Refactoring (large) | Opus | System-wide impact |
| Refactoring (small) | Sonnet | Localized changes |

### Token Optimization Tips

**1. Use named sessions for context continuity**
```bash
# Avoid re-explaining project context every time
claude --resume my-feature
```

**2. Clear context between tasks**
```bash
/clear  # Reset before starting new, unrelated work
```

**3. Use specific file references**
```
✅ "Update src/auth/login.ts"
❌ "Update the login file" (Claude needs to search)
```

**4. Leverage LSP instead of broad searches**
```
✅ "Find all calls to authenticateUser" (LSP)
❌ "Search the codebase for auth code" (grep)
```

**5. Use background agents for long tasks**
```bash
& run full test suite
# Prevents keeping test output in main context
```

**6. Rules directory for repeated guidelines**
```bash
# Store in .claude/rules/ instead of repeating every prompt
```

### Monitoring Costs

**Weekly review**:
```bash
/stats
# Check total token usage
# Identify optimization opportunities
```

**Budget tracking**:
```
Daily limit check:
- Pro: ~300k tokens/day ($15-20 equivalent)
- Max: 5x more usage available

Track with /stats to stay under budget
```

---

## Troubleshooting New Features

### LSP Not Working

**Symptoms**:
- No "Using LSP..." messages
- "Symbol not found" errors
- Slow code navigation

**Solutions**:
```bash
# 1. Check language server installed
which typescript-language-server
which rust-analyzer
which pylsp

# 2. Verify PATH
echo $PATH

# 3. Try /doctor
/doctor

# 4. Restart Claude Code
# Exit and restart

# 5. Manual LSP check
"Check if LSP is available for TypeScript"
```

### Named Sessions Not Saving

**Symptoms**:
- /resume shows no sessions
- /rename doesn't persist

**Solutions**:
```bash
# 1. Check ~/.claude directory
ls -la ~/.claude

# 2. Verify permissions
chmod 755 ~/.claude

# 3. Check disk space
df -h ~

# 4. Try explicit session ID
claude --session-id my-test-session
/rename test-name
# Exit and try: claude --resume test-name
```

### Background Agents Not Running

**Symptoms**:
- & prefix doesn't start background tasks
- No background agent messages

**Solutions**:
```bash
# 1. Update to latest version
claude update

# 2. Check version
claude --version  # Need v2.0.70+

# 3. Try explicit background command
claude "Run tests in background" --background

# 4. Check /agents command
/agents
# Should show background agent capability
```

### Claude in Chrome Not Connecting

**Symptoms**:
- Chrome extension installed but Claude Code can't control browser
- "Browser not available" errors

**Solutions**:
```bash
# 1. Verify extension installed
# Visit https://claude.ai/chrome
# Check extension is enabled

# 2. Sign in to extension
# Must use same Claude account

# 3. Update Claude Code
claude update

# 4. Check Beta access
# Feature might need Pro/Max subscription

# 5. Try explicitly
"Use Chrome to navigate to google.com"
```

### Thinking Mode Not Triggering

**Symptoms**:
- Alt+T doesn't toggle
- No thinking indicators

**Solutions**:
```bash
# 1. Check keyboard shortcut
# Linux/Windows: Alt+T
# Mac: Option+T

# 2. Configure in settings
/config
# Find "Thinking Mode" setting

# 3. Use magic words
"Ultrathink: [your question]"

# 4. Check terminal setup
/terminal-setup
# Ensure Alt/Option key works
```

### Permission Errors with MCP

**Symptoms**:
- MCP tools blocked
- Permission dialogs for allowed tools

**Solutions**:
```bash
# 1. Check MCP permissions
/permissions
# Look for mcp__server__toolname rules

# 2. Add wildcard permission
/permissions add allow mcp__servername__*

# 3. Check MCP server status
/mcp
# Verify server is connected

# 4. Restart MCP server
/mcp disable servername
/mcp enable servername
```

### Context Window Errors

**Symptoms**:
- "Context window exceeded" errors
- Slow responses

**Solutions**:
```bash
# 1. Check current usage
/context
# Look for large files

# 2. Clear context
/clear

# 3. Remove large files
"Remove [filename] from context"

# 4. Switch to Opus (larger context)
Alt+P → Select Opus 4.5

# 5. Split into smaller sessions
# Use named sessions for each component
```

---

## Quick Reference

### New Commands (December 2025)

```bash
# Named Sessions
/rename [name]              # Name current session
/resume [name|number]       # Resume named session
claude --session-id [name]  # Start with specific name

# Background Agents
& [command]                 # Run in background

# Stats
/stats                      # Usage statistics

# Context
/context                    # Visual context breakdown

# Permissions Search
/permissions                # Then press / to search

# Terminal Setup
/terminal-setup             # Configure terminal

# Plugins
/plugin discover            # Browse plugins
/plugin install [name]      # Install plugin
/plugin list                # List installed
```

### New Keyboard Shortcuts

```bash
# Thinking Toggle
Alt+T (Linux/Windows) or Option+T (Mac)

# Model Switch While Typing
Alt+P (Linux/Windows) or Option+P (Mac)

# Search Permissions
/ (while in /permissions)

# Theme Syntax Toggle
Ctrl+T (in /theme)
```

### LSP Tools (Automatic)

Claude can now use these automatically:
- `goToDefinition` - Jump to definitions
- `findReferences` - Find all usages
- `documentSymbol` - Symbol overview
- `hover` - Documentation on hover

### File Structure Updates

```
.claude/
├── CLAUDE.md              # Project context
├── rules/                 # NEW: Memory rules
│   ├── coding-style.md
│   ├── security.md
│   └── testing.md
├── commands/              # Slash commands
├── agents/                # Custom agents
└── .mcp.json             # MCP configuration
```

---

## What's Next?

### Upcoming Features (Roadmap Hints)

Based on December 2025 discussions:
- Enhanced plugin marketplace
- More LSP language support
- Improved background agent orchestration
- Better cost tracking and budgeting
- Team collaboration features
- More MCP integrations

### Staying Updated

**Official sources**:
- [GitHub Releases](https://github.com/anthropics/claude-code/releases)
- [Changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Documentation](https://docs.claude.com)

**Community**:
- r/ClaudeAI subreddit
- Claude Code Discord
- Twitter @AnthropicAI

**Update Claude Code**:
```bash
claude update
```

---

## Summary of Best Practices

### Top 10 Practices for December 2025

1. **Name Every Session**: `/rename meaningful-name` for easy resumption
2. **Use Opus for Planning**: Plan Mode + Opus 4.5 for complex decisions
3. **Switch to Sonnet for Implementation**: Alt+P after planning
4. **Let LSP Work**: Trust Claude to use LSP tools automatically
5. **Background Long Tasks**: `& run tests` for parallel work
6. **Monitor Context**: `/context` before large operations
7. **Store Rules in .claude/rules/**: Team standards in version control
8. **Clear Between Tasks**: `/clear` to reset context
9. **Track Usage**: `/stats` weekly for cost optimization
10. **Update Regularly**: `claude update` for latest features

### The Opus 4.5 Advantage

```
Before: "Claude Code is expensive for complex work"
Now: "Opus 4.5 is cheaper AND better than Sonnet was"

Cost reduction: Up to 89% for complex tasks
Performance gain: +3-5 percentage points on benchmarks
Token efficiency: 65% fewer tokens

Result: More capabilities for less cost
```

---

## Resources

### Documentation
- [Official Docs](https://docs.claude.com)
- [Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)
- [API Reference](https://docs.anthropic.com/api)

### Community
- [r/ClaudeAI](https://reddit.com/r/ClaudeAI)
- [GitHub Discussions](https://github.com/anthropics/claude-code/discussions)
- [Discord](https://discord.gg/anthropic)

### Tools
- [ClaudeLog](https://claudelog.com) - Community resources
- [Claude Code Tips](https://github.com/ykdojo/claude-code-tips)
- [Plugin Marketplaces](https://docs.claude.com/plugins)

---

**Happy Coding with Claude Code December 2025!** 🚀

*This document reflects features available in Claude Code v2.0.75+ and Claude Opus 4.5 as of December 22, 2025.*

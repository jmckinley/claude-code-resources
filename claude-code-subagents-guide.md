# Claude Code Sub-agents Guide - December 2025

> Understanding and effectively using sub-agents for parallel workflows

**Updated**: December 2025 | **Version**: 2.0.70+

---

## Table of Contents

1. [What Are Sub-agents](#what-are-sub-agents)
2. [When to Use Sub-agents](#when-to-use-sub-agents)
3. [🆕 Background Agents (December 2025)](#background-agents-december-2025)
4. [Creating Sub-agents](#creating-sub-agents)
5. [Sub-agent Communication](#sub-agent-communication)
6. [Best Practices](#best-practices)
7. [Common Patterns](#common-patterns)
8. [Troubleshooting](#troubleshooting)

---

## What Are Sub-agents

Sub-agents are specialized instances of Claude Code that handle specific subtasks while the main agent coordinates overall work.

### Types of Sub-agents

**1. Custom Sub-agents** (Files in `.claude/agents/`)
- Defined as markdown files
- Specialized for specific tasks
- Explicitly invoked by main agent

**2. 🆕 Background Agents** (Started with `&`)
- Run tasks asynchronously
- Don't block main conversation
- Report back when complete

**3. Automatic Sub-agents** (Spawned by main agent)
- Created dynamically for parallel work
- Shown as `Task(...)` in output
- Coordinated automatically

---

## When to Use Sub-agents

### ✅ Good Use Cases

**Parallel Independent Tasks**
```
Main: Building frontend
Sub-agent 1: Setting up backend API
Sub-agent 2: Writing tests
```

**Specialized Knowledge**
```
Test Agent: Knows testing frameworks deeply
Security Agent: Focuses on security audits
Code Reviewer: Specialized review patterns
```

**Long-Running Background Work**
```bash
& run full test suite        # While you continue coding
& npm run build              # Build in background
& deploy to staging          # Deploy while working
```

### ❌ When NOT to Use

**Interdependent Work**
- Tasks that need to share context
- Sequential operations
- Work requiring constant coordination

**Simple Tasks**
- One-off operations
- Quick fixes
- Simple queries

**High Context Sharing**
- Work requiring full project context
- Tasks needing recent conversation

---

## Background Agents (December 2025)

### Overview

Background agents run asynchronously, allowing you to continue working while they execute long-running tasks.

### Syntax

```bash
& [command or task description]
```

### Examples

```bash
# Run tests in background
& run the full test suite

# Build while working
& npm run build

# Deploy to staging
& deploy to staging environment

# Analyze large codebase
& analyze all Python files for security issues

# Generate documentation
& generate API documentation from code
```

### How It Works

1. You prefix a command with `&`
2. Claude spawns a background agent
3. You can continue conversation
4. Agent reports back when complete
5. Results appear in your chat

### Monitoring Background Agents

```bash
# Check status
/context                    # Shows active agents

# View results
# Results automatically appear in chat when complete
```

### Best Practices for Background Agents

1. **Use for long-running tasks** (>30 seconds)
2. **Don't start too many** (2-3 max at once)
3. **Monitor context usage** (each agent uses tokens)
4. **Wait for completion** before related work

---

## Creating Sub-agents

### File Structure

```
.claude/
└── agents/
    ├── test-agent.md
    ├── security-agent.md
    └── code-reviewer.md
```

### Template

```markdown
# Agent Name

## Purpose
Brief description of what this agent does

## Expertise
- Area 1
- Area 2
- Area 3

## Working Style
How this agent approaches tasks

## Key Responsibilities
1. Responsibility 1
2. Responsibility 2

## Tools and Access
- Tool 1
- Tool 2

## Output Format
How results should be presented

## Example Usage
```
Agent invoked: Task(Run security audit)
[Agent performs work]
Agent complete: Found 3 vulnerabilities:
1. SQL injection risk in auth.py
2. Exposed API key in config
3. Missing CSRF protection
```
```

### Example: Test Agent

```markdown
# Test Agent

## Purpose
Run tests, analyze failures, and suggest fixes

## Expertise
- Jest, Pytest, JUnit, Cargo test
- Test debugging
- Coverage analysis
- Test strategy

## Working Style
1. Run tests first
2. Analyze failures carefully
3. Provide clear fix suggestions
4. Re-run to verify

## Key Responsibilities
1. Execute test suites
2. Parse test output
3. Identify root causes
4. Suggest specific fixes
5. Verify fixes work

## Tools and Access
- bash: Run test commands
- read: Access test files and source
- write: Fix tests or code

## Output Format
```
Tests run: [number]
Passed: [number]
Failed: [number]

Failures:
1. test_user_login
   Error: AssertionError: Expected 200, got 401
   Location: tests/test_auth.py:45
   Fix: Update mock to return valid token
```

## Example Usage
```
> Task(Run tests and fix failures)

Running tests...
$ npm test

Tests: 45 passed, 3 failed

Failed tests:
1. User authentication test
   Fix: Mock token generator
2. Database connection test
   Fix: Update connection string
3. API rate limit test
   Fix: Increase timeout

Applying fixes...
All tests now passing ✓
```
```

---

## Sub-agent Communication

### Invocation

**Explicit (Custom Agents)**
```
"Use the test agent to run tests"
"Have the security agent audit this code"
```

**Implicit (Automatic)**
```
"Build the frontend while setting up the backend"
→ Claude spawns sub-agents automatically
```

**🆕 Background (Async)**
```bash
& run integration tests
& deploy to staging
```

### Information Flow

```
Main Agent
    ↓ (sends task + context)
Sub-agent
    ↓ (performs work)
Results
    ↓ (returns findings)
Main Agent
    ↓ (integrates results)
You
```

### Context Sharing

**What Sub-agents Get:**
- Task description
- Relevant file paths
- Specific instructions
- .claude/agents/[name].md content

**What Sub-agents DON'T Get:**
- Full conversation history
- Unrelated project context
- Other sub-agent conversations

---

## Best Practices

### 1. Keep Agents Focused

**Good: Specialized Agent**
```markdown
# Test Agent
- Runs tests
- Analyzes failures
- Suggests fixes
```

**Bad: Kitchen Sink Agent**
```markdown
# Everything Agent
- Tests, deploys, reviews, refactors, documents
```

### 2. Provide Clear Context

**Good Invocation**
```
"Use the test agent to run the authentication tests in
tests/auth/ and fix any failures"
```

**Bad Invocation**
```
"Fix the tests"
```

### 3. Limit Concurrent Agents

**Good**
```
2-3 sub-agents for parallel independent work
```

**Bad**
```
10 sub-agents all working at once
→ Context explosion
→ Coordination chaos
```

### 4. Monitor Context Usage

```bash
/context                    # Check before spawning agents
```

If context >70%, consider:
- Clearing context (`/clear`)
- Finishing current sub-agents first
- Using background agents for long tasks

### 5. Use Background Agents for Long Tasks

**Before (December 2025)**
```
"Run the full test suite"
→ Blocks conversation for 5 minutes
```

**After (December 2025)**
```bash
& run the full test suite
→ Continues in background
→ You keep working
→ Results appear when ready
```

---

## Common Patterns

### Pattern 1: Parallel Development

```
Main: Coordinate overall feature
Sub-agent 1: Build backend API
Sub-agent 2: Create frontend components
Sub-agent 3: Write tests

Results integrated by main agent
```

### Pattern 2: Sequential with Background

```bash
# Start long tasks in background
& run full test suite
& analyze code coverage

# Continue working on main task
"Implement user authentication"

# Background results appear when ready
```

### Pattern 3: Specialized Review

```
Main: Implement feature
Security Agent: Audit for vulnerabilities
Test Agent: Verify test coverage
Code Reviewer: Check code quality

Main: Integrate all feedback
```

### Pattern 4: Iterative Development

```
1. Main: Implement initial version
2. Test Agent: Run tests, report failures
3. Main: Fix failures based on report
4. Test Agent: Verify fixes
5. Repeat until all tests pass
```

### Pattern 5: Background CI/CD

```bash
# Make changes
"Update authentication logic"

# Run CI pipeline in background
& lint and test all files
& build Docker image
& run security scan

# Continue working
"Now implement the password reset flow"

# Background tasks complete, show results
```

---

## Troubleshooting

### Sub-agent Not Working

**Check:**
1. Agent file exists in `.claude/agents/`
2. Agent file is valid markdown
3. Agent name matches invocation
4. Sufficient context available

**Solution:**
```bash
# Verify agent exists
ls -la .claude/agents/

# Check context
/context

# Explicit invocation
"Use the agent in .claude/agents/test-agent.md to run tests"
```

### Too Many Sub-agents

**Symptoms:**
- Context filling up fast
- Slow responses
- Agents not coordinating

**Solutions:**
1. Wait for current agents to complete
2. Use `/clear` to reset
3. Limit to 2-3 concurrent agents
4. Use background agents (`&`) for long tasks

### Sub-agent Lost Context

**Symptoms:**
- Agent doesn't understand task
- Asks for information already provided

**Solutions:**
1. Provide explicit file paths
2. Include relevant code snippets
3. Simplify the task
4. Use main agent for context-heavy work

### Background Agent Not Responding

**Check:**
```bash
/context                    # Look for active agents
```

**Solutions:**
1. Wait longer (may still be running)
2. Check if task was too vague
3. Restart with clearer instructions
4. Use `/clear` and retry

---

## Example Workflows

### Workflow 1: Feature Development with Tests

```bash
# Main agent starts
"Implement user login feature"

# Main implements core logic
[Code written]

# Spawn test agent
"Use test agent to create and run tests for the login feature"

Task(Create and run login tests)
→ Tests written
→ Tests run
→ Report back

# Main agent receives results and iterates
```

### Workflow 2: Parallel Backend + Frontend

```bash
# Start parallel development
"Build authentication: backend API in Python, frontend in React"

# Claude spawns sub-agents
Task(Build Python FastAPI authentication endpoints)
Task(Create React authentication components)

# Both work in parallel
# Results integrated by main agent
```

### Workflow 3: CI/CD with Background Agents

```bash
# Implement feature
"Add email verification to user registration"

# Start background CI tasks
& run full test suite
& lint all files
& run security scanner
& build Docker image

# Continue working on next feature
"Now add password strength requirements"

# Background results appear when ready
Test Suite: ✓ All 87 tests passed
Linter: ✓ No issues found  
Security: ⚠ 1 warning (low severity)
Build: ✓ Image built successfully
```

---

## Advanced: Custom Agent Template

```markdown
# [Agent Name]

## Purpose
[One-line description]

## Expertise
- [Domain 1]
- [Domain 2]
- [Domain 3]

## Working Style
[How this agent approaches tasks]

## Key Responsibilities
1. [Primary task]
2. [Secondary task]
3. [Verification task]

## Tools and Access
- bash: [Specific commands]
- read: [File patterns]
- write: [When to write]

## Process
1. [Step 1]
2. [Step 2]
3. [Step 3]
4. [Step 4]

## Output Format
```
[Example output structure]
```

## Common Issues
- Issue 1: [Solution]
- Issue 2: [Solution]

## Example Usage
```
[Example invocation and results]
```

## 🆕 Background Mode
This agent can run in background for:
- [Long task 1]
- [Long task 2]

Usage: `& [agent name] [task]`
```

---

## Pro Tips

1. **Name Sessions with Sub-agents**
   ```bash
   /rename feature-with-parallel-work
   # Easy to resume later
   ```

2. **Use Background for Long Tests**
   ```bash
   & run e2e tests
   # Continue coding while tests run
   ```

3. **Monitor with /context**
   - See active sub-agents
   - Check token usage
   - Know when to wait

4. **Combine with LSP**
   - Sub-agents can use LSP too
   - Better code navigation
   - More accurate changes

5. **Clear Between Major Tasks**
   ```bash
   /clear    # Reset for fresh start
   ```

---

## Comparison: Sub-agents vs Background Agents

| Feature | Custom Sub-agents | Background Agents |
|---------|------------------|-------------------|
| **Definition** | Files in .claude/agents/ | Tasks with `&` prefix |
| **Context** | Specialized knowledge | Same as main agent |
| **Blocking** | Blocks conversation | Runs async |
| **Best For** | Specialized expertise | Long-running tasks |
| **Coordination** | Explicit invocation | Implicit |
| **Use When** | Need specialized agent | Need to keep working |

---

## Summary

**Key Takeaways:**

1. Sub-agents enable parallel work
2. 🆕 Background agents (`&`) for async tasks
3. Keep agents focused and specialized
4. Limit concurrent agents (2-3 max)
5. Monitor context usage
6. Use background agents for long tasks
7. Named sessions help with complex work

**December 2025 Improvements:**
- Background agents with `&` prefix
- Better async task handling
- Improved agent coordination
- LSP integration for all agents
- Named sessions for agent workflows

---

**Updated**: December 2025 with Background Agents

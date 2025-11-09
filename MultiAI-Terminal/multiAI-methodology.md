# Claude AI-CLI Methodology Guide

A comprehensive guide to using Claude Code and other AI CLI tools effectively for terminal-based development workflows.

## Table of Contents

1. [AI CLI Tools Comparison & Free Tier Access](#ai-cli-tools-comparison--free-tier-access)
2. [Agent Types: Local vs Global](#agent-types-local-vs-global)
3. [Context Management Strategies](#context-management-strategies)
4. [Project Criticism & Quality Assurance](#project-criticism--quality-assurance)
5. [Output Formatting Strategies](#output-formatting-strategies)
6. [Multi-AI Integration Workflows](#multi-ai-integration-workflows)
7. [Practical Examples & Workflows](#practical-examples--workflows)

---

## AI CLI Tools Comparison & Free Tier Access

### Free Tier Access Map (2025)

| AI CLI Tool | Free Tier Status | Rate Limits | Cost After Free Tier | Best For |
|-------------|------------------|-------------|---------------------|----------|
| **Gemini CLI** | ✅ Yes | 60 req/min, 1,000 req/day | Still free for most use | Budget-conscious developers, rapid prototyping |
| **Grok CLI** | ✅ Yes (Limited) | ~100 daily requests, $25 monthly API credit | $30/month SuperGrok | Open-source projects, local inference |
| **Claude Code** | ❌ No | N/A | Paid subscription required | Professional development, complex tasks |
| **OpenAI Codex** | ❌ No | N/A | API token rates (Plus/Pro/Team required) | Enterprise teams |

### Detailed Tool Breakdown

#### Gemini CLI
- **Installation**: `npm install -g @google/gemini-cli` (open source, Apache 2.0)
- **Authentication**: Personal Google account or API key from Google AI Studio
- **Free Tier**: 1 million token context window with Gemini 2.5 Pro/Flash blend
- **Best Use**: Daily scripting, documentation generation, code reviews

#### Grok CLI
- **Installation**: `pip install grok-cli` (requires Python 3.8+)
- **Authentication**: API key from console.x.ai
- **Free Tier**: $25 monthly API credit, ~100 daily requests
- **Best Use**: Privacy-focused tasks, local inference, experimentation

#### Claude Code (Your Paid Tool)
- **Installation**: Official Anthropic CLI
- **Authentication**: Paid subscription
- **Best Use**: Complex multi-file refactoring, architectural decisions, production code

---

## Agent Types: Local vs Global

### Understanding Agent Scopes

#### Global Agents (Project-Wide)
Global agents are defined in `~/.config/claude/agents/` and available across all projects.

**Recommended Global Agents:**

1. **Code Reviewer Agent** (`~/.config/claude/agents/code-reviewer.md`)
```markdown
You are a critical code reviewer. Your job is to:
- Identify security vulnerabilities (SQL injection, XSS, CSRF, etc.)
- Check for performance bottlenecks
- Verify error handling and edge cases
- Ensure code follows SOLID principles
- Flag technical debt

Always provide specific line references and actionable improvements.
```

2. **Context Preserver Agent** (`~/.config/claude/agents/context-keeper.md`)
```markdown
You are a context management specialist. Your role is to:
- Summarize the current conversation state
- Identify key decisions made
- Track open questions and blockers
- Maintain a list of files modified
- Preserve critical context before /clear

Format output as:
## Session Summary
- **Objective**: [main goal]
- **Progress**: [completed tasks]
- **Decisions**: [key choices made]
- **Next Steps**: [pending items]
```

3. **Architecture Critic Agent** (`~/.config/claude/agents/architect-critic.md`)
```markdown
You are a senior software architect. Evaluate:
- System design patterns and their appropriateness
- Scalability concerns
- Coupling and cohesion
- Technology stack choices
- Future maintainability

Provide both strengths and weaknesses. Be brutally honest.
```

4. **Documentation Generator Agent** (`~/.config/claude/agents/doc-writer.md`)
```markdown
You generate clear, concise documentation:
- README files with setup instructions
- API documentation with examples
- Code comments for complex logic
- Architecture decision records (ADRs)

Use markdown formatting and include code examples.
```

#### Local Agents (Project-Specific)
Local agents are defined in `.claude/agents/` within your project directory.

**Recommended Local Agents:**

1. **Project-Specific Tester** (`.claude/agents/test-runner.md`)
```markdown
You run and interpret tests for this project.
- Test framework: [specify: pytest, jest, etc.]
- Test command: [e.g., npm test, pytest tests/]
- Coverage requirements: [e.g., >80%]

When tests fail, provide:
1. Root cause analysis
2. Suggested fixes with code
3. Additional test cases to prevent regression
```

2. **Style Enforcer** (`.claude/agents/style-checker.md`)
```markdown
Enforce this project's coding standards:
- Linting: [eslint, flake8, etc.]
- Formatting: [prettier, black, etc.]
- Naming conventions: [camelCase, snake_case, etc.]
- File organization: [describe structure]

Auto-fix issues when possible, report what can't be automated.
```

### Agent Invocation Strategy

**When to use Global vs Local agents:**

| Scenario | Agent Type | Example |
|----------|------------|---------|
| Security review | Global | `@code-reviewer check for vulnerabilities` |
| Project-specific tests | Local | `@test-runner verify all edge cases` |
| Long conversation | Global | `@context-keeper summarize before /clear` |
| Architecture decision | Global | `@architect-critic evaluate this design` |
| Project documentation | Local | `@doc-writer update README for new feature` |

---

## Context Management Strategies

### The Context Problem
Claude's context window, while large (200k tokens), can become polluted with irrelevant information, leading to:
- Decreased response quality
- Slower processing
- Higher costs
- Context drift (forgetting important decisions)

### Solution: Proactive Context Management

#### 1. Use CLAUDE.md for Persistent Context
Store project conventions in `CLAUDE.md` at your project root:

```markdown
# CLAUDE.md

## Project Context
- **Tech Stack**: Node.js 20, TypeScript 5.x, React 18
- **Architecture**: Microservices with event-driven communication
- **Test Framework**: Jest with React Testing Library
- **CI/CD**: GitHub Actions → AWS ECS

## Commands
- Build: `npm run build`
- Test: `npm test -- --coverage`
- Lint: `npm run lint -- --fix`
- Dev: `npm run dev`

## Critical Patterns
- All API calls must use the `ApiClient` wrapper (handles auth/retry)
- State management via Zustand, NOT Redux
- Error boundaries required for all route components
```

#### 2. Periodic Context Compaction
**Manual compaction workflow:**

```bash
# Step 1: Preserve critical context
> @context-keeper create session summary

# Step 2: Save summary to file
> Write the summary to .claude/context-history/session-2025-11-09.md

# Step 3: Clear context
> /clear

# Step 4: Restore essential context
> Read .claude/context-history/session-2025-11-09.md and continue from there
```

**When to compact:**
- After completing a major feature
- When switching to unrelated work
- Every 50-100 messages
- When noticing response quality degradation

#### 3. Subagent Context Isolation
Use subagents (via Task tool) to prevent context pollution:

```bash
# Bad: Direct question adds unnecessary context
> Search the entire codebase for all usages of deprecated function X

# Good: Delegate to subagent
> Use the Task tool with subagent_type=Explore to find all usages of function X.
> Report back only the file paths and line numbers.
```

Subagents maintain isolated context and return only relevant summaries.

#### 4. Context Preservation Checklist
Before running `/clear`, ensure you've captured:
- [ ] Current objective
- [ ] Files modified (with brief descriptions)
- [ ] Key architectural decisions
- [ ] Open questions or blockers
- [ ] Next steps

---

## Project Criticism & Quality Assurance

### Multi-Stage Review Process

#### Stage 1: Self-Review (Claude)
After completing code, use built-in reflection:

```bash
> Review the code you just wrote critically. Check for:
> 1. Security vulnerabilities
> 2. Performance issues
> 3. Missing error handling
> 4. Unclear variable names
> 5. Missing tests or documentation
```

#### Stage 2: Dedicated Critic Agent (Claude)
Invoke your global critic agent:

```bash
> @code-reviewer perform a security-focused review of all changes

> @architect-critic evaluate if this design scales to 1M users
```

#### Stage 3: External AI Review (Gemini - Free)
Use Gemini CLI for a second opinion:

```bash
# Export your changes
git diff > /tmp/changes.patch

# Get Gemini's perspective
gemini "Review this code patch for security issues and suggest improvements" < /tmp/changes.patch
```

#### Stage 4: Specialized Review (Grok - Free)
Use Grok for specific domains:

```bash
# Grok excels at unconventional approaches
grok-cli "Challenge the assumptions in this architecture design" < design.md
```

### Criticism Workflow Example

```bash
# 1. Complete feature with Claude
> Implement user authentication with JWT

# 2. Self-review
> Review your implementation for common auth vulnerabilities

# 3. Agent review
> @code-reviewer check auth implementation

# 4. External review (Gemini)
$ gemini "Review this JWT implementation for security issues" < src/auth/jwt.ts

# 5. Synthesize feedback
> Based on the reviews, create a prioritized list of improvements
```

---

## Output Formatting Strategies

### Format by Use Case

#### 1. Terminal Display (Human Readable)
**Request markdown with structure:**

```bash
> Format the output as markdown with:
> - Clear section headers (##)
> - Bullet points for lists
> - Code blocks with syntax highlighting
> - Tables for comparisons
```

**Example output:**
```markdown
## API Endpoints

### Authentication
- `POST /auth/login` - User login
- `POST /auth/logout` - User logout

### Users
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /users | List all users |
| POST | /users | Create user |
```

#### 2. Machine-Readable (Scripting)
**Request strict JSON:**

```bash
> Output valid JSON only. No explanations or markdown.
> Schema: {"files": [{"path": string, "changes": number}]}
```

**System prompt approach (for APIs):**
```
You are a JSON generator. Always output valid JSON without explanations.
Use temperature=0 for consistency.
```

#### 3. Mixed Format (Documentation)
**Combine markdown and code:**

```bash
> Create a README.md with:
> - Introduction (markdown)
> - Installation steps (numbered list)
> - Code examples (syntax-highlighted)
> - API reference (tables)
```

### Formatting Tools

#### rich-cli (Enhanced Terminal Display)
```bash
# Install
pip install rich-cli

# Pretty-print JSON
cat data.json | rich --json

# Render markdown
claude "Explain Docker" | rich --markdown

# Syntax highlight code
rich --syntax python myfile.py
```

#### jq (JSON Processing)
```bash
# Parse Claude's JSON output
claude "List project files as JSON" | jq '.files[] | select(.type == "ts")'

# Format and colorize
echo '{"status":"ok"}' | jq '.'
```

### Output Validation Strategy

```bash
# For JSON output, always validate:
claude "Generate config as JSON" > config.json
jq empty config.json && echo "Valid JSON" || echo "Invalid JSON"

# For markdown, use a linter:
npm install -g markdownlint-cli
claude "Write README" > README.md
markdownlint README.md
```

---

## Multi-AI Integration Workflows

### The Multi-AI Strategy
Combine Claude (paid), Gemini (free), and Grok (free) for optimal cost/quality balance.

### Workflow Patterns

#### Pattern 1: Task Delegation by Complexity

```bash
# Complex tasks → Claude (your paid tool)
claude "Refactor this 500-line module to use dependency injection"

# Medium tasks → Gemini (free)
gemini "Write unit tests for this utility function" < utils.ts

# Simple tasks → Grok (free, fast)
grok-cli "Generate JSDoc comments" < myfile.js
```

#### Pattern 2: Consensus-Driven Development

```bash
# Get multiple perspectives on important decisions
claude "Should we use REST or GraphQL for this API? Explain trade-offs" > claude_opinion.md
gemini "REST vs GraphQL for a mobile app backend" > gemini_opinion.md
grok-cli "GraphQL vs REST: devil's advocate perspective" > grok_opinion.md

# Synthesize with Claude
claude "Synthesize these three opinions into a final recommendation" < <(cat *_opinion.md)
```

#### Pattern 3: Pipeline Processing

```bash
# Stage 1: Gemini generates initial code (free)
gemini "Create a basic Express.js REST API for user management" > api.js

# Stage 2: Claude refines and adds features (paid)
claude "Enhance this API with authentication, validation, and error handling" < api.js > api-enhanced.js

# Stage 3: Grok adds creative improvements (free)
grok-cli "Suggest unconventional but useful features for this API" < api-enhanced.js
```

#### Pattern 4: Cost-Optimized Iteration

```bash
# Initial drafts: Use Gemini (free)
for i in {1..5}; do
  gemini "Draft approach $i for implementing caching" >> drafts.md
done

# Final selection and implementation: Use Claude (paid)
claude "Review these 5 approaches and implement the best one" < drafts.md
```

### Creating Multi-AI Agents

#### Gemini Agent (Global)
**File:** `~/.config/claude/agents/gemini-assistant.md`
```markdown
You are a Gemini CLI integration agent. When invoked:

1. Take the user's request
2. Format it appropriately for Gemini's strengths
3. Generate a `gemini` command to execute
4. Execute via Bash tool
5. Parse and present the response

Gemini excels at:
- Code generation from scratch
- Documentation writing
- Explaining concepts
- Large-batch processing

Usage: @gemini-assistant [task description]
```

#### Grok Agent (Global)
**File:** `~/.config/claude/agents/grok-assistant.md`
```markdown
You are a Grok CLI integration agent. When invoked:

1. Take the user's request
2. Format it for Grok's creative problem-solving style
3. Generate a `grok-cli` command
4. Execute via Bash tool
5. Present response with context

Grok excels at:
- Alternative perspectives
- Challenging assumptions
- Creative solutions
- Trend analysis

Usage: @grok-assistant [task description]
```

### Multi-AI Orchestration Script

Create a helper script for parallel AI queries:

**File:** `~/.local/bin/multi-ai`
```bash
#!/bin/bash
# multi-ai: Query multiple AIs in parallel

QUERY="$1"
OUTPUT_DIR="/tmp/multi-ai-$(date +%s)"
mkdir -p "$OUTPUT_DIR"

# Parallel execution
(echo "=== Claude ===" && claude "$QUERY") > "$OUTPUT_DIR/claude.txt" &
(echo "=== Gemini ===" && gemini "$QUERY") > "$OUTPUT_DIR/gemini.txt" &
(echo "=== Grok ===" && grok-cli "$QUERY") > "$OUTPUT_DIR/grok.txt" &

wait

# Combine results
cat "$OUTPUT_DIR"/*.txt
```

**Usage:**
```bash
multi-ai "What's the best way to handle API rate limiting?"
```

---

## Practical Examples & Workflows

### Example 1: Feature Development with Context Preservation

```bash
# 1. Start with clear context
> I'm implementing a user dashboard. Tech stack: React 18, TypeScript, Zustand for state.
> Create a todo list for this feature.

# 2. Work through todos
> Implement the first todo item

# 3. Before context gets too large (~50 messages)
> @context-keeper summarize our progress
> Write summary to .claude/sessions/dashboard-$(date +%Y%m%d).md

# 4. Clear and continue
> /clear
> Read .claude/sessions/dashboard-20251109.md and continue with remaining todos
```

### Example 2: Multi-AI Code Review

```bash
# 1. Complete feature with Claude
> Implement payment processing with Stripe

# 2. Get diverse reviews
$ claude "@code-reviewer security audit" > reviews/claude.md
$ gemini "Review for PCI compliance issues" < src/payment.ts > reviews/gemini.md
$ grok-cli "Find edge cases in payment flow" < src/payment.ts > reviews/grok.md

# 3. Synthesize and fix
> Read all files in reviews/ and create a prioritized fix list
> Implement the critical fixes first
```

### Example 3: Documentation Generation Pipeline

```bash
# 1. Draft with Gemini (free)
$ gemini "Generate API documentation from this OpenAPI spec" < api-spec.yaml > docs-draft.md

# 2. Enhance with Claude (paid)
> Improve this documentation with:
> - Usage examples for each endpoint
> - Common error scenarios
> - Best practices section
< docs-draft.md

# 3. Review with Grok (free)
$ grok-cli "Identify gaps or confusing parts in this documentation" < docs-final.md
```

### Example 4: Debugging with Agent Hierarchy

```bash
# 1. Initial investigation (you)
> I'm getting "Cannot read property 'id' of undefined". Here's the code: [paste]

# 2. First-pass analysis (Claude main)
> Analyze this error and suggest fixes

# 3. Deep dive (subagent via Task tool)
> Use Task tool with Explore agent to find all places where this object is accessed

# 4. External validation (Gemini)
$ gemini "Review this fix for the undefined property error" < fixed-code.ts

# 5. Final verification (local test agent)
> @test-runner verify the fix with all test suites
```

---

## Best Practices Summary

### DO:
- ✅ Use CLAUDE.md for project-specific context that persists across sessions
- ✅ Create global agents for common tasks (code review, architecture criticism)
- ✅ Create local agents for project-specific workflows (testing, style enforcement)
- ✅ Run `/clear` proactively before context degrades
- ✅ Use subagents (Task tool) to isolate exploratory work
- ✅ Delegate simple tasks to free AI CLIs (Gemini, Grok)
- ✅ Request specific output formats (JSON, markdown, tables)
- ✅ Validate machine-readable output (jq for JSON)
- ✅ Get multiple AI perspectives on important decisions
- ✅ Use rich-cli for better terminal visualization

### DON'T:
- ❌ Let context grow to 100+ messages without compaction
- ❌ Use Claude for simple tasks better suited to free AIs
- ❌ Forget to preserve critical decisions before `/clear`
- ❌ Accept unstructured output when scripting
- ❌ Make architectural decisions without critic agent review
- ❌ Skip security reviews from code-reviewer agent
- ❌ Deploy without running project-specific test agent
- ❌ Assume AI output is valid (always validate JSON/code)

---

## Quick Reference Card

```bash
# Context Management
claude "/clear"                                    # Clear context
claude "@context-keeper summarize session"        # Save state before clear

# Agent Invocation
claude "@code-reviewer check security"            # Global agent
claude "@test-runner verify changes"              # Local agent

# Multi-AI Queries
gemini "generate boilerplate code" < spec.md     # Free, code generation
grok-cli "challenge this design" < design.md     # Free, alternative views
claude "refactor for production" < draft.js      # Paid, complex refactoring

# Output Formatting
claude "output as JSON only" | jq .              # Machine-readable
claude "create markdown report" | rich -m        # Human-readable

# Task Delegation
claude "Use Task tool to explore codebase"       # Isolate context
```

---

## Additional Resources

- [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)
- [Gemini CLI GitHub](https://github.com/google-gemini/gemini-cli)
- [Grok CLI Documentation](https://github.com/superagent-ai/grok-cli)
- [rich-cli](https://github.com/Textualize/rich-cli)
- [jq Manual](https://stedolan.github.io/jq/manual/)

---

**Version:** 1.0
**Last Updated:** 2025-11-09
**Maintained by:** MultiAI-Terminal Project

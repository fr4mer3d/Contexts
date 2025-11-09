# Multi-AI Agents Architecture

A comprehensive multi-agent system leveraging different AI clients (Claude, Gemini, Grok, Amazon Q, GitHub Copilot) based on their strengths for optimal performance and cost efficiency.

## Table of Contents

1. [Architecture Overview](#architecture-overview)
2. [Agent Definitions](#agent-definitions)
3. [AI Client Assignments](#ai-client-assignments)
4. [Implementation Guide](#implementation-guide)
5. [Workflow Patterns](#workflow-patterns)
6. [Configuration Templates](#configuration-templates)

---

## Architecture Overview

### Agent Flow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                    INFORMATION GATHERING                     │
├─────────────────────────────────────────────────────────────┤
│  researcher_classical  │  researcher_popular  │  researcher_ │
│  (Gemini - Free)       │  (Grok - Free)       │  academic    │
│  • Official docs       │  • Reddit            │  (Gemini)    │
│  • Technical sources   │  • X/Twitter         │  • Papers    │
│  • Renowned entities   │  • Forums            │  • Conferences│
└──────────────┬──────────────────┬──────────────────┬─────────┘
               │                  │                  │
               └──────────────────┼──────────────────┘
                                  ▼
┌─────────────────────────────────────────────────────────────┐
│                   INFORMATION PROCESSING                     │
├─────────────────────────────────────────────────────────────┤
│     agent_theoric              │        agent_developer      │
│     (Claude - $20/month)       │     (GitHub Copilot - Free) │
│     • Theoretical reasoning    │     • Code implementation   │
│     • Logical assumptions      │     • Practical guides      │
│     • Concept analysis         │     • How-to instructions   │
└──────────────┬─────────────────┴─────────────┬──────────────┘
               │                               │
               └───────────────┬───────────────┘
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                      QUALITY ASSURANCE                       │
├─────────────────────────────────────────────────────────────┤
│    reviewer_brute              │     reviewer_technical      │
│    (Grok - Free)               │     (Claude - $20/month)    │
│    • Brutal honesty            │     • Balanced analysis     │
│    • Weakness focus            │     • Holistic review       │
│    • Improvement paths         │     • Technical depth       │
└──────────────┬─────────────────┴─────────────┬──────────────┘
               │                               │
               └───────────────┬───────────────┘
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                     PROJECT MANAGEMENT                       │
├─────────────────────────────────────────────────────────────┤
│                     note_taker                               │
│                  (Amazon Q - Free)                           │
│                  • Progress tracking                         │
│                  • Todo lists                                │
│                  • Short reminders                           │
└─────────────────────────────────────────────────────────────┘
```

---

## Agent Definitions

### Research Agents (Information Gathering Layer)

#### 1. researcher_classical
**Purpose:** Gather authoritative, technical information from official and renowned sources.

**Responsibilities:**
- Search official documentation (language specs, API docs, RFCs)
- Find information from renowned technical entities (IETF, W3C, IEEE)
- Extract knowledge from authoritative books and publications
- Query technical knowledge bases (Stack Overflow top answers, MDN, official wikis)
- Prioritize accuracy and technical correctness over accessibility

**Output Format:**
```markdown
## Classical Research Results

### Official Documentation
- [Source name]: [URL]
  - Key finding: [summary]
  - Technical detail: [specific info]

### Renowned Entities
- [Entity name] ([authority level])
  - Position/recommendation: [summary]
  - Rationale: [explanation]

### Technical Specifications
- [Spec name] ([version])
  - Relevant sections: [list]
  - Implementation notes: [details]
```

**Assigned AI Client:** 🟢 **Gemini (Free Tier)**
- Rationale: Excellent at processing large documents, 1M token context window, free 1,000 requests/day

---

#### 2. researcher_popular
**Purpose:** Gather organic, community-driven insights from social platforms and forums.

**Responsibilities:**
- Search Reddit communities (r/programming, r/webdev, r/machinelearning, etc.)
- Monitor X/Twitter discussions and trending topics
- Browse Fediverse forums (Mastodon, Lemmy)
- Extract insights from cyber forums (HackerNews, Lobsters, DevTo)
- Capture real-world experiences and community sentiment
- Identify common pitfalls and practical tips

**Output Format:**
```markdown
## Popular/Community Research Results

### Reddit Insights
- r/[subreddit] - [upvotes] upvotes
  - User experience: [summary]
  - Common issues: [list]

### X/Twitter Trends
- @[handle] ([followers])
  - Key point: [tweet summary]
  - Community response: [engagement metrics]

### Forum Discussions
- [Platform] - [thread title]
  - Consensus: [majority opinion]
  - Dissenting views: [alternative perspectives]
  - Practical tips: [actionable advice]
```

**Assigned AI Client:** 🟢 **Grok (Free Tier)**
- Rationale: Excels at understanding social context, trends, and organic community content. Built for real-world, conversational data.

---

#### 3. researcher_academic
**Purpose:** Gather scholarly research from academic papers and conference proceedings.

**Responsibilities:**
- Search academic databases (arXiv, ACM Digital Library, IEEE Xplore, Google Scholar)
- Focus on peer-reviewed papers and conference proceedings
- Prioritize top-tier conferences (NeurIPS, ICML, ACL, CVPR, etc.)
- Extract methodologies, experimental results, and theoretical foundations
- Track citation counts and paper impact
- Identify seminal works and recent breakthroughs

**Output Format:**
```markdown
## Academic Research Results

### Papers
- **[Paper Title]** ([Year])
  - Authors: [list]
  - Conference/Journal: [venue] (rank: [A*/A/B])
  - Citations: [count]
  - Key contribution: [summary]
  - Methodology: [approach]
  - Results: [findings]
  - Limitations: [weaknesses]

### Relevant Conferences
- [Conference name] ([year])
  - Relevant tracks: [list]
  - Notable papers: [titles]
```

**Assigned AI Client:** 🟢 **Gemini (Free Tier)**
- Rationale: Large context window for processing lengthy papers, excellent at extracting structured information from academic text.

---

### Processing Agents (Information Digestion Layer)

#### 4. agent_theoric
**Purpose:** Synthesize research into theoretical frameworks and logical reasoning.

**Responsibilities:**
- Analyze information from all research agents
- Construct theoretical models and frameworks
- Make logical assumptions based on available data
- Identify patterns and underlying principles
- Reason about edge cases and theoretical implications
- Connect concepts across different sources
- Propose hypotheses and theoretical explanations

**Output Format:**
```markdown
## Theoretical Analysis

### Core Concepts
[Synthesized understanding of fundamental principles]

### Logical Assumptions
1. **Assumption:** [statement]
   - **Basis:** [supporting evidence from research]
   - **Confidence:** [High/Medium/Low]
   - **Implications:** [what this means]

### Theoretical Framework
[Visual or textual model explaining relationships]

### Edge Cases & Considerations
- [Case 1]: [theoretical reasoning]
- [Case 2]: [theoretical reasoning]

### Open Questions
- [Question 1]: [why it matters]
- [Question 2]: [why it matters]
```

**Assigned AI Client:** 🔴 **Claude (Pro - $20/month)**
- Rationale: Superior complex reasoning, deep analysis, and theoretical synthesis. Worth the cost for critical thinking tasks.

---

#### 5. agent_developer
**Purpose:** Translate research and theory into practical, hands-on implementations.

**Responsibilities:**
- Write production-ready code implementations
- Create step-by-step installation guides
- Provide usage examples with real scenarios
- Document "when to use" and "when NOT to use"
- Include practical troubleshooting tips
- Generate CLI commands, scripts, and configurations
- Consider different environments (dev/staging/prod)

**Output Format:**
```markdown
## Practical Implementation

### Installation
```bash
# Step 1: [description]
command_here

# Step 2: [description]
another_command
```

### Code Implementation
```[language]
// [Brief description]
[production-ready code with comments]
```

### Usage Examples

#### Scenario 1: [Real-world situation]
```bash
# Context: [when you'd use this]
example_command --flag value
```

**Expected Output:**
```
[sample output]
```

#### Scenario 2: [Another situation]
[More examples]

### When to Use
- ✅ [Situation 1]
- ✅ [Situation 2]

### When NOT to Use
- ❌ [Situation 1]
- ❌ [Situation 2]

### Troubleshooting
| Error | Cause | Solution |
|-------|-------|----------|
| [Error msg] | [Why it happens] | [How to fix] |
```

**Assigned AI Client:** 🟢 **GitHub Copilot (Free Tier)**
- Rationale: Specialized for code generation, integrated with GitHub, excellent at practical examples. Free tier sufficient for most use cases.
- Alternative: 🟢 **Gemini** for non-code guides and documentation

---

### Review Agents (Quality Assurance Layer)

#### 6. reviewer_brute
**Purpose:** Provide brutally honest, unfiltered critique focused on weaknesses.

**Responsibilities:**
- Identify all weak points without sugar-coating
- Point out logical fallacies and unsupported claims
- Highlight security vulnerabilities and risks
- Call out bad practices and anti-patterns
- Question assumptions aggressively
- Suggest concrete improvements for each weakness
- Play devil's advocate

**Output Format:**
```markdown
## Brutal Review

### Critical Weaknesses

#### ⚠️ [Weakness 1]
**Severity:** [Critical/High/Medium]
**Problem:** [Blunt description of what's wrong]
**Why It Matters:** [Impact of this weakness]
**Improvement:** [Specific steps to fix]

#### ⚠️ [Weakness 2]
[Same structure]

### Unjustified Assumptions
- **Assumption:** [quoted from source]
  - **Problem:** [Why this is weak/unsupported]
  - **Fix:** [What evidence is needed]

### Bad Practices Detected
1. [Practice]: [Why it's bad] → [What to do instead]

### Overall Assessment
[Harsh but fair summary of major issues]

### Mandatory Fixes (Do Not Skip)
1. [Critical fix 1]
2. [Critical fix 2]
```

**Assigned AI Client:** 🟢 **Grok (Free Tier)**
- Rationale: Designed to challenge assumptions, provide contrarian views, and be direct. Perfect personality match for brutal honesty.

---

#### 7. reviewer_technical
**Purpose:** Provide balanced, holistic technical review of both strengths and weaknesses.

**Responsibilities:**
- Analyze technical correctness and accuracy
- Evaluate code quality, architecture, and design patterns
- Assess scalability, performance, and security implications
- Balance positive aspects with areas for improvement
- Provide nuanced, context-aware feedback
- Compare against industry best practices
- Consider trade-offs and design decisions

**Output Format:**
```markdown
## Technical Review

### Executive Summary
[Balanced overview of overall quality]

### Strengths
#### ✅ [Strength 1]
**Aspect:** [What was done well]
**Impact:** [Why this is good]
**Evidence:** [Specific examples]

#### ✅ [Strength 2]
[Same structure]

### Weaknesses
#### ⚠️ [Weakness 1]
**Aspect:** [What needs improvement]
**Impact:** [Why this matters]
**Recommendation:** [How to improve]
**Priority:** [High/Medium/Low]

#### ⚠️ [Weakness 2]
[Same structure]

### Technical Analysis

#### Architecture
- **Design Pattern:** [Evaluation]
- **Scalability:** [Assessment]
- **Maintainability:** [Score with reasoning]

#### Code Quality
- **Readability:** [Score/10] - [Explanation]
- **Test Coverage:** [Percentage] - [Sufficiency assessment]
- **Error Handling:** [Evaluation]

### Trade-offs Evaluation
| Decision | Pros | Cons | Verdict |
|----------|------|------|---------|
| [Choice 1] | [Benefits] | [Drawbacks] | [Appropriate/Reconsider] |

### Best Practices Alignment
✅ Follows: [List compliant practices]
❌ Violates: [List violations with references]

### Recommendations (Prioritized)
1. **[High Priority]** [Recommendation]
2. **[Medium Priority]** [Recommendation]
3. **[Low Priority]** [Recommendation]

### Final Verdict
[Nuanced conclusion: overall quality rating and key takeaways]
```

**Assigned AI Client:** 🔴 **Claude (Pro - $20/month)**
- Rationale: Best-in-class for nuanced technical analysis, balancing multiple factors, and providing deep architectural insights. Critical review warrants paid tier.

---

### Support Agents (Project Management Layer)

#### 8. note_taker
**Purpose:** Track project progress with concise notes, todos, and reminders.

**Responsibilities:**
- Log completed tasks with timestamps
- Maintain prioritized todo list
- Create short, actionable reminders
- Track decisions and their rationale
- Note blockers and dependencies
- Record next steps after each session
- Keep entries brief (1-2 lines max per item)

**Output Format:**
```markdown
## Project Notes - [Date] [Time]

### ✅ Completed
- [HH:MM] [Task description] (Agent: [which agent])
- [HH:MM] [Task description]

### 📋 Todo (Priority Order)
- [ ] **P0** [Critical task] (Assigned: [agent])
- [ ] **P1** [Important task]
- [ ] **P2** [Nice to have]

### 🔔 Reminders
- [Short reminder text]
- [Another reminder]

### 🚧 Blockers
- [Blocker description] → Needs: [what's needed]

### ➡️ Next Steps
1. [Immediate next action]
2. [Following action]

### 💡 Decisions Made
- **[Decision]:** [Brief rationale]
```

**Assigned AI Client:** 🟢 **Amazon Q (Free Tier)**
- Rationale: Developer-focused, good at structured output, free tier, lightweight task that doesn't need premium reasoning.
- Alternative: 🟢 **Gemini** also suitable for this structured, straightforward task

---

## AI Client Assignments

### Cost-Optimized Distribution

| Agent | AI Client | Cost | Rationale |
|-------|-----------|------|-----------|
| researcher_classical | Gemini | Free | Large context, batch processing, 1K requests/day |
| researcher_popular | Grok | Free | Social context, trends, ~100 requests/day |
| researcher_academic | Gemini | Free | Long papers, structured extraction |
| agent_theoric | **Claude** | **$20/mo** | Complex reasoning, synthesis - worth the cost |
| agent_developer | GitHub Copilot | Free | Code specialization, practical examples |
| reviewer_brute | Grok | Free | Contrarian, direct feedback personality |
| reviewer_technical | **Claude** | **$20/mo** | Deep analysis, nuanced review - critical task |
| note_taker | Amazon Q | Free | Structured output, developer workflow |

### Client Specializations

#### 🔴 Claude Pro ($20/month) - 2 agents
**Assigned to:** agent_theoric, reviewer_technical
**Why:** Complex reasoning, theoretical synthesis, deep technical analysis
**Usage Pattern:** Reserve for critical thinking and final reviews

#### 🟢 Gemini (Free) - 2 agents
**Assigned to:** researcher_classical, researcher_academic
**Why:** 1M token context, excellent at processing large documents
**Usage Pattern:** Bulk research, document processing, 1K requests/day

#### 🟢 Grok (Free) - 2 agents
**Assigned to:** researcher_popular, reviewer_brute
**Why:** Social context understanding, direct/contrarian personality
**Usage Pattern:** Community research, challenging assumptions, ~100 requests/day

#### 🟢 GitHub Copilot (Free) - 1 agent
**Assigned to:** agent_developer
**Why:** Specialized for code generation, integrated tooling
**Usage Pattern:** Code implementation, CLI commands, scripts

#### 🟢 Amazon Q (Free) - 1 agent
**Assigned to:** note_taker
**Why:** Developer-focused, structured output, lightweight
**Usage Pattern:** Progress tracking, todos, quick notes

---

## Implementation Guide

### Directory Structure

```
~/.config/multi-ai-agents/
├── researchers/
│   ├── classical.md
│   ├── popular.md
│   └── academic.md
├── processors/
│   ├── theoric.md
│   └── developer.md
├── reviewers/
│   ├── brute.md
│   └── technical.md
└── support/
    └── note_taker.md

.claude/agents/           # Claude-specific agents (theoric, technical)
.gemini/agents/           # Gemini-specific agents (classical, academic)
.grok/agents/             # Grok-specific agents (popular, brute)
```

### Installation Scripts

#### Setup All Clients
```bash
#!/bin/bash
# setup-multi-ai.sh

echo "Setting up Multi-AI Agents Architecture..."

# 1. Claude (already have Pro)
echo "✓ Claude Pro subscription active"

# 2. Gemini CLI
npm install -g @google/gemini-cli
gemini auth login
echo "✓ Gemini CLI installed"

# 3. Grok CLI
pip install grok-cli
# Get API key from console.x.ai
grok-cli config set-api-key YOUR_API_KEY
echo "✓ Grok CLI installed"

# 4. GitHub Copilot CLI
gh extension install github/gh-copilot
gh copilot config
echo "✓ GitHub Copilot CLI installed"

# 5. Amazon Q CLI
# Install AWS CLI first if needed
brew install awscli  # or: pip install awscli
# Configure Amazon Q
aws configure  # Follow prompts
echo "✓ Amazon Q configured"

# 6. Create agent directories
mkdir -p ~/.config/multi-ai-agents/{researchers,processors,reviewers,support}
echo "✓ Agent directories created"

echo "Setup complete!"
```

### Agent Invocation Helper

```bash
#!/bin/bash
# agent - Universal agent invocation script
# Usage: agent <agent_name> <query>

AGENT_NAME="$1"
shift
QUERY="$*"

case "$AGENT_NAME" in
  researcher_classical)
    echo "🔍 Classical Research Agent (Gemini)..."
    gemini --system "$(cat ~/.config/multi-ai-agents/researchers/classical.md)" "$QUERY"
    ;;

  researcher_popular)
    echo "🔍 Popular Research Agent (Grok)..."
    grok-cli --system "$(cat ~/.config/multi-ai-agents/researchers/popular.md)" "$QUERY"
    ;;

  researcher_academic)
    echo "🔍 Academic Research Agent (Gemini)..."
    gemini --system "$(cat ~/.config/multi-ai-agents/researchers/academic.md)" "$QUERY"
    ;;

  agent_theoric)
    echo "🧠 Theoretical Agent (Claude)..."
    claude --agent theoric "$QUERY"
    ;;

  agent_developer)
    echo "👨‍💻 Developer Agent (GitHub Copilot)..."
    gh copilot suggest "$QUERY"
    ;;

  reviewer_brute)
    echo "⚠️ Brutal Review Agent (Grok)..."
    grok-cli --system "$(cat ~/.config/multi-ai-agents/reviewers/brute.md)" "$QUERY"
    ;;

  reviewer_technical)
    echo "📊 Technical Review Agent (Claude)..."
    claude --agent technical "$QUERY"
    ;;

  note_taker)
    echo "📝 Note Taker Agent (Amazon Q)..."
    aws q chat --message "$(cat ~/.config/multi-ai-agents/support/note_taker.md) $QUERY"
    ;;

  *)
    echo "Unknown agent: $AGENT_NAME"
    echo "Available agents:"
    echo "  researcher_classical, researcher_popular, researcher_academic"
    echo "  agent_theoric, agent_developer"
    echo "  reviewer_brute, reviewer_technical"
    echo "  note_taker"
    exit 1
    ;;
esac
```

Make it executable:
```bash
chmod +x ~/bin/agent
# Add ~/bin to PATH if not already there
export PATH="$HOME/bin:$PATH"
```

---

## Workflow Patterns

### Pattern 1: Comprehensive Research Workflow

```bash
# 1. Parallel research across all sources
agent researcher_classical "Kubernetes security best practices" > research/classical.md &
agent researcher_popular "Kubernetes security issues Reddit" > research/popular.md &
agent researcher_academic "Kubernetes security papers" > research/academic.md &
wait

# 2. Synthesize with theoretical agent (Claude - paid)
cat research/*.md | agent agent_theoric "Synthesize research into security framework" > analysis/theory.md

# 3. Create practical implementation (GitHub Copilot - free)
agent agent_developer "Implement Kubernetes security hardening script based on $(cat analysis/theory.md)" > implementation/k8s-hardening.sh

# 4. Dual review
agent reviewer_brute "Review $(cat implementation/k8s-hardening.sh)" > reviews/brute.md
agent reviewer_technical "Review $(cat implementation/k8s-hardening.sh)" > reviews/technical.md

# 5. Take notes (Amazon Q - free)
agent note_taker "Summarize progress on Kubernetes security project" > notes/session-$(date +%Y%m%d).md
```

### Pattern 2: Quick Development Cycle

```bash
# 1. Quick popular research only (Grok - free)
agent researcher_popular "Best logging library for Python 2024"

# 2. Implement directly (GitHub Copilot - free)
agent agent_developer "Create Python logging setup with rotation and structured output"

# 3. Brutal review for quick iteration (Grok - free)
agent reviewer_brute "Review this logging setup: $(cat logger.py)"

# 4. Log progress (Amazon Q - free)
agent note_taker "Completed logging setup, needs testing"
```

### Pattern 3: Academic Deep Dive

```bash
# 1. Academic research (Gemini - free)
agent researcher_academic "Transformer attention mechanisms papers" > research/papers.md

# 2. Theoretical analysis (Claude - paid, worth it for this)
agent agent_theoric "Analyze attention mechanisms and propose optimization" < research/papers.md > analysis/attention-theory.md

# 3. Implement (GitHub Copilot - free)
agent agent_developer "Implement optimized attention mechanism in PyTorch" < analysis/attention-theory.md > attention.py

# 4. Technical review (Claude - paid, critical for correctness)
agent reviewer_technical "Review attention mechanism implementation" < attention.py
```

### Pattern 4: Daily Standup Automation

```bash
#!/bin/bash
# daily-standup.sh

# Get yesterday's notes
YESTERDAY=$(date -d "yesterday" +%Y%m%d)
NOTES_FILE="notes/session-$YESTERDAY.md"

# Generate standup update
agent note_taker "Generate standup summary from: $(cat $NOTES_FILE)" > standup-$(date +%Y%m%d).md

# Display
cat standup-$(date +%Y%m%d).md
```

### Pattern 5: Multi-Perspective Decision Making

```bash
# Use case: Choosing between technologies (e.g., REST vs GraphQL)

# 1. Gather all perspectives
agent researcher_classical "REST vs GraphQL official recommendations" > decision/classical.md
agent researcher_popular "REST vs GraphQL developer experiences" > decision/popular.md
agent researcher_academic "REST vs GraphQL performance research" > decision/academic.md

# 2. Theoretical analysis (Claude - for critical decision)
cat decision/*.md | agent agent_theoric "Compare REST vs GraphQL with trade-offs" > decision/analysis.md

# 3. Get harsh reality check (Grok)
agent reviewer_brute "Challenge this analysis: $(cat decision/analysis.md)" > decision/critique.md

# 4. Final balanced view (Claude)
agent reviewer_technical "Final recommendation REST vs GraphQL for our use case" < decision/analysis.md > decision/final.md

# 5. Log decision
agent note_taker "Decided on [REST/GraphQL] because $(grep 'Verdict' decision/final.md)"
```

---

## Configuration Templates

### researcher_classical.md
```markdown
# Researcher Classical Agent

You are a technical research specialist focused on authoritative sources.

## Your Mission
Find and extract information ONLY from:
- Official documentation and specifications
- Renowned technical entities (W3C, IETF, IEEE, IANA)
- Authoritative books by recognized experts
- Technical standards and RFCs

## Research Strategy
1. Start with official docs (highest priority)
2. Cross-reference with technical standards
3. Verify against recognized authority statements
4. Ignore non-authoritative sources (blogs, tutorials, unless from core maintainers)

## Output Requirements
- Always cite sources with URLs
- Include version numbers for specs/docs
- Note any conflicts between sources
- Flag outdated information
- Provide technical depth over accessibility

## Quality Criteria
✅ From official source or renowned entity
✅ Technically precise and accurate
✅ Includes references and citations
❌ Blog posts (unless from core team)
❌ Unverified claims
❌ Overly simplified explanations
```

### researcher_popular.md
```markdown
# Researcher Popular Agent

You are a community insights specialist focused on real-world experiences.

## Your Mission
Extract practical wisdom from:
- Reddit (r/programming, r/webdev, r/devops, niche subreddits)
- X/Twitter (developer community, tech influencers)
- Fediverse (Mastodon, Lemmy tech instances)
- Forums (HackerNews, Lobsters, DevTo, StackOverflow comments)

## Research Strategy
1. Look for highly upvoted/engaged content
2. Identify common patterns in complaints/praise
3. Extract practical tips and gotchas
4. Note community sentiment and trends
5. Find real-world success/failure stories

## Output Requirements
- Include engagement metrics (upvotes, likes, comments)
- Quote actual users (anonymize if needed)
- Summarize common themes
- Highlight contrarian viewpoints
- Note recency of discussions

## Quality Criteria
✅ High community engagement
✅ Practical, experience-based insights
✅ Recent discussions (prefer <6 months old)
✅ Multiple perspectives represented
❌ Single-source opinions
❌ Marketing content disguised as advice
```

### researcher_academic.md
```markdown
# Researcher Academic Agent

You are an academic research specialist focused on scholarly publications.

## Your Mission
Find and analyze:
- Peer-reviewed papers (arXiv, ACM, IEEE, Springer)
- Top-tier conference proceedings (NeurIPS, ICML, CVPR, SIGMOD, etc.)
- Journal articles from respected publications
- Citation analysis and paper impact

## Research Strategy
1. Search academic databases (Google Scholar, arXiv, ACM DL, IEEE Xplore)
2. Prioritize top-tier conferences and journals (A*/A rank)
3. Check citation counts for impact assessment
4. Read abstracts, methodology, and conclusions
5. Note limitations and future work sections

## Output Requirements
- Full paper citations (Authors, Title, Venue, Year)
- Conference/journal ranking (A*/A/B/C)
- Citation count and h-index of authors if relevant
- Key contributions in your own words
- Methodology summary
- Results and their significance
- Noted limitations

## Quality Criteria
✅ Peer-reviewed publication
✅ Reputable venue (check CORE rankings)
✅ Recent (prefer <3 years unless seminal work)
✅ High citation count for field
❌ Preprints without peer review (flag if using)
❌ Predatory journals
```

### agent_theoric.md
```markdown
# Agent Theoric

You are a theoretical reasoning specialist. Synthesize research into logical frameworks.

## Your Mission
Transform raw research into:
- Theoretical models and frameworks
- Logical assumptions with confidence levels
- Pattern identification across sources
- Edge case reasoning
- Hypothesis generation

## Analysis Process
1. Read ALL provided research (classical, popular, academic)
2. Identify common themes and contradictions
3. Build conceptual models
4. Make logical inferences with stated assumptions
5. Reason about edge cases
6. Propose theoretical explanations

## Output Requirements
- Start with core concepts (what's fundamental)
- List assumptions explicitly with confidence levels
- Create or describe theoretical frameworks
- Reason about edge cases systematically
- Note open questions and uncertainties
- Connect concepts across research sources

## Reasoning Style
- Prefer "if X, then likely Y because Z" over definitive claims
- State confidence: High/Medium/Low
- Show your reasoning chain
- Acknowledge limitations
- Be intellectually honest about uncertainty

## Quality Criteria
✅ Logical consistency
✅ Grounded in provided research
✅ Explicit assumptions
✅ Considers edge cases
❌ Unsupported leaps
❌ Ignoring contradictory evidence
```

### agent_developer.md
```markdown
# Agent Developer

You are a practical implementation specialist. Turn concepts into working code.

## Your Mission
Create:
- Production-ready code implementations
- Step-by-step installation guides
- Usage examples with real scenarios
- Troubleshooting guides
- When-to-use and when-NOT-to-use guidance

## Implementation Standards
- Write clean, documented code
- Include error handling
- Provide multiple examples (simple → complex)
- Show actual command output
- Consider cross-platform compatibility
- Include security considerations

## Output Requirements
### Installation Section
- Prerequisites
- Step-by-step commands
- Verification steps

### Code Section
- Syntax-highlighted code blocks
- Inline comments for complex logic
- Configuration options explained

### Usage Examples
- Real-world scenarios (not toy examples)
- Expected output shown
- Common options/flags explained

### Best Practices
- When to use this approach
- When NOT to use (important!)
- Performance considerations
- Security notes

### Troubleshooting
- Common errors in table format
- Root causes
- Solutions

## Quality Criteria
✅ Code actually works (test mentally)
✅ Production-grade error handling
✅ Clear documentation
✅ Practical scenarios
❌ Toy examples only
❌ Missing error handling
❌ Uncommented complex code
```

### reviewer_brute.md
```markdown
# Reviewer Brute

You are the brutal truth teller. Your job is to find and highlight weaknesses mercilessly.

## Your Mission
Identify and articulate:
- Every weakness and flaw
- Security vulnerabilities
- Logical fallacies
- Unsupported assumptions
- Bad practices and anti-patterns
- What could break in production

## Review Style
- Be direct and unfiltered
- Focus on problems, not praise
- Challenge every assumption
- Play devil's advocate
- Ask "what could go wrong?"
- No sugar-coating

## Output Requirements
- List critical weaknesses first
- Explain WHY each is a problem
- State the potential impact
- Provide specific fixes (not vague advice)
- Categorize severity: Critical/High/Medium
- Flag "DO NOT SHIP" issues

## Review Checklist
Security:
- [ ] Input validation missing?
- [ ] Injection vulnerabilities?
- [ ] Auth/authz properly implemented?
- [ ] Secrets hardcoded?
- [ ] HTTPS enforced?

Reliability:
- [ ] Error handling missing?
- [ ] Edge cases unhandled?
- [ ] Race conditions possible?
- [ ] Memory leaks potential?

Code Quality:
- [ ] Overly complex?
- [ ] Untestable code?
- [ ] Tight coupling?
- [ ] Poor naming?

## Tone
Direct, harsh if needed, but professional. Think "tough mentor" not "toxic critic."

## Quality Criteria
✅ Specific, actionable criticism
✅ Explains impact of weaknesses
✅ Provides concrete fixes
❌ Vague complaints
❌ Nitpicking without substance
```

### reviewer_technical.md
```markdown
# Reviewer Technical

You are a balanced technical reviewer providing holistic analysis.

## Your Mission
Provide comprehensive assessment of:
- Strengths and what was done well
- Weaknesses and improvement areas
- Technical correctness
- Architecture and design quality
- Trade-offs evaluation
- Best practices alignment

## Review Approach
- Be balanced: acknowledge both good and bad
- Provide context for feedback
- Explain trade-offs
- Compare against industry standards
- Consider the use case
- Be nuanced, not absolute

## Output Requirements
### Executive Summary
High-level verdict and key points

### Strengths Section
- What was done well
- Why it's good
- Specific examples

### Weaknesses Section
- What needs improvement
- Why it matters
- Recommendations with priority

### Technical Analysis
- Architecture evaluation
- Code quality metrics
- Performance assessment
- Security review
- Scalability analysis

### Trade-offs Evaluation
Table of decisions with pros/cons

### Best Practices
What aligns, what violates

### Recommendations
Prioritized list (High/Medium/Low)

### Final Verdict
Nuanced conclusion with overall quality rating

## Review Framework
Consider:
1. Correctness (does it work?)
2. Quality (is it maintainable?)
3. Performance (is it efficient?)
4. Security (is it safe?)
5. Scalability (will it grow?)

## Tone
Professional, balanced, educational. Think "senior tech lead" providing mentorship.

## Quality Criteria
✅ Balanced (strengths + weaknesses)
✅ Context-aware feedback
✅ Prioritized recommendations
✅ Explains trade-offs
❌ One-sided assessment
❌ Ignoring context
❌ Absolute statements without nuance
```

### note_taker.md
```markdown
# Note Taker Agent

You are a project progress tracker. Keep notes concise and actionable.

## Your Mission
Maintain:
- Completed tasks log (with timestamps)
- Todo list (prioritized)
- Short reminders
- Blocker tracking
- Next steps
- Key decisions made

## Note-Taking Rules
1. **Be brief**: 1-2 lines per item MAX
2. **Be specific**: "Implemented JWT auth" not "did auth stuff"
3. **Include timestamps**: [HH:MM] for completed tasks
4. **Prioritize todos**: P0 (critical), P1 (important), P2 (nice-to-have)
5. **Track blockers**: What's blocked and what unblocks it
6. **Note decisions**: What was decided and why (briefly)

## Output Format
Use markdown with clear sections:
- ✅ Completed (past tense, timestamped)
- 📋 Todo (future tense, prioritized, assigned)
- 🔔 Reminders (imperative, actionable)
- 🚧 Blockers (what's stuck and why)
- ➡️ Next Steps (numbered, ordered)
- 💡 Decisions (key choices with brief rationale)

## Length Guidelines
- Completed task: 1 line max
- Todo item: 1 line max
- Reminder: 5-7 words max
- Blocker: 1 line max
- Next step: 1 line max
- Decision: 1-2 lines max

## Quality Criteria
✅ Concise (no fluff)
✅ Actionable (clear what to do)
✅ Specific (not vague)
✅ Timestamped (for completed items)
❌ Verbose descriptions
❌ Vague todos like "work on feature"
❌ Missing priorities

## Example Good Notes
✅ "15:30 Implemented JWT auth with RS256 (agent_developer)"
✅ "[ ] **P0** Fix CORS issue in API (Assigned: reviewer_technical)"
✅ "Decided: Use PostgreSQL over MongoDB (better ACID guarantees for financial data)"

## Example Bad Notes
❌ "Worked on authentication system and implemented various security measures..."
❌ "Todo: do some stuff with the database"
❌ "Reminder: don't forget about that thing"
```

---

## Usage Examples

### Example 1: Building a New Feature from Scratch

```bash
#!/bin/bash
# Feature: Real-time chat system

# Step 1: Research phase (all free agents)
echo "=== Research Phase ==="
agent researcher_classical "WebSocket protocols official spec" > research/websocket-spec.md
agent researcher_popular "WebSocket vs Server-Sent Events Reddit developer experiences" > research/community-insights.md
agent researcher_academic "Real-time messaging scalability papers" > research/academic-papers.md

# Step 2: Theoretical synthesis (Claude - paid, worth it)
echo "=== Analysis Phase ==="
cat research/*.md | agent agent_theoric "Design scalable real-time chat architecture" > analysis/chat-architecture.md

# Step 3: Implementation (GitHub Copilot - free)
echo "=== Development Phase ==="
agent agent_developer "Implement WebSocket chat server in Node.js based on: $(cat analysis/chat-architecture.md)" > src/chat-server.js

# Step 4: Dual review
echo "=== Review Phase ==="
agent reviewer_brute "Find all weaknesses: $(cat src/chat-server.js)" > reviews/brute-review.md
agent reviewer_technical "Comprehensive review: $(cat src/chat-server.js)" > reviews/technical-review.md

# Step 5: Iterate based on critical issues
CRITICAL_FIXES=$(grep "Critical\|DO NOT SHIP" reviews/brute-review.md)
if [ -n "$CRITICAL_FIXES" ]; then
  echo "=== Fixing Critical Issues ==="
  agent agent_developer "Fix these critical issues in chat-server.js: $CRITICAL_FIXES"
fi

# Step 6: Document progress
echo "=== Logging Progress ==="
agent note_taker "Session summary: Designed and implemented WebSocket chat server, fixed $(grep -c 'Critical' reviews/brute-review.md) critical issues"
```

### Example 2: Quick Bug Investigation

```bash
# Scenario: Production bug, need fast diagnosis

# 1. Quick community check (Grok - fast, free)
agent researcher_popular "Error: ECONNRESET Node.js production causes" > bug-insights.md

# 2. Implement fix (GitHub Copilot - free)
agent agent_developer "Create robust ECONNRESET handler for Node.js HTTP client" > fix.js

# 3. Brutal review to ensure no new issues (Grok - free)
agent reviewer_brute "Review this fix for edge cases: $(cat fix.js)"

# 4. Log for standup
agent note_taker "Fixed ECONNRESET in prod, added retry logic with exponential backoff"
```

### Example 3: Research-Heavy Decision

```bash
# Scenario: Choosing state management library for React app

# 1. Comprehensive research (all research agents in parallel)
agent researcher_classical "React state management official recommendations" > decision/official.md &
agent researcher_popular "Redux vs Zustand vs Jotai 2024 Reddit" > decision/community.md &
agent researcher_academic "State management performance benchmarks papers" > decision/academic.md &
wait

# 2. Theoretical analysis (Claude - critical decision)
cat decision/*.md | agent agent_theoric "Compare Redux, Zustand, Jotai for large-scale app" > decision/analysis.md

# 3. Challenge the analysis (Grok - free)
agent reviewer_brute "Challenge assumptions in: $(cat decision/analysis.md)" > decision/challenges.md

# 4. Final balanced view (Claude - paid)
cat decision/analysis.md decision/challenges.md | agent reviewer_technical "Final recommendation with trade-offs"

# 5. Document decision
agent note_taker "Decided: Zustand for state management (simpler API, good performance, smaller bundle)"
```

### Example 4: Daily Developer Workflow

```bash
#!/bin/bash
# morning-routine.sh

DATE=$(date +%Y%m%d)

# 1. Review yesterday's progress
agent note_taker "Summarize yesterday's session" < notes/session-$(date -d "yesterday" +%Y%m%d).md

# 2. Check todos
cat notes/session-*.md | grep "^- \[ \]" | head -5

# 3. Start today's session
agent note_taker "Starting session: Focus on $(cat notes/session-*.md | grep 'P0' | head -1)"

# ... do work ...

# Evening: Wrap up
agent note_taker "End of day summary: completed X, blocked on Y, tomorrow focus on Z" > notes/session-$DATE.md
```

---

## Cost Monitoring

### Monthly Cost Breakdown

```
Claude Pro: $20/month (fixed)
├─ agent_theoric (unlimited)
└─ reviewer_technical (unlimited)

Gemini: $0/month
├─ researcher_classical (1,000 req/day)
└─ researcher_academic (1,000 req/day)

Grok: $0/month (with API credit)
├─ researcher_popular (~100 req/day)
└─ reviewer_brute (~100 req/day)

GitHub Copilot: $0/month (free tier)
└─ agent_developer (reasonable free limits)

Amazon Q: $0/month (free tier)
└─ note_taker (minimal usage)

TOTAL: $20/month
```

### Usage Optimization Tips

1. **Batch Gemini requests**: Combine multiple queries to stay within 1K/day limit
2. **Use Grok strategically**: ~100/day limit means ~3-4 per hour, space them out
3. **Reserve Claude for critical paths**: Theoric analysis and technical reviews only
4. **GitHub Copilot for all code**: Free tier is generous, use it fully
5. **Amazon Q for quick notes**: Very light usage, won't hit limits

### If You Hit Free Tier Limits

**Gemini (1K/day)**:
- Unlikely to hit unless doing massive research
- If hit: Use Claude for urgent research (you're paying anyway)
- Or wait until next day (resets at midnight PST)

**Grok (~100/day)**:
- More likely to hit
- If hit: Use Gemini for popular research (less optimal but works)
- Or use Claude for brutal reviews (more polite but effective)

---

## Advanced Patterns

### Pattern: Automated Code Review Pipeline

```bash
#!/bin/bash
# cr-pipeline.sh - Automated code review for Git commits

COMMIT_SHA="$1"
CHANGED_FILES=$(git diff-tree --no-commit-id --name-only -r $COMMIT_SHA)

echo "Reviewing commit $COMMIT_SHA..."

# 1. Brutal pass (Grok - free, find obvious issues)
for file in $CHANGED_FILES; do
  git show $COMMIT_SHA:$file | agent reviewer_brute "Quick brutal review of $file"
done > reviews/brute-$COMMIT_SHA.md

# 2. If critical issues found, block and notify
CRITICAL=$(grep -c "Critical\|DO NOT SHIP" reviews/brute-$COMMIT_SHA.md)
if [ $CRITICAL -gt 0 ]; then
  echo "❌ $CRITICAL critical issues found. Blocking merge."
  agent note_taker "Blocked commit $COMMIT_SHA due to $CRITICAL critical issues"
  exit 1
fi

# 3. Deep technical review (Claude - paid, final check)
for file in $CHANGED_FILES; do
  git show $COMMIT_SHA:$file | agent reviewer_technical "Comprehensive review of $file"
done > reviews/technical-$COMMIT_SHA.md

# 4. Log review
agent note_taker "Reviewed commit $COMMIT_SHA: $(echo $CHANGED_FILES | wc -w) files, $CRITICAL critical issues"

echo "✅ Review complete: reviews/technical-$COMMIT_SHA.md"
```

### Pattern: Weekly Knowledge Synthesis

```bash
#!/bin/bash
# weekly-synthesis.sh

WEEK=$(date +%W)
OUTPUT="weekly-reports/week-$WEEK.md"

# 1. Gather all research from the week
cat research/*.md > /tmp/week-research.md

# 2. Synthesize learnings (Claude - worthwhile for weekly summary)
agent agent_theoric "Synthesize key learnings from this week's research" < /tmp/week-research.md > $OUTPUT

# 3. Add todos for next week
echo -e "\n## Next Week's Focus\n" >> $OUTPUT
cat notes/session-*.md | grep "^\- \[ \]" | sort | uniq >> $OUTPUT

# 4. Note completion
agent note_taker "Generated week $WEEK synthesis report"

echo "📊 Weekly report: $OUTPUT"
```

---

## Troubleshooting

### Agent Not Responding

```bash
# Check which client is having issues

# Gemini
gemini --version
gemini auth status

# Grok
grok-cli --version
grok-cli config check

# GitHub Copilot
gh copilot --version
gh auth status

# Amazon Q
aws sts get-caller-identity
```

### Rate Limit Hit

```bash
# Check usage for Gemini
gemini usage stats

# For Grok (manual tracking needed)
echo "Grok requests today: $(grep $(date +%Y-%m-%d) ~/.grok-cli/logs/*.log | wc -l)"

# Switch to backup agent temporarily
alias agent_researcher_popular_backup='agent researcher_classical'  # Use Gemini instead of Grok
```

### Output Format Issues

```bash
# If AI output is malformed, add explicit formatting instructions

# Bad
agent researcher_classical "WebSocket info"

# Good
agent researcher_classical "WebSocket info. Output as markdown with ## headers and bullet points."

# For JSON, validate
agent agent_developer "Generate config JSON" | jq empty && echo "Valid" || echo "Invalid"
```

---

## Maintenance

### Weekly Tasks
- [ ] Check free tier usage (Gemini, Grok)
- [ ] Review note_taker outputs for patterns
- [ ] Archive old research files (move to `archive/YYYY-MM/`)
- [ ] Update agent prompts if patterns change

### Monthly Tasks
- [ ] Review Claude usage (worth the $20?)
- [ ] Update agent configurations based on learnings
- [ ] Clean up old reviews and research files
- [ ] Generate monthly synthesis report

### Quarterly Tasks
- [ ] Evaluate new AI clients (free tiers, capabilities)
- [ ] Reassess agent-to-client mappings
- [ ] Update cost-benefit analysis
- [ ] Refine agent prompts based on experience

---

**Version:** 1.0
**Last Updated:** 2025-11-09
**Maintained by:** MultiAI-Terminal Project

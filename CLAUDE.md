# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Status

This repository is a multi-agent AI framework that was previously developed with agents for different AI clients. The working directories are currently empty following the most recent commit. The git history contains comprehensive documentation of the architecture.

## Git Workflow

- Current branch: `context-offensive-security`
- This branch contains the cleaned state of the repository
- Previous commits contain full agent implementations and framework documentation

## Exploring Historical Context

To understand the full architecture and agent implementations, refer to git history:

```bash
# View the agents framework documentation from the initial commit
git show c94b7c3:MultiAI-Terminal/agents_architecture.md

# View product agents and configurations
git show 6c2aa0d:ProductMap/.claude/agents/

# View output formatting styles
git show f32faa3:MultiAI-Terminal/output-styles/
```

## Key Git Commits

- `c94b7c3`: Initial agents framework with multi-AI architecture
- `0dd9847`: Removed external AI tool references from agent docs
- `f32faa3`: Added response formatting system with 6 style templates
- `6c2aa0d`: Added product agents and specific configurations
- `9e8ee43`: Initial cleanup commit (current state)

## Architecture Overview

The repository was structured as a multi-agent system leveraging different AI clients based on their strengths:

### Agent Categories
- **Research Agents**: Information gathering (classical, popular, academic sources)
- **Processing Agents**: Theoretical reasoning and code implementation
- **Quality Assurance**: Brutal and technical code review
- **Project Management**: Progress tracking and todo management

### Component Organization
- `MultiAI-Terminal/`: Core framework with generic and Claude-specific agents
- `ProductMap/`: Specialized agents for product-related tasks (P2P buyers, product specialists, etc.)
- `output-styles/`: Response formatting templates for different communication styles

## Common Development Commands

Since the repository directories are currently empty, development typically involved:

1. **Managing agent definitions**: Editing markdown files in `agents/` directories
2. **Adding new output styles**: Creating markdown templates in `output-styles/`
3. **Version control**: Using conventional commit format (feat, docs, fix)
4. **Reviewing changes**: Using git show to inspect agent configurations

## Conventional Commit Format

The repository follows conventional commits:
- `feat(scope): description` - New features
- `docs(scope): description` - Documentation changes
- `fix(scope): description` - Bug fixes

## Pentester Philosophy

See `.claude/agents/pentester-philosopher.md` for the core philosophy governing all procedures in this project.

**Core Principle:** Every command is a choice. Every technique has a theory. Every attack serves a purpose. Document the why before the how.

When writing procedures, always consider:
1. **Methodology** - What strategic approach? (deception, signaling, etc.)
2. **Tactics** - Noise profile? (noiseless, alarming, quick, slow)
3. **Trends** - Is this technique current in 2024+?
4. **Theory-to-Practice** - What do we learn from this? (CTF/lab value)
5. **Sources** - Academic, professional, community, operational intelligence

---

## Cyber-Security Workflow Documentation Pattern

The `/knowyourenemy/` directory contains procedural reference guides for security assessments using the chess kingdom metaphor. These guides follow a specific pattern:

### Reference Guide Best Practices

1. **Helper Functions First**: Define reusable bash functions at the top of reference docs
   - Extract common patterns into functions (e.g., `crt()`, `rapiddns()`, `resolve()`)
   - Source functions once, use throughout
   - Reduces repetition and keeps guide clean

2. **Command-Focused Structure**:
   - Lead with executable commands and one-liners
   - Minimal explanatory text (just enough to understand context)
   - Include practical tool usage with real syntax
   - Reference external resources instead of copying documentation

3. **Code Examples - Minimal & Crucial Only**:
   - Include Python/complex logic ONLY when it's core to the procedure
   - Use external resources/links for detailed documentation
   - Keep code examples focused (e.g., `fav_hash()` function for favicon hashing)
   - Avoid polluting with verbose explanations

4. **Workflow Sections**:
   - Organize by phase (Survey → Map → Enumerate → etc.)
   - Use metaphorical titles (kingdom, territories, fortifications)
   - Include manual checks with source links when automation unavailable
   - End with quick workflow pipelines combining multiple steps

5. **Example Reference Files**:
   - `knowyourenemy/KingsAssets.md` - External infrastructure reconnaissance workflow
     - Covers: ASN mapping, domain enumeration, reverse lookups, secret discovery
     - Phase: Pre-breach reconnaissance from outside network
   - `knowyourenemy/FortressInfrastructure.md` - Internal network reconnaissance workflow
     - Covers: Active host discovery, port scanning, traffic sniffing
     - Phase: Post-breach internal network mapping from within
   - `knowyourenemy/RumorsAndLeaks.md` - Leaked credentials and exposed intelligence workflow
     - Covers: Email enumeration, breach database lookups, GitHub credential scanning
     - Phase: Zero-click access via previously leaked/compromised accounts
   - All three use helper functions + tool-specific commands
   - All three include complete quick-start workflows at end

### File Organization

- `knowyourenemy/` - "Know Your Enemy" reconnaissance phases
- `attackplan/` - Attack planning and strategy
- `library/` - Reference materials and tools
- `weaponization/` - Exploitation and payload techniques
- `whispering/` - Lateral movement and persistence

## Notes for Future Development

If restoring or extending this framework:

1. Review the agents_architecture.md for complete system design
2. Check MultiAI-Terminal/ for generic agent patterns
3. Reference ProductMap/ for specialized domain implementations
4. Use output-styles/ as templates for response formatting
5. Follow the established pattern of using AI client strengths for specific agent roles
6. When creating new procedural guides, follow the KingsAssets.md pattern:
   - Start with helper functions
   - Focus on commands over explanations
   - Use external resources for detailed docs
   - End with practical quick-start workflows
7. Always apply **pentester-philosopher** lens:
   - Document methodology and tactical choices
   - Note current trends and maturity level
   - Reference academic/professional sources
   - Include learning value (CTF/lab applicability)
   - Explain noise profile (stealth vs. aggressive options)
- whenever you're using the @agent-devops-commit-message you should do it as fr4mer3d github user
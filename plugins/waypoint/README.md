# Claude Code Waypoint Plugin

[![Version](https://img.shields.io/badge/version-1.0.0-blue)](https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace)
[![License](https://img.shields.io/badge/license-MIT-green)](../../LICENSE)
[![Plugin](https://img.shields.io/badge/marketplace-dojo--claude--plugins-purple)](https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace)

**Never lose context again.** Strategic waypoints for long-running AI projects with persistent memory and intentional context management.

---

## The Problem

Working with AI on complex projects means:
- ❌ **Context resets** force you to start from scratch
- ❌ **Lost decisions** - no record of why choices were made
- ❌ **Forgotten progress** - can't remember what's done
- ❌ **Broken continuity** - every session feels like day one
- ❌ **Manual repetition** - explaining the same things over and over

## The Solution

Claude Code Waypoint Plugin creates **strategic checkpoints** throughout your project journey:

- ✅ **Resume in < 60 seconds** after any context reset
- ✅ **Persistent memory** across sessions, days, weeks
- ✅ **Auto-activating skills** provide the right context automatically
- ✅ **Decision logging** captures the "why" behind choices
- ✅ **Works beyond code** - planning, research, any long-running work

**Waypoints are navigation markers** that help you track progress, return to known points, and resume quickly from exactly where you left off.

---

## 🚀 Quick Start

### Installation

Install from the Dojo Claude Plugins marketplace:

```bash
# Add the marketplace (one time)
/plugin marketplace add DojoCodingLabs/dojo-claude-code-plugin-marketplace

# Install Waypoint plugin
/plugin install waypoint@dojo-claude-code-plugin-marketplace

# Restart Claude Code
```

Verify installation:
```bash
/help  # Should show /create-plan, /update-context, /resume
```

### Your First Waypoint

```bash
# Create a waypoint for a feature
/create-plan auth-implementation

# Work on your feature...
# Waypoint tracks context automatically

# Before taking a break
/update-context

# Resume later (even after context reset)
/resume auth-implementation
# → Back to work in < 60 seconds
```

[→ Detailed Installation Guide](INSTALLATION.md)

---

## 🎯 Core Features

### 1. Persistent Memory System (PRIMARY)

The **3-file waypoint structure** captures everything:

```
plans/active/auth-implementation/
├── auth-implementation-plan.md      # Strategy (rarely changes)
├── auth-implementation-context.md   # Current state (updated frequently)
└── auth-implementation-tasks.md     # Actionable checklist (daily updates)
```

**What gets tracked:**
- **Decisions** - Why you chose specific approaches
- **Progress** - What's done, in progress, and pending
- **Blockers** - What's preventing progress
- **Next actions** - Specific file + function to work on
- **Key discoveries** - Important findings during work

**SESSION PROGRESS tracking** enables the < 60 second resume:

```markdown
## SESSION PROGRESS (2025-01-15 14:30)

### ✅ COMPLETED
- [x] Supabase Auth configured
- [x] Created auth utilities

### 🟡 IN PROGRESS
- [ ] Implementing token refresh (CURRENT)

### 🎯 NEXT ACTION
Implement refreshToken() in lib/supabase/auth.ts
```

Even after complete context reset, Claude reads this and knows exactly where you are.

[→ Learn the waypoint pattern](WAYPOINT_PATTERN.md)

### 2. Auto-Activation System (CRITICAL)

**Skills activate themselves based on your context** - no manual invocation needed.

**How it works:**
1. You type a prompt or edit a file
2. Hooks analyze keywords, file paths, and code content
3. Relevant skills auto-suggest
4. You get perfect context automatically

**Example triggers:**
- Edit file in `plans/` → context-persistence skill activates
- Keywords "memory", "remember" → memory-management suggests
- Keywords "waypoint", "resume" → appropriate guidance appears

The result: **Right knowledge at the right time**, automatically.

**Status:** CRITICAL until Claude Code improves built-in activation (then becomes optional).

[→ Auto-activation details](.claude/hooks/AUTO_ACTIVATION.md)

### 3. Modular Building Blocks

Mix and match components:

**Skills** (6 included):
- `memory-management` - Intentional memory & preference learning
- `context-persistence` - Waypoint pattern implementation
- `plan-approval` - User control over complex changes
- `frontend-dev-guidelines` - Next.js + React 19 scaffold (example)
- `backend-dev-guidelines` - Supabase + Edge Functions scaffold (example)
- `skill-developer` - Meta-skill for creating skills

**Agents** (6 included):
- `scaffold-customizer` - Adapt scaffolds for your tech stack
- `documentation-architect` - Generate comprehensive docs
- `code-architecture-reviewer` - Review code for consistency
- `refactor-planner` - Plan refactoring strategies
- `plan-reviewer` - Review plans before execution
- `web-research-specialist` - Research technical issues

**Commands:**
- `/create-plan [name]` - Create new waypoint (3-file structure)
- `/update-context` - Update before context reset
- `/resume [name]` - Resume from waypoint (< 60 seconds)

[→ Complete component catalog](#component-catalog)

---

## 💡 Why "Waypoint"?

In navigation, **waypoints mark your path** on a long journey:
- Track progress across complex routes
- Return to known positions after detours
- Navigate systematically with strategic markers
- Resume travel from exact locations

This plugin brings waypoints to AI-assisted work:
- **Track progress** on long-running projects
- **Return to known states** after interruptions
- **Navigate complexity** with strategic checkpoints
- **Resume quickly** from exactly where you stopped

AI assistance that feels **continuous and intentional**, not fragmented and forgetful.

---

## 🎬 Use Cases

### Software Development
```bash
/create-plan stripe-integration

# Tracks:
- Implementation approach and architecture
- Files created/modified
- Integration points with existing code
- Test coverage decisions
- Migration plan for existing users

# Resume seamlessly after lunch, next day, or context reset
```

### Business Process Design
```bash
/create-plan customer-onboarding-redesign

# Tracks:
- Current process mapping
- Pain point identification
- New workflow design decisions
- Stakeholder feedback
- Implementation phases

# Maintain continuity across multiple planning sessions
```

### Research Projects
```bash
/create-plan market-analysis-saas-tools

# Tracks:
- Research sources and findings
- Key insights and patterns
- Competitive analysis notes
- Synthesis of information
- Next research directions

# Build knowledge progressively, never lose discoveries
```

### Content Creation
```bash
/create-plan technical-article-series

# Tracks:
- Article outline and structure
- Research notes and sources
- Draft progress
- Editor feedback
- Publishing checklist

# Maintain flow across writing sessions
```

**Not just for coding** - Any complex work that spans multiple sessions benefits from waypoints.

---

## 📚 Component Catalog

### Core Skills (Memory & Context)

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| **memory-management** | Track preferences, decisions, corrections | Building persistent knowledge |
| **context-persistence** | 3-file waypoint pattern | Long-running features/projects |
| **plan-approval** | User control over AI actions | Complex/risky changes |

These skills **work with any tech stack** - they're about process, not technology.

### Scaffold Skills (Examples)

| Skill | Tech Stack | Purpose |
|-------|------------|---------|
| **frontend-dev-guidelines** | Next.js 14+, React 19, shadcn/ui | Example scaffold demonstrating patterns |
| **backend-dev-guidelines** | Supabase, Edge Functions, TypeScript | Example scaffold demonstrating patterns |
| **skill-developer** | N/A | Meta-skill for creating skills |

**Note:** Scaffold skills are **examples** showing how to use the plugin. Customize them for your stack using the `scaffold-customizer` agent.

### Agents

| Agent | Purpose | Customization |
|-------|---------|---------------|
| **scaffold-customizer** | Adapt skills to your tech stack | None needed |
| **documentation-architect** | Generate comprehensive docs | None needed |
| **code-architecture-reviewer** | Review for consistency | None needed |
| **refactor-planner** | Create refactoring strategies | None needed |
| **plan-reviewer** | Review plans before execution | None needed |
| **web-research-specialist** | Research technical issues | None needed |

---

## 🔧 Customization

### Adapt for Your Tech Stack

The scaffold skills come configured for specific stacks, but the core memory/context system **works with anything**.

Use the `scaffold-customizer` agent:
```bash
# It will:
1. Detect your tech stack
2. Customize skill triggers and patterns
3. Update examples to match your project
4. Verify changes work
```

[→ Full customization guide](CUSTOMIZATION.md)

### Create Your Own Skills

```bash
# Add new skill
.claude/skills/my-skill/SKILL.md

# Configure triggers in skill-rules.json
{
  "my-skill": {
    "type": "domain",
    "promptTriggers": {
      "keywords": ["my", "custom", "triggers"]
    }
  }
}
```

[→ Skill development guide](.claude/skills/skill-developer/SKILL.md)

---

## 📖 Documentation

### Getting Started
- **[INSTALLATION.md](INSTALLATION.md)** - Complete setup guide
- **[WAYPOINT_PATTERN.md](WAYPOINT_PATTERN.md)** - Deep dive into 3-file structure
- **[PHILOSOPHY.md](PHILOSOPHY.md)** - Why intentional memory matters

### Advanced
- **[WAYPOINT_INTEGRATION_GUIDE.md](WAYPOINT_INTEGRATION_GUIDE.md)** - Integration walkthrough
- **[CUSTOMIZATION.md](CUSTOMIZATION.md)** - Adapt to your stack
- **[AUTO_ACTIVATION.md](.claude/hooks/AUTO_ACTIVATION.md)** - Hook system details

### Contributing
- **[CONTRIBUTING.md](CONTRIBUTING.md)** - How to contribute
- **[CHANGELOG.md](CHANGELOG.md)** - Version history

---

## 🌟 Credits & Origin

This plugin packages the incredible work shared by **[diet103](https://github.com/diet103)** in their repository **[claude-code-infrastructure-showcase](https://github.com/diet103/claude-code-infrastructure-showcase)**.

Their infrastructure was born from **6 months of intensive Claude Code use** managing complex production systems:

- 50,000+ lines of TypeScript
- Microservices architecture
- Sophisticated workflow engine
- Real production challenges, not toy examples

After sharing lessons learned in the viral Reddit post **["Claude Code is a Beast – Tips from 6 Months of Hardcore Use"](https://www.reddit.com/r/ClaudeAI/comments/1oivjvm/claude_code_is_a_beast_tips_from_6_months_of/)**, the community asked for an easier way to use this infrastructure.

**This plugin makes that possible** - packaging the proven patterns into a Claude Code plugin marketplace for easy installation and distribution.

### The Core Innovation

An automatic hook-based system where skills activate themselves based on context, combined with a waypoint pattern (originally called "dev docs") that survives context resets.

**The result:** AI assistance that feels **continuous and intentional**, not fragmented and forgetful.

### What We Did

Building on this foundation, we added our own twist and improvements:

- **Introduced the "Waypoint" metaphor** - Renamed "dev docs" to "waypoints" with navigation-focused terminology
- **Transformed into proper Claude Code plugin** - Full marketplace structure following Anthropic best practices
- **Enhanced documentation** - Created comprehensive guides (WAYPOINT_PATTERN.md, AUTO_ACTIVATION.md, CUSTOMIZATION.md)
- **Improved organization** - Restructured as `plans/` directory with clearer purpose
- **Added plugin manifests** - Official plugin.json and marketplace.json for native installation
- **Positioned correctly** - Memory/context management as PRIMARY, auto-activation as CRITICAL enabler
- **Expanded use cases** - Emphasized applicability beyond coding (planning, research, content)
- **Made it accessible** - Two-command installation via Claude Code plugin system

**Core credit** goes to the original author for the innovative infrastructure patterns. We packaged, refined, and positioned it for broader community adoption.

📦 **Original Repository:** [claude-code-infrastructure-showcase](https://github.com/diet103/claude-code-infrastructure-showcase)
💬 **Original Reddit Post:** ["Claude Code is a Beast"](https://www.reddit.com/r/ClaudeAI/comments/1oivjvm/claude_code_is_a_beast_tips_from_6_months_of/)

---

## 🤝 Contributing

Contributions welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Development Setup

```bash
# Clone marketplace
git clone https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace
cd dojo-claude-code-plugin-marketplace

# Test waypoint plugin
cp -r plugins/waypoint/.claude/* /path/to/test/project/.claude/

# Or use plugin installation for testing
/plugin marketplace add file:///path/to/dojo-claude-code-plugin-marketplace
/plugin install waypoint@dojo-claude-code-plugin-marketplace
```

---

## 📄 License

MIT License - See [LICENSE](../../LICENSE) for details.

---

## 💬 Support

- **Issues:** [GitHub Issues](https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace/issues)
- **Discussions:** [GitHub Discussions](https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace/discussions)
- **Marketplace:** [Dojo Claude Plugins](https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace)

---

<p align="center">
  <strong>Claude Code Waypoint Plugin</strong><br>
  AI assistance that remembers, learns, and never loses its way.
</p>

<p align="center">
  <a href="#-quick-start">Get Started</a> •
  <a href="WAYPOINT_PATTERN.md">Learn the Pattern</a> •
  <a href="CUSTOMIZATION.md">Customize</a> •
  <a href="CONTRIBUTING.md">Contribute</a>
</p>

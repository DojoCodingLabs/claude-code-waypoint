# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Important: Claude Code Waypoint Plugin for Context Management

This repository is the **Claude Code Waypoint Plugin** - a system for **intentional context management and persistent memory** in Claude Code projects.

**Primary Purpose**: Create strategic waypoints (checkpoints) throughout project work so you never lose context and can resume in < 60 seconds after resets.

**Secondary Purpose**: Auto-activation hooks that suggest relevant skills based on context (critical until Claude improves built-in activation).

**This is a Plugin**: Not a working application. It's a Claude Code plugin that provides fundamental building blocks for AI-assisted project planning and execution. Users install it via `/plugin install waypoint`.

## Development Commands

### Setting Up Hooks
```bash
cd .claude/hooks && npm install && chmod +x *.sh
```

### Validating Configuration
```bash
# Validate skill-rules.json syntax
cat .claude/skills/skill-rules.json | jq .

# Check hooks are executable
ls -la .claude/hooks/*.sh | grep rwx
```

### Testing Hook Integration (Manual)
```bash
# Test skill activation logic
echo '{"prompt":"create backend route"}' | cd .claude/hooks && npx tsx skill-activation-prompt.ts
```

## High-Level Architecture

Claude Code Waypoint Plugin implements a **three-pillar system**:

### 1. Skills (.claude/skills/) - MEMORY & CONTEXT (PRIMARY)

Knowledge bases that provide context-aware guidance:

**Core Skills** (Memory & Context):
- **memory-management** - Intentional memory tracking and preference learning
- **context-persistence** - Waypoint pattern implementation (3-file structure)
- **plan-approval** - User control workflows for complex changes

**Scaffold Skills** (Examples):
- **frontend-dev-guidelines** - Next.js + React 19 + shadcn/ui patterns (example)
- **backend-dev-guidelines** - Supabase + Edge Functions patterns (example)
- **skill-developer** - Meta-skill for creating new skills

**How they work**:
- Each follows the **500-line rule**: Main SKILL.md (<500 lines) + modular resource files
- Auto-activate based on keywords, file paths, or code content (via hooks)
- Progressive disclosure: load main file first, resources only when needed

### 2. Hooks (.claude/hooks/) - AUTO-ACTIVATION (CRITICAL)

Event-driven scripts that run at specific workflow moments:
- **UserPromptSubmit**: Analyzes prompts and auto-suggests relevant skills
- **PostToolUse**: Tracks file changes for context management
- **Stop**: Optional code quality checks (TypeScript, formatting)
- Technology: Bash wrappers + TypeScript logic with Node.js dependencies

**Status**: CRITICAL until Claude Code improves built-in activation.

### 3. Agents (.claude/agents/) - SPECIALIZED WORKFLOWS

Standalone specialized agents for complex tasks:
- **scaffold-customizer** - Customize skills for your tech stack
- **documentation-architect** - Generate comprehensive documentation
- **code-architecture-reviewer** - Review code for architectural consistency
- **refactor-planner** - Create refactoring strategies
- **plan-reviewer** - Review development plans before execution
- **web-research-specialist** - Research technical issues online

Copy-and-use - no configuration required.

**Plus:**
- **Commands** (.claude/commands/): 3 slash commands for waypoint management
- **Waypoint Pattern** (plans/): Methodology for surviving context resets

## The Waypoint Pattern (PRIMARY INNOVATION)

The core value of Claude Code Waypoint Plugin is **intentional context management**:

```
Waypoint Structure:
plans/active/task-name/
├── task-name-plan.md      # Strategy (rarely changes)
├── task-name-context.md   # Current state (updated frequently)
└── task-name-tasks.md     # Actionable checklist (updated daily)
```

**The Critical SESSION PROGRESS Section**:
Located in `context.md`, this section enables < 60 second resume after context resets:

```markdown
## SESSION PROGRESS (2025-01-15 14:30)

### ✅ COMPLETED
- [x] Task 1 done
- [x] Task 2 done

### 🟡 IN PROGRESS
- [ ] Task 3 (CURRENT)

### 🎯 NEXT ACTION
Specific next step here
```

**Why this works:**
- Context resets no longer mean starting from scratch
- All decisions and context preserved in waypoint files
- Resume command loads context and continues seamlessly
- Works for coding AND general system building (planning, research, etc.)

## Auto-Activation System (CRITICAL FEATURE)

The hook-based skill activation system:

```
skill-rules.json (configuration)
    ↓
UserPromptSubmit hook (analyzes prompts + active files)
    ↓
Auto-suggests relevant skills
    ↓
Claude receives perfect context automatically
```

**How it works:**
1. User types a prompt or edits a file
2. `skill-activation-prompt.ts` reads `skill-rules.json`
3. Matches prompt keywords, intent patterns, file paths, or code content
4. Injects skill suggestions into the prompt
5. Claude activates with the right context, no manual intervention

**Example triggers:**
- Edit file in `plans/` → context-persistence activates
- Keywords "memory", "remember" → memory-management suggests
- Keywords "create component" → frontend-dev-guidelines suggests (if using that scaffold)

**Status**: CRITICAL until Claude Code improves built-in skill activation.

## Progressive Disclosure Pattern (500-Line Rule)

Large skills hit Claude's context limits. Claude Code Waypoint Plugin solves it with modular structure:

```
skill-name/
  SKILL.md                 # <500 lines, high-level overview + navigation
  resources/
    topic-1.md            # <500 lines each, deep dive
    topic-2.md
    topic-3.md
```

**Benefits:**
- Main file loads first (quick context)
- Resource files load only when needed
- Each file stays under 500 lines
- Claude navigates progressively

**Example:** backend-dev-guidelines:
- Main SKILL.md: 304 lines
- 12 resource files: routing, controllers, services, repositories, database, middleware, validation, error handling, testing, async patterns, dependency injection, migration from legacy patterns

## Repository Structure

```
claude-code-waypoint/
├── plugins/
│   └── waypoint/
│       ├── .claude-plugin/
│       │   └── plugin.json                  # Plugin manifest
│       │
│       ├── .claude/
│       │   ├── agents/                      # 6 specialized agents (copy-and-use)
│       │   │   ├── code-architecture-reviewer.md
│       │   │   ├── documentation-architect.md
│       │   │   ├── plan-reviewer.md
│       │   │   ├── refactor-planner.md
│       │   │   ├── scaffold-customizer.md
│       │   │   └── web-research-specialist.md
│       │   │
│       │   ├── commands/                    # 3 slash commands
│       │   │   ├── create-plan.md           # Create waypoint (3-file structure)
│       │   │   ├── update-context.md        # Update waypoint before context reset
│       │   │   └── resume.md                # Resume from existing waypoint
│       │   │
│       │   ├── hooks/                       # Event-driven automation
│       │   │   ├── skill-activation-prompt.sh/ts  # CRITICAL: Auto-suggest skills
│       │   │   ├── post-tool-use-tracker.sh       # CRITICAL: Track file changes
│       │   │   ├── package.json                   # Node dependencies
│       │   │   ├── CONFIG.md                      # Configuration guide
│       │   │   ├── AUTO_ACTIVATION.md             # Auto-activation documentation
│       │   │   └── README.md
│       │   │
│       │   ├── skills/                      # Knowledge bases
│       │   │   ├── memory-management/       # CORE: Intentional memory patterns
│       │   │   ├── context-persistence/     # CORE: Waypoint pattern implementation
│       │   │   ├── plan-approval/           # CORE: User control workflows
│       │   │   ├── frontend-dev-guidelines/ # EXAMPLE: Next.js + React 19 scaffold
│       │   │   ├── backend-dev-guidelines/  # EXAMPLE: Supabase + Edge Functions scaffold
│       │   │   ├── skill-developer/         # META: How to create skills
│       │   │   ├── skill-rules.json         # ⚠️ CORE CONFIG: Trigger definitions
│       │   │   └── README.md
│       │   │
│       │   └── settings.json                # ⚠️ Hook configuration (contains EXAMPLES)
│       │
│       ├── plans/                           # Waypoint pattern examples
│       │   └── README.md                    # Methodology: plan/context/tasks structure
│       │
│       ├── README.md                        # Project overview & quick start
│       ├── WAYPOINT_INTEGRATION_GUIDE.md    # AI-assisted setup guide
│       ├── WAYPOINT_PATTERN.md              # Deep dive into 3-file pattern
│       ├── PHILOSOPHY.md                    # Why intentional memory matters
│       ├── CUSTOMIZATION.md                 # Adapting to your tech stack
│       ├── INSTALLATION.md                  # Plugin installation instructions
│       ├── CONTRIBUTING.md                  # Contributing guidelines
│       ├── LICENSE                          # MIT License
│       └── .gitignore
```

## Plugin Installation Workflow

From [README.md](README.md#quick-start):

### Installation
1. **Install Claude Code Waypoint Plugin**:
   ```
   /plugin install waypoint
   ```

2. **Or install from marketplace**:
   ```
   /plugin marketplace add DojoCodingLabs/claude-code-waypoint
   /plugin install waypoint@claude-code-waypoint
   ```

3. **Restart Claude Code**

4. **Verify installation**: `/help` should show /create-plan, /update-context, /resume commands

5. **Create first waypoint**: `/create-plan my-task`

## Key Documentation Files

### [README.md](README.md)
- Project overview and positioning
- Waypoint metaphor explanation
- Core features (memory PRIMARY, auto-activation CRITICAL)
- Quick start and installation
- Usage examples (coding AND non-coding)
- Component catalog

### [WAYPOINT_PATTERN.md](WAYPOINT_PATTERN.md)
**Purpose:** Deep dive into the 3-file waypoint structure
- plan.md, context.md, tasks.md structure
- SESSION PROGRESS tracking methodology
- When to update waypoints
- How to resume from waypoints
- Best practices for context persistence

### [WAYPOINT_INTEGRATION_GUIDE.md](WAYPOINT_INTEGRATION_GUIDE.md)
**Purpose:** Guide for Claude (AI) to integrate plugin into user projects
- Understanding waypoints first
- Memory-first setup (core skills)
- Auto-activation setup (hooks + skill-rules.json)
- Waypoint management commands
- Customization for different projects

### [PHILOSOPHY.md](PHILOSOPHY.md)
**Purpose:** Why intentional memory matters
- The waypoint metaphor
- Fundamental building blocks for AI projects
- Auto-activation as enabler
- Not just for coding - general system building

### [plans/README.md](plans/README.md)
**Purpose:** Waypoint pattern methodology
- Problem: Context resets lose project knowledge
- Solution: 3-file structure (plan.md, context.md, tasks.md)
- Integration with /create-plan, /update-context, /resume commands
- Best practices for persistence across sessions

## Critical Warnings

### 1. Example Paths MUST Be Customized
The `skill-rules.json` and `settings.json` files contain **example paths**:
- Example project paths
- Example file patterns
- Example service names

**YOU MUST** update these to match your actual project structure before use. Don't blindly copy.

### 2. Tech Stack Dependencies (Scaffold Skills)

The scaffold skills (frontend-dev-guidelines, backend-dev-guidelines) are **examples** demonstrating how to use Claude Code Waypoint Plugin with specific tech stacks:

| Scaffold Skill | Tech Stack | Note |
|----------------|------------|------|
| frontend-dev-guidelines | Next.js 14+, React 19, shadcn/ui | Example only - customize for your stack |
| backend-dev-guidelines | Supabase, Edge Functions, TypeScript | Example only - customize for your stack |

**The core memory/context system works with ANY tech stack.** Use `scaffold-customizer` agent to adapt examples.

### 3. settings.json Contains Examples
The provided `settings.json` references:
- **Example hook configurations**
- **Example MCP servers** that may not exist in your project
- **Example Stop hooks** that require customization

**Extract only UserPromptSubmit and PostToolUse hooks for safety.** Add others after customization.

### 4. Stop Hooks Require Heavy Customization
- Optional quality gate hooks need project-specific configuration
- Can block the Stop event if misconfigured
- Test manually before adding to settings.json
- Not required for core Claude Code Waypoint Plugin functionality

### 5. This is a Plugin, Not an Application
- No package.json at root
- No source code to run/build/test
- Install via `/plugin install waypoint`
- Don't try to `npm install` or `npm run build` at the root

## Skills Summary

### Core Skills (Memory & Context)

| Skill | Purpose | Type |
|-------|---------|------|
| **memory-management** | Intentional memory tracking, preference learning, decision logging | Core |
| **context-persistence** | Waypoint pattern implementation (3-file structure) | Core |
| **plan-approval** | User control workflows for complex changes | Core |

These are the **foundation** of Claude Code Waypoint Plugin. They work with any tech stack.

### Scaffold Skills (Examples)

| Skill | Tech Stack | Purpose |
|-------|------------|---------|
| **frontend-dev-guidelines** | Next.js + React 19 + shadcn/ui | Example scaffold demonstrating memory patterns |
| **backend-dev-guidelines** | Supabase + Edge Functions | Example scaffold demonstrating memory patterns |
| **skill-developer** | N/A | Meta-skill for creating new skills |

These are **examples** showing how to use Claude Code Waypoint Plugin. Customize for your stack using `scaffold-customizer` agent.

### Activation Triggers

Core skills activate based on context:

| Skill | Keywords | File Paths | Content Patterns |
|-------|----------|------------|------------------|
| memory-management | "remember", "preference", "decision" | `.claude/memory/**/*` | Memory-related operations |
| context-persistence | "waypoint", "resume", "context reset" | `plans/**/*` | Waypoint file operations |
| plan-approval | "plan", "approve", "review plan" | Any complex operation | Plan-approval workflows |

## Components Quick Reference

### Agents (Copy-and-Use)

| Agent | Purpose | Customization Required |
|-------|---------|------------------------|
| scaffold-customizer | Customize scaffold skills for your tech stack | None |
| documentation-architect | Generate comprehensive docs | None |
| code-architecture-reviewer | Review code for architectural consistency | None |
| refactor-planner | Create refactoring strategies | None |
| plan-reviewer | Review development plans before execution | None |
| web-research-specialist | Research technical issues online | None |

### Hooks (Event-Driven)

| Hook | Event | Essential? | Customization |
|------|-------|-----------|---------------|
| skill-activation-prompt | UserPromptSubmit | ✅ CRITICAL | None - fully generic |
| post-tool-use-tracker | PostToolUse | ✅ CRITICAL | None - auto-detects |

### Slash Commands (Waypoint Management)

| Command | Purpose |
|---------|---------|
| /create-plan [task-name] | Create new waypoint (3-file structure) |
| /update-context | Update waypoint before context reset |
| /resume [task-name] | Resume from existing waypoint (< 60 seconds) |

## skill-rules.json Structure

The configuration for auto-activation. For each skill, define:

```json
{
  "skill-name": {
    "type": "domain" | "guardrail",
    "enforcement": "suggest" | "block" | "warn",
    "priority": "critical" | "high" | "medium" | "low",
    "promptTriggers": {
      "keywords": ["explicit", "words"],
      "intentPatterns": ["(regex|for).*?user.*?intent"]
    },
    "fileTriggers": {
      "pathPatterns": ["glob/patterns/**/*.ts"],
      "contentPatterns": ["regex.*?in.*?file.*?content"]
    },
    "skipConditions": {
      "sessionTracking": true,
      "fileMarkers": ["// @skip-validation"],
      "envVars": ["SKIP_SKILL_NAME"]
    }
  }
}
```

**Trigger Types:**
1. **Keywords**: Match exact words in prompts ("waypoint", "memory", "remember")
2. **Intent Patterns**: Regex for user intentions (e.g., `"(create|update|resume).*?plan"`)
3. **Path Patterns**: File globs (e.g., `"plans/**/*"`, `.claude/memory/**/*`)
4. **Content Patterns**: Regex in file content (e.g., `"SESSION PROGRESS"`)

**Enforcement Levels:**
- `"suggest"`: Non-blocking suggestion (default, used by memory skills)
- `"block"`: Can prevent tool execution (use for guardrails)
- `"warn"`: Show warning but don't block

**Skip Conditions:**
- `sessionTracking: true`: Don't suggest same skill twice in one session
- `fileMarkers`: Comment patterns to skip
- `envVars`: Environment variables to disable skill

## What This Solves

### Before Claude Code Waypoint Plugin:
- Context resets mean starting from scratch → lose all progress
- No persistent memory → repeat same explanations
- Skills don't activate automatically → must remember which to use
- No navigation markers → get lost in complex work
- Manual workflows → friction and mistakes

### After Claude Code Waypoint Plugin:
- Waypoint pattern preserves context → resume in < 60 seconds
- Intentional memory persists → preferences and decisions remembered
- Skills auto-activate → right context at right time
- Strategic checkpoints → always know where you are
- Automated workflows → smooth, consistent execution

**Result**: AI assistance that feels continuous and intentional, not fragmented and forgetful.

## Getting Help

For detailed setup and usage:
1. Read [README.md](README.md) for overview and installation
2. Read [WAYPOINT_PATTERN.md](WAYPOINT_PATTERN.md) for 3-file structure details
3. Use [WAYPOINT_INTEGRATION_GUIDE.md](WAYPOINT_INTEGRATION_GUIDE.md) with Claude for guided setup
4. Check [PHILOSOPHY.md](PHILOSOPHY.md) for why this approach works
5. See [plans/README.md](plans/README.md) for waypoint methodology

For troubleshooting auto-activation:
- Verify hooks are executable: `ls -la .claude/hooks/*.sh | grep rwx`
- Validate JSON syntax: `cat .claude/skills/skill-rules.json | jq .`
- Check hook output: Logs appear in Claude Code interface when hooks run
- Test manually: `echo '{"prompt":"test"}' | cd .claude/hooks && npx tsx skill-activation-prompt.ts`

For customizing scaffolds:
- Use the `scaffold-customizer` agent
- It will detect your tech stack and adapt the scaffold skills
- See [CUSTOMIZATION.md](CUSTOMIZATION.md) for details

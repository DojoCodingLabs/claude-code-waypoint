# Claude Code Waypoint Plugin - Integration Guide

**FOR CLAUDE CODE:** When a user asks you to integrate components from this showcase repository into their project, follow these instructions carefully.

---

## Overview

This repository is a **reference library** of Claude Code Waypoint Plugin infrastructure components. Users will ask you to help integrate specific pieces into their projects. Your role is to:

1. **Understand waypoint pattern** - Memory/context management system
2. **Ask clarifying questions** about their project structure
3. **Copy the appropriate files**
4. **Customize configurations** for their setup
5. **Verify the integration** works correctly

**Key Principle:** ALWAYS ask before assuming project structure. What works for one project won't work for another.

---

## Understanding Waypoints: Memory-First Architecture

### What Are Waypoints?

Waypoints are persistent memory anchors that survive Claude Code context resets. They're stored in the `plans/` directory and follow a three-file structure:

1. **`plan.md`** - Strategic plan (rarely changes)
2. **`context.md`** - Current state with SESSION PROGRESS tracking
3. **`tasks.md`** - Actionable checklist

### Why Waypoints Matter

**Traditional workflow:**
- Explain context → Work → Context reset → Re-explain → Repeat
- 20-30% of time wasted on re-explaining

**With Waypoints:**
- Explain once → Memory persists → Resume instantly
- Work survives session resets automatically
- AI learns from corrections over time

### The SESSION PROGRESS Format

The heart of waypoint functionality:

```markdown
## SESSION PROGRESS (2025-01-15 14:30)

### ✅ COMPLETED
[Specific completed tasks with file names]

### 🟡 IN PROGRESS
[Exact file and function being worked on]

### ⏳ PENDING
[What's coming next]

### 🎯 NEXT ACTION
[VERY specific: file + function + action]
```

This format enables:
- **Instant context recovery**: Read NEXT ACTION and resume immediately
- **Progress visibility**: See what's done, what's being worked on
- **No ambiguity**: Exact state persisted at end of session

---

## Memory Setup

### 1. Create Waypoint Directories

```bash
# Create plans directories for waypoint storage
mkdir -p plans/active
mkdir -p plans/archived
mkdir -p plans/templates

# Create memory storage for preferences and decisions
mkdir -p .claude/memory/project
mkdir -p .claude/memory/preferences
```

### 2. Initialize Git Ignore

Add to your `.gitignore`:

```gitignore
# Claude Code Memory (may contain sensitive context)
.claude/memory/**/*.json

# But DO track skills, agents, and commands
!.claude/skills/
!.claude/agents/
!.claude/commands/
```

This ensures:
- Memory stays private and local
- Plans stay committed for team visibility
- No accidental credential leaks

### 3. Understand Memory Philosophy

**In Memory** (`.claude/memory/`):
- User preferences ("always use X pattern")
- Decision rationale (the "why")
- Correction patterns (learned preferences)
- Project-specific knowledge

**In Plans** (`plans/`):
- Strategic decisions (plan.md)
- Work progress (context.md)
- Task breakdown (tasks.md)

**In Code/Config**:
- Linting rules, type definitions, constants
- Architecture (visible in file structure)

**Rule**: If it can be in code, put it in code. Memory captures what code can't.

---

## Tech Stack Compatibility Check

**CRITICAL:** Before integrating a skill, verify the user's tech stack matches the skill requirements.

### Frontend Skills

**frontend-dev-guidelines requires:**
- React (18+)
- MUI v7
- TanStack Query
- TanStack Router
- TypeScript

**Before integrating, ask:**
"Do you use React with MUI v7?"

**If NO:**
```
The frontend-dev-guidelines skill is designed specifically for React + MUI v7. I can:
1. Help you create a similar skill adapted for [their stack] using this as a template
2. Extract the framework-agnostic patterns (file organization, performance, etc.)
3. Skip this skill if not relevant

Which would you prefer?
```

### Backend Skills

**backend-dev-guidelines requires:**
- Node.js/Express
- TypeScript
- Prisma ORM
- Sentry

**Before integrating, ask:**
"Do you use Node.js with Express and Prisma?"

**If NO:**
```
The backend-dev-guidelines skill is designed for Express/Prisma. I can:
1. Help you create similar guidelines adapted for [their stack] using this as a template
2. Extract the architecture patterns (layered architecture works for any framework)
3. Skip this skill

Which would you prefer?
```

### Skills That Are Tech-Agnostic

These work for ANY tech stack:
- ✅ **skill-developer** - Meta-skill, no tech requirements
- ✅ **route-tester** - Only requires JWT cookie auth (framework agnostic)
- ✅ **error-tracking** - Sentry works with most stacks

---

## General Integration Pattern

When user says: **"Add [component] to my project"**

1. Identify component type (skill/hook/agent/command)
2. **CHECK TECH STACK COMPATIBILITY** (for frontend/backend skills)
3. Ask about their project structure
4. Copy files OR adapt for their stack
5. Customize for their setup
6. Verify integration
7. Provide next steps

---

## Integrating Skills

### Step-by-Step Process

**When user requests a skill** (e.g., "add backend-dev-guidelines"):

#### 1. Understand Their Project

**ASK THESE QUESTIONS:**
- "What's your project structure? Single app, monorepo, or multi-service?"
- "Where is your [backend/frontend] code located?"
- "What frameworks/technologies do you use?"

#### 2. Copy the Skill

```bash
cp -r /path/to/showcase/.claude/skills/[skill-name] \
      $CLAUDE_PROJECT_DIR/.claude/skills/
```

#### 3. Handle skill-rules.json

**Check if it exists:**
```bash
ls $CLAUDE_PROJECT_DIR/.claude/skills/skill-rules.json
```

**If NO (doesn't exist):**
- Copy the template from showcase
- Remove skills user doesn't want
- Customize for their project

**If YES (exists):**
- Read their current skill-rules.json
- Add the new skill entry
- Merge carefully to avoid breaking existing skills

#### 4. Customize Path Patterns

**CRITICAL:** Update `pathPatterns` in skill-rules.json to match THEIR structure:

**Example - User has monorepo:**
```json
{
  "backend-dev-guidelines": {
    "fileTriggers": {
      "pathPatterns": [
        "packages/api/src/**/*.ts",
        "packages/server/src/**/*.ts",
        "apps/backend/**/*.ts"
      ]
    }
  }
}
```

**Example - User has single backend:**
```json
{
  "backend-dev-guidelines": {
    "fileTriggers": {
      "pathPatterns": [
        "src/**/*.ts",
        "backend/**/*.ts"
      ]
    }
  }
}
```

**Safe Generic Patterns** (when unsure):
```json
{
  "pathPatterns": [
    "**/*.ts",          // All TypeScript files
    "src/**/*.ts",      // Common src directory
    "backend/**/*.ts"   // Common backend directory
  ]
}
```

#### 5. Verify Integration

```bash
# Check skill was copied
ls -la $CLAUDE_PROJECT_DIR/.claude/skills/[skill-name]

# Validate skill-rules.json syntax
cat $CLAUDE_PROJECT_DIR/.claude/skills/skill-rules.json | jq .
```

**Tell user:** "Try editing a file in [their-backend-path] and the skill should activate."

---

### Skill-Specific Notes

#### backend-dev-guidelines
- **Tech Requirements:** Node.js/Express, Prisma, TypeScript, Sentry
- **Ask:** "Do you use Express with Prisma?" "Where's your backend code?"
- **If different stack:** Offer to adapt using this as template
- **Customize:** pathPatterns
- **Example paths:** `api/`, `server/`, `backend/`, `services/*/src/`
- **Adaptation tip:** Architecture patterns (Routes→Controllers→Services) transfer to most frameworks

#### frontend-dev-guidelines
- **Tech Requirements:** React 18+, MUI v7, TanStack Query/Router, TypeScript
- **Ask:** "Do you use React with MUI v7?" "Where's your frontend code?"
- **If different stack:** Offer to create adapted version (Vue, Angular, etc.)
- **Customize:** pathPatterns + all framework-specific examples
- **Example paths:** `frontend/`, `client/`, `web/`, `apps/web/src/`
- **Adaptation tip:** File organization and performance patterns transfer, component code doesn't

#### route-tester
- **Tech Requirements:** JWT cookie-based authentication (framework agnostic)
- **Ask:** "Do you use JWT cookie-based authentication?"
- **If NO:** "This skill is designed for JWT cookies. Want me to adapt it for [their auth type] or skip it?"
- **Customize:** Service URLs, auth patterns
- **Works with:** Any backend framework using JWT cookies

#### error-tracking
- **Tech Requirements:** Sentry (works with most backends)
- **Ask:** "Do you use Sentry?" "Where's your backend code?"
- **If NO Sentry:** "Want to use this as template for [their error tracking]?"
- **Customize:** pathPatterns
- **Adaptation tip:** Error tracking philosophy transfers to other tools (Rollbar, Bugsnag, etc.)

#### skill-developer
- **Tech Requirements:** None!
- **Copy as-is** - meta-skill, fully generic, teaches skill creation for ANY tech stack

---

## Adapting Skills for Different Tech Stacks

When user's tech stack differs from skill requirements, you have options:

### Option 1: Adapt Existing Skill (Recommended)

**When to use:** User wants similar guidelines but for different tech

**Process:**
1. **Copy the skill as a starting point:**
   ```bash
   cp -r showcase/.claude/skills/frontend-dev-guidelines \
         $CLAUDE_PROJECT_DIR/.claude/skills/vue-dev-guidelines
   ```

2. **Identify what needs changing:**
   - Framework-specific code examples (React → Vue)
   - Library APIs (MUI → Vuetify/PrimeVue)
   - Import statements
   - Component patterns

3. **Keep what transfers:**
   - File organization principles
   - Performance optimization strategies
   - TypeScript standards
   - General best practices

4. **Replace examples systematically:**
   - Ask user for equivalent patterns in their stack
   - Update code examples to their framework
   - Keep the overall structure and sections

5. **Update skill name and triggers:**
   - Rename skill appropriately
   - Update skill-rules.json triggers for their stack
   - Test activation

**Example - Adapting frontend-dev-guidelines for Vue:**
```
I'll create vue-dev-guidelines based on the React skill structure:
- Replace React.FC → Vue defineComponent
- Replace useSuspenseQuery → Vue composables
- Replace MUI components → [their component library]
- Keep: File organization, performance patterns, TypeScript guidelines

This will take a few minutes. Sound good?
```

### Option 2: Extract Framework-Agnostic Patterns

**When to use:** Stacks are very different, but core principles apply

**Process:**
1. Read through the existing skill
2. Identify framework-agnostic patterns:
   - Layered architecture (backend)
   - File organization strategies
   - Performance optimization principles
   - Testing strategies
   - Error handling philosophy

3. Create new skill with just those patterns
4. User can add framework-specific examples later

**Example:**
```
The backend-dev-guidelines uses Express, but the layered architecture
(Routes → Controllers → Services → Repositories) works for Django too.

I can create a skill with:
- Layered architecture pattern
- Separation of concerns principles
- Error handling best practices
- Testing strategies

Then you can add Django-specific examples as you establish patterns.
```

### Option 3: Use as Reference Only

**When to use:** Too different to adapt, but user wants inspiration

**Process:**
1. User browses the existing skill
2. You help create a new skill from scratch
3. Use existing skill's structure as a template
4. Follow modular pattern (main + resource files)

### What Usually Transfers Across Tech Stacks

**Architecture & Organization:**
- ✅ Layered architecture (Routes/Controllers/Services pattern)
- ✅ Separation of concerns
- ✅ File organization strategies (features/ pattern)
- ✅ Progressive disclosure (main + resource files)
- ✅ Repository pattern for data access

**Development Practices:**
- ✅ Error handling philosophy
- ✅ Input validation importance
- ✅ Testing strategies
- ✅ Performance optimization principles
- ✅ TypeScript best practices

**Framework-Specific Code:**
- ❌ React hooks → Don't transfer to Vue/Angular
- ❌ MUI components → Different component libraries
- ❌ Prisma queries → Different ORM syntax
- ❌ Express middleware → Different framework patterns
- ❌ Routing implementations → Framework-specific

### When to Recommend Adaptation vs Skipping

**Recommend adaptation if:**
- User wants similar guidelines for their stack
- Core patterns apply (layered architecture, etc.)
- User has time to help with framework-specific examples

**Recommend skipping if:**
- Stacks are completely different
- User doesn't need those patterns
- Would take too long to adapt
- User prefers creating from scratch

---

## Integrating Hooks

### Essential Hooks (Always Safe to Copy)

#### skill-activation-prompt (UserPromptSubmit)

**Purpose:** Auto-suggests skills based on user prompts

**Integration (NO customization needed):**

```bash
# Copy both files
cp showcase/.claude/hooks/skill-activation-prompt.sh \
   $CLAUDE_PROJECT_DIR/.claude/hooks/
cp showcase/.claude/hooks/skill-activation-prompt.ts \
   $CLAUDE_PROJECT_DIR/.claude/hooks/

# Make executable
chmod +x $CLAUDE_PROJECT_DIR/.claude/hooks/skill-activation-prompt.sh

# Install dependencies if needed
if [ -f "showcase/.claude/hooks/package.json" ]; then
  cp showcase/.claude/hooks/package.json \
     $CLAUDE_PROJECT_DIR/.claude/hooks/
  cd $CLAUDE_PROJECT_DIR/.claude/hooks && npm install
fi
```

**Add to settings.json:**
```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/skill-activation-prompt.sh"
          }
        ]
      }
    ]
  }
}
```

**This hook is FULLY GENERIC** - works anywhere, no customization needed!

#### post-tool-use-tracker (PostToolUse)

**Purpose:** Tracks file changes for waypoint context management

**Integration (NO customization needed):**

```bash
# Copy file
cp showcase/.claude/hooks/post-tool-use-tracker.sh \
   $CLAUDE_PROJECT_DIR/.claude/hooks/

# Make executable
chmod +x $CLAUDE_PROJECT_DIR/.claude/hooks/post-tool-use-tracker.sh
```

**Add to settings.json:**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|MultiEdit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/post-tool-use-tracker.sh"
          }
        ]
      }
    ]
  }
}
```

**This hook is FULLY GENERIC** - auto-detects project structure!

---

### Optional Hooks (Require Heavy Customization)

#### tsc-check.sh and trigger-build-resolver.sh (Stop hooks)

⚠️ **WARNING:** These hooks are configured for a specific multi-service monorepo structure.

**Before integrating, ask:**
1. "Do you have a monorepo with multiple TypeScript services?"
2. "What are your service directory names?"
3. "Where are your tsconfig.json files located?"

**For SIMPLE projects (single service):**
- **RECOMMEND SKIPPING** these hooks
- They're overkill for single-service projects
- User can run `tsc --noEmit` manually instead

**For COMPLEX projects (multi-service monorepo):**

1. Copy the files
2. **MUST EDIT** tsc-check.sh - find this section:
```bash
case "$repo" in
    email|exports|form|frontend|projects|uploads|users|utilities|events|database)
        echo "$repo"
        return 0
        ;;
esac
```

3. Replace with USER'S actual service names:
```bash
case "$repo" in
    api|web|auth|payments|notifications)  # ← User's services
        echo "$repo"
        return 0
        ;;
esac
```

4. Test manually before adding to settings.json:
```bash
./.claude/hooks/tsc-check.sh
```

**IMPORTANT:** If this hook fails, it will block Stop events. Only add if you're sure it works for their setup.

---

## Integrating Agents

**Agents are STANDALONE** - easiest to integrate!

### Standard Agent Integration

```bash
# Copy the agent file
cp showcase/.claude/agents/[agent-name].md \
   $CLAUDE_PROJECT_DIR/.claude/agents/
```

**That's it!** Agents work immediately, no configuration needed.

### Check for Hardcoded Paths

Some agents may reference paths. **Before copying, read the agent file and check for:**

- `~/git/old-project/` → Should be `$CLAUDE_PROJECT_DIR` or `.`
- `/root/git/project/` → Should be `$CLAUDE_PROJECT_DIR` or `.`
- Hardcoded screenshot paths → Ask user where they want screenshots

**If found, update them:**
```bash
sed -i 's|~/git/old-project/|.|g' $CLAUDE_PROJECT_DIR/.claude/agents/[agent].md
sed -i 's|/root/git/.*PROJECT.*DIR|$CLAUDE_PROJECT_DIR|g' \
    $CLAUDE_PROJECT_DIR/.claude/agents/[agent].md
```

### Agent-Specific Notes

**auth-route-tester / auth-route-debugger:**
- Requires JWT cookie-based authentication in user's project
- Ask: "Do you use JWT cookies for auth?"
- If NO: "These agents are for JWT cookie auth. Skip them or want me to adapt?"

**frontend-error-fixer:**
- May reference screenshot paths
- Ask: "Where should screenshots be saved?"

**All other agents:**
- Copy as-is, they're fully generic

---

## Integrating Slash Commands

```bash
# Copy command file
cp showcase/.claude/commands/[command].md \
   $CLAUDE_PROJECT_DIR/.claude/commands/
```

### Customize Paths

Commands may reference waypoint paths. **Check and update:**

**create-plan and update-context:**
- Look for `plans/` path references
- Ask: "Where do you want waypoint plans stored?"
- Update paths in the command files to match your `plans/` location

**resume:**
- References waypoint structure
- Usually needs no changes

---

## Common Patterns & Best Practices

### Pattern: Asking About Project Structure

**DON'T assume:**
- ❌ "I'll add this for your blog-api service"
- ❌ "Configuring for your frontend directory"

**DO ask:**
- ✅ "What's your project structure? Monorepo or single app?"
- ✅ "Where is your backend code located?"
- ✅ "Do you use workspaces or have multiple services?"

### Pattern: Customizing skill-rules.json

**User has monorepo with workspaces:**
```json
{
  "pathPatterns": [
    "packages/*/src/**/*.ts",
    "apps/*/src/**/*.tsx"
  ]
}
```

**User has Nx monorepo:**
```json
{
  "pathPatterns": [
    "apps/api/src/**/*.ts",
    "libs/*/src/**/*.ts"
  ]
}
```

**User has simple structure:**
```json
{
  "pathPatterns": [
    "src/**/*.ts",
    "backend/**/*.ts"
  ]
}
```

### Pattern: settings.json Integration

**NEVER copy the showcase settings.json directly!**

Instead, **extract and merge** the sections they need:

1. Read their existing settings.json
2. Add the hook configurations they want
3. Preserve their existing config

**Example merge:**
```json
{
  // ... their existing config ...
  "hooks": {
    // ... their existing hooks ...
    "UserPromptSubmit": [  // ← Add this section
      {
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/skill-activation-prompt.sh"
          }
        ]
      }
    ]
  }
}
```

---

## Verification Checklist

After integration, **verify these items:**

```bash
# 1. Hooks are executable
ls -la $CLAUDE_PROJECT_DIR/.claude/hooks/*.sh
# Should show: -rwxr-xr-x

# 2. skill-rules.json is valid JSON
cat $CLAUDE_PROJECT_DIR/.claude/skills/skill-rules.json | jq .
# Should parse without errors

# 3. Hook dependencies installed (if TypeScript hooks)
ls $CLAUDE_PROJECT_DIR/.claude/hooks/node_modules/
# Should show packages if package.json exists

# 4. Settings.json is valid JSON
cat $CLAUDE_PROJECT_DIR/.claude/settings.json | jq .
# Should parse without errors

# 5. Plans directory created
ls -la $CLAUDE_PROJECT_DIR/plans/
# Should show active, archived, templates directories
```

**Then ask user to test:**
- "Try editing a file in [relevant-path] - the skill should activate"
- "Try asking me a question about [topic] - I should suggest the skill"
- "Try running `/create-plan [test-task]` - waypoint files should be created"

---

## Common Mistakes to Avoid

### ❌ DON'T: Copy settings.json as-is
**Why:** The Stop hooks reference non-existent services
**DO:** Extract only UserPromptSubmit and PostToolUse sections

### ❌ DON'T: Keep example service names
**Why:** User doesn't have blog-api, auth-service, etc.
**DO:** Ask about their actual structure and update

### ❌ DON'T: Skip making hooks executable
**Why:** Hooks won't run without execute permission
**DO:** Always `chmod +x` after copying

### ❌ DON'T: Assume monorepo structure
**Why:** Most projects are single-service
**DO:** Ask first, then customize

### ❌ DON'T: Add all skills at once
**Why:** Overwhelming and may not all be relevant
**DO:** Ask which skills are relevant to their work

### ❌ DON'T: Copy Stop hooks without testing
**Why:** They can block the Stop event if they fail
**DO:** Test manually first, only add if working

### ❌ DON'T: Forget to create plans/ directory
**Why:** Waypoint commands won't work
**DO:** Create plans/, plans/active, plans/archived, plans/templates

### ❌ DON'T: Skip memory setup
**Why:** Memory/preferences won't persist
**DO:** Create .claude/memory/ directories and add to .gitignore

---

## Example Integration Conversations

### Example 1: Adding Backend Skill

**User:** "Add the backend-dev-guidelines skill to my project"

**Claude should respond:**
```
I'll add the backend-dev-guidelines skill. First, I need to understand
your project structure:

1. Is this a monorepo with multiple services, or a single backend application?
2. Where is your backend code located? (e.g., src/, backend/, server/)
3. Do you use Express.js?
```

**User:** "Single app, code is in src/api/, yes we use Express"

**Claude should:**
1. Copy the skill directory
2. Update skill-rules.json with pathPattern: `"src/api/**/*.ts"`
3. Verify it's copied correctly
4. Respond:
```
✅ Added backend-dev-guidelines skill!

Configuration:
- Skill triggers when editing files in src/api/
- Activated by keywords like "controller", "service", "route", "API"

Try it: Edit a file in src/api/ and you should see the skill suggestion.
```

### Example 2: Setting Up Waypoint System

**User:** "Set up waypoint plans for my project"

**Claude should respond:**
```
I'll set up the Claude Code Waypoint Plugin system for you. This includes:
1. Creating plans/ directory structure
2. Setting up skill activation hooks
3. Initializing memory storage
4. Creating example waypoint files

This enables context persistence so your work survives session resets.

Do you have a .claude/settings.json file already, or should I create one?
```

**User:** "I have settings.json already"

**Claude should:**
1. Create plans/, plans/active, plans/archived, plans/templates directories
2. Create .claude/memory/ directories
3. Copy both hook files
4. Run npm install in hooks directory
5. Add hook configurations to their settings.json (preserving existing config)
6. Make hooks executable
7. Respond:
```
✅ Waypoint system installed!

What I did:
- Created plans/ directory structure for waypoint storage
- Created .claude/memory/ for preferences and decisions
- Copied skill-activation-prompt hook
- Copied post-tool-use-tracker hook
- Installed dependencies
- Added to your settings.json

How waypoints work:
- Run `/create-plan [task-name]` to create a new waypoint
- Work on the task, updating context.md with SESSION PROGRESS
- When context resets, run `/resume [task-name]` to pick up instantly

Try it: Run `/create-plan setup-test` and you'll see three files created.
```

### Example 3: Understanding Memory Management

**User:** "How do I store my development preferences?"

**Claude should:**
```
The Claude Code Waypoint Plugin has two memory systems:

1. WAYPOINT PLANS (in plans/ directory):
   - Created by `/create-plan` command
   - Three files: plan.md (strategy), context.md (progress), tasks.md (checklist)
   - Survives context resets
   - Good for: Work that spans multiple sessions

2. PREFERENCES & DECISIONS (in .claude/memory/):
   - Stored as JSON, never committed
   - Good for: Coding preferences, architectural decisions, patterns
   - Example: "always use named imports", "prefer async/await over promises"
   - Example: "database uses Prisma", "frontend uses React 19"

I can store preferences automatically when you tell me:
"Always use [pattern]" → I'll remember and apply it
"Why did we choose [decision]?" → I'll explain the rationale
```

### Example 4: Tech Stack Mismatch

**User:** "Add the frontend-dev-guidelines skill to my Vue project"

**Claude should:**
```
I see you're using Vue. The frontend-dev-guidelines skill is specifically
designed for React + MUI v7. I have a few options:

Option 1 (Recommended): I can create a vue-dev-guidelines skill using the
React skill as a template. I'll adapt:
- Component patterns for Vue (Composition API, defineComponent)
- Replace MUI examples with your component library
- Adapt data fetching for Vue patterns
- Keep: File organization, performance principles, TypeScript standards

Option 2: Extract only the framework-agnostic patterns (file organization,
performance, testing) without React-specific code.

Option 3: Skip it and help you create Vue guidelines from scratch.

Which would you prefer?
```

**User:** "Option 1 sounds good, I use Vuetify"

**Claude should:**
1. Copy frontend-dev-guidelines as vue-dev-guidelines
2. Go through each resource file
3. Replace React patterns with Vue Composition API
4. Replace MUI components with Vuetify
5. Update skill-rules.json with Vue-specific triggers (.vue files)
6. Respond:
```
✅ Created vue-dev-guidelines skill adapted for Vue 3 + Vuetify!

What I changed:
- React.FC components → Vue defineComponent
- MUI v7 examples → Vuetify components
- TanStack Query → Your data fetching approach
- File patterns → .vue files

What I kept:
- File organization (features/ pattern)
- Performance optimization strategies
- TypeScript standards
- Loading/error state principles

Try editing a .vue file - the skill should activate.
```

---

## Quick Reference Tables

### What Needs Customization?

| Component | Tech Requirements | Customization | What to Ask |
|-----------|------------------|--------------|-------------|
| **skill-developer** | None | ✅ None | Copy as-is |
| **backend-dev-guidelines** | Express/Prisma/Node | ⚠️ Paths + tech check | "Use Express/Prisma?" "Where's backend?" |
| **frontend-dev-guidelines** | React/MUI v7 | ⚠️⚠️ Paths + framework | "Use React/MUI v7?" "Where's frontend?" |
| **route-tester** | JWT cookies | ⚠️ Auth + paths | "JWT cookie auth?" |
| **error-tracking** | Sentry | ⚠️ Paths | "Use Sentry?" "Where's backend?" |
| **skill-activation-prompt** | ✅ None | Copy as-is |
| **post-tool-use-tracker** | ✅ None | Copy as-is |
| **tsc-check** | ⚠️⚠️⚠️ Heavy | "Monorepo or single service?" |
| **All agents** | ✅ Minimal | Check paths |
| **All commands** | ⚠️ Paths | "Where for waypoint plans?" |

### When to Recommend Skipping

| Component | Skip If... |
|-----------|-----------|
| **tsc-check hooks** | Single-service project or different build setup |
| **route-tester** | Not using JWT cookie authentication |
| **frontend-dev-guidelines** | Not using React + MUI |
| **auth agents** | Not using JWT cookie auth |

---

## Final Tips for Claude

**When user says "add everything":**
- Start with essentials: skill-activation hooks + 1-2 relevant skills
- Set up waypoint system (plans/ directory + /create-plan command)
- Don't overwhelm them with all 5 skills + 10 agents
- Ask what they actually need

**When something doesn't work:**
- Check verification checklist
- Verify paths match their structure
- Test hooks manually
- Check for JSON syntax errors
- Verify plans/ directory exists

**When user is unsure:**
- Recommend starting with just waypoint system
- Add skill-activation hooks second
- Add backend OR frontend skill (whichever they use)
- Add more later as needed

**Always explain what you're doing:**
- Show the commands you're running
- Explain why you're asking questions
- Provide clear next steps after integration
- Show how waypoints enable context persistence

---

**Remember:** This is a reference library, not a working application. Your job is to help users cherry-pick and adapt components for THEIR specific project structure, with special focus on enabling memory management through the waypoint system.

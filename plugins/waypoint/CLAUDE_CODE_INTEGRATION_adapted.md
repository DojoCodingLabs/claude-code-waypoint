# Claude Code Integration Guide - VibeCoding Bootcamp

**Complete documentation of the Skills + Hooks + Commands system**

**Last Updated:** 2025-11-03
**Version:** 2.0

---

## Table of Contents

1. [Overview](#overview)
2. [Three-Pillar System](#three-pillar-system)
3. [Skills System](#skills-system)
4. [Hooks System](#hooks-system)
5. [Commands System](#commands-system)
6. [Integration Process](#integration-process)
7. [Practical Workflows](#practical-workflows)
8. [Troubleshooting](#troubleshooting)
9. [Maintenance & Updates](#maintenance--updates)

---

## Overview

### What is Claude Code Integration?

Claude Code integration transforms how you work with AI by providing **context-aware assistance** that automatically activates based on what you're working on. Instead of explaining your project every time, Claude knows your patterns, conventions, and best practices.

### Benefits for VibeCoding Bootcamp

✅ **Consistency** - Enforces Supabase, Stripe, and Resend patterns across all backend code
✅ **Speed** - Auto-suggests relevant skills based on file context
✅ **Quality** - Prevents common mistakes with guardrails (when needed)
✅ **Onboarding** - New developers get instant guidance
✅ **Documentation** - Living documentation that activates when needed

### How It Works

```
User edits file → Hook detects context → Skill auto-activates → Claude has full context
```

---

## Three-Pillar System

### 1. Skills 📚
**What:** Knowledge bases with best practices and patterns
**When:** Auto-activate based on keywords, file paths, or code content
**Example:** Editing `supabase/functions/stripe-webhook/index.ts` → backend-dev-guidelines activates

### 2. Hooks 🪝
**What:** Scripts that run at specific points in Claude's workflow
**When:** UserPromptSubmit, PreToolUse, PostToolUse, Stop events
**Example:** PostToolUse hook tracks which files you've edited

### 3. Commands ⚡
**What:** Slash commands for repetitive tasks
**When:** You type `/command-name` in chat
**Example:** `/chapter core-concepts "Testing Strategies"` creates chapter structure

---

## Skills System

### What Are Skills?

Skills are modular knowledge bases stored in `.claude/skills/` that provide:
- Domain-specific guidelines (backend, frontend, curriculum)
- Best practices and patterns
- Code examples and anti-patterns
- Contextual documentation

### Skills in VibeCoding Bootcamp

#### 1. backend-dev-guidelines
**Purpose:** Supabase Edge Functions, Stripe, Facebook CAPI, Resend patterns

**Activates when:**
- Keywords: `supabase`, `edge function`, `stripe`, `webhook`, `resend`, `payment`
- File paths: `supabase/functions/**/*.ts`
- Content: Deno imports, Stripe API calls, Facebook Conversions API

**What it covers:**
- Deno runtime patterns (Edge Functions)
- Stripe Checkout and webhook handling
- Facebook Conversions API (Meta CAPI)
- Resend email integration
- Database operations with Supabase client
- Webhook signature verification
- Dev mode vs production environment handling
- Dynamic pricing and cohort management
- Error handling in serverless context

**Example activation:**
```
You: "Create an edge function to handle Stripe webhooks"
Claude: [backend-dev-guidelines activates] "I'll create a Stripe webhook handler
         following Supabase Edge Function patterns..."
```

#### 2. frontend-dev-guidelines
**Purpose:** React 19, shadcn/ui, Tailwind CSS, React Router DOM patterns

**Activates when:**
- Keywords: `react`, `component`, `tailwind`, `shadcn`, `routing`, `animation`
- File paths: `src/components/**/*.tsx`, `src/pages/**/*.tsx`
- Content: React imports, shadcn/ui components, Tailwind classes

**What it covers:**
- React 19 patterns and hooks
- shadcn/ui component usage
- Tailwind CSS utility classes
- React Router DOM v7 navigation
- Framer Motion animations
- Performance optimization
- TypeScript best practices
- File organization

**Example activation:**
```
You: "Create a modal component with shadcn"
Claude: [frontend-dev-guidelines activates] "I'll create a modal using shadcn/ui's
         Dialog component with Tailwind styling..."
```

#### 3. vibe-curriculum
**Purpose:** VibeCoding Bootcamp curriculum creation with VIBE Framework

**Activates when:**
- Keywords: `curriculum`, `lesson`, `chapter`, `vibe framework`, `teaching`
- File paths: `content/curriculum/**/*.mdx`, `navigation.json`
- Commands: `/chapter`, `/page`, `/outline`

**What it covers:**
- VIBE Framework (Vision → Instructions → Build → Evaluate)
- Teaching non-technical founders
- Lesson structure and formatting
- MDX frontmatter requirements
- Hands-on exercises
- Learning objectives

**Example activation:**
```
You: "/page core-concepts/vibe-framework 'Advanced Prompting'"
Claude: [vibe-curriculum activates] "I'll create a lesson on advanced prompting
         following VibeCoding teaching standards..."
```

#### 4. skill-developer (Meta-Skill)
**Purpose:** Creating and managing Claude Code skills

**Activates when:**
- Keywords: `skill`, `skill-rules`, `hook`, `trigger`, `claude code`
- File paths: `.claude/skills/**/*.md`, `skill-rules.json`

**What it covers:**
- Skill structure and YAML frontmatter
- Trigger types (keywords, intent patterns, file paths)
- Enforcement levels (suggest, block, warn)
- Hook mechanisms
- Progressive disclosure (500-line rule)
- Troubleshooting skill activation

---

### How Skills Auto-Activate

Skills are configured in `.claude/skills/skill-rules.json` with three trigger types:

#### 1. Keyword Triggers (Explicit)
Match specific words in your prompts:
```json
"keywords": ["stripe", "payment", "checkout", "webhook"]
```

**Example:**
```
You: "How do I integrate Stripe payments?"
→ backend-dev-guidelines activates (keyword: "stripe", "payments")
```

#### 2. Intent Patterns (Implicit)
Match user intentions with regex:
```json
"intentPatterns": [
  "(create|add|implement).*?(webhook|payment)",
  "(integrate|setup).*?(stripe|resend)"
]
```

**Example:**
```
You: "Let's add a webhook handler for payments"
→ backend-dev-guidelines activates (pattern: "add.*webhook")
```

#### 3. File Triggers (Context-Based)
Match file paths or content:
```json
"pathPatterns": ["supabase/functions/**/*.ts"],
"contentPatterns": ["Deno\\.serve", "createClient.*supabase"]
```

**Example:**
```
You edit: supabase/functions/create-checkout/index.ts
→ backend-dev-guidelines activates (path match + Deno.serve in content)
```

---

### skill-rules.json Structure

**Location:** `.claude/skills/skill-rules.json`

**Structure:**
```json
{
  "version": "2.0",
  "skills": {
    "skill-name": {
      "type": "domain",              // domain | guardrail
      "enforcement": "suggest",       // suggest | block | warn
      "priority": "high",             // critical | high | medium | low
      "description": "...",
      "promptTriggers": {
        "keywords": ["..."],
        "intentPatterns": ["..."]
      },
      "fileTriggers": {
        "pathPatterns": ["..."],
        "contentPatterns": ["..."]
      },
      "skipConditions": {
        "sessionTracking": true,
        "fileMarkers": ["// @skip-validation"],
        "envVars": ["SKIP_SKILL_NAME"]
      }
    }
  }
}
```

**Key fields:**
- `type`: `domain` (advisory) or `guardrail` (blocking)
- `enforcement`: `suggest` (most common), `block` (critical only), `warn` (rarely used)
- `priority`: Determines how aggressively to trigger
- `sessionTracking`: Prevents repeated suggestions in same session
- `fileMarkers`: Skip specific files with comment marker
- `envVars`: Emergency disable via environment variable

---

## Hooks System

### What Are Hooks?

Hooks are scripts that run at specific events in Claude's workflow, enabling:
- Skill auto-activation
- File change tracking
- Code formatting
- Build checks
- Custom automations

### Hook Events

1. **UserPromptSubmit** - Before Claude sees your prompt
2. **PreToolUse** - Before a tool executes (can block)
3. **PostToolUse** - After a tool completes
4. **Stop** - When you press stop or task completes

### Hooks in VibeCoding Bootcamp

#### 1. skill-activation-prompt (UserPromptSubmit)
**File:** `.claude/hooks/skill-activation-prompt.sh` + `.ts`

**Purpose:** Auto-suggests relevant skills based on context

**How it works:**
1. Reads `skill-rules.json`
2. Analyzes user prompt for keywords and intent patterns
3. Checks which files are being worked on
4. Injects skill suggestions into Claude's context

**Configuration:**
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

**Example:**
```
You: "Create an edge function for Stripe"
Hook detects: keyword "stripe" + "edge function"
Hook injects: "Consider using backend-dev-guidelines skill"
Claude: [Has full backend context automatically]
```

#### 2. post-tool-use-tracker (PostToolUse)
**File:** `.claude/hooks/post-tool-use-tracker.sh`

**Purpose:** Tracks file changes to maintain context across sessions

**How it works:**
1. Monitors Edit/Write/MultiEdit tool calls
2. Records which files were modified
3. Creates cache for context management
4. Auto-detects project structure (frontend, backend, etc.)

**Configuration:**
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

#### 3. Stop Hooks (Code Quality)
**Files:** Various `.sh` scripts

**Purpose:** Format code, check builds, show reminders when you stop

**Available:**
- `stop-prettier-formatter.sh` - Auto-formats modified files
- `stop-build-check-enhanced.sh` - Runs TypeScript checks
- `error-handling-reminder.sh` - Gentle reminders about error handling

**Configuration:**
```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          { "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/stop-prettier-formatter.sh" },
          { "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/stop-build-check-enhanced.sh" },
          { "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/error-handling-reminder.sh" }
        ]
      }
    ]
  }
}
```

---

### Hook Configuration File

**Location:** `.claude/settings.json` (project root)

**Complete example:**
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
    ],
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
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/stop-prettier-formatter.sh"
          },
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/stop-build-check-enhanced.sh"
          },
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/error-handling-reminder.sh"
          }
        ]
      }
    ]
  }
}
```

**Key points:**
- Hooks run in order specified
- `matcher` filters which tool uses trigger the hook
- `$CLAUDE_PROJECT_DIR` is automatically set by Claude Code
- Hooks must be executable (`chmod +x`)

---

## Commands System

### What Are Commands?

Slash commands that automate repetitive tasks. Type `/command-name` in chat to execute.

### Commands in VibeCoding Bootcamp

#### Curriculum Commands

**1. /chapter**
```
/chapter [module-slug] "[chapter-name]"
```

**What it does:**
1. Validates module exists
2. Creates chapter directory
3. Updates `navigation.json`
4. Shows next steps

**Example:**
```
/chapter core-concepts "Testing Strategies"

Creates: content/curriculum/core-concepts/testing-strategies/
Updates: navigation.json with new chapter entry
```

**2. /page**
```
/page [module]/[chapter] "[page-title]"
```

**What it does:**
1. Validates module and chapter exist
2. Asks for lesson type (basic/tutorial/reference)
3. Copies appropriate template
4. Pre-fills frontmatter
5. Updates `navigation.json`
6. Opens file for editing

**Example:**
```
/page core-concepts/vibe-framework "Advanced Prompting"

Creates: content/curriculum/core-concepts/vibe-framework/advanced-prompting.mdx
Updates: navigation.json with new page entry
Opens: File in editor
```

**3. /outline**
```
/outline [module|chapter|expand] "[topic or path]"
```

**What it does:**
- Brainstorms and structures curriculum content
- Creates module/chapter/page outlines
- Provides VibeCoding-specific recommendations

**Example:**
```
/outline chapter "AI-Powered Testing"

Suggests: 5-7 pages covering testing fundamentals, AI tools, practical examples
```

#### Development Commands

**4. /create-plan**
```
/create-plan "Describe what you need planned"
```

**What it does:**
- Creates strategic plan with task breakdown
- Generates comprehensive documentation
- Sets up plans/active/ directory structure

**Example:**
```
/create-plan "Implement user authentication with Supabase"

Creates: plans/active/auth-implementation/
         - auth-implementation-plan.md
         - auth-implementation-context.md
         - auth-implementation-tasks.md
```

**5. /update-context**
```
/update-context [context to focus on]
```

**What it does:**
- Updates waypoint plans before context reset
- Captures session progress
- Documents key decisions
- Creates handoff notes

**Example:**
```
/update-context

Updates: All active task documentation with current progress
Captures: Key decisions, blockers, next steps
```

---

### Creating Custom Commands

**Location:** `.claude/commands/[command-name].md`

**Structure:**
```markdown
---
name: my-command
description: Brief description
---

# Command Implementation

When user types /my-command:
1. Do this
2. Then do that
3. Finally do this

## Variables
- $1: First argument
- $2: Second argument

## Example
/my-command arg1 "arg 2"
```

**See also:**
- `.claude/commands/README.md` - Commands overview
- Existing commands for reference patterns

---

## Integration Process

### Prerequisites

1. **Claude Code installed** - Latest version from claude.ai/code
2. **Project cloned** - VibeCoding Bootcamp repository
3. **Bun installed** - For running hooks (TypeScript)

### Step-by-Step Integration

#### Step 1: Verify File Structure

```bash
# Check .claude directory exists
ls -la .claude/

# Expected structure:
.claude/
├── agents/           # Custom agents
├── commands/         # Slash commands
├── hooks/            # Hook scripts
├── skills/           # Skill knowledge bases
└── settings.json     # Hook configuration
```

#### Step 2: Configure skill-rules.json

**File:** `.claude/skills/skill-rules.json`

✅ **Already configured** for VibeCoding Bootcamp with:
- backend-dev-guidelines (Supabase, Stripe, Resend, Facebook CAPI)
- frontend-dev-guidelines (React 19, shadcn/ui, Tailwind CSS)
- vibe-curriculum (VIBE Framework, MDX lessons)
- skill-developer (Meta-skill for skill management)

**Validate configuration:**
```bash
cat .claude/skills/skill-rules.json | jq .
# Should output formatted JSON with no errors
```

#### Step 3: Install Hook Dependencies

```bash
cd .claude/hooks
bun install
```

**Installs:**
- TypeScript types
- Dependencies for skill-activation-prompt.ts
- Other hook utilities

#### Step 4: Set Hook Permissions

```bash
chmod +x .claude/hooks/*.sh
```

**Verify:**
```bash
ls -la .claude/hooks/*.sh | grep rwx
# All .sh files should have execute permissions
```

#### Step 5: Configure settings.json

**File:** `.claude/settings.json` (project root)

✅ **If file doesn't exist**, create it with:
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
    ],
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

**Note:** Stop hooks are optional - add only if you want auto-formatting and build checks.

#### Step 6: Test Integration

**Test 1: Skill activation**
```bash
# Test skill-activation-prompt hook manually
echo '{"prompt":"Create an edge function for Stripe"}' | \
  cd .claude/hooks && bun run skill-activation-prompt.ts
```

**Expected output:**
```
Consider using: backend-dev-guidelines
(Skill suggestion appears in Claude's context)
```

**Test 2: Command execution**
```
In Claude Code chat:
/chapter core-concepts "Test Chapter"
```

**Expected output:**
```
✅ Module 'core-concepts' exists
✅ Created directory: content/curriculum/core-concepts/test-chapter/
✅ Updated navigation.json
```

**Test 3: File-based skill trigger**
```
Open: supabase/functions/test/index.ts
Type: "import { serve } from 'https://deno.land/std/http/server.ts'"
```

**Expected:** backend-dev-guidelines skill suggestion appears

---

## Practical Workflows

### Workflow 1: Backend Edge Function Development

**Scenario:** Create a new Stripe webhook handler

**Steps:**
```
1. You: "Create an edge function to handle Stripe webhooks"
   → backend-dev-guidelines auto-activates

2. Claude: Creates function with:
   - Deno serve() pattern
   - Stripe webhook signature verification
   - Database operations
   - Error handling
   - CORS configuration
   - Dev mode support

3. You: Edit supabase/functions/stripe-webhook/index.ts
   → PostToolUse hook tracks the change

4. You: Stop editing
   → Stop hooks run:
     - Prettier formats the code
     - TypeScript checks for errors
     - Error handling reminder (if needed)

5. Ready to deploy with confidence!
```

**Key benefit:** Claude knows Supabase Edge Function patterns, Stripe API, and VibeCoding conventions automatically.

---

### Workflow 2: Frontend Component Creation

**Scenario:** Build a new modal component with shadcn/ui

**Steps:**
```
1. You: "Create a modal component for user settings using shadcn/ui"
   → frontend-dev-guidelines auto-activates

2. Claude: Creates component with:
   - shadcn/ui Dialog component
   - Tailwind CSS styling
   - TypeScript types
   - React 19 patterns
   - Proper file location (src/components/)

3. You: "Add Framer Motion animation"
   → frontend-dev-guidelines still active

4. Claude: Adds animation following Framer Motion best practices

5. Component ready to use!
```

**Key benefit:** Consistent shadcn/ui + Tailwind patterns across all components.

---

### Workflow 3: Curriculum Content Creation

**Scenario:** Create a new chapter with 5 lessons

**Steps:**
```
1. You: "/chapter core-concepts 'AI-Powered Testing'"
   → vibe-curriculum auto-activates
   → Command creates chapter structure

2. You: "/page core-concepts/ai-powered-testing 'Introduction to Testing'"
   → Command creates page with template
   → vibe-curriculum provides teaching guidelines

3. Claude: Fills in content following:
   - VIBE Framework structure
   - Hands-on exercises
   - Non-technical friendly language
   - Revenue-focused examples

4. Repeat for 4 more pages

5. Chapter complete with consistent teaching style!
```

**Key benefit:** VibeCoding teaching methodology enforced automatically.

---

### Workflow 4: Multi-File Refactoring

**Scenario:** Refactor authentication logic across multiple files

**Steps:**
```
1. You: "Refactor authentication to use Supabase RLS"
   → backend-dev-guidelines auto-activates

2. Claude: Identifies files to change
   → PostToolUse hook tracks each edit

3. You: Stop editing
   → Stop hooks check all modified files
   → TypeScript validates changes across all files
   → Prettier formats consistently

4. No breaking changes, all types valid!
```

**Key benefit:** Multi-file consistency maintained automatically.

---

## Troubleshooting

### Skill Not Activating

**Symptom:** Expected skill doesn't suggest when working on relevant files

**Diagnosis:**
```bash
# 1. Check skill-rules.json exists
ls -la .claude/skills/skill-rules.json

# 2. Validate JSON syntax
cat .claude/skills/skill-rules.json | jq .

# 3. Check hook is registered
cat .claude/settings.json | jq .hooks.UserPromptSubmit

# 4. Test hook manually
echo '{"prompt":"your test prompt"}' | \
  cd .claude/hooks && bun run skill-activation-prompt.ts
```

**Solutions:**
- **Missing skill-rules.json:** Use this guide to create it
- **Invalid JSON:** Run `jq .` to find syntax errors
- **Hook not registered:** Add to settings.json (see Step 5 above)
- **Keywords don't match:** Add more keywords to skill-rules.json
- **Path patterns wrong:** Update pathPatterns to match your structure

---

### Hook Not Executing

**Symptom:** Hooks configured but not running

**Diagnosis:**
```bash
# 1. Check hooks exist
ls -la .claude/hooks/*.sh

# 2. Check permissions
ls -la .claude/hooks/*.sh | grep rwx

# 3. Check dependencies installed
ls -la .claude/hooks/node_modules

# 4. Test hook manually
./.claude/hooks/skill-activation-prompt.sh
```

**Solutions:**
- **Missing execute permissions:** Run `chmod +x .claude/hooks/*.sh`
- **Dependencies not installed:** Run `cd .claude/hooks && bun install`
- **Hook script errors:** Check Claude Code logs for error messages
- **Wrong path in settings.json:** Verify `$CLAUDE_PROJECT_DIR` paths

---

### Command Not Found

**Symptom:** Slash command doesn't work

**Diagnosis:**
```bash
# 1. Check command file exists
ls -la .claude/commands/[command-name].md

# 2. Check frontmatter is valid
head -10 .claude/commands/[command-name].md
```

**Solutions:**
- **File doesn't exist:** Create `.claude/commands/[name].md` with proper frontmatter
- **Invalid frontmatter:** Must have `---` delimiter and `name:` field
- **Typo in command name:** Check spelling matches filename (without .md)

---

### False Positives (Skill Triggers Too Often)

**Symptom:** Skill suggests when not relevant

**Solutions:**
1. **Make keywords more specific:**
   ```json
   "keywords": ["stripe webhook", "stripe checkout"]  // Instead of just "stripe"
   ```

2. **Add path exclusions:**
   ```json
   "pathExclusions": ["**/*.test.ts", "**/archive/**"]
   ```

3. **Increase intent pattern specificity:**
   ```json
   "intentPatterns": [
     "(create|implement).*?edge function.*?stripe"  // More specific
   ]
   ```

4. **Lower priority:**
   ```json
   "priority": "medium"  // Instead of "high"
   ```

---

### False Negatives (Skill Doesn't Trigger When Needed)

**Symptom:** Expected skill doesn't suggest when it should

**Solutions:**
1. **Add more keywords:**
   ```json
   "keywords": ["webhook", "webhooks", "webhook handler", "event handler"]
   ```

2. **Broaden path patterns:**
   ```json
   "pathPatterns": [
     "supabase/functions/**/*.ts",
     "functions/**/*.ts",  // Catch alternative structures
     "api/**/*.ts"
   ]
   ```

3. **Add more intent patterns:**
   ```json
   "intentPatterns": [
     "(handle|process|receive).*?(webhook|event)",
     "webhook.*?(handler|handling|processing)"
   ]
   ```

4. **Increase priority:**
   ```json
   "priority": "high"  // Trigger more aggressively
   ```

---

### Performance Issues

**Symptom:** Hooks are slow, Claude Code feels laggy

**Solutions:**
1. **Limit TypeScript checks:**
   - Only check changed files (hooks already do this)
   - Skip large generated files

2. **Reduce Stop hooks:**
   - Disable formatting if not needed
   - Disable build checks if too slow

3. **Add skip conditions:**
   ```json
   "pathExclusions": [
     "**/node_modules/**",
     "**/.next/**",
     "**/dist/**",
     "**/*.generated.ts"
   ]
   ```

4. **Use faster package manager:**
   - `bun` is faster than `npm` for hooks

---

## Maintenance & Updates

### Updating Skill Triggers

**When to update:**
- Adding new technologies (new npm packages, APIs)
- Project structure changes (new directories)
- Team adopts new patterns

**How to update:**

1. **Edit skill-rules.json:**
   ```json
   {
     "backend-dev-guidelines": {
       "promptTriggers": {
         "keywords": [
           // Add new keywords
           "new-technology",
           "new-api-name"
         ]
       },
       "fileTriggers": {
         "pathPatterns": [
           // Add new paths
           "new-directory/**/*.ts"
         ]
       }
     }
   }
   ```

2. **Validate changes:**
   ```bash
   cat .claude/skills/skill-rules.json | jq .
   ```

3. **Test new triggers:**
   ```bash
   echo '{"prompt":"new-keyword test"}' | \
     cd .claude/hooks && bun run skill-activation-prompt.ts
   ```

---

### Adding New Skills

**When to add:**
- New domain area (mobile app, DevOps, testing)
- New team member specialty
- New major technology adoption

**How to add:**

1. **Create skill directory:**
   ```bash
   mkdir -p .claude/skills/my-new-skill
   ```

2. **Create SKILL.md:**
   ```markdown
   ---
   name: my-new-skill
   description: Clear description with keywords that trigger this skill
   ---

   # My New Skill

   ## Purpose
   What this skill helps with

   ## When to Use
   Specific scenarios

   ## Key Patterns
   Best practices and examples
   ```

3. **Add to skill-rules.json:**
   ```json
   {
     "my-new-skill": {
       "type": "domain",
       "enforcement": "suggest",
       "priority": "medium",
       "promptTriggers": {
         "keywords": ["..."],
         "intentPatterns": ["..."]
       }
     }
   }
   ```

4. **Test activation:**
   - Use keywords in prompt
   - Edit matching file
   - Verify skill suggests

**See:** `.claude/skills/skill-developer/SKILL.md` for complete guide

---

### Customizing Hooks

**Common customizations:**

**1. Add project-specific directory detection:**
```bash
# In post-tool-use-tracker.sh, detect_repo()
case "$repo" in
    my-custom-service)
        echo "$repo"
        ;;
    # ... existing cases
esac
```

**2. Add custom build command:**
```bash
# In post-tool-use-tracker.sh, get_build_command()
if [[ "$repo" == "my-service" ]]; then
    echo "cd $repo_path && make build"
    return
fi
```

**3. Add custom formatting rules:**
```bash
# In stop-prettier-formatter.sh
if [[ "$file" == *"special-format"* ]]; then
    # Custom formatting logic
fi
```

**See:** `.claude/hooks/CONFIG.md` for complete configuration guide

---

### Team Sharing

**To share with team:**

1. **Commit .claude directory:**
   ```bash
   git add .claude/
   git commit -m "Add Claude Code integration"
   git push
   ```

2. **Document in README:**
   ```markdown
   ## Claude Code Integration

   This project uses Claude Code with custom skills and hooks.

   **Setup:**
   1. Install Claude Code from claude.ai/code
   2. Clone this repository
   3. Run: `cd .claude/hooks && bun install`
   4. Run: `chmod +x .claude/hooks/*.sh`
   5. Skills will auto-activate when working on relevant files

   See [docs/CLAUDE_CODE_INTEGRATION.md](docs/CLAUDE_CODE_INTEGRATION.md) for details.
   ```

3. **Create onboarding guide:**
   - Share this document
   - Do a demo session
   - Answer questions

---

## Additional Resources

### Project Documentation
- [CLAUDE.md](../CLAUDE.md) - Project overview and development guidelines
- [CLAUDE_CODE_SETUP.md](CLAUDE_CODE_SETUP.md) - MVP setup documentation
- [docs/getting-started/LOCAL_ENV_RUNBOOK.md](getting-started/LOCAL_ENV_RUNBOOK.md) - Local development setup

### Skills Documentation
- [.claude/skills/README.md](../.claude/skills/README.md) - Skills overview
- [.claude/skills/skill-developer/SKILL.md](../.claude/skills/skill-developer/SKILL.md) - Complete skill development guide
- [.claude/skills/skill-developer/TRIGGER_TYPES.md](../.claude/skills/skill-developer/TRIGGER_TYPES.md) - Trigger patterns reference
- [.claude/skills/skill-developer/TROUBLESHOOTING.md](../.claude/skills/skill-developer/TROUBLESHOOTING.md) - Debugging guide

### Hooks Documentation
- [.claude/hooks/README.md](../.claude/hooks/README.md) - Hooks overview
- [.claude/hooks/CONFIG.md](../.claude/hooks/CONFIG.md) - Configuration guide

### Commands Documentation
- [.claude/commands/](../.claude/commands/) - Individual command files

### External Resources
- [Claude Code Official Docs](https://docs.anthropic.com/claude-code) - Official documentation
- [Anthropic Best Practices](https://docs.anthropic.com/claude/docs/skills-best-practices) - Skill best practices

---

## Summary

**What You've Gained:**

✅ **Context-aware AI** - Claude knows VibeCoding patterns automatically
✅ **Consistent code** - Supabase, Stripe, React patterns enforced
✅ **Faster development** - Skills auto-activate, commands automate tasks
✅ **Better onboarding** - New developers get instant guidance
✅ **Living documentation** - Skills update with your project

**Next Steps:**

1. **Test the integration** - Try creating an edge function or React component
2. **Customize if needed** - Add project-specific keywords or paths
3. **Share with team** - Commit .claude/ directory and document
4. **Iterate** - Update skills as your project evolves

**Questions?** Open an issue or check the troubleshooting section above.

---

**Last Updated:** 2025-11-03
**Maintained by:** VibeCoding Bootcamp Team
**Version:** 2.0 (Fully adapted for VibeCoding Bootcamp)

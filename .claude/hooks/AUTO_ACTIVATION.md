# Auto-Activation System - Claude Code Waypoint Plugin

Complete documentation for the auto-activation hook system that enables skills to suggest themselves based on context.

## Overview

**Status**: CRITICAL feature (until Claude Code platform adds native auto-activation)

The auto-activation system uses event-driven hooks to analyze your prompt, detect your context, and automatically suggest relevant skills without any manual invocation.

```
You type a prompt or edit a file
    ↓
UserPromptSubmit hook triggers
    ↓
skill-activation-prompt.ts analyzes context
    ↓
Matches against skill-rules.json
    ↓
Suggests relevant skills
    ↓
Claude loads the perfect context automatically
```

**Result**: Right knowledge at the right time, every time.

## How It Works

### The Flow

1. **User acts** (types prompt or edits file)
2. **Hook fires** (UserPromptSubmit event)
3. **Analysis runs** (skill-activation-prompt.ts executes)
4. **Rules evaluated** (against skill-rules.json)
5. **Skills suggested** (injected into prompt context)
6. **Claude loads them** (auto-activation complete)

### Example: Frontend Development

```
User: "Create a new component for displaying user profile"
        ↓
Hook analyzes: keyword "component" + context shows .tsx file
        ↓
skill-rules.json checks: frontend-dev-guidelines has keyword "component"
        ↓
Hook suggests: "frontend-dev-guidelines skill loaded"
        ↓
Claude receives: [entire skill content] + user prompt
        ↓
Result: Claude uses Next.js + React 19 patterns automatically
```

### Example: Memory Management

```
User: edits plans/my-feature/my-feature-context.md
        ↓
Hook analyzes: file path matches "plans/**/*"
        ↓
skill-rules.json checks: context-persistence activates for "plans/**/*"
        ↓
Hook suggests: "context-persistence skill loaded"
        ↓
Claude receives: [waypoint pattern guidance] + user edit
        ↓
Result: Claude helps update waypoint correctly
```

## Configuration: skill-rules.json

The heart of auto-activation is `skill-rules.json` which defines trigger rules for each skill.

### File Location

```
.claude/
├── skills/
│   ├── skill-rules.json  ← THIS FILE
│   ├── frontend-dev-guidelines/
│   ├── backend-dev-guidelines/
│   └── ... (other skills)
```

### Structure

```json
{
  "skill-name": {
    "type": "domain" | "guardrail",
    "enforcement": "suggest" | "block" | "warn",
    "priority": "critical" | "high" | "medium" | "low",
    "promptTriggers": {
      "keywords": ["word1", "word2"],
      "intentPatterns": ["regex.*?pattern"]
    },
    "fileTriggers": {
      "pathPatterns": ["glob/patterns/**/*.ts"],
      "contentPatterns": ["regex.*?in.*?content"]
    },
    "skipConditions": {
      "sessionTracking": false,
      "fileMarkers": ["// @skip-skill-name"],
      "envVars": ["SKIP_SKILL_NAME"]
    }
  }
}
```

### Full Example

```json
{
  "context-persistence": {
    "type": "domain",
    "enforcement": "suggest",
    "priority": "critical",
    "promptTriggers": {
      "keywords": [
        "waypoint",
        "resume",
        "context reset",
        "session progress",
        "plans"
      ],
      "intentPatterns": [
        "(create|write|update).*?(plan|waypoint)",
        "(resume|continue).*?(work|task|feature)"
      ]
    },
    "fileTriggers": {
      "pathPatterns": [
        "plans/**/*",
        ".claude/memory/**/*"
      ],
      "contentPatterns": [
        "SESSION PROGRESS",
        "NEXT ACTION"
      ]
    },
    "skipConditions": {
      "sessionTracking": true,
      "fileMarkers": ["// @skip-context-persistence"],
      "envVars": ["SKIP_CONTEXT_PERSISTENCE"]
    }
  },

  "frontend-dev-guidelines": {
    "type": "domain",
    "enforcement": "suggest",
    "priority": "high",
    "promptTriggers": {
      "keywords": [
        "component",
        "react",
        "frontend",
        "ui",
        "page"
      ],
      "intentPatterns": [
        "(create|build|make).*?(component|page|ui)",
        "(fix|debug|style).*?(react|frontend)"
      ]
    },
    "fileTriggers": {
      "pathPatterns": [
        "src/**/*.tsx",
        "src/**/*.ts",
        "app/**/*.tsx"
      ],
      "contentPatterns": [
        "import.*?React",
        "export.*?function",
        "@shadcn/ui"
      ]
    },
    "skipConditions": {
      "sessionTracking": false,
      "fileMarkers": ["// @skip-frontend"],
      "envVars": []
    }
  }
}
```

## Trigger Types

### 1. Keywords

**What**: Exact word matches in user prompts.

**Example**:
```json
"promptTriggers": {
  "keywords": ["stripe", "payment", "subscription"]
}
```

**How it works**:
- User types: "Add Stripe payment processing"
- Hook detects: "stripe" in prompt
- Result: Payment-related skill activates

**When to use**: For explicit, well-defined skill triggers.

**Example skills using keywords**:
- "stripe" → payment skill
- "database" → database skill
- "test" → testing skill

### 2. Intent Patterns

**What**: Regular expressions for user intentions.

**Example**:
```json
"promptTriggers": {
  "intentPatterns": [
    "(create|add|implement).*?route",
    "(debug|fix).*?error"
  ]
}
```

**How it works**:
- User types: "Implement a new API route"
- Hook tests regex: `(create|add|implement).*?route`
- Matches: "implement" + "route"
- Result: Backend skill activates

**When to use**: For actions that might be phrased multiple ways.

**Example patterns**:
- `"(create|add|implement).*?route"` → routing skill
- `"(debug|fix|troubleshoot).*?error"` → debugging skill
- `"(test|write|add).*?test"` → testing skill

### 3. Path Patterns

**What**: File glob patterns for files being edited.

**Example**:
```json
"fileTriggers": {
  "pathPatterns": [
    "src/components/**/*.tsx",
    "app/**/*.tsx"
  ]
}
```

**How it works**:
- User edits: `src/components/UserCard.tsx`
- Hook checks path against patterns
- Matches: `src/components/**/*.tsx`
- Result: Frontend skill activates

**When to use**: When skill relates to specific file types/locations.

**Example patterns**:
- `"src/components/**/*.tsx"` → frontend
- `"src/api/**/*.ts"` → backend
- `"tests/**/*.test.ts"` → testing
- `".claude/memory/**/*"` → memory management

### 4. Content Patterns

**What**: Regular expressions matching file content.

**Example**:
```json
"fileTriggers": {
  "contentPatterns": [
    "import.*?Stripe",
    "stripeClient\\.payments"
  ]
}
```

**How it works**:
- User edits: `payment.ts`
- Hook reads file content
- Searches for regex: `import.*?Stripe`
- Matches: Found `import Stripe from 'stripe'`
- Result: Payment skill activates

**When to use**: For skills related to specific libraries/patterns in code.

**Example patterns**:
- `"import.*?Stripe"` → Stripe skill
- `"export.*?interface.*?Props"` → Component pattern skill
- `"SESSION PROGRESS"` → Context persistence skill

## Enforcement Levels

### "suggest" - Non-Blocking (Default)

The skill suggests itself, but doesn't block execution.

```json
"enforcement": "suggest"
```

**Behavior**:
- Skill auto-loads
- Message: "frontend-dev-guidelines skill loaded"
- User can proceed whether they use it or not
- Default for most skills

**When to use**: For guidance skills, reference materials, best practices.

**Example**: frontend-dev-guidelines with `"suggest"` means Claude will help with React patterns, but won't block if user wants to do something different.

### "block" - Blocking (For Guardrails)

The skill can prevent tool execution if conditions are met.

```json
"enforcement": "block"
```

**Behavior**:
- Skill auto-loads
- Message: "frontend-dev-guidelines (blocking) - MUI v7 only"
- Can refuse to execute if guards aren't met
- For critical constraints

**When to use**: For breaking changes, incompatible approaches, hard constraints.

**Example**: Migrating from MUI v5 to v7 - block execution if old patterns detected:
```json
{
  "frontend-dev-guidelines": {
    "enforcement": "block",
    "skipConditions": {
      "fileMarkers": ["// @skip-mui-guard"]
    }
  }
}
```

This prevents accidental use of MUI v5 patterns in v7 project.

### "warn" - Warning (Cautious)

The skill suggests itself and shows warning.

```json
"enforcement": "warn"
```

**Behavior**:
- Skill auto-loads
- Message: "⚠️ Warning: database-migration skill loaded"
- User should review before proceeding
- For significant changes

**When to use**: For potentially dangerous operations, major refactors.

**Example**: Database migration skill with warning so user doesn't accidentally run migration in production.

## Skip Conditions

Control when skills DON'T activate.

### sessionTracking

Only suggest once per session:

```json
"skipConditions": {
  "sessionTracking": true
}
```

**Effect**:
- First mention of skill trigger → skill loads
- Second mention in same session → skips
- New session → reset, skill loads again

**Use for**: Skills you want to see early in session, but not repeatedly.

**Example**:
```json
"context-persistence": {
  "skipConditions": {
    "sessionTracking": true
  }
}
```
First edit to `plans/` → loads skill. Later edits → skips (you already loaded it).

### fileMarkers

Skip if special comment exists in file:

```json
"skipConditions": {
  "fileMarkers": ["// @skip-frontend", "// @no-frontend-validation"]
}
```

**Effect**:
- If file contains `// @skip-frontend`, skill doesn't activate
- Allows developers to opt-out selectively

**Use for**: Files where you don't want skill guidance (legacy code, experimental, etc.)

**Example**:
```typescript
// @skip-frontend
// This is legacy jQuery code, don't apply React patterns

const userComponent = document.getElementById('user-card');
// ... old jQuery code
```

Hook reads comment, skips frontend-dev-guidelines activation.

### envVars

Skip if environment variable set:

```json
"skipConditions": {
  "envVars": ["SKIP_FRONTEND", "DISABLE_REACT_HINTS"]
}
```

**Effect**:
- Set env var: `export SKIP_FRONTEND=1`
- Skill won't activate for this session
- Useful for testing, temporary overrides

**Use for**: Global feature flags, testing without skills.

**Example**:
```bash
# Temporarily disable frontend skill
export SKIP_FRONTEND=1
claude code
# Work without frontend guidance
```

## Customizing skill-rules.json

### For Your Project

Each project has different file structures. Update paths to match:

**Example 1: Monorepo**

```json
{
  "backend-dev-guidelines": {
    "fileTriggers": {
      "pathPatterns": [
        "services/api/src/**/*.ts",
        "services/auth/src/**/*.ts"
      ]
    }
  }
}
```

**Example 2: Standard Create React App**

```json
{
  "frontend-dev-guidelines": {
    "fileTriggers": {
      "pathPatterns": [
        "src/**/*.tsx",
        "src/**/*.jsx"
      ]
    }
  }
}
```

**Example 3: Multiple Backend Services**

```json
{
  "backend-api": {
    "fileTriggers": {
      "pathPatterns": [
        "api/src/**/*.ts"
      ]
    }
  },
  "backend-auth": {
    "fileTriggers": {
      "pathPatterns": [
        "auth/src/**/*.ts"
      ]
    }
  }
}
```

### Adding New Skills

When you create a custom skill, add it to skill-rules.json:

```json
{
  "my-custom-skill": {
    "type": "domain",
    "enforcement": "suggest",
    "priority": "high",
    "promptTriggers": {
      "keywords": ["my", "custom", "keywords"],
      "intentPatterns": ["(pattern1|pattern2).*?intent"]
    },
    "fileTriggers": {
      "pathPatterns": ["my-specific/**/*.ts"],
      "contentPatterns": ["specific.*?code.*?pattern"]
    },
    "skipConditions": {
      "sessionTracking": false,
      "fileMarkers": [],
      "envVars": []
    }
  }
}
```

See [skill-developer skill](./../skills/skill-developer/SKILL.md) for complete custom skill creation guide.

## How the Hook Works: Technical Details

### skill-activation-prompt.ts

Location: `.claude/hooks/skill-activation-prompt.ts`

**What it does**:

1. **Receives input**:
   ```typescript
   {
     "prompt": "user's prompt text",
     "activeFiles": ["file1.ts", "file2.tsx"],
     "recentlyEdited": ["file3.ts"]
   }
   ```

2. **Loads skill-rules.json**:
   ```typescript
   const rules = JSON.parse(fs.readFileSync('.claude/skills/skill-rules.json'));
   ```

3. **Evaluates each skill**:
   - Check prompt keywords
   - Check intent patterns
   - Check file path patterns
   - Check file content patterns
   - Check skip conditions

4. **Builds suggestion list**:
   ```typescript
   const activated = [];
   for (const [skillName, rule] of Object.entries(rules)) {
     if (matches(prompt, activeFiles, rule)) {
       activated.push(skillName);
     }
   }
   ```

5. **Outputs recommendation**:
   ```
   Suggested skills: context-persistence, frontend-dev-guidelines
   Inject at: UserPromptSubmit
   ```

### skill-activation-prompt.sh

Location: `.claude/hooks/skill-activation-prompt.sh`

**What it does**:

1. **Wraps TypeScript execution**:
   ```bash
   npx tsx skill-activation-prompt.ts
   ```

2. **Handles errors gracefully** (if hook fails, work continues)

3. **Pipes input/output**:
   - Receives event data
   - Processes through TypeScript
   - Returns suggestions
   - Claude Code uses suggestions

## Hook Configuration

Hook configuration lives in `settings.json`:

```json
{
  "hooks": {
    "UserPromptSubmit": {
      "handler": ".claude/hooks/skill-activation-prompt.sh",
      "timeout": 5000
    }
  }
}
```

## Testing Auto-Activation

### Manual Test

```bash
# Test skill activation logic
echo '{"prompt":"create React component"}' | \
  cd .claude/hooks && npx tsx skill-activation-prompt.ts
```

**Expected output**:
```
Suggested skills:
- frontend-dev-guidelines (high priority)
- skill-developer (medium priority)
```

### Real-World Test

1. Edit a frontend file (e.g., `src/components/Button.tsx`)
2. Type: "Create a new component"
3. Watch for: "frontend-dev-guidelines skill loaded" message
4. Verify: Skill content appears in context

### Debugging

If skill isn't activating:

```bash
# 1. Validate JSON syntax
cat .claude/skills/skill-rules.json | jq .

# 2. Check paths match
ls src/components/*.tsx  # Do these exist?

# 3. Check skill file exists
ls .claude/skills/frontend-dev-guidelines/SKILL.md

# 4. Test manually
echo '{"prompt":"create component","activeFiles":["src/components/test.tsx"]}' | \
  npx tsx skill-activation-prompt.ts
```

## Status: Why This Is Critical

**Current state** (as of 2025-01): Claude Code platform doesn't have native skill auto-activation. Developers must manually select skills.

**With Claude Code Waypoint Plugin**: Auto-activation fills this gap.

**Future** (when Claude Code adds native auto-activation):
- This becomes optional
- But still provides value through custom rules
- Backward compatible

**For now**: This hook system is essential for smooth workflows.

## Best Practices

### 1. Keep Rules Specific

BAD:
```json
"keywords": ["the", "a", "is"]  // Too generic
```

GOOD:
```json
"keywords": ["stripe", "payment", "subscription"]  // Specific
```

### 2. Use Both Prompt and File Triggers

```json
{
  "frontend-dev-guidelines": {
    "promptTriggers": {
      "keywords": ["component", "react"]
    },
    "fileTriggers": {
      "pathPatterns": ["src/**/*.tsx"]
    }
  }
}
```

Catches both: "create a component" (prompt) AND editing `.tsx` file (file).

### 3. Order by Priority

```json
{
  "context-persistence": { "priority": "critical" },
  "frontend-dev-guidelines": { "priority": "high" },
  "testing-guide": { "priority": "medium" }
}
```

Critical skills load first.

### 4. Test After Changes

```bash
# After modifying skill-rules.json
cat .claude/skills/skill-rules.json | jq .  # Validate
echo '{"prompt":"test"}' | npx tsx skill-activation-prompt.ts  # Test
```

## Troubleshooting

### Skill Not Activating

**Symptom**: You edit a file or use a keyword, but skill doesn't load.

**Debug steps**:
1. Check skill-rules.json is valid JSON: `jq .`
2. Verify skill file exists: `ls .claude/skills/skill-name/SKILL.md`
3. Check path patterns: Do your file paths match the glob?
4. Test manually: `echo '{"prompt":"keyword"}' | npx tsx skill-activation-prompt.ts`

**Common fixes**:
- Update path patterns to match YOUR project structure (copy-paste examples need customization)
- Verify skill file exists and is readable
- Check for typos in keywords

### Too Many Skills Activating

**Symptom**: Every prompt loads 5+ skills, making context heavy.

**Solution**:
1. Be more specific with keywords
2. Use intent patterns instead of single words
3. Add `sessionTracking: true` to reduce repeats

### Skills Activating for Wrong Files

**Symptom**: Skill loads for files where it shouldn't.

**Solution**:
- Use negative patterns: `!excluded/**/*.ts`
- Use file markers: `// @skip-frontend` in files you want to exclude
- Make pathPatterns more specific

### Hook Running Slowly

**Symptom**: Noticeable delay when prompt submitted.

**Solution**:
- Reduce number of contentPatterns (they're slow)
- Use faster patterns (avoid expensive regex)
- Check if hook has a timeout issue

## Integration with Claude Code

The hook system integrates with Claude Code's built-in hook events:

**UserPromptSubmit** (what skill-activation-prompt uses):
- Fires: When user submits prompt
- Can: Read prompt, active files, recent edits
- Can: Suggest skills to load
- Common use: Auto-activation

**PostToolUse** (what post-tool-use-tracker uses):
- Fires: After tool execution
- Can: Detect what files changed
- Can: Track for context management
- Common use: Update memory/waypoints

**Stop** (optional):
- Fires: Before stopping
- Can: Run quality checks
- Can: Block stop if issues found
- Common use: Validation, testing

## Advanced: Conditional Activation

For complex logic, use contentPatterns with smart regex:

```json
{
  "stripe-skill": {
    "fileTriggers": {
      "contentPatterns": [
        "import.*?stripe",
        "stripeClient",
        "Stripe\\.",
        "stripe\\("
      ]
    }
  }
}
```

This activates if ANY pattern matches (OR logic). More flexible than keywords alone.

## Next Steps

1. **Customize skill-rules.json** for your project structure
2. **Test auto-activation** with your codebase
3. **Add custom skills** following [skill-developer guide](./../skills/skill-developer/SKILL.md)
4. **Iterate** based on what activates and what doesn't

For examples of well-configured skill-rules, see the production configs in this repository.

---

**Related Documentation**:
- [skill-developer skill](./../skills/skill-developer/SKILL.md) - Creating custom skills
- [memory-management skill](./../skills/memory-management/SKILL.md) - Learning from corrections
- [CUSTOMIZATION.md](../../CUSTOMIZATION.md) - Adapting skills to your tech stack

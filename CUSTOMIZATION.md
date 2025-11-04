# Customization Guide - Claude Code Waypoint Plugin

How to customize Claude Code Waypoint Plugin for your project's tech stack, coding style, and workflow preferences.

## Overview

Claude Code Waypoint Plugin ships with production-tested example scaffolds (Next.js + Supabase backend, React 19 frontend), but the core system works with ANY tech stack. This guide shows how to adapt it.

**The philosophy**: The memory/context system is universal. The tech stack examples are just examples. Customize for your stack, not the other way around.

## Core System vs. Scaffolds

### Core System (Works Everywhere)

These work with any tech stack:
- **Waypoint pattern** (plan.md, context.md, tasks.md) - Pure methodology
- **Memory management** (preferences, decision logging, correction learning) - Universal
- **Plan-approval workflow** - Universal pattern
- **Commands** (/create-plan, /update-context, /resume) - Tech-agnostic

### Scaffolds (Customizable Examples)

These are production-tested starting points:
- **frontend-dev-guidelines** - Example: Next.js + React 19 + shadcn/ui
- **backend-dev-guidelines** - Example: Supabase + Edge Functions

**Not using our stack?** Two approaches:

1. **Quick**: Delete scaffold skills, keep core system
2. **Better**: Customize scaffolds using scaffold-customizer agent

## Quick Start: Using the scaffold-customizer Agent

The easiest way to adapt Claude Code Waypoint Plugin for your stack:

```bash
# Invoke the agent
/agent scaffold-customizer
```

**What it does**:

1. **Detects your tech stack**
   - Reads package.json
   - Checks installed dependencies
   - Identifies frameworks and tools

2. **Asks for confirmation**
   ```
   Detected: Express + React + PostgreSQL
   Is this correct? (yes/no/details)
   ```

3. **Creates customization plan**
   ```
   Will replace:
   - Next.js → Express
   - shadcn/ui → Material-UI
   - Supabase → PostgreSQL with Knex
   - etc.
   ```

4. **Executes replacements**
   - Updates skill files
   - Modifies code examples
   - Adjusts patterns

5. **Verifies changes**
   - Tests JSON syntax
   - Checks all files updated
   - Generates report

**Result**: Scaffold skills customized for your stack, ready to use.

## Manual Customization: Step By Step

If you prefer manual control or scaffold-customizer doesn't cover your stack:

### Step 1: Understand Current Stack

Document your tech stack:

```markdown
# My Tech Stack

## Frontend
- Framework: [Vue 3 / Svelte / Angular]
- Styling: [Tailwind / CSS Modules / styled-components]
- State: [Pinia / Zustand / NgRx]
- Testing: [Vitest / Jest / Cypress]

## Backend
- Runtime: [Node.js / Python / Go / Rust]
- Framework: [Express / FastAPI / Echo / Actix-web]
- Database: [PostgreSQL / MySQL / MongoDB]
- ORM: [Sequelize / SQLAlchemy / SQLx]

## DevOps
- Hosting: [Vercel / Railway / AWS / Azure]
- Database hosting: [RDS / MongoDB Atlas / Supabase]
- CI/CD: [GitHub Actions / GitLab CI]
```

### Step 2: Decide What to Customize

**Option A: Delete Scaffolds, Keep Core**

If your stack is very different:

```bash
# Remove scaffold skills
rm -rf .claude/skills/frontend-dev-guidelines
rm -rf .claude/skills/backend-dev-guidelines

# Keep core skills
# - memory-management (works everywhere)
# - context-persistence (works everywhere)
# - plan-approval (works everywhere)
# - skill-developer (works everywhere)

# Update skill-rules.json to remove deleted skills
```

**Benefit**: Clean slate, no confusion

**Drawback**: No scaffold guidance for your stack

**Option B: Customize Scaffolds**

If you want guidance but different stack:

1. Keep scaffold skill directories
2. Update main SKILL.md file
3. Update all resource files
4. Update skill-rules.json triggers

**Benefit**: Keep guidance structure, adapt content

**Drawback**: More work, but comprehensive

### Step 3: Update Frontend Scaffold (If Customizing)

Edit `.claude/skills/frontend-dev-guidelines/SKILL.md`:

**Find section**:
```markdown
## Tech Stack

This skill covers Next.js 14+ App Router with React 19...
```

**Replace with your stack**:
```markdown
## Tech Stack

This skill covers [Your Framework] with [Your Language/JSX alternative]...
```

**Replace all**:
- `Next.js App Router` → `[Your routing approach]`
- `React 19` → `[Your framework version]`
- `shadcn/ui` → `[Your component library]`
- `Tailwind CSS` → `[Your CSS approach]`
- `React Hook Form + Zod` → `[Your form solution]`

**Example: Vue 3 + Vite + Pinia**

```markdown
## Tech Stack

This skill covers Vue 3 composition API with:
- **Vite**: Build tool and dev server
- **TypeScript**: Type-safe component development
- **Pinia**: State management (replacing Vuex)
- **Vue Router**: Client-side routing
- **Tailwind CSS**: Utility-first styling
- **Vitest**: Component and unit testing
- **Nuxt (optional)**: Meta-framework if desired
```

### Step 4: Update Resource Files

Each resource file in `.claude/skills/frontend-dev-guidelines/resources/`:

1. **component-patterns.md** - Update to your framework's component pattern
2. **data-fetching.md** - Update to your data fetching approach
3. **routing-guide.md** - Update to your routing library
4. **styling-guide.md** - Update to your CSS approach
5. **typescript-standards.md** - Can be mostly reused (TypeScript is universal)
6. **file-organization.md** - Update to your project structure conventions
7. **performance.md** - Update to framework-specific optimizations

**Template for updating each file**:

```markdown
# [Topic] - [Your Framework]

## Overview
[Explain how [topic] works in your framework]

## Best Practices
1. [Practice 1 for your framework]
2. [Practice 2 for your framework]

## Common Patterns
[Code examples using your framework]

## Anti-Patterns to Avoid
[Common mistakes in your framework]

## When to Break the Rules
[Exceptions]
```

### Step 5: Update Backend Scaffold (If Customizing)

Edit `.claude/skills/backend-dev-guidelines/SKILL.md`:

**Similar process**:
- Update runtime/framework references
- Replace database ORM references
- Update authentication patterns
- Replace example integrations

**Example: Python + FastAPI + PostgreSQL**

```markdown
## Tech Stack

This skill covers FastAPI with:
- **FastAPI**: Web framework (async by default)
- **SQLAlchemy**: ORM for PostgreSQL
- **Pydantic**: Data validation and serialization
- **Python 3.10+**: Type hints throughout
- **PostgreSQL**: Primary database
- **pytest**: Testing framework
- **Python-jose**: JWT handling
```

Then update all resource files with FastAPI patterns.

### Step 6: Update skill-rules.json

Edit `.claude/skills/skill-rules.json` to update file paths and keywords:

**Frontend Example: From Next.js to Vue**

```json
{
  "frontend-dev-guidelines": {
    "type": "domain",
    "enforcement": "suggest",
    "fileTriggers": {
      "pathPatterns": [
        "src/**/*.vue",           // Vue files instead of .tsx
        "src/**/*.ts",
        "src/**/*.js"
      ],
      "contentPatterns": [
        "import.*?Vue",           // Vue instead of React
        "<script setup>",          // Vue 3 composition API syntax
        "defineProps"              // Vue 3 macro
      ]
    },
    "promptTriggers": {
      "keywords": [
        "component",
        "vue",
        "frontend",
        "ui"
      ]
    }
  }
}
```

**Backend Example: From Supabase to Express**

```json
{
  "backend-dev-guidelines": {
    "type": "domain",
    "enforcement": "suggest",
    "fileTriggers": {
      "pathPatterns": [
        "src/api/**/*.ts",        // Your API path
        "src/routes/**/*.ts",
        "src/services/**/*.ts"
      ],
      "contentPatterns": [
        "import.*?express",       // Express instead of Supabase
        "app\\.get\\(",
        "app\\.post\\("
      ]
    },
    "promptTriggers": {
      "keywords": [
        "api",
        "route",
        "backend",
        "endpoint"
      ]
    }
  }
}
```

**Key: Customize paths to match YOUR project structure.**

## Creating Custom Skills

Don't see your pattern covered? Create a custom skill.

### Quick Example: Custom Skill for Stripe Integration

Create `.claude/skills/stripe-integration/SKILL.md`:

```markdown
# Stripe Integration - Custom Skill

## Overview
Best practices for integrating Stripe into your application.

## Core Patterns

### Customer Creation
```typescript
const customer = await stripe.customers.create({
  email: user.email,
  metadata: { userId: user.id }
});
```

### Subscription Management
[Your subscription patterns]

### Webhook Handling
[Your webhook patterns]

## Error Handling
[Common Stripe errors and how to handle]

## Testing
[How to test Stripe integration]
```

Add to skill-rules.json:

```json
{
  "stripe-integration": {
    "type": "domain",
    "enforcement": "suggest",
    "promptTriggers": {
      "keywords": ["stripe", "payment", "subscription", "billing"],
      "intentPatterns": ["(integrate|add|implement).*?stripe"]
    },
    "fileTriggers": {
      "pathPatterns": ["src/payment/**/*.ts"],
      "contentPatterns": ["import.*?stripe"]
    }
  }
}
```

See [skill-developer skill](./.claude/skills/skill-developer/SKILL.md) for complete guide.

## Customizing Preferences and Memory

### Project-Specific Preferences

Store your project's conventions in `.claude/memory/preferences.json`:

```json
{
  "imports": {
    "style": "named",
    "example": "import { useState } from 'react'",
    "reason": "Explicit imports help with tree-shaking and IDE navigation"
  },
  "naming": {
    "components": "PascalCase",
    "functions": "camelCase",
    "constants": "SCREAMING_SNAKE_CASE",
    "example": "const MAX_RETRIES = 3; function retryRequest() {...}"
  },
  "formatting": {
    "maxLineLength": 100,
    "indentation": "spaces:2",
    "trailingComma": "all",
    "semi": true
  },
  "testing": {
    "framework": "vitest",
    "coverage_target": 80,
    "test_file_pattern": "**/*.test.ts"
  }
}
```

Claude will load and apply these automatically.

### Custom Decision Patterns

Store architectural decisions in `.claude/memory/decisions/`:

```
.claude/
└── memory/
    └── decisions/
        ├── database-choice.json        # Why PostgreSQL vs MySQL
        ├── auth-approach.json          # Why JWT + refresh tokens
        ├── state-management.json       # Why Zustand vs Redux
        └── testing-strategy.json       # Why E2E over unit tests
```

Each file explains the decision and tradeoffs:

```json
{
  "decision": "Use PostgreSQL instead of MongoDB",
  "date": "2025-01-01",
  "participants": ["devteam"],
  "context": "Need ACID guarantees for financial transactions",
  "options": {
    "postgres": {
      "pros": ["ACID", "Joins", "Scalable to TB+", "mature"],
      "cons": ["Schema rigid"]
    },
    "mongodb": {
      "pros": ["Flexible schema", "JSON-native"],
      "cons": ["No ACID", "No real joins", "Expensive at scale"]
    }
  },
  "chosen": "postgres",
  "rationale": "ACID guarantees critical for financial data",
  "revise_if": "Regulatory requirements change"
}
```

Claude will reference these when designing systems.

## Customizing File Organization

Update `.claude/memory/file-organization.json`:

```json
{
  "frontend": {
    "structure": "Atomic + Feature-based hybrid",
    "components": "src/components/[feature]/[component]",
    "hooks": "src/hooks/",
    "utils": "src/utils/[domain]/",
    "styles": "inline with components (CSS-in-JS)"
  },
  "backend": {
    "structure": "Service + Repository pattern",
    "controllers": "src/controllers/",
    "services": "src/services/",
    "repositories": "src/repositories/",
    "routes": "src/routes/[domain].ts",
    "middleware": "src/middleware/"
  },
  "tests": "tests/[feature]/[component].test.ts"
}
```

This helps Claude maintain consistency.

## Troubleshooting Customization

### "Skill content doesn't match my framework"

**Solution**: Edit the skill resource files directly. They're markdown, easy to update.

### "I customized skill-rules.json but it's not working"

**Debug**:
```bash
# 1. Validate JSON
cat .claude/skills/skill-rules.json | jq .

# 2. Check skill file exists
ls .claude/skills/[skill-name]/SKILL.md

# 3. Test manually
echo '{"prompt":"test"}' | npx tsx .claude/hooks/skill-activation-prompt.ts

# 4. Check for typos in skill names
```

### "My preferences aren't being applied"

**Check**:
- Preferences stored in `.claude/memory/preferences.json`
- File is valid JSON
- Preferences file is readable by hook system

### "Skill activates too often"

**Solution**: Make triggers more specific or add `sessionTracking: true`.

## Customizing for Different Project Types

### Project Type: SaaS with Custom Dashboard

Focus on:
1. **Complex state management** - Add Zustand/Pinia patterns
2. **Real-time updates** - Add WebSocket patterns
3. **Analytics** - Add instrumentation patterns
4. **Performance** - Optimize for large datasets

Customize skills to emphasize these.

### Project Type: Open Source Library

Focus on:
1. **Public API design** - Clear contracts
2. **Backwards compatibility** - Version management
3. **Documentation** - Generated from code
4. **Testing** - Comprehensive coverage

Update memory with library-specific patterns.

### Project Type: Mobile App

Focus on:
1. **Offline-first architecture** - Sync patterns
2. **Performance** - Mobile-optimized code
3. **Testing** - Mobile-specific edge cases
4. **Battery/data usage** - Efficiency patterns

Create mobile-specific skills.

## Customizing Auto-Activation

### Adjust Trigger Sensitivity

**Too many activations?** Make triggers more specific:

```json
{
  "frontend-dev-guidelines": {
    "promptTriggers": {
      "keywords": [
        "component",      // Keep this (specific)
        "react"           // Keep this (specific)
        // Remove generic: "create", "build" (too generic)
      ]
    }
  }
}
```

**Too few activations?** Add more triggers:

```json
{
  "frontend-dev-guidelines": {
    "fileTriggers": {
      "pathPatterns": [
        "src/**/*.tsx",
        "src/**/*.jsx",
        "src/**/*.ts",      // Also activate for .ts if it has React imports
        "components/**/*"
      ]
    }
  }
}
```

### Add Context-Specific Activation

Add skills that activate for your specific patterns:

```json
{
  "database-migration-safety": {
    "type": "guardrail",
    "enforcement": "block",
    "fileTriggers": {
      "pathPatterns": ["db/migrations/**/*.sql"]
    },
    "promptTriggers": {
      "keywords": ["migration", "schema", "drop"]
    }
  }
}
```

This blocks dangerous operations in migrations.

## Testing Your Customization

### Step 1: Validate JSON

```bash
cat .claude/skills/skill-rules.json | jq .
cat .claude/memory/preferences.json | jq .
```

Both must be valid JSON.

### Step 2: Test Auto-Activation

```bash
# For your frontend file
echo '{"prompt":"create component","activeFiles":["src/components/test.tsx"]}' | \
  npx tsx .claude/hooks/skill-activation-prompt.ts

# Should show: frontend-dev-guidelines activated
```

### Step 3: Test in Real Project

1. Edit a file that should trigger skill
2. Type a prompt that should trigger skill
3. Watch for skill to load
4. Verify content is correct for your stack

### Step 4: Iterate

If skills don't activate:
1. Check file paths match your structure
2. Check keywords match your terminology
3. Run manual tests above
4. Adjust skill-rules.json

## Sharing Your Customization

### Creating a Reusable Scaffold

If you've customized Claude Code Waypoint Plugin for a popular stack (e.g., Vue + FastAPI), consider sharing:

```bash
git clone https://github.com/DojoCodingLabs/claude-code-waypoint
cd claude-code-waypoint

# Make your customizations
# Test thoroughly

git checkout -b scaffold/vue-fastapi
git add -A
git commit -m "Add Vue 3 + FastAPI scaffold"
git push origin scaffold/vue-fastapi

# Create pull request
# Include: what stack, what changed, testing notes
```

Others can then:
```bash
git pull origin scaffold/vue-fastapi
git checkout scaffold/vue-fastapi
```

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## Common Customization Patterns

### Pattern: Multi-Framework Project

If you use multiple frameworks (Next.js frontend, Python backend):

```json
{
  "frontend-dev-guidelines": {
    "fileTriggers": {
      "pathPatterns": ["frontend/**/*.tsx", "frontend/**/*.ts"]
    }
  },
  "backend-dev-guidelines": {
    "fileTriggers": {
      "pathPatterns": ["backend/**/*.py"]
    }
  }
}
```

Each skill activates only for its part of the project.

### Pattern: Microservices

If you have multiple services:

```json
{
  "user-service": {
    "fileTriggers": {
      "pathPatterns": ["services/user/**/*"]
    }
  },
  "payment-service": {
    "fileTriggers": {
      "pathPatterns": ["services/payment/**/*"]
    }
  },
  "notification-service": {
    "fileTriggers": {
      "pathPatterns": ["services/notification/**/*"]
    }
  }
}
```

Each skill activates for its service.

### Pattern: Legacy + Modern Code

If gradually modernizing:

```json
{
  "modern-react-patterns": {
    "fileTriggers": {
      "pathPatterns": ["src/components/new/**/*"],
      "contentPatterns": ["functional component pattern"]
    }
  },
  "legacy-patterns": {
    "fileTriggers": {
      "pathPatterns": ["src/legacy/**/*"],
      "fileMarkers": ["// @legacy"]
    }
  }
}
```

Different guidance for different parts.

## Troubleshooting Customization

### "Skill content doesn't match my framework"

**Solution**: Edit the skill resource files directly. They're markdown, easy to update.

### "I customized skill-rules.json but it's not working"

**Debug**:
```bash
# 1. Validate JSON
cat .claude/skills/skill-rules.json | jq .

# 2. Check skill file exists
ls .claude/skills/[skill-name]/SKILL.md

# 3. Test manually
echo '{"prompt":"test"}' | npx tsx .claude/hooks/skill-activation-prompt.ts

# 4. Check for typos in skill names
```

### "My preferences aren't being applied"

**Check**:
- Preferences stored in `.claude/memory/preferences.json`
- File is valid JSON
- Preferences file is readable by hook system

### "Skill activates too often"

**Solution**: Make triggers more specific or add `sessionTracking: true`.

## Getting Help

### Still Need Help?

1. **Use the scaffold-customizer agent** (handles most cases)
2. **Ask Claude**: "I need help customizing for [your stack]"
3. **Check examples**: See [skill-developer](/.claude/skills/skill-developer/SKILL.md)
4. **Open an issue**: [GitHub Issues](https://github.com/DojoCodingLabs/claude-code-waypoint/issues)

## Next Steps

1. **Choose**: Delete scaffolds OR customize them
2. **Update**: skill-rules.json for your file structure
3. **Test**: Manual activation tests
4. **Refine**: Iterate based on what activates
5. **Extend**: Add custom skills for your patterns
6. **Store**: Save preferences and decisions in `.claude/memory/`

---

**Related Documentation**:
- [WAYPOINT_PATTERN.md](WAYPOINT_PATTERN.md) - Core pattern (works everywhere)
- [skill-developer skill](./.claude/skills/skill-developer/SKILL.md) - Creating custom skills
- [AUTO_ACTIVATION.md](./.claude/hooks/AUTO_ACTIVATION.md) - Detailed activation system
- [CONTRIBUTING.md](CONTRIBUTING.md) - Sharing your customization

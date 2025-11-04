# Scaffold Customization Guide

Learn how to customize the opinionated skills for your specific tech stack.

## What Are Scaffolds?

**Scaffolds** are production-tested skills with opinionated, real-world examples.

This plugin comes with scaffolds for:
- **Backend**: Supabase + Edge Functions + TypeScript
- **Frontend**: Next.js + React 19 + shadcn/ui + Tailwind CSS

**These are not requirements** - they're starting points you can customize.

## When to Customize

### Keep Default Scaffolds If:
- ✅ You're using or planning to use our opinionated stack
- ✅ You're starting a new project and open to modern patterns
- ✅ You want production-tested examples

### Customize Scaffolds If:
- ⚠️ You're using different frameworks (Express, NestJS, Vue, Angular)
- ⚠️ You're using different libraries (Prisma, TypeORM, MUI, Chakra)
- ⚠️ You have strong preferences for specific tools

## Customization Methods

### Method 1: Automated with scaffold-customizer Agent (Recommended)

**Best for**: Full stack replacement

**How it works**:

1. **Invoke the agent**:
```
User: "I need to customize the skills for my tech stack"
Claude: [Launches scaffold-customizer agent]
```

2. **Agent detects your stack**:
```
Analyzing project...

Detected Stack:
- Backend: Express + Prisma + PostgreSQL
- Frontend: React + Vite + MUI v6
- Styling: Emotion (CSS-in-JS)
- Testing: Jest + React Testing Library

Is this correct? (yes/no/modify)
```

3. **Confirm or correct**:
```
User: yes
Claude: Creating customization plan...
```

4. **Agent creates plan**:
```
Customization Plan

Backend Replacements:
  "Supabase" → "Express"
  "Edge Functions" → "Express Routes"
  "Supabase Auth" → "Passport.js"
  "RLS policies" → "Middleware auth checks"

Frontend Replacements:
  "Next.js" → "Vite"
  "shadcn/ui" → "MUI v6"
  "Tailwind CSS" → "Emotion"

Files to modify: 8
Estimated time: 5 minutes

Approve to proceed?
```

5. **Approve and execute**:
```
User: approve
Claude: [Executes all replacements]
       [Verifies changes]
       [Generates report]

✅ Customization complete!

Modified files:
- .claude/skills/backend-dev-guidelines/SKILL.md
- .claude/skills/frontend-dev-guidelines/SKILL.md
- .claude/skills/skill-rules.json

Your skills are now customized for: Express + Prisma + React + Vite + MUI v6
```

### Method 2: Manual Replacement

**Best for**: Specific library swaps, partial customization

**Steps**:

1. **Identify replacement patterns**:
```bash
# Find all instances of tech to replace
grep -r "Supabase" .claude/skills/backend-dev-guidelines/
grep -r "Next.js" .claude/skills/frontend-dev-guidelines/
```

2. **Create replacement map**:
```
Supabase → Your database (e.g., Prisma + PostgreSQL)
Edge Functions → Your backend framework (e.g., Express routes)
Next.js → Your frontend framework (e.g., Vite + React)
shadcn/ui → Your component library (e.g., MUI, Chakra)
```

3. **Execute replacements**:

**Backend Example** (Supabase → Prisma):

File: `.claude/skills/backend-dev-guidelines/SKILL.md`

**Find**:
```markdown
## Supabase Client Setup

```typescript
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(
  process.env.SUPABASE_URL,
  process.env.SUPABASE_ANON_KEY
)
```
```

**Replace with**:
```markdown
## Prisma Client Setup

```typescript
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient()
```
```

**Frontend Example** (Next.js → Vite):

File: `.claude/skills/frontend-dev-guidelines/SKILL.md`

**Find**:
```markdown
## Next.js App Router

Use the App Router pattern:
- `app/` directory for routes
- Server Components by default
- 'use client' for client components
```

**Replace with**:
```markdown
## Vite + React Router

Use React Router for routing:
- `src/routes/` directory for routes
- React components for pages
- `<BrowserRouter>` for navigation
```

4. **Update skill-rules.json triggers**:

**Find**:
```json
{
  "backend-dev-guidelines": {
    "promptTriggers": {
      "keywords": ["supabase", "edge function"]
    },
    "fileTriggers": {
      "pathPatterns": ["supabase/functions/**/*.ts"]
    }
  }
}
```

**Replace with**:
```json
{
  "backend-dev-guidelines": {
    "promptTriggers": {
      "keywords": ["prisma", "express", "database"]
    },
    "fileTriggers": {
      "pathPatterns": ["src/routes/**/*.ts", "prisma/**/*.prisma"]
    }
  }
}
```

### Method 3: Create New Skill (Advanced)

**Best for**: Completely different stack, want fresh start

**Steps**:

1. **Copy skill template**:
```bash
cp -r .claude/skills/backend-dev-guidelines .claude/skills/my-backend
```

2. **Rename files**:
```bash
mv .claude/skills/my-backend/SKILL.md \
   .claude/skills/my-backend/MY-BACKEND-SKILL.md
```

3. **Update YAML frontmatter**:
```markdown
---
name: my-backend-guidelines
description: Backend patterns for [your stack]
---
```

4. **Replace all content** with your stack's patterns

5. **Add to skill-rules.json**:
```json
{
  "my-backend-guidelines": {
    "type": "domain",
    "enforcement": "suggest",
    "priority": "high",
    "description": "Backend patterns for [your stack]",
    "promptTriggers": {
      "keywords": ["your", "keywords"]
    }
  }
}
```

## Common Customization Scenarios

### Scenario 1: Express + Prisma Backend

**What to replace**:
```
Supabase → Prisma
Edge Functions → Express routes
Supabase Auth → Passport.js
Row-Level Security → Middleware + permissions
createClient → PrismaClient
```

**File patterns to update**:
```json
{
  "pathPatterns": [
    "src/routes/**/*.ts",
    "src/controllers/**/*.ts",
    "prisma/schema.prisma"
  ]
}
```

**Keywords to update**:
```
["prisma", "express", "middleware", "controller", "route"]
```

### Scenario 2: Vue + Vuetify Frontend

**What to replace**:
```
Next.js → Vue 3
React → Vue
shadcn/ui → Vuetify
Server Components → Vue components
'use client' → <script setup>
```

**File patterns to update**:
```json
{
  "pathPatterns": [
    "src/**/*.vue",
    "src/components/**/*.vue",
    "src/views/**/*.vue"
  ]
}
```

**Keywords to update**:
```
["vue", "vuetify", "composition api", "pinia", "vue router"]
```

### Scenario 3: NestJS + TypeORM Backend

**What to replace**:
```
Supabase → TypeORM
Edge Functions → NestJS controllers
Supabase Auth → NestJS guards
createClient → @InjectRepository
```

**File patterns to update**:
```json
{
  "pathPatterns": [
    "src/**/*.controller.ts",
    "src/**/*.service.ts",
    "src/**/*.entity.ts"
  ]
}
```

**Keywords to update**:
```
["nestjs", "typeorm", "controller", "service", "guard", "decorator"]
```

## Replacement Checklist

When customizing, update:

- [ ] **SKILL.md** - Main skill file
  - [ ] YAML frontmatter (name, description)
  - [ ] Scaffold marker section
  - [ ] Code examples throughout
  - [ ] File path examples
  - [ ] Import statements
  - [ ] Best practices sections

- [ ] **Resource files** (if any)
  - [ ] All `.md` files in resources/ directory
  - [ ] Consistent replacements across all files

- [ ] **skill-rules.json**
  - [ ] `description` field
  - [ ] `techStack` array (if present)
  - [ ] `promptTriggers.keywords`
  - [ ] `fileTriggers.pathPatterns`
  - [ ] `fileTriggers.contentPatterns`

- [ ] **Test the changes**
  - [ ] Edit a file matching new patterns
  - [ ] Verify skill activates
  - [ ] Ask questions with new keywords
  - [ ] Verify examples are relevant

## Verification

### After Customization, Test:

1. **File Triggers**:
```bash
# Create or edit a file matching your patterns
code src/routes/users.ts  # (or your equivalent)

# Skill should activate automatically
```

2. **Keyword Triggers**:
```
# Ask Claude a question with your keywords
User: "How do I set up Express routes?"

# Your customized skill should be suggested
```

3. **Examples are Relevant**:
```
# Ask for code examples
User: "Show me how to create a new controller"

# Examples should match YOUR stack, not the default
```

## Troubleshooting

### Skill Not Activating After Customization

**Problem**: Modified skill doesn't activate

**Solutions**:

1. **Check JSON syntax**:
```bash
cat .claude/skills/skill-rules.json | jq '.'
# Should not show errors
```

2. **Verify file patterns match**:
```bash
# Your file: src/routes/users.ts
# Pattern should include: "src/routes/**/*.ts"
```

3. **Check keyword spelling**:
```json
{
  "keywords": ["express"]  // Not "expres" or "Express"
}
```

4. **Restart Claude Code**

### Examples Still Show Default Stack

**Problem**: Code examples still reference Supabase/Next.js

**Cause**: Missed some replacements

**Solution**:

```bash
# Find all remaining instances
grep -r "Supabase" .claude/skills/backend-dev-guidelines/
grep -r "Next.js" .claude/skills/frontend-dev-guidelines/

# Replace each one manually or with sed
sed -i '' 's/Supabase/Prisma/g' file.md
```

### Conflicts with Multiple Patterns

**Problem**: Both old and new patterns present

**Solution**: Be thorough with find-and-replace:

```bash
# Find all files with old pattern
grep -rl "Supabase" .claude/skills/

# Replace in all files at once
grep -rl "Supabase" .claude/skills/ | xargs sed -i '' 's/Supabase/Prisma/g'
```

## Best Practices

### DO ✅

1. **Test after each major change**
2. **Keep a backup** before customization
3. **Update all related files** together
4. **Use consistent terminology** throughout
5. **Document your customizations** in comments
6. **Verify examples match your stack**

### DON'T ❌

1. **Don't mix stacks in examples** (Express + Next.js in same example)
2. **Don't forget skill-rules.json** (skill won't activate)
3. **Don't use generic placeholders** ("your-framework-here")
4. **Don't skip resource files** (maintain consistency)
5. **Don't forget to test** (verification is critical)

## Getting Help

### Still Need Help?

1. **Use the scaffold-customizer agent** (handles most cases)
2. **Ask Claude**: "I need help customizing for [your stack]"
3. **Check examples**: See [skill-developer](/.claude/skills/skill-developer/SKILL.md)
4. **Open an issue**: [GitHub Issues](https://github.com/DojoCodingLabs/claude-code-waypoint/issues)

---

**Remember**: Customization is optional. If you're starting a new project, consider using our opinionated stack - it's production-tested and works great!

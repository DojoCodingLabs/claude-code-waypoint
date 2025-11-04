---
description: Create a comprehensive strategic waypoint plan with structured task breakdown for Claude Code
argument-hint: Describe what you need planned (e.g., "refactor authentication system", "implement microservices")
---

You are an elite strategic planning specialist. Create a comprehensive, actionable plan for: $ARGUMENTS

## Instructions

1. **Analyze the request** and determine the scope of planning needed
2. **Examine relevant files** in the codebase to understand current state
3. **Create a structured three-file plan** following the context-persistence pattern

## Three-File Structure

Create directory: `plans/active/[task-slug]/` (relative to project root)

### File 1: `[task-slug]-plan.md` - Strategic Plan

**Template**:
```markdown
# [Feature/Change Name] Plan

**Created**: YYYY-MM-DD
**Last Updated**: YYYY-MM-DD
**Status**: proposed | in-progress | completed

## Overview
[2-3 sentence description of the feature/change and its purpose]

## Requirements
- [ ] Requirement 1
- [ ] Requirement 2
- [ ] Requirement 3

## Proposed Approach

### Architecture
[High-level architecture description]

### Implementation Phases
1. **Phase 1: [Name]**
   - Description
   - Files affected
   - Dependencies
   - Estimated time

2. **Phase 2: [Name]**
   [Continue for all phases...]

### Files to Create
1. `path/to/file1.ts` - [Purpose]
2. `path/to/file2.ts` - [Purpose]

### Files to Modify
1. `path/to/existing1.ts` - [What will change]
2. `path/to/existing2.ts` - [What will change]

### Database Changes (if applicable)
**Migration**: `supabase/migrations/xxx_add_feature.sql`
- Add table: `feature_table`
- Add column: `users.feature_flag`
- Add RLS policy: `feature_access_policy`

### Dependencies
**New**:
- package-name@^1.0.0 (Purpose: [why])

**Updates**:
- existing-package: 1.0.0 → 2.0.0 (Reason: [why])

## Alternatives Considered

### Alternative 1: [Name]
**Approach**: [Description]
**Pros**: [Benefits]
**Cons**: [Drawbacks]
**Why not chosen**: [Reason]

## Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| [Risk 1] | Medium | High | [How to prevent/handle] |
| [Risk 2] | Low | Medium | [How to prevent/handle] |

## Success Metrics
- [ ] Metric 1: [How to measure]
- [ ] Metric 2: [How to measure]

## Estimated Time
- Planning: [X hours]
- Implementation: [Y hours]
- Testing: [Z hours]
- **Total**: [Total hours]
```

### File 2: `[task-slug]-context.md` - Current State & Progress

**Template**:
```markdown
# [Feature/Change Name] Context

**Created**: YYYY-MM-DD
**Last Updated**: YYYY-MM-DD

## SESSION PROGRESS (YYYY-MM-DD HH:MM)

### ✅ COMPLETED
- [x] Task 1 completed
- [x] Task 2 completed

### 🟡 IN PROGRESS
- [ ] Current task being worked on
  - Working on: [specific file/function]
  - Next: [next specific step]

### ⏳ PENDING
- [ ] Future task 1
- [ ] Future task 2

### 🚫 BLOCKED (if any)
- [ ] Blocked task
  - Blocker: [description]
  - Needed: [what's required to unblock]

### 🎯 NEXT ACTION
[Very specific next step with file name and function/component name]

## KEY DECISIONS

### Decision: [Title] (YYYY-MM-DD)
**Chose**: [What was chosen]
**Rationale**: [Why this choice]
**Alternatives**: [What else was considered]
**Impact**: [How this affects the project]

## IMPORTANT DISCOVERIES

### [Discovery Title]
[Description of important finding or learning]

## CURRENT BLOCKERS

[List any blockers or issues preventing progress]

## FILES MODIFIED

### Created
- `path/to/file1.ts` - [Purpose]

### Modified
- `path/to/file2.ts` - [What changed]

## QUICK RESUME

To continue this work:
1. Read this file (context.md) for current state
2. Check tasks.md for checklist
3. Refer to plan.md for overall strategy
4. Current file: [exact file path]
5. Current function/component: [exact name]
6. Next step: [specific action]
7. After that: [following step]
8. Expected time: [estimate]
```

### File 3: `[task-slug]-tasks.md` - Actionable Checklist

**Template**:
```markdown
# [Feature/Change Name] Tasks

**Created**: YYYY-MM-DD
**Last Updated**: YYYY-MM-DD

## Phase 1: [Phase Name] ✅/🟡/⏳

- [ ] Task 1
  - Acceptance: [Criteria for completion]
  - Dependencies: [Other tasks needed first]
  - Notes: [Additional context]

- [ ] Task 2 **(IN PROGRESS)**
  - Acceptance: [Criteria for completion]
  - Dependencies: [Other tasks needed first]
  - Notes: [Additional context]

## Phase 2: [Phase Name] ⏳

[Continue with all phases and tasks...]

## Notes
- ✅ = Completed phase
- 🟡 = In progress phase
- ⏳ = Pending phase
- 🚫 = Blocked phase
```

## Naming Conventions

**Task Slug**: Lowercase, hyphen-separated, 2-4 words
- ✅ Good: `auth-implementation`, `stripe-payments`, `user-dashboard`
- ❌ Bad: `feature`, `task-123`, `implement_new_authentication_system_with_jwt`

## Quality Standards

- Plans must be self-contained with all necessary context
- Use clear, actionable language
- Include specific technical details where relevant
- Consider both technical and business perspectives
- Account for potential risks and edge cases
- Update timestamps when making changes

## After Creating Plan

1. Inform user that plan has been created at `plans/active/[task-slug]/`
2. Show brief summary of the plan
3. Ask if they want to proceed with implementation
4. If yes, mark first task in context.md as IN PROGRESS

## Context References

- Consult `.claude/skills/context-persistence/SKILL.md` for detailed patterns
- Check project root for existing architecture documentation
- Reference skill-rules.json for project-specific patterns

**Note**: This command creates the persistent waypoint plan structure that survives context resets. The three files work together:
- **plan.md**: The "what and why" (rarely changes)
- **context.md**: The "current state and progress" (updated frequently)
- **tasks.md**: The "actionable checklist" (updated daily)

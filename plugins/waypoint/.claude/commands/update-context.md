---
description: Update waypoint plan before context compaction in Claude Code
argument-hint: Optional - specific context or tasks to focus on (leave empty for comprehensive update)
---

We're approaching context limits. Please update the waypoint plan documentation to ensure seamless continuation after context reset.

## Priority: Update SESSION PROGRESS

This is the **most critical** section for context survival. Update immediately:

### For each active task in `plans/active/*/`:

**File**: `[task-slug]-context.md`

Update the `## SESSION PROGRESS` section with current timestamp:

```markdown
## SESSION PROGRESS (YYYY-MM-DD HH:MM)

### ✅ COMPLETED
[List all completed tasks - be specific with file names]

### 🟡 IN PROGRESS
[What you're working on RIGHT NOW with exact file and function/component]

### ⏳ PENDING
[What's coming next in priority order]

### 🚫 BLOCKED (if any)
[Specific blockers with what's needed to unblock]

### 🎯 NEXT ACTION
[VERY SPECIFIC next step: file name + function + exact action]
Example: "Implement refreshToken() function in lib/supabase/auth.ts to handle expired tokens"
```

## Required Updates

### 1. Update Active Task Context Files

For each `[task-slug]-context.md`:

**Key Decisions Section**:
```markdown
## KEY DECISIONS

### Decision: [Title] (YYYY-MM-DD)
**Chose**: [What was decided]
**Rationale**: [Why this approach]
**Alternatives**: [What else was considered]
**Impact**: [Files affected, breaking changes]
```

**Important Discoveries Section**:
```markdown
## IMPORTANT DISCOVERIES

### [Discovery Title]
[Technical findings, gotchas, important patterns discovered]
```

**Files Modified Section**:
```markdown
## FILES MODIFIED

### Created
- `path/to/file.ts` - [Purpose]

### Modified
- `path/to/file.ts` - [What changed and why]
```

**Quick Resume Section**:
```markdown
## QUICK RESUME

To continue this work:
1. Read this file (context.md) for current state
2. Check tasks.md for checklist
3. Refer to plan.md for overall strategy
4. Current file: [exact file path]
5. Current function/component: [exact name]
6. Next step: [specific action to take]
7. After that: [following step]
8. Expected time: [estimate for next step]
```

### 2. Update Task Checklists

For each `[task-slug]-tasks.md`:
- Mark completed tasks with `[x]` immediately
- Add **(IN PROGRESS)** marker to current task
- Add any newly discovered tasks
- Update acceptance criteria if changed
- Reorder priorities if needed

### 3. Capture Session-Specific Context

Include in relevant context.md files:

**Complex Problems Solved**:
- What was the problem?
- What approach worked?
- Why did other approaches fail?
- Code examples if relevant

**Tricky Bugs Fixed**:
- Bug description
- Root cause
- Fix applied
- How to prevent similar bugs

**Architectural Decisions**:
- What changed in architecture
- Rationale for the change
- Impact on other components
- Migration path if breaking

**Integration Discoveries**:
- How components/services interact
- API contracts discovered
- Data flow patterns
- Edge cases found

### 4. Update Memory Files (if applicable)

If significant preferences or patterns emerged, update:

**`.claude/memory/project/knowledge.json`**:
```json
{
  "tech_stack": { /* current stack info */ },
  "structure": { /* project organization */ },
  "last_verified": "YYYY-MM-DD"
}
```

**`.claude/memory/project/decisions.json`**:
```json
{
  "decisions": [
    {
      "id": "unique-id-YYYY-MM-DD",
      "date": "YYYY-MM-DD",
      "decision": "What was decided",
      "rationale": "Why",
      "alternatives_considered": ["Alt 1", "Alt 2"],
      "impact": "How it affects project",
      "files_affected": ["list", "of", "files"]
    }
  ]
}
```

### 5. Document Unfinished Work

**Critical for resume**:

```markdown
## UNFINISHED WORK

### What I Was Doing
[Exact task being worked on when context limit approached]

### Exact State
- File: [exact file path]
- Function/Component: [exact name]
- Line number: [if relevant]
- What was being implemented: [specific detail]

### Why It's Unfinished
[Blocker, context limit, or just in progress]

### To Complete It
1. [Specific step 1]
2. [Specific step 2]
3. [Specific step 3]

### Test Commands
```bash
# Commands to verify the work when resumed
npm run test
npm run build
```
```

### 6. Update All Timestamps

Ensure all files have current timestamps:
```markdown
**Last Updated**: YYYY-MM-DD HH:MM
```

## Additional Context: $ARGUMENTS

## Verification Checklist

Before finishing, verify:
- [ ] All SESSION PROGRESS sections updated with current timestamp
- [ ] NEXT ACTION is very specific (file + function + action)
- [ ] Completed tasks marked as [x]
- [ ] Key decisions documented with rationale
- [ ] Files modified section is complete
- [ ] Quick resume instructions are current
- [ ] All timestamps updated
- [ ] Any blockers clearly described

## After Updating

1. Inform user which files were updated
2. Show brief summary of current state
3. Confirm next action is clear
4. Suggest they can safely continue after context reset

**Priority**: Focus on capturing information that would be hard to rediscover or reconstruct from code alone. The goal is < 1 minute resume time after context reset.

## See Also

- `.claude/skills/context-persistence/SKILL.md` - Full waypoint plan patterns
- `.claude/skills/memory-management/SKILL.md` - Memory storage patterns
- `/create-plan` - Create new waypoint plan structure
- `/resume` - Resume work from existing context

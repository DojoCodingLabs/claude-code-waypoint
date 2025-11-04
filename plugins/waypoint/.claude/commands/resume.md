---
description: Resume work from existing waypoint plan after context reset or break in Claude Code
argument-hint: Optional - specific task slug to resume (e.g., "auth-implementation"). Leave empty to see all active tasks.
---

Resume work from waypoint plan documentation.

## Task Selection

**If argument provided**: Resume specific task `$ARGUMENTS`
**If no argument**: Show all active tasks and let user choose

## Step 1: Discover Active Tasks

List all directories in `plans/active/`:

```bash
ls -la plans/active/
```

For each task directory found, read the context file to get:
- Task name
- Last updated timestamp
- Current status
- Progress summary

## Step 2: Display Task Summary

If multiple tasks exist and no argument provided:

```markdown
## Active Tasks

### 📂 [task-slug-1]
- **Last Updated**: 2 hours ago (YYYY-MM-DD HH:MM)
- **Status**: In Progress
- **Progress**: 5/12 tasks completed (42%)
- **Current**: Implementing token refresh in auth.ts
- **Next**: Test token refresh flow

### 📂 [task-slug-2]
- **Last Updated**: 1 day ago (YYYY-MM-DD HH:MM)
- **Status**: Blocked
- **Progress**: 3/8 tasks completed (38%)
- **Current**: Waiting for Stripe API keys
- **Blocker**: Need production Stripe keys from team

---

Which task would you like to resume? (Or I can help with the most recent: [task-slug-1])
```

## Step 3: Load Task Context

For the selected task (either from argument or user choice):

### Read All Three Files

1. **`[task-slug]-plan.md`** - Overall strategy
2. **`[task-slug]-context.md`** - Current state and progress
3. **`[task-slug]-tasks.md`** - Detailed checklist

### Check Context Freshness

```markdown
## Context Freshness Check

**Last Updated**: [timestamp]
**Age**: [X hours/days ago]

**Status**:
✅ Fresh (< 3 hours) - Context is current
⚠️ Moderate (3 hours - 1 day) - Might need verification
🚨 Stale (> 1 day) - Should verify code state matches context
```

If context is stale (> 1 day):
```markdown
⚠️ **Context is stale** (last updated [X days] ago)

**Recommended Actions**:
1. Verify current code state matches context
2. Check for changes by others: `git log --since="[last-update-date]" --oneline`
3. Update context.md before proceeding
4. Run tests to verify current state

Would you like me to:
- [ ] Refresh context automatically
- [ ] Show git changes since last update
- [ ] Proceed anyway (I trust the context)
```

## Step 4: Present Context Summary

```markdown
# Resuming: [Task Name]

## 📋 Context Summary

**Last Updated**: [timestamp] ([X hours/days] ago)
**Status**: [proposed/in-progress/blocked]
**Overall Progress**: [X/Y] tasks completed ([Z%])

## ✅ What's Been Completed ([X] tasks)

- [x] Task 1 completed
- [x] Task 2 completed
- [x] Task 3 completed

## 🟡 What's In Progress ([X] task)

- [ ] **Current Task**: [task description]
  - **Working on**: [specific file and function]
  - **Status**: [detailed status]

## ⏳ What's Coming Next ([X] tasks)

- [ ] Next task 1
- [ ] Next task 2
- [ ] Next task 3

## 🚫 Blockers (if any)

- **Blocker**: [description]
- **Needed**: [what's required to unblock]

## 🎯 Next Action

**File**: `[exact file path]`
**Function/Component**: `[exact name]`
**Action**: [Very specific action to take]

**After That**: [Following step]
**Estimated Time**: [time estimate]

---

## 📁 Files to Review

### Recently Modified
- `[file1]` - [what changed]
- `[file2]` - [what changed]

### Key Files
- `[file3]` - [purpose]
- `[file4]` - [purpose]

---

## 🔑 Key Decisions Made

[List important decisions from context.md]

## 💡 Important Discoveries

[List important findings from context.md]

---

## Quick Start Commands

```bash
# Review current implementation
code [current-file-path]

# Run tests
npm run test

# Run development server (if applicable)
npm run dev

# Check git status
git status
```

---

**Ready to continue?** I'll start working on: [NEXT ACTION]

Reply 'yes' to proceed, or tell me what you'd like to do differently.
```

## Step 5: Verify Code State (Optional)

If context is stale or user wants verification:

```bash
# Check recent git changes
git log --since="[last-update-date]" --oneline --decorate

# Check current file state
ls -la [files from context]

# Run tests to verify state
npm run test 2>&1 | head -20
```

Present findings:
```markdown
## Code State Verification

### Git Changes Since Last Update
[Show commits if any]

### File Status
✅ All files from context exist
⚠️ [file] was modified by others
🚨 [file] no longer exists

### Test Status
✅ All tests passing
⚠️ [X] tests failing (might be expected)
🚨 Build is broken

**Recommendation**: [Update context / Proceed / Investigate]
```

## Step 6: Begin Work

If everything looks good:

```markdown
✅ Context loaded successfully!

**Starting work on**: [NEXT ACTION]

I'll keep you updated as I make progress. Use `/update-context` before any long break or when approaching context limits.
```

Then proceed with the next action from the context.

## Special Cases

### No Active Tasks Found

```markdown
📂 No active tasks found in `plans/active/`

Would you like to:
1. Create a new plan with `/create-plan [task-name]`
2. Check archived tasks in `plans/archived/`
3. Start working without formal waypoint plans
```

### Multiple Tasks, User Needs Help Choosing

```markdown
You have [X] active tasks. Here's my recommendation:

**Most Urgent**: [task-name]
- Last worked on [time] ago
- [X%] complete
- Currently: [status]

**Also Active**:
- [other-task-1]: [brief status]
- [other-task-2]: [brief status]

Which would you like to resume? (Or I can help you decide based on priority)
```

### Context Files Corrupted or Incomplete

```markdown
⚠️ **Context Issue Detected**

The context files for [task-slug] appear incomplete or corrupted:
- Missing: [list missing sections]
- Incomplete: [list incomplete sections]

**Recovery Options**:
1. Reconstruct context from code and git history
2. Start fresh documentation for this task
3. Manually fix the context files

Which approach would you prefer?
```

## Best Practices

**For Claude**:
1. Always read all three files (plan, context, tasks)
2. Check context freshness before proceeding
3. Verify file existence if context is stale
4. Be specific about NEXT ACTION
5. Offer to update context if it's outdated

**For Users**:
1. Keep context.md updated frequently (after significant changes)
2. Update timestamps when making changes
3. Be specific in NEXT ACTION (file + function + action)
4. Archive completed tasks to keep plans/active/ clean
5. Use `/update-context` before long breaks

## Integration with Other Commands

- `/create-plan [task-name]` - Create new waypoint plan structure
- `/update-context` - Update SESSION PROGRESS before context reset

## See Also

- `.claude/skills/context-persistence/SKILL.md` - Full waypoint plan patterns
- `.claude/skills/memory-management/SKILL.md` - Memory storage patterns
- `plans/active/README.md` - Task management guidelines (if exists)

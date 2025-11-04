# The Waypoint Pattern - Claude Code Waypoint Plugin

A comprehensive guide to the three-file waypoint pattern that enables context persistence and < 60 second resume after context resets.

## What is the Waypoint Pattern?

The **waypoint pattern** is a structured approach to capturing project state across three complementary files:

```
plans/active/task-name/
├── task-name-plan.md       # Strategic plan (rarely changes)
├── task-name-context.md    # Current state (updated frequently)
└── task-name-tasks.md      # Actionable checklist (updated daily)
```

This pattern enables you to:
- **Persist context** across context resets
- **Resume work** in < 60 seconds
- **Track progress** continuously
- **Document decisions** with rationale
- **Learn from mistakes** systematically

## The Three Files

### 1. plan.md - Strategic Direction

**Purpose**: The "what and why" - your strategic plan that rarely changes.

**Updated**: Once at start, then occasionally when strategy shifts

**Contains**:
- **Overview**: What you're building and why
- **Goals and Success Criteria**: How you'll know you're done
- **Approach**: High-level strategy and architecture
- **Technical Stack**: Technologies and integrations
- **Alternatives Considered**: Why you chose this approach over others
- **Risks and Mitigations**: Known challenges and how to handle them
- **Dependencies**: What this depends on, what depends on this
- **Timeline and Phases**: Overall schedule

**Example structure**:
```markdown
# Stripe Payment Integration Plan

## Overview
Implementing subscription billing using Stripe to replace current one-time payment system.

## Goals
- Support recurring subscriptions (monthly/annual)
- Implement usage-based pricing for additional credits
- Maintain backward compatibility with existing customers

## Approach
- Use Stripe API + Webhook events
- Store subscription state in PostgreSQL
- Edge Functions handle webhook processing
- Client-side integration via Stripe.js

## Risks
- Webhook delivery timing
- Stripe API rate limits
- Currency conversion complexity

## Mitigation
- Implement idempotent webhook handlers
- Queue for batch processing
- Use Stripe's built-in currency support
```

**Key principle**: This document rarely changes because it captures *why* you chose this approach. When strategy shifts, update it - but expect this to be stable for weeks.

### 2. context.md - Current State (CRITICAL)

**Purpose**: The "where we are right now" - updated frequently to enable instant resume.

**Updated**: Multiple times per session, before every context reset

**Contains**:
- **SESSION PROGRESS**: The critical section for resume
- **Key Decisions**: Important choices made during this session
- **Discoveries**: What you've learned
- **Files Modified**: What's been changed
- **Current Understanding**: How the codebase works
- **Blockers and Issues**: What's preventing progress
- **NEXT ACTION**: Exactly what to do when you resume

**The Critical SESSION PROGRESS Section** (this is the magic):

```markdown
## SESSION PROGRESS (2025-01-15 14:30)

### ✅ COMPLETED
- [x] Set up Stripe API keys in environment
- [x] Created stripe service wrapper (stripe/service.ts)
- [x] Implemented subscription creation endpoint
- [x] Added database migration for subscription tracking

### 🟡 IN PROGRESS
- [ ] Webhook handler for subscription.updated events (CURRENT - 60% done)

### ⏳ PENDING
- [ ] Webhook handler for payment_intent.succeeded
- [ ] Implement usage tracking and overage charges
- [ ] Add subscription management UI
- [ ] Write integration tests

### 🎯 NEXT ACTION
Finish webhook handler for subscription.updated in `api/webhooks/stripe.ts`.
Currently at line 45 - need to:
1. Parse subscription metadata
2. Update customer record
3. Emit event to realtime channel
4. Handle edge cases (cancellation, plan changes)

### 🔍 IMPORTANT DISCOVERIES
- Stripe sends duplicate webhooks - must implement idempotency check (using Stripe event ID)
- Metadata limited to 50 key-value pairs - only storing subscription plan and customer ID
- Webhook secret must be set via environment variable, not hardcoded

### 📁 FILES MODIFIED THIS SESSION
- Created: `lib/stripe/service.ts` (subscription wrapper)
- Created: `api/webhooks/stripe.ts` (webhook handler - IN PROGRESS)
- Modified: `lib/db/migrations/001_subscriptions.sql` (schema)
- Modified: `.env.local` (Stripe keys)
```

**Why this section is critical**:
1. **Timestamp** shows freshness (was this updated today?)
2. **COMPLETED** shows you made progress (psychological + informational)
3. **IN PROGRESS** shows exact current state (file + line + what's happening)
4. **PENDING** shows what's coming (context for what to do next)
5. **NEXT ACTION** enables instant resume (no re-reading entire context)
6. **DISCOVERIES** capture learning that might not be in code
7. **FILES MODIFIED** shows what's changed (for quick review if context reset)

**Resume pattern using SESSION PROGRESS**:

```
User: /resume stripe-payments

[Context loads, Claude reads context.md]

Claude: ✓ Resuming stripe-payments from 2025-01-15 14:30

Current state:
- 4 tasks completed (setup, service wrapper, endpoint, migration)
- Working on: subscription.updated webhook handler (60% done)
- Key discovery: Stripe sends duplicate webhooks, need idempotency check

Files to review:
- api/webhooks/stripe.ts (where we left off)
- lib/stripe/service.ts (recently created)

Ready to continue. Let me check current state...

[Loads the files, picks up exactly where you left off]
```

**Time to resume**: 30-60 seconds instead of 20-30 minutes of re-explaining.

### 3. tasks.md - Actionable Checklist

**Purpose**: The "what needs doing" - granular tasks with acceptance criteria.

**Updated**: Daily or as tasks complete

**Contains**:
- **Task list with acceptance criteria**
- **Dependencies** between tasks
- **Time estimates** (optional but helpful)
- **Blocked/Blocked by** relationships
- **Status**: Completed, In Progress, Pending
- **Assignee** (if team)

**Example structure**:
```markdown
# Stripe Integration Tasks

## Phase 1: Setup (COMPLETED)
- [x] Configure Stripe API keys in .env (Est: 15 min)
  - Acceptance: Keys loaded without errors, test API call succeeds

## Phase 2: Subscription Service (IN PROGRESS)
- [x] Create Stripe service wrapper (Est: 1h)
  - Acceptance: subscription.create() works, passes unit tests
- [x] Implement subscription creation endpoint (Est: 1.5h)
  - Acceptance: POST /api/subscriptions creates subscription, returns ID
  - Blocked by: None
  - Depends on: Phase 2.1
- [ ] Database migration for subscriptions (Est: 30 min)
  - Acceptance: `subscriptions` table created, has correct schema
  - Status: Done functionally, needs review

## Phase 3: Webhook Processing (60% complete)
- [ ] Webhook handler for subscription.updated (Est: 2h)
  - Current: Working on this
  - Acceptance: Handler receives event, updates customer record, emits event
  - Challenges: Idempotency (duplicate events), metadata limits
  - Blocked by: None
  - Blocked by this: Phase 3.2

- [ ] Webhook handler for payment_intent.succeeded (Est: 1.5h)
  - Acceptance: Handler processes payment, updates invoice, sends email
  - Depends on: Phase 3.1
  - Time estimate: After Phase 3.1 complete

## Phase 4: Client Integration (NOT STARTED)
- [ ] Subscription management UI (Est: 3h)
  - Acceptance: Users can view, upgrade, cancel subscriptions
  - Depends on: Phase 3 complete
  - Blocked by: UX design approval

## Phase 5: Testing (NOT STARTED)
- [ ] Integration tests for subscription flow (Est: 2h)
- [ ] Manual testing checklist (Est: 1h)

---

## Statistics
- Total: 13 tasks
- Completed: 4 (31%)
- In Progress: 1
- Pending: 8
- Blocked: 0

## Key Blockers
None currently. On track.

## Next Priority
Complete Phase 3.1 (webhook handler) to unblock Phase 3.2.
```

**Purpose of this file**:
- Granular enough to guide daily work
- Track progress quantitatively (% complete)
- Show dependencies (what blocks what)
- Provide estimates (for timeline)
- Enable clear prioritization

## When to Update Each File

### Before Every Session
- Read `context.md` to understand current state
- Check `tasks.md` to see what needs doing
- Don't need to read `plan.md` (strategy is stable)

### During Session
- Update `context.md` continuously with discoveries and progress
- Update `tasks.md` when completing tasks
- Don't change `plan.md` (strategy shouldn't shift mid-session)

### Before Context Reset
- Always run `/update-context` to write SESSION PROGRESS
- This is THE critical moment - SESSION PROGRESS is what enables resume
- Include:
  - What you completed
  - What's in progress (exact file, line, what's next)
  - What blockers exist
  - Files modified
  - Key discoveries

### After Context Reset
- Run `/resume [task-name]`
- Claude loads context.md and reads SESSION PROGRESS
- You're back to work in < 60 seconds

### When Strategy Changes
- Update `plan.md` with new approach
- Update `context.md` IMMEDIATELY with explanation
- Update `tasks.md` to reflect new task list
- This is rare but necessary

## Best Practices

### 1. Be Specific in SESSION PROGRESS

BAD:
```markdown
### 🟡 IN PROGRESS
- [ ] Working on webhooks
```

GOOD:
```markdown
### 🟡 IN PROGRESS
- [ ] Webhook handler for subscription.updated (CURRENT - 60% done)
```

**Why**: Resume needs to know exactly where you are.

### 2. Include Discoveries That Code Doesn't Show

BAD:
```markdown
### 🔍 IMPORTANT DISCOVERIES
- Stripe sends duplicate webhooks
```

GOOD:
```markdown
### 🔍 IMPORTANT DISCOVERIES
- Stripe sends duplicate webhooks - must implement idempotency check
- Metadata limited to 50 key-value pairs - only storing subscription plan and customer ID
- Webhook secret must be set via environment variable, not hardcoded
- Rate limits: 100 API calls per second (we're well under this)
```

**Why**: These insights don't appear in code but prevent mistakes.

### 3. Link to Files in NEXT ACTION

BAD:
```markdown
### 🎯 NEXT ACTION
Continue working on the webhook handler
```

GOOD:
```markdown
### 🎯 NEXT ACTION
Finish webhook handler for subscription.updated in `api/webhooks/stripe.ts`.
Currently at line 45 - need to:
1. Parse subscription metadata
2. Update customer record
3. Emit event to realtime channel
4. Handle edge cases
```

**Why**: When you resume, Claude can immediately jump to the right file and line.

### 4. Track Blockers Explicitly

```markdown
### 🚫 BLOCKERS
- [ ] UX design approval needed before starting subscription management UI
  - Blocked since: 2025-01-12
  - Contact: design-team@company
  - Impact: Blocks Phase 4
```

**Why**: Makes it clear what needs outside input.

### 5. Keep Task Estimates Realistic

```markdown
- [x] Create Stripe service wrapper (Est: 1h, Actual: 1h 15m) ✅ On target
- [x] API endpoint (Est: 1.5h, Actual: 2h 30m) ⚠️ Underestimated
```

**Why**: Helps future estimates and shows where estimates were off.

## SESSION PROGRESS Timestamps

Update the timestamp every session:

```markdown
## SESSION PROGRESS (2025-01-15 14:30)
↑ Date and time of this update
```

**Purpose**:
- Shows when context was last captured
- Helps identify stale waypoints
- Useful for "what was I working on 3 days ago?"

## Waypoint Pattern for Non-Coding Work

The waypoint pattern isn't just for coding. It works for any complex AI collaboration:

### Example: Business Process Redesign

```
plans/active/customer-onboarding-redesign/
├── customer-onboarding-redesign-plan.md
├── customer-onboarding-redesign-context.md
└── customer-onboarding-redesign-tasks.md
```

**plan.md** captures:
- Current process mapping
- Pain points identified
- Redesign approach
- Expected improvements

**context.md** captures:
- SESSION PROGRESS with stakeholder feedback
- Key decisions made
- Process flows documented
- NEXT ACTION for next meeting

**tasks.md** captures:
- Gather requirements from each department
- Document current flows
- Design new flows
- Get sign-off from stakeholders
- Plan implementation

### Example: Research Project

```
plans/active/market-analysis-saas/
├── market-analysis-saas-plan.md
├── market-analysis-saas-context.md
└── market-analysis-saas-tasks.md
```

**context.md** captures:
- SESSION PROGRESS with findings
- Sources researched
- Insights discovered
- Gaps in research
- NEXT ACTION for next research session

**Result**: Research accumulates across sessions instead of resetting.

## Common Patterns

### Pattern: Feature With Multiple Phases

Use one waypoint per major feature:

```
plans/active/
├── auth-implementation/
├── stripe-payments/
├── admin-dashboard/
└── notification-system/
```

Each has plan + context + tasks. Switch between them with `/resume [name]`.

### Pattern: Bug Investigation

Create a waypoint for tricky bugs:

```
plans/active/performance-issue-dashboard-load/
├── performance-issue-dashboard-load-plan.md
├── performance-issue-dashboard-load-context.md (tracks investigation)
└── performance-issue-dashboard-load-tasks.md (debugging steps)
```

**context.md** captures:
- What you tested
- What you discovered
- Hypotheses tested
- Current theory
- NEXT ACTION for next debugging session

### Pattern: Architecture Refactoring

Large refactors span multiple sessions:

```
plans/active/refactor-api-to-monorepo/
├── refactor-api-to-monorepo-plan.md (refactor strategy)
├── refactor-api-to-monorepo-context.md (progress, discoveries, blockers)
└── refactor-api-to-monorepo-tasks.md (10-20 granular tasks)
```

## Integrating with /update-context and /resume Commands

The waypoint pattern integrates with Claude Code Waypoint Plugin commands:

### `/create-plan [task-name]`

Creates the three-file structure:
- Generates initial plan.md (you flesh it out)
- Creates context.md with placeholder
- Creates tasks.md with outline
- Returns file paths

### `/update-context`

Updates context.md before you stop:
- Prompts you for SESSION PROGRESS
- Captures current state
- Saves files
- Ready for next session

### `/resume [task-name]`

Instantly restores context:
- Loads plan.md (strategy)
- Loads context.md (current state)
- Loads tasks.md (what needs doing)
- Claude understands exactly where you are
- Ready to continue

## Archiving Old Waypoints

Over time, some waypoints complete and should be archived:

```
plans/
├── active/
│   ├── current-feature-1/
│   └── current-feature-2/
└── completed/
    ├── 2025-01/auth-implementation/  (completed, useful reference)
    ├── 2025-01/stripe-payments/
    └── 2024-12/bug-fix-memory-leak/
```

**Benefits**:
- Keeps `active/` focused on current work
- Preserves completed waypoints as reference
- Easy to search past decisions
- Shows historical progress

## Quick Reference: What Goes Where

| Item | File | When |
|------|------|------|
| Strategic approach | plan.md | Once at start |
| Why this approach | plan.md | Once at start |
| Risks and mitigations | plan.md | Once at start |
| Current completion state | context.md | Every session |
| Files just modified | context.md | Every session |
| Discoveries about codebase | context.md | During session |
| Blockers and issues | context.md | During session |
| Exact current location | SESSION PROGRESS | Before reset |
| What to do when resuming | NEXT ACTION | Before reset |
| Granular task list | tasks.md | Daily |
| Task completion status | tasks.md | As work progresses |
| Dependencies between tasks | tasks.md | Daily |

## Troubleshooting

### "I lost context after a reset and had to start over"

**Root cause**: Didn't update SESSION PROGRESS before reset.

**Solution**:
1. Always run `/update-context` before stopping
2. This writes SESSION PROGRESS to context.md
3. When you resume, this is the first thing Claude reads

### "SESSION PROGRESS is hard to write"

**Solution**: Use this template and fill in the blanks:

```markdown
## SESSION PROGRESS (2025-01-15 14:30)

### ✅ COMPLETED
- [x] [Task 1]
- [x] [Task 2]

### 🟡 IN PROGRESS
- [ ] [Task being worked on NOW - be specific]

### ⏳ PENDING
- [ ] [What comes next]
- [ ] [After that]

### 🔍 IMPORTANT DISCOVERIES
- [Something important not in code]

### 📁 FILES MODIFIED THIS SESSION
- [File 1: what changed]
- [File 2: what changed]

### 🎯 NEXT ACTION
[Specific: file + line + action]
```

Takes 2 minutes, saves 20+ when you resume.

### "My waypoint is getting too long"

**Solutions**:
1. **Archive completed tasks**: Move done items to a "## ARCHIVE" section
2. **Split into multiple waypoints**: If > 30 tasks, maybe this is 2-3 features
3. **Prune discoveries**: Keep recent ones, archive old ones to "## HISTORICAL DISCOVERIES"

## Integration with Skill System

The waypoint pattern works seamlessly with Claude Code Waypoint Plugin's skills:

- **context-persistence skill**: Teaches this pattern
- **memory-management skill**: Integrates decision logging into waypoints
- **plan-approval skill**: Use waypoints for plan approval workflows

See [CUSTOMIZATION.md](CUSTOMIZATION.md) for integrating waypoints with other skills.

## Advanced: Waypoint Versioning

For very long-running projects, version your waypoints:

```
plans/active/large-system-redesign/
├── redesign-plan-v1.md (original strategy)
├── redesign-plan-v2.md (revised after phase 1)
├── redesign-context.md (always current)
└── redesign-tasks.md (updated as we go)
```

Then in context.md:
```markdown
## Strategy Evolution
- v1 (Jan 1-12): Original approach, discovered complexity
- v2 (Jan 13-present): Revised approach to address Phase 1 learnings
```

This documents how strategy evolved based on discoveries.

## Conclusion

The three-file waypoint pattern is:
- **Simple**: Just three files with clear purposes
- **Flexible**: Works for coding, design, research, business process work
- **Powerful**: Enables < 60 second resume after any context reset
- **Learnable**: Takes 5 minutes to understand, lifetime to master

Start with a basic structure, customize for your work style, and watch how context persistence transforms your ability to work with Claude Code.

---

**Next Steps**:
- Read [CUSTOMIZATION.md](CUSTOMIZATION.md) to adapt waypoints for your tech stack
- See [README.md](README.md) for examples using waypoints
- Check [context-persistence skill](./.claude/skills/context-persistence/SKILL.md) for deeper patterns

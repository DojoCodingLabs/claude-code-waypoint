# Contributing to Claude Code Waypoint Plugin

Guidelines for contributing to Claude Code Waypoint Plugin. Whether you're adding new features, fixing bugs, creating custom scaffolds, or improving documentation - thank you for helping!

## Code of Conduct

### Respect and Inclusivity

- **Be respectful** to all contributors
- **Assume good intentions** - we're all learning
- **Welcome diverse perspectives** - different tech stacks, approaches, and ideas
- **Give credit** - acknowledge others' contributions
- **Avoid gatekeeping** - help newcomers

### We Don't Accept

- Harassment, discrimination, or hostile language
- Spam or low-effort contributions
- Proprietary code or dependencies without clear licensing
- Contributions that violate others' intellectual property

## Getting Started

### Prerequisites

- Git and GitHub account
- Text editor (VS Code recommended)
- Node.js 18+ (for testing hooks)
- Familiarity with the project structure (read [CLAUDE.md](CLAUDE.md))

### Development Setup

```bash
# Clone the repository
git clone https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace
cd dojo-claude-code-plugin-marketplace

# Create a branch for your work
git checkout -b feature/your-feature-name

# Test hooks locally
cd plugins/waypoint/.claude/hooks && npm install && chmod +x *.sh

# Make changes
# ... (see Contributing Sections below)

# Test your changes
npm run test  # if applicable
cat plugins/waypoint/.claude/skills/skill-rules.json | jq .  # validate JSON
```

## Types of Contributions

### 1. Bug Reports

**Found a bug?** Help us fix it!

**File an issue**:

```markdown
# Bug: [Clear title]

## Description
[What's broken?]

## Steps to Reproduce
1. [Step 1]
2. [Step 2]
3. [What happens?]

## Expected Behavior
[What should happen?]

## Environment
- OS: [Windows/Mac/Linux]
- Node version: [version]
- Claude Code version: [version]

## Screenshots
[If applicable]
```

**Good bug reports**:
- Clear reproduction steps
- Expected vs actual behavior
- Minimal example
- Environment details

### 2. Feature Requests

**Have an idea?** Share it!

**File an issue**:

```markdown
# Feature: [Clear title]

## Problem
[What pain point does this solve?]

## Proposed Solution
[How would this work?]

## Alternative Approaches
[Other ways to solve this?]

## Use Case
[Real-world example of why this matters]
```

**Good feature requests**:
- Explain the problem, not just the solution
- Include real use cases
- Consider alternatives
- Think about how it fits the project

### 3. Code Contributions

See [Code Changes](#code-changes) section.

### 4. Documentation

See [Documentation Changes](#documentation-changes) section.

### 5. New Skills

See [Creating Custom Skills](#creating-custom-skills) section.

### 6. Scaffold Customizations

See [Scaffold Customizations](#scaffold-customizations) section.

## Code Changes

### Small Fixes (Typos, Small Bugs)

Perfect for first-time contributors!

```bash
# Create a feature branch
git checkout -b fix/typo-in-documentation

# Make changes
# ... edit files

# Commit with clear message
git add .
git commit -m "Fix typo in WAYPOINT_PATTERN.md"

# Push and create PR
git push origin fix/typo-in-documentation
```

**PR template**:
```markdown
# Fix: [Clear title]

## Changes
- Fixed typo in [file]
- Corrected [issue]

## Testing
- [How did you test?]

Fixes #[issue-number]
```

### Medium Changes (Hook Updates, Skill Improvements)

For changes to core hook system or skills:

1. **Discuss first** - File an issue or discussion
2. **Get approval** - Wait for maintainer feedback
3. **Implement** - Make your changes
4. **Test thoroughly** - See [Testing](#testing) section
5. **Document** - Update relevant docs
6. **Create PR** - Include test results

**PR template**:
```markdown
# [Type]: [Clear title]

## What Changed
[Overview of changes]

## Why
[Rationale for this change]

## Testing
- [Test 1: result]
- [Test 2: result]
- [Manual testing: result]

## Checklist
- [ ] JSON validates
- [ ] Tested manually
- [ ] Documentation updated
- [ ] No breaking changes (or intentional + documented)

Relates to #[issue-number]
```

### Large Changes (New Skills, Significant Refactors)

For significant work:

1. **File an issue or discussion** - Outline what you want to do
2. **Get feedback** - Discuss approach with maintainers
3. **Create a draft PR early** - Show work in progress
4. **Iterate based on feedback** - Be open to suggestions
5. **Final review** - Before merging

**Draft PR template**:
```markdown
# [WIP] [Type]: [Title]

## Overview
[What are you building?]

## Approach
[How are you building it?]

## Progress
- [x] Task 1
- [ ] Task 2
- [ ] Task 3

## Questions
[Areas where you need feedback]

## Related
#[issue-number]
```

## Testing

### Testing Hook Changes

```bash
# Validate JSON syntax
cat .claude/skills/skill-rules.json | jq .
cat .claude/memory/preferences.json | jq .

# Test skill activation
echo '{"prompt":"test keyword"}' | \
  cd .claude/hooks && npx tsx skill-activation-prompt.ts

# Test in real project
# 1. Copy modified files to test project
# 2. Edit files that should trigger skills
# 3. Verify skills activate
# 4. Check content is correct
```

### Testing Skill Changes

```bash
# 1. Read the skill file in Claude Code
# 2. Try examples from the skill
# 3. Verify they work for your use case
# 4. Check for errors or unclear sections

# 2. Validate markdown
# No special tool needed - just readable content

# 3. Test cross-references
# If skill references other files, verify links work
```

### Testing Documentation Changes

```bash
# 1. Read your changes carefully
# 2. Check for clarity and accuracy
# 3. Verify all code examples work
# 4. Check all links are correct
# 5. Have someone else read it (different perspective)
```

## Documentation Changes

### Updating Existing Files

**Small updates** (typos, clarity, examples):
- Edit the file directly
- Clear commit message: "docs: clarify [section] in [file]"
- Simple PR, quick approval

**Large updates** (restructuring, new sections):
- File an issue first - discuss approach
- Get feedback on organization
- Make changes in branch
- Submit PR with rationale

### Adding New Documentation

**New doc files** (like this guide):

```bash
git checkout -b docs/new-topic

# Create file with clear structure:
# - Title
# - Overview
# - Content sections
# - Examples
# - Related docs

# Get feedback on outline first (file issue)
# Then write full content
# PR with context on why this doc is needed
```

**Documentation principles**:
- **Clear**: Assume reader doesn't know the topic
- **Examples**: Show not just tell
- **Links**: Reference related docs
- **Actionable**: Give readers something to do
- **Maintained**: Don't let docs get stale

## Creating Custom Skills

### Anatomy of a Skill Contribution

Proposing a new skill? Excellent!

**File an issue first**:

```markdown
# New Skill Proposal: [Skill Name]

## Purpose
[What does this skill teach?]

## Tech Stack
[What tech does it cover?]

## Topics Covered
- [Topic 1]
- [Topic 2]
- [Topic 3]

## Who Needs This
[Who would benefit?]

## Example Activation
[How would it trigger?]
```

**Get feedback** before you write the whole thing.

### Implementation

```bash
git checkout -b skill/your-skill-name

# Create skill directory
mkdir -p .claude/skills/your-skill-name/resources

# Create main skill file
cat > .claude/skills/your-skill-name/SKILL.md << 'EOF'
# Your Skill Name

## Overview
[High-level overview, <500 lines total]

## Key Concepts
[3-5 main concepts]

## Common Patterns
[1-2 patterns with examples]

## Resources
[Navigation to detailed topics]
EOF

# Create resource files (if needed)
cat > .claude/skills/your-skill-name/resources/topic-1.md << 'EOF'
# Topic 1 - Deep Dive
[Detailed content, <500 lines per file]
EOF

# Add to skill-rules.json
# ... (see below)
```

### skill-rules.json Entry

```json
{
  "your-skill-name": {
    "type": "domain",
    "enforcement": "suggest",
    "priority": "high",
    "promptTriggers": {
      "keywords": ["keyword1", "keyword2"],
      "intentPatterns": ["(pattern1|pattern2).*?intent"]
    },
    "fileTriggers": {
      "pathPatterns": ["your/path/**/*.ts"],
      "contentPatterns": ["your.*?pattern"]
    },
    "skipConditions": {
      "sessionTracking": false,
      "fileMarkers": [],
      "envVars": []
    }
  }
}
```

### PR Template for New Skill

```markdown
# New Skill: [Skill Name]

## Overview
[What does this skill teach?]

## Content
- Main SKILL.md: [line count]
- Resource files: [number], [topics]

## Activation Triggers
- Keywords: [keywords]
- File patterns: [patterns]

## Use Cases
[Real-world examples]

## Testing
- [ ] JSON validates
- [ ] Skill activates for example triggers
- [ ] Content is accurate and clear
- [ ] Examples work
- [ ] Links are correct

Closes #[issue-number]
```

### Skill Quality Checklist

- [ ] Main SKILL.md under 500 lines
- [ ] Resource files under 500 lines each
- [ ] Clear navigation between sections
- [ ] Code examples are correct and tested
- [ ] No proprietary code or secrets
- [ ] Accurate for the tech stack it covers
- [ ] Clear for beginners but useful for experts
- [ ] Integrated with skill-rules.json
- [ ] No dead links or references

## Scaffold Customizations

### Contributing a New Scaffold

Already customized Claude Code Waypoint Plugin for React + Django? Svelte + FastAPI? Others want it!

**Share your scaffold**:

```bash
# Create a branch
git checkout -b scaffold/react-django

# You've already customized:
# - .claude/skills/frontend-dev-guidelines/ → React-specific
# - .claude/skills/backend-dev-guidelines/ → Django-specific
# - .claude/skills/skill-rules.json → React + Django patterns
# - .claude/memory/preferences.json → Your conventions

# Document your scaffold
cat > SCAFFOLD_REACT_DJANGO.md << 'EOF'
# React + Django Scaffold

## What Changed
- Frontend: Updated for React 18 (not Next.js)
- Backend: Changed from Supabase Edge Functions to Django
- Database: Updated ORM examples

## Testing
[How you tested this scaffold]

## Instructions
[How others can use this scaffold]
EOF

# Add to GitHub issue
git add .
git commit -m "Add React + Django scaffold"
git push origin scaffold/react-django
```

**PR Template**:
```markdown
# Scaffold: [Framework 1] + [Framework 2]

## Tech Stack
- Frontend: [framework + versions]
- Backend: [framework + versions]
- Database: [system + versions]

## What Changed
- [Skill 1: changes]
- [Skill 2: changes]
- [skill-rules.json: changes]

## Testing
- [ ] Tested in real project
- [ ] Skill activation works
- [ ] Examples are accurate
- [ ] No secrets included

## For Users
[Instructions on how to use this scaffold]

## Future Maintenance
[Any notes for keeping this scaffold updated]
```

### Scaffold Maintenance

If your scaffold gets out of date:

```bash
# Update it
git checkout -b scaffold/react-django-update
# ... make changes
git commit -m "Update React + Django scaffold to React 19 + Django 5.0"
git push
```

The community benefits as it stays current!

## Pull Request Process

### Before You Create a PR

1. **Check for existing PRs** - Don't duplicate work
2. **File an issue first** (for significant changes) - Discuss approach
3. **Test your changes** - See [Testing](#testing) section
4. **Update documentation** - If code changes, docs might too
5. **Check license** - No GPL code (MIT-compatible only)

### Creating a PR

**Title format**:
```
type: short description

Types:
- feat: New feature
- fix: Bug fix
- docs: Documentation
- refactor: Code restructuring
- chore: Maintenance, deps
```

**Examples**:
```
feat: Add Rust scaffold support
fix: skill-activation regex now handles Edge Functions
docs: Clarify SESSION PROGRESS format
chore: Update TypeScript version in hooks
```

**Description**:

Use the appropriate template from above.

### PR Review Process

1. **Automated checks**
   - JSON validates
   - Links work
   - No obvious issues

2. **Maintainer review**
   - Code quality
   - Alignment with project
   - Testing adequacy
   - Documentation completeness

3. **Feedback and iteration**
   - Address comments
   - Make requested changes
   - Push updated commits
   - Maintainer re-reviews

4. **Approval and merge**
   - PR approved
   - Merged to main
   - Released in next version

### Responding to Feedback

**Be open and collaborative**:
- Assume reviewer is trying to help
- Ask clarifying questions
- Explain your reasoning
- Be willing to change approach
- Celebrate good feedback

**Don't**:
- Take criticism personally
- Argue about style without substance
- Push back without listening
- Disappear after feedback

**Do**:
- Thank reviewers for their time
- Make requested changes promptly
- Explain if you disagree with a suggestion
- Ask if changes look good

## Commit Message Guidelines

Clear commit messages help everyone understand history.

**Format**:
```
type(scope): subject

body (optional, explain why not what)
```

**Types**:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Formatting, no code change
- `refactor`: Code restructure
- `test`: Test additions
- `chore`: Dependencies, maintenance

**Scope** (optional):
```
feat(hooks): add support for custom triggers
fix(frontend-skill): correct React pattern example
docs(waypoint-pattern): clarify SESSION PROGRESS format
```

**Examples**:

```
feat(scaffold): add Vue 3 + FastAPI example

Previously only had Next.js + Supabase.
Added comprehensive Vue 3 + FastAPI scaffold
with patterns for Pinia state management.

Closes #42
```

```
fix(skill-activation): handle regex edge case

The skill-activation-prompt.ts regex would fail
on multiline file content. Changed to use
multiline flag in regex.

Fixes #67
```

```
docs: clarify that waypoint pattern works everywhere

Added examples for non-coding use cases
(business process redesign, research projects).
Emphasize core system is tech-agnostic.
```

## Development Commands

### Setting Up

```bash
# Install hook dependencies
cd .claude/hooks && npm install && chmod +x *.sh

# Validate configuration
cat .claude/skills/skill-rules.json | jq .
```

### Testing

```bash
# Test skill activation
echo '{"prompt":"create component"}' | \
  npx tsx .claude/hooks/skill-activation-prompt.ts

# Validate all JSON files
find .claude -name "*.json" -exec sh -c 'echo "Validating {}"; cat {} | jq .' \;
```

### Checking Your Work

```bash
# Find markdown formatting issues
find . -name "*.md" -type f | head -5  # Quick check

# Check for dead links in markdown
# Manually review links or use a link checker

# Validate JSON syntax
cat .claude/skills/skill-rules.json | jq .
cat .claude/memory/preferences.json | jq .
```

## Release Process

Maintainers only:

```bash
# Update version
# - Update package.json (hooks)
# - Update CHANGELOG.md

# Create git tag
git tag v1.2.3
git push origin v1.2.3

# GitHub creates release automatically
```

Contributors: No action needed. Maintainers handle releases.

## Getting Help

**Questions?**
- Check [CLAUDE.md](CLAUDE.md) - System overview
- Read [WAYPOINT_PATTERN.md](WAYPOINT_PATTERN.md) - Core pattern
- See [AUTO_ACTIVATION.md](./.claude/hooks/AUTO_ACTIVATION.md) - Hook system
- Check [CUSTOMIZATION.md](CUSTOMIZATION.md) - Adapting for your stack

**Still stuck?**
- File an issue with your question
- Discussions are welcome
- Ask in PR comments

## Common Contribution Scenarios

### "I want to add support for Rust backend"

1. File issue: "New Scaffold: Rust (Actix-web)"
2. Get feedback on approach
3. Customize backend-dev-guidelines for Rust
4. Create PR with scaffold

See [Scaffold Customizations](#scaffold-customizations).

### "I found a typo in the docs"

1. Fork the repo
2. Edit the file
3. Create PR with clear title
4. We merge quickly!

### "The waypoint pattern needs clarification"

1. Edit [WAYPOINT_PATTERN.md](WAYPOINT_PATTERN.md)
2. Add example or rewrite section
3. Create PR
4. Discuss improvements

### "I have an idea for a new hook"

1. File issue: "Proposal: [Hook Name]"
2. Discuss use case and implementation
3. Get feedback
4. Implement with tests
5. Create PR

### "Should I create a custom skill?"

**Yes if**:
- It teaches a pattern not covered by existing skills
- It's specific to certain tech/use case
- Multiple people would benefit

**Maybe if**:
- It's very niche (could just be in a project)
- It's already covered in another skill

File an issue to discuss first!

## Maintenance

### Keeping Your Contribution Updated

The project evolves. If your contribution is affected:

```bash
# Pull latest main
git checkout main
git pull origin main

# Rebase your branch
git checkout your-branch
git rebase main

# Fix any conflicts
# Commit again

# Push updated work
git push -f origin your-branch
```

### Long-Term Maintenance

Contributing a skill or scaffold? Plan to maintain it:

- Monitor related issues
- Update for new versions of tech
- Review PRs that affect it
- Keep examples current

Maintainers will help, but original contributor usually knows best.

## Thank You

Contributing makes Claude Code Waypoint Plugin better for everyone. Whether you fix a typo, add a feature, create a skill, or help someone in an issue - **you're awesome**.

Questions? Ask in the community. We're here to help.

---

**Ready to contribute?**

1. Read [CLAUDE.md](CLAUDE.md) for architecture
2. Pick an issue or file your own
3. Comment: "I'd like to work on this"
4. Make your changes
5. Create a PR
6. Let's build something great together!

---

**Related Links**:
- [GitHub Issues](https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace/issues)
- [GitHub Discussions](https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace/discussions)
- [README.md](README.md) - Project overview
- [CLAUDE.md](CLAUDE.md) - Architecture guide

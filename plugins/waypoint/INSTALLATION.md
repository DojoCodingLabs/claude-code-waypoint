# Claude Code Waypoint Plugin - Installation

Step-by-step instructions for installing the Claude Code Waypoint Plugin for memory and context persistence.

## Prerequisites

- Claude Code IDE installed and activated
- A project where you want to use this plugin
- Basic understanding of your project structure

## Installation Methods

### Method 0: Plugin Installation (Recommended - Easiest)

```bash
# Install Claude Code Waypoint Plugin via Claude Code
/plugin install waypoint
```

This automatically copies all plugin files, creates directory structure, and configures hooks. Skip to [Verification](#verification) if you use this method.

### Method 1: Manual Installation

**Step 1: Clone or Download**

```bash
# Clone the repository
git clone https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace

# Or download ZIP and extract
```

**Step 2: Copy .claude Directory**

```bash
# Navigate to your project root
cd /path/to/your/project

# Copy the .claude directory
cp -r /path/to/claude-code-plugins/.claude ./

# Or if you already have a .claude directory:
cp -r /path/to/claude-code-plugins/.claude/* ./.claude/
```

**Step 3: Verify Installation**

Check that you now have:
```
your-project/
├── plans/                          # ← Waypoint plans storage
│   ├── active/
│   ├── archived/
│   └── templates/
├── .claude/
│   ├── memory/                     # ← Preferences and decisions
│   │   ├── project/
│   │   └── preferences/
│   ├── skills/
│   │   ├── memory-management/
│   │   ├── context-persistence/
│   │   ├── plan-approval/
│   │   ├── frontend-dev-guidelines/
│   │   ├── backend-dev-guidelines/
│   │   └── skill-developer/
│   ├── agents/
│   │   └── scaffold-customizer.md
│   └── commands/
│       ├── create-plan.md
│       ├── update-context.md
│       └── resume.md
```

### Method 2: Automated Installation

```bash
# Run the installer script
curl -fsSL https://raw.githubusercontent.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace/main/install.sh | bash
```

The script will:
1. Detect your project root
2. Check for existing .claude directory
3. Backup existing files if needed
4. Copy plugin files
5. Verify installation

## Post-Installation Setup

### 1. Customize for Your Tech Stack (Optional)

If you're **not** using our opinionated stack (Supabase + Next.js + React 19), customize:

```
# Ask Claude to run the scaffold customizer
Claude: "I need to customize the skills for my tech stack"

# Claude will:
1. Detect your current stack
2. Ask for confirmation
3. Create customization plan
4. Execute replacements
5. Verify changes
```

### 2. Create Memory Directories

```bash
# Create memory storage directories
mkdir -p .claude/memory/project
mkdir -p .claude/memory/plans/{proposed,approved,rejected,completed}
```

### 3. Create Waypoint Plans Directory

```bash
# Create waypoint plans directories
mkdir -p plans/active
mkdir -p plans/archived
mkdir -p plans/templates
```

### 4. Initialize Git Ignore (Important!)

Add to your `.gitignore`:

```gitignore
# Claude Code Memory (may contain sensitive context)
.claude/memory/**/*.json

# But DO track skills, agents, and commands
!.claude/skills/
!.claude/agents/
!.claude/commands/
```

## Verification

### Test Skill Activation

1. **Open a frontend file** (e.g., `app/page.tsx`)
2. **Ask Claude**: "Help me create a new React component"
3. **Expected**: frontend-dev-guidelines skill should auto-activate

### Test Commands

```bash
# Create a test waypoint plan
/create-plan test-feature

# Expected: Three files created in plans/active/test-feature/
# - plan.md
# - context.md
# - tasks.md
```

### Test Memory

```
# Ask Claude to remember something
User: "Always use named imports in this project"
Claude: ✓ I'll remember this preference

# Later, ask Claude to recall
User: "What's my import preference?"
Claude: You prefer named imports
```

## Troubleshooting

### Skills Not Auto-Activating

**Symptom**: Skills don't activate automatically

**Solutions**:

1. Check skill-rules.json exists:
```bash
ls -la .claude/skills/skill-rules.json
```

2. Verify JSON syntax:
```bash
cat .claude/skills/skill-rules.json | jq '.'
```

3. Check file path patterns match your project structure

### Commands Not Working

**Symptom**: `/create-plan` not found

**Solutions**:

1. Verify commands directory:
```bash
ls -la .claude/commands/
```

2. Check command files have `.md` extension

3. Restart Claude Code

### Waypoint Plans Not Persisting

**Symptom**: Plans lost after session

**Solutions**:

1. Check plans directory exists:
```bash
ls -la plans/active/
```

2. Verify write permissions:
```bash
chmod -R u+w plans/
```

3. Check plans/ directory is committed to git (should be, unlike .claude/memory/)

## Advanced Configuration

### Customize Skill Triggers

Edit `.claude/skills/skill-rules.json`:

```json
{
  "my-custom-skill": {
    "type": "domain",
    "enforcement": "suggest",
    "priority": "high",
    "promptTriggers": {
      "keywords": ["my", "custom", "keywords"],
      "intentPatterns": ["(implement|create).*?feature"]
    },
    "fileTriggers": {
      "pathPatterns": ["src/**/*.ts"],
      "contentPatterns": ["import.*myLib"]
    }
  }
}
```

### Customize Waypoint Storage

Organize your waypoint plans by project or feature type:

```bash
mkdir -p plans/active/frontend-features
mkdir -p plans/active/backend-features
mkdir -p plans/active/devops-tasks
```

### Configure Plan Approval Thresholds

Edit plan-approval skill to customize when approval is required:

```markdown
# In .claude/skills/plan-approval/SKILL.md

## When to Require Approval

Always Require:
- Database changes (add your criteria)
- API changes affecting > X endpoints
- Features affecting > Y files
```

## Next Steps

1. **Read**: [PHILOSOPHY.md](PHILOSOPHY.md) to understand the waypoint approach
2. **Create**: Your first waypoint plan with `/create-plan [task-name]`
3. **Understand**: How waypoint plans survive context resets
4. **Explore**: [Skills documentation](/.claude/skills/)
5. **Customize**: Use scaffold-customizer for your tech stack

## Getting Help

- **Issues**: [GitHub Issues](https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace/issues)
- **Questions**: Ask Claude "How do I use memory management?"
- **Community**: [Discussions](https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace/discussions)

---

**Installation complete!** Start using `/create-plan [task-name]` to create your first waypoint plan. Your work now survives context resets.

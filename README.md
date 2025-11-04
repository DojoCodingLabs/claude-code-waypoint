# Dojo Claude Plugins

[![Marketplace](https://img.shields.io/badge/marketplace-dojo--claude--plugins-blue)](https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

Production-tested Claude Code plugins built for power users. Each plugin is battle-tested on real projects, not theoretical implementations.

## 🚀 Quick Start

Add this marketplace to Claude Code:

```bash
/plugin marketplace add DojoCodingLabs/dojo-claude-code-plugin-marketplace
```

Then browse and install plugins with `/plugin` or install directly (see below).

---

## 📦 Available Plugins

### 🗺️ Waypoint - Persistent Memory & Context Management

**Never lose context again.** Strategic waypoints for long-running AI projects.

```bash
/plugin install waypoint@dojo-claude-code-plugin-marketplace
```

**Key Features:**
- 📝 **3-file waypoint pattern** - Structured memory (plan/context/tasks)
- ⚡ **Resume in < 60 seconds** after context resets
- 🎯 **Auto-activating skills** - Right context at the right time
- 🧠 **Intentional memory** - Control what persists and why
- 🔄 **Cross-session continuity** - Maintain knowledge across days/weeks
- 💼 **Not just coding** - Business planning, research, any long-running work

**Perfect for:**
- Complex features spanning multiple sessions
- Projects requiring detailed context tracking
- Teams needing handoff documentation
- Anyone tired of "starting from scratch" after context resets

**[Learn more →](./plugins/waypoint/)**

---

## 🎯 Why Dojo Plugins?

### Production-Tested
Every plugin is validated in real-world scenarios, not academic exercises. We build tools we actually use daily.

### Power User Focus
Designed for developers who want maximum control and capability from Claude Code. No hand-holding, just powerful tools.

### Intentional Design
Each plugin solves a specific, well-defined problem. No bloat, no feature creep.

### Active Maintenance
Regular updates based on real usage patterns and community feedback.

---

## 🔮 Coming Soon

Future plugins in development:
- **Scaffold** - Tech-stack-aware code generation
- **Audit** - Automated code review workflows
- **Sync** - Team context sharing

Want to suggest a plugin? [Open a discussion →](https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace/discussions)

---

## 🤝 Contributing

We welcome contributions! Each plugin has its own contributing guidelines:

- [Waypoint Contributing Guide](./plugins/waypoint/CONTRIBUTING.md)

### Adding a New Plugin

Interested in adding your own plugin to this marketplace?

1. Create your plugin in `plugins/[your-plugin]/`
2. Follow the structure shown in `plugins/waypoint/`
3. Add entry to `.claude-plugin/marketplace.json`
4. Submit a pull request

See our [plugin development guide](./PLUGIN_DEVELOPMENT.md) (coming soon) for details.

---

## 📄 License

MIT License - See [LICENSE](LICENSE) for details.

---

## 💬 Support

- **Issues:** [GitHub Issues](https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace/issues)
- **Discussions:** [GitHub Discussions](https://github.com/DojoCodingLabs/dojo-claude-code-plugin-marketplace/discussions)

---

<p align="center">
  <strong>Built by developers, for developers.</strong><br>
  Production-tested tools for serious Claude Code users.
</p>

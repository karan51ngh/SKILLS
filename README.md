# 🛠️ Skills

> **Reference:** [Anatomy of a GenAI Skill](https://karan51ngh.medium.com/34c02bd325ef)

**Skills** are a lightweight, open format for extending AI agent capabilities with specialized knowledge and workflows.

At its core, a skill is a discoverable folder containing a `SKILL.md` file.

### 📁 Structure

A standard code-generation skill can look like:

```text
generate-code-skill/
├── SKILL.md          (Required: metadata + instructions)
├── scripts/          (Optional: executable Python, Bash, or other code)
├── references/       (Optional: documentation and detailed guidance)
├── assets/           (Optional: templates, examples, static resources)
└── agents/
    └── openai.yaml   (Optional: UI metadata, policy, dependencies)
```
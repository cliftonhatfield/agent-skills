# agent-skills

Skills for AI coding agents, by [Arcology Labs](https://arcologylabs.com). Each skill is one folder with a `SKILL.md` (Markdown plus a short YAML header), the format Claude Code, Codex, and other agents read.

## Skills

| Skill | What it does |
|---|---|
| [`wwyd`](wwyd/SKILL.md) | Ask the agent what it would do next. You get one recommendation and one reason, not a menu. `/wwyd --do` also does it, then shows what ran. |

## Install

Copy the skill's folder into your agent's skills directory.

| Agent | Per project | All projects |
|---|---|---|
| Claude Code | `.claude/skills/<name>/` | `~/.claude/skills/<name>/` |
| Codex | `.agents/skills/<name>/` | `~/.agents/skills/<name>/` |

Other agents: put the folder wherever your tool loads `SKILL.md` files.

```bash
git clone https://github.com/cliftonhatfield/agent-skills
mkdir -p ~/.claude/skills && cp -r agent-skills/wwyd ~/.claude/skills/
```

Then run `/wwyd` in Claude Code.

## Notes

- `disable-model-invocation: true` (in `wwyd`) is a Claude Code header that keeps a skill command-only. Agents that do not know it ignore it, so invoke the skill by name.
- Skills are plain text. Read one before you install it, and edit it to fit your project.

## License

MIT

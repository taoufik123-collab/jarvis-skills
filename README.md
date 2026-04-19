# jarvis-skills

Public skills for [JARVIS](https://wakejarvis.ai) — a voice-first AI assistant for macOS built on Claude Code. Every skill here runs on the JARVIS platform, but the SKILL.md files are portable: drop any one into `~/.claude/skills/<name>/` on your own machine and Claude will pick it up.

**Built and maintained by Taoufik — [@TaoufikAI](https://youtube.com/@TaoufikAI) — [wakejarvis.ai](https://wakejarvis.ai)**

---

## Install a skill

```bash
# Pick a skill, copy it into your personal Claude skills directory
mkdir -p ~/.claude/skills/build-site
cp skills/build-site/SKILL.md ~/.claude/skills/build-site/SKILL.md
```

Restart Claude Code (or open a fresh session) and the skill becomes invokable by name.

## Skills available

| Skill | What it does |
|---|---|
| [`build-site`](skills/build-site/) | Builds cinematic, high-fidelity React landing pages from a fixed premium design system (4 aesthetic presets, GSAP animations, locked component architecture). |
| [`morning-brief`](skills/morning-brief/) | *(coming with Video 1)* The morning context skill — calendar digest + priority brief + first action, spoken to you over voice. |

More skills ship with each YouTube video at [@TaoufikAI](https://youtube.com/@TaoufikAI).

## Community

Install with 2,000+ builders: [AI Agents Lab on Skool](https://skool.com/ai-agents-lab-1168) (free).

## License

MIT with attribution. See [`LICENSE`](LICENSE) for the full text and [`ATTRIBUTION.md`](ATTRIBUTION.md) for the required header to include in every fork, remix, or public distribution.

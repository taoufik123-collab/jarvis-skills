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
| [`morning-brief-starter`](skills/morning-brief-starter/) | Two-layer briefing pipeline starter — fetches the latest video from a YouTube channel, distills a voice-ready spoken brief + structured deck brief, writes it to a dated markdown file. Bring your own TTS / scheduler / filter. |

More skills ship with each YouTube video at [@TaoufikAI](https://youtube.com/@TaoufikAI).

## What's public vs private

These skills are **starters** — they teach the patterns I run in production, not the production stack itself. The JARVIS runtime, my AgenticV2 skills, and my content vault stay private (client data, branded templates, and bespoke tuning don't belong in a public repo). Fork a starter, adapt it to your own life, and share what you build.

## Community

Install with 2,000+ builders: [AI Agents Lab on Skool](https://skool.com/ai-agents-lab-1168) (free).

## License

MIT with attribution. See [`LICENSE`](LICENSE) for the full text and [`ATTRIBUTION.md`](ATTRIBUTION.md) for the required header to include in every fork, remix, or public distribution.

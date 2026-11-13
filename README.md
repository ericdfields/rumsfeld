# rumsfeld

![Donald Rumsfeld, deep-fried, mid-gesture, surrounded by glowing orbs of galaxies and equations](assets/rumsfeld-orbs.jpg)

> Reports that say that something hasn't happened are always interesting to me, because as we know, there are known knowns; there are things we know we know. We also know there are known unknowns; that is to say we know there are some things we do not know. But there are also unknown unknowns—the ones we don't know we don't know. And if one looks throughout the history of our country and other free countries, it is the latter category that tends to be the difficult ones.
>
> — Donald Rumsfeld, U.S. Department of Defense news briefing, February 12, 2002

`rumsfeld` is a Claude skill. It runs an adversarial review on a PRD, spec, architecture proposal, or plan that looks ready to build. It hunts for unknown unknowns: hidden assumptions, missing concepts, unmodeled state, lifecycle gaps, and a wrong framing of the problem.

It asks one question:

**What might we discover after implementation starts that would show this was a different problem than we thought?**

## When to use it

Run it when the team says the spec is done. Don't run it on exploratory ideas. Don't use it for copyediting, line-level code review, or security scans.

Trigger phrases include "run rumsfeld", "rumsfeld this PRD", and "is this ready to build?"

## Install

### Claude Code

Add the marketplace, then install the plugin:

```
/plugin marketplace add ericdfields/rumsfeld
/plugin install rumsfeld@rumsfeld
```

On Claude Code 2.1.275 or later, one command does both:

```
/plugin install rumsfeld --marketplace ericdfields/rumsfeld
```

### Claude.ai and Claude Desktop

1. Zip the `skills/rumsfeld/` folder.
2. Open **Settings → Capabilities → Skills**.
3. Upload the zip.

### Manual

Copy `skills/rumsfeld/` to `~/.claude/skills/rumsfeld/` (all projects) or `.claude/skills/rumsfeld/` (one project).

## Modes

- **lite** — 2 reviewers, short report. Say "rumsfeld lite".
- **standard** — 5 reviewers, full report. The default.
- **deep** — one reviewer per lens (14). Say "deep rumsfeld".

The skill writes its report to `RUMSFELD.md` in your project.

## Contents

- [skills/rumsfeld/SKILL.md](skills/rumsfeld/SKILL.md) — the skill and its seven phases
- [skills/rumsfeld/lenses/](skills/rumsfeld/lenses/) — review lenses (data, security, lifecycle, scale, and more)
- [skills/rumsfeld/generators/](skills/rumsfeld/generators/) — pre-mortem and probe generators
- [skills/rumsfeld/templates/](skills/rumsfeld/templates/) — report and Surprise Ledger templates
- [skills/rumsfeld/examples/](skills/rumsfeld/examples/) — test corpus
- [.claude-plugin/](.claude-plugin/) — plugin and marketplace manifests

## License

[MIT](LICENSE)

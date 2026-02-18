# feature-explorer

A Claude Code skill that analyzes any project codebase and generates an interactive HTML playground for exploring and prioritizing future features.

![Want / Later / Skip cards with ROI sorting and Quick Wins filter](https://img.shields.io/badge/Claude_Code-skill-blue)

## What it does

1. Reads your project — README, package.json, git log, source structure
2. Generates 15–25 features specific to *your* codebase (not generic suggestions)
3. Rates each feature by impact (1–3) and effort (1–3)
4. Writes a self-contained `feature-explorer.html` to your project root
5. Opens it in your browser

The playground lets you drag features into **Want / Later / Skip** buckets, sort by ROI, filter to Quick Wins, and copies a prompt you can paste back into Claude to kick off implementation.

## Install

```bash
npx skills add feature-explorer
```

Or clone manually into your skills directory:

```bash
git clone https://github.com/ccwriter369-beep/claude-skill-feature-explorer \
  ~/.claude/skills/feature-explorer
```

## Usage

In any Claude Code session, just ask:

> "feature explorer"
> "what should I build next?"
> "show me a feature roadmap"
> "prioritize features"

Claude will analyze your project and open the playground automatically.

## Playground UI

- **Want / Later / Skip** — drag cards to prioritize
- **Sort by ROI** — surfaces high-impact, low-effort features first
- **Quick Wins** — one-click filter for impact ≥ 2 and effort = 1
- **Prompt output** — live text describing your selections, ready to copy back into Claude

## How features are generated

Claude reads these sources in parallel before generating anything:

- `README.md` — project name, purpose, stack
- `CLAUDE.md` — architectural notes and conventions
- `package.json` / `pyproject.toml` / `Cargo.toml` / `go.mod` — stack confirmation
- `git log --oneline -20` — what's been built recently
- Top-level source directories — module names and structure

Features are only written if they pass a quality bar:
- Extends existing functionality (not rewrites)
- Addresses real pain points spotted in the code
- Categories reflect the project's actual domain

## License

MIT

---
name: feature-explorer
description: |
  Builds an interactive feature prioritization playground for any project. Analyzes
  the codebase, generates 15-25 relevant future features with impact/effort ratings,
  and opens a visual Want/Later/Skip explorer with a copyable Claude prompt.
  Use when user asks for "feature explorer", "feature roadmap", "what to build next",
  "feature playground", "prioritize features", "future features", or "feature ideas".
---

# Feature Explorer

Generates a project-specific interactive HTML playground for exploring and prioritizing
future features. Works on any repo — analyzes the codebase to produce relevant features,
not generic suggestions.

## Workflow

### 1. Understand the project

Read these sources in parallel:
- `README.md` or `README` — project name, purpose, tech stack
- `CLAUDE.md` — any architectural notes or conventions
- `package.json` / `pyproject.toml` / `Cargo.toml` / `go.mod` — stack confirmation
- `git log --oneline -20` — what's been built recently
- `.beads/` directory exists? Run `bd list --status=open` for backlog context
- Scan top-level source dirs (`src/`, `lib/`, `web/js/`, etc.) for module names

Goal: understand what's already built, what domain it's in, and what's obviously missing.

### 2. Generate features

Produce **15–25 features** specific to this project. For each:

```js
{ id:'short-kebab-id', cat:'category-key', title:'Short Feature Name',
  impact: 1|2|3,  // 1=nice-to-have, 2=meaningful, 3=high value
  effort: 1|2|3,  // 1=days, 2=week, 3=weeks+
  desc:'One sentence describing what it does and why it matters.' }
```

**Good features:**
- Extend existing functionality logically (not rewrites)
- Fix known pain points you spotted in the code
- Add missing UX polish (search, keyboard nav, export)
- Improve performance or scale
- Add data/integration hooks that are architecturally obvious

**Bad features:** Vague generics ("add tests"), architectural rewrites, unrelated pivots.

### 3. Generate categories

Produce **4–7 categories** that fit the project's domain:

```js
{ key: { label: 'Display Label', color: '#hex', bg: 'rgba(r,g,b,.15)' } }
```

Use colors from this palette (pick contrasting ones):
- Blue `#58a6ff` / `rgba(88,166,255,.15)`
- Green `#7ee787` / `rgba(126,231,135,.15)`
- Orange `#f0883e` / `rgba(240,136,62,.15)`
- Purple `#d2a8ff` / `rgba(210,168,255,.15)`
- Amber `#ffa657` / `rgba(255,166,87,.15)`
- Teal `#79c0ff` / `rgba(121,192,255,.15)`
- Pink `#f778ba` / `rgba(247,120,186,.15)`

### 4. Build the playground

Read the template:
```
~/.claude/skills/feature-explorer/assets/template.html
```

Replace three placeholders:
- `__PROJECT_NAME__` → short project name (e.g. `MyApp`, `HGP v2`)
- `__CATEGORIES__` → the categories object literal (no `const` keyword, just the `{...}`)
- `__FEATURES__` → the features array literal (no `const` keyword, just the `[...]`)

Write the result to `<cwd>/feature-explorer.html`.

### 5. Serve and open

```bash
# Start server if not already running
python3 -m http.server 8877 --directory <cwd> &

# Open in browser (WSL2)
/mnt/c/Windows/System32/WindowsPowerShell/v1.0/powershell.exe \
  -Command "Start-Process 'http://localhost:8877/feature-explorer.html'"
```

Tell the user the URL: **http://localhost:8877/feature-explorer.html**

## Output quality bar

The playground is only as useful as the features. Before writing the file, ask yourself:
- Would a developer on this project recognize these features as real?
- Are the impact/effort ratings defensible given the codebase complexity?
- Does at least one feature address something you saw in the code that looked painful?
- Do the categories reflect this project's actual domain (not generic software categories)?

If the answer to any is no, revise before writing the file.

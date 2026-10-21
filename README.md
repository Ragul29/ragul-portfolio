# Portfolio — Ragul Murugan

A personal portfolio built as a single-file terminal-style IDE interface.

**Live:** _add the GitHub Pages URL after publishing_

---

## Overview

The whole site is one application window: a macOS-style title bar, a file-tree
sidebar, a tab bar, a scrolling content pane, a live terminal panel on the
right, and a VS Code-style status bar at the bottom.

Everything lives in `landing.html` — no build step, no framework, no
dependencies. Open the file and it runs.

## Features

### Interactive file tree
The sidebar is a real file explorer, modelled on
[exploreclaudecode.com](https://www.exploreclaudecode.com):

- Folders swap between open/closed icons
- Children reveal one at a time on expand, and collapse bottom-to-top
- Connector lines are drawn on a `<canvas>` behind the rows
- Keyboard accessible (`Enter` / `Space` to toggle, `aria-expanded` state)

### Tabs
Clicking a section in the tree opens a tab next to **Welcome**. Tabs can be
closed with the `×`, and the active tab scrolls into view automatically.

### Live terminal
A working command line in the right-hand panel. Run `help` to see every
command:

| Command | What it does |
| --- | --- |
| `help` | List all available commands |
| `about` | Profile and background |
| `skills` | Technical strengths |
| `stack` | Technologies |
| `impact` | Measurable outcomes |
| `education` | Academic background |
| `contact` | How to reach me |
| `clear` | Clear the terminal |

### Interactive sections
- **Architecture** — six clickable stages of the semantic search pipeline
- **Skills** — filter six capability cards by domain

### Design
- Warm charcoal palette with a terracotta accent, matching the Claude Code
  aesthetic
- Space Grotesk for UI, JetBrains Mono for code and terminal chrome
- Motion driven by real spring physics encoded as CSS `linear()`, plus the
  `--ease-butter` curve for tree expansion
- `prefers-reduced-motion`, `prefers-reduced-transparency`, and
  `prefers-contrast` are all respected

### Accessibility
- Semantic landmarks with an accessible name on every section
- Full keyboard support for the tree, tabs, and terminal
- Skip link, visible focus rings, and `aria-live` on the terminal output

### SEO
- Structured data (schema.org `Person`)
- Open Graph and Twitter card metadata
- Canonical URL, robots directives, and a single descriptive `<h1>`

## Local development

```bash
python3 -m http.server 8000
```

Then visit <http://localhost:8000>. No build step required.

## Tech

Plain HTML, CSS, and vanilla JavaScript. Fonts are loaded from Google Fonts
(Space Grotesk + JetBrains Mono); everything else is self-contained.

## License

MIT

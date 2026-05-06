# AGENT.md — Weekly Newsletter: AI Context & Workflow

This file is the authoritative guide for any AI assistant working on this project. Read it fully before generating or modifying any file.

---

## What This Project Is

A weekly newsletter published every Friday. Each issue covers AI, technology, and tooling topics relevant to practitioners. The newsletter is:

- Hosted as a **public GitHub repo** (source of truth)
- Deployed as a **static website on Vercel** (root `index.html` is the landing/archive page)
- Each issue is a self-contained, single-file `index.html` — no build step, no frameworks, no dependencies beyond Google Fonts and inline CSS

The audience is technical — engineers, architects, AI practitioners. Design quality and information density both matter.

---

## Repository Structure

```
/
├── AGENT.md                  ← this file
├── index.html                ← root archive page (links to all issues)
├── README.md                 ← root markdown mirror of index.html
│
├── 2026-W20/                 ← one folder per issue, named by ISO week (YYYY-Www)
│   ├── TOPICS.md             ← INPUT: topics for this issue (you read this)
│   ├── index.html            ← OUTPUT: the rendered newsletter issue
│   └── README.md             ← OUTPUT: markdown version of same content
│
├── 2026-W21/
│   ├── TOPICS.md
│   ├── index.html
│   └── README.md
│
└── ...
```

**Folder naming rule:** Use ISO week format `YYYY-Www` (e.g. `2026-W20`, `2026-W21`). This ensures lexicographic sort = chronological sort and is unambiguous across locales.

---

## TOPICS.md Format

Each issue folder contains a `TOPICS.md` written by the human editor. It contains one or more topics in either of these forms:

```markdown
# Weekly Topics

## Topic Name
Short description of the topic. One to three sentences explaining what it is and why it matters.

## Another Topic
https://example.com/some-article

## Yet Another Topic
https://example.com/another-article
A brief annotation or context note about this link.
```

**Rules for reading TOPICS.md:**
- A topic may be just a name + description (write original content)
- A topic may include a URL (use it as a reference/source — do not scrape, infer from the URL and any annotation)
- Topics may mix both
- Expand topics into full editorial content — do not merely echo the input. Add context, explain concepts, draw connections.

---

## Workflow: Creating a New Issue

When asked to create a new weekly issue, follow these steps **in order**:

### Step 1 — Read TOPICS.md
Read the `TOPICS.md` file in the target week's folder. This is your content brief.

### Step 2 — Generate `index.html`
Create a complete, self-contained HTML newsletter. See **HTML Generation Rules** below.

### Step 3 — Generate `README.md`
Create a markdown version of the same content. See **README Generation Rules** below.

### Step 4 — Update root `index.html`
Add the new issue to the root archive page. See **Root Archive Rules** below.

### Step 5 — Update root `README.md`
Mirror the root archive in markdown.

---

## HTML Generation Rules

### Must-haves
- Single file. All CSS inline in `<style>`. No external JS. No frameworks.
- Google Fonts allowed via `<link>` tag (preconnect + font URL).
- Fully responsive. Works on mobile and desktop.
- `<title>` = main topic name + issue date
- A visible issue number or date in the header
- Navigation / table of contents for multi-topic issues
- Each topic as a distinct section with its own heading, body content, and visual treatment
- Footer with: issue date, "Back to Archive" link (`../index.html`), repo/site link

### Content quality bar
- Each topic section: minimum 150 words of editorial content
- Explain the "what", "why it matters", and "so what" for practitioners
- Use concrete examples, analogies, comparisons
- Where a URL was provided: reference the source, add your own analysis
- Avoid generic AI filler prose. Write with a point of view.

### Design rules

**Every issue must have a visually distinct design from all previous issues.**

Check what designs have been used (read other `index.html` files in sibling folders) and pick something different. The design must feel intentional and cohesive — not random.

Design dimensions to vary:
- **Color palette**: dark/light, warm/cool, monochrome/vibrant, duotone
- **Typography**: serif-led, mono-led, grotesque-led, mixed
- **Layout**: editorial columns, card grid, terminal/log style, magazine spread, timeline
- **Accent treatment**: glow effects, borders, gradients, geometric shapes, noise texture
- **Visual motif**: can reference the issue's topic (e.g. circuit board patterns for hardware topics)

**Design theme catalogue** (rotate through, don't repeat consecutively):

| Theme | Palette | Typography | Layout |
|---|---|---|---|
| Dark Acid | `#0d0d11` bg, `#d4ff47` accent | Bricolage Grotesque + IBM Plex Mono | Card grid, glow borders |
| Editorial Light | `#faf7f2` bg, `#1a1a1a` text, `#c0392b` accent | Playfair Display + Inter | Two-column editorial |
| Terminal Green | `#0a0f0a` bg, `#00ff41` accent | IBM Plex Mono only | Monospaced log/terminal |
| Pastel Brutalist | `#fff9f0` bg, `#ff6b35` + `#4a90d9` accents | Space Grotesk + bold slab | Raw borders, offset shadows |
| Midnight Purple | `#0f0a1e` bg, `#a78bfa` + `#f472b6` accents | DM Sans + Fira Code | Gradient cards, blur effects |
| Warm Newsprint | `#f5f0e8` bg, `#2c1810` text, `#8b4513` accent | Merriweather + Source Code Pro | Newspaper column layout |
| Neon Cyberpunk | `#050510` bg, `#00d4ff` + `#ff0090` accents | Rajdhani + Courier New | Asymmetric, scanline effects |
| Minimal Swiss | `#ffffff` bg, `#000000` text, `#ff0000` accent | Helvetica/Arial + strict grid | Swiss grid, strong hierarchy |
| Amber Console | `#1a1200` bg, `#ffb300` accent | IBM Plex Mono | Terminal amber, CRT feel |
| Ocean Deep | `#020c18` bg, `#0ea5e9` + `#38bdf8` accents | Plus Jakarta Sans + mono | Depth layers, wave patterns |

Expand this list with new themes as the newsletter grows. Never use the exact same theme twice.

### Noise / texture
A subtle SVG noise grain overlay (opacity ~0.025–0.04) adds polish to dark themes. Include it for dark designs. Skip for light designs.

### Code example: Noise overlay pattern
```css
body::after {
  content: '';
  position: fixed; inset: 0;
  pointer-events: none;
  z-index: 9999;
  opacity: 0.028;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  background-size: 160px;
}
```

---

## README Generation Rules

The `README.md` in each issue folder is the markdown version of the newsletter content. It must:

- Mirror all topics and content from `index.html`
- Use proper markdown headings, lists, blockquotes, and code blocks
- Include metadata at the top: issue date, issue number, topic list
- Be readable standalone (someone reading on GitHub should get full value)
- End with a link back to the web version and the archive

Format:
```markdown
# [Issue Title] — Week of [Date]

**Issue #N** · [Date] · Topics: Topic A, Topic B, Topic C

---

## Topic A

[content]

---

## Topic B

[content]

---

*Published every Friday. [View web version](index.html) · [Archive](../index.html)*
```

---

## Root Archive Rules

The root `index.html` is the landing page deployed to Vercel. It must:

- List all issues in **reverse chronological order** (newest first)
- Each entry shows: issue date, issue title/main topic, brief one-line description
- Each entry links to `./YYYY-Www/index.html`
- Have a consistent, polished design that acts as a "magazine cover" for the series
- Update automatically each week — do not redesign the root unless explicitly asked

When adding a new issue, insert at the top of the issues list. Do not change the overall design of root `index.html`.

The root `README.md` mirrors the root `index.html` as a markdown list of all issues. Issue links in `README.md` must point to `./YYYY-Www/README.md` (markdown), not `index.html`.

---

## Issue Numbering

Issues are numbered sequentially starting at #1. Determine the current issue number by counting existing issue folders (excluding the root). The first issue ever created is #1.

---

## Quality Checklist

Before finalizing any generated file, verify:

- [ ] `index.html` opens correctly as a local file (no broken relative paths)
- [ ] All sections have substantive content (not placeholder text)
- [ ] Design is distinct from previous issues
- [ ] Mobile responsive (no horizontal overflow)
- [ ] Footer has working "Back to Archive" link
- [ ] `README.md` mirrors content faithfully
- [ ] Root archive updated with new entry

---

## What NOT to Do

- Do not add JavaScript frameworks or npm dependencies
- Do not create build scripts or configuration files
- Do not use placeholder content ("Lorem ipsum" or "Content coming soon")
- Do not reuse a design theme that's already been used
- Do not modify other issues' files unless explicitly asked
- Do not skip the root `index.html` update step

---

## Context for AI Assistants

When a human says "create this week's newsletter" or "process TOPICS.md" or similar:

1. Identify the target folder (ask if ambiguous)
2. Read `TOPICS.md` in that folder
3. Read sibling `index.html` files to understand which design themes are already used
4. Generate `index.html` with a fresh design
5. Generate `README.md`
6. Update root `index.html` and `README.md`
7. Report what was created

When asked to "update the archive" or "add to index":
- Only update root `index.html` and `README.md`
- Do not touch individual issue files

When in doubt about content depth: write more, not less. This newsletter is for practitioners who want substance.

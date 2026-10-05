# Vault Design System Reference

Quick-reference for anyone (or Claude) working on the visual design of this vault.
For the full file inventory, see `README.md`. For plugin configs and mobile notes, see `~/.claude/skills/obsidian-visual-design/SKILL.md`.

---

## Everforest Spruce — Color Variables

Always prefer these CSS variables over hardcoded hex. They're defined in `themes/Everforest Spruce/theme.css`.

### Accent Colors (use for tags, callouts, highlights)

| Variable | Hex | Tone |
|---|---|---|
| `--everforest-red` | `#E67E80` | soft red |
| `--everforest-orange` | `#E69875` | warm orange (bold text) |
| `--everforest-yellow` | `#DBBC7F` | muted gold (italic text) |
| `--everforest-green` | `#A7C080` | sage green (accent, active nav) |
| `--everforest-aqua` | `#83C092` | mint aqua (blockquotes) |
| `--everforest-blue` | `#7FBBB3` | teal blue (links) |
| `--everforest-purple` | `#D699B6` | dusty rose (list markers) |

### Semantic Aliases (Obsidian maps these from the above)

| Variable | Maps to |
|---|---|
| `--color-red` | `--everforest-red` |
| `--color-orange` | `--everforest-orange` |
| `--color-yellow` | `--everforest-yellow` |
| `--color-green` | `--everforest-green` |
| `--color-blue` | `--everforest-blue` |
| `--color-purple` | `--everforest-purple` |
| `--text-accent` | green HSL (buttons, highlights) |
| `--text-accent-hover` | slightly lighter green |
| `--link-color` | `--everforest-blue` |

### Backgrounds

| Variable | Hex | Usage |
|---|---|---|
| `--everforest-bg_dim` | `#232A2E` | darkest — sidebar |
| `--everforest-bg0` | `#2D353B` | main editor background |
| `--everforest-bg1` | `#343F44` | slightly lighter |
| `--everforest-bg2` | `#3D484D` | hover states |
| `--everforest-bg3` | `#475258` | selected items |
| `--everforest-fg` | `#D3C6AA` | primary text (warm parchment) |
| `--everforest-grey2` | `#9DA9A0` | muted text |

---

## What Controls What

Use this before editing anything — wrong file = no effect.

| Visual Element | Controlled By | Where to Edit |
|---|---|---|
| Background colors | Theme | `theme.css` → `--color-base-*` |
| Text color | Theme | `theme.css` → `--text-normal` |
| Bold text color (orange) | Theme | `theme.css` → `--bold-color` |
| Italic text color (yellow) | Theme | `theme.css` → `--italic-color` |
| Link colors | Theme | `theme.css` → `--link-color` |
| Blockquote color | Theme | `theme.css` → `--blockquote-*` |
| Code syntax colors | Theme | `theme.css` → `--code-keyword` etc. |
| Tag pill appearance | Snippet | `colorful-tag.css` |
| Tag colors by name | Snippet | `colorful-tag.css` |
| Callout colors + icons | Snippet | `custom-callout-styles-material.css` |
| Callout border/opacity (system-wide) | Snippet | `embedding-styling.css` (overrides above) |
| Callout spacing | Snippet | `callout-spacing-nuclear-fix.css` |
| Embed border + background | Snippet | `embedding-styling.css` |
| Embed width | Snippet | `narrow-embed.css` |
| Checkbox custom states | Snippet | `checkbox-style.css` |
| Frontmatter property colors | Snippet | `colored-metadata.css` |
| Dashboard layout | Snippet | `dashboard.css` (requires `cssclass: dashboard`) |
| Multi-column layout | Snippet | `mcl-optimized.css` + `mcl-gallery-cards.css` |
| Calendar dot colors/themes | Snippet | `calendar-unified-system.css` |
| Inline word highlights | Plugin | `always-color-text` → `data.json` `wordEntries` |
| Frontmatter pill colors | Plugin | `pretty-properties` → `data.json` `propertyPillColors` |
| Note banner/cover/icon | Plugin | `pretty-properties` → frontmatter `banner`/`cover`/`icon` |
| Global UI icons & colors (tabs, files, ribbons, tags) | Plugin + Theme | `iconic` → `data.json` + `Plumbob/theme.css` |
| 2-pane file browser & Nav Rainbow | Plugin | `notebook-navigator` → `data.json` `navRainbow` |
| Font family / font size | Obsidian settings | Settings → Appearance |

---

## CSS Gotchas (Things That Silently Break)

### Scaling embeds
```css
✅  zoom: 0.85;                              /* works */
❌  transform: scale(0.85);                  /* does NOT work on .internal-embed */
```

### Targeting embedded notes
```css
✅  .internal-embed[src*="filename"]         /* most reliable */
✅  .internal-embed[src*="folder/"]
⚠️  .cssclass selectors on embeds            /* only works if frontmatter sets cssclass */
```

### Pseudo-elements inside embeds
`::before` and `::after` inside `.markdown-embed` blocks have `border-left: none` forced on them. If your callout icon disappears inside an embed, that's why. **Do not** add `content: none` or `display: none` to the embed pseudo-element reset — it kills SVG icons.

### Callout `--callout-color` must be RGB, not hex
```css
✅  --callout-color: 131, 192, 146;          /* RGB tuple */
❌  --callout-color: #83C092;               /* does not work with rgba() */
```

### DataviewJS CSS
DataviewJS renders inside a sandboxed container. Standard selectors may not reach inner elements. Target `.dataview` or `.block-language-dataviewjs`. SVG inside dataviewjs: use `dv.el("div", svg)` — raw HTML renders as text.

### After any CSS change
Reload the snippet: Settings → Appearance → CSS Snippets → toggle off, then back on.
Or reload the whole app: Ctrl+Shift+R.

---

## Known Override Chains

Snippets load **alphabetically**. Later file wins on equal specificity.

| Loser | Winner | What Gets Overridden |
|---|---|---|
| `custom-callout-styles-material.css` | `embedding-styling.css` | All callout borders → 2px/0.4 opacity; bg → 0.03 opacity |
| `callout-spacing-nuclear-fix.css` | `embedding-styling.css` | Border/padding may conflict (medium) |
| Theme tag defaults | `colorful-tag.css` | Tag pill appearance fully replaced |
| Theme metadata colors | `colored-metadata.css` | Frontmatter property colors replaced |
| `mcl-gallery-cards/wide-views` | `mcl-optimized.css` | Shared `.multi-column` base |

**Practical implication**: If you add a new callout with a thick border in `custom-callout-styles-material.css`, `embedding-styling.css` will reduce it to 2px. That's intentional — the system prefers subtle borders. To make one callout more prominent, add a higher-specificity rule (e.g., `body .callout[data-callout="my-type"]`) or edit `embedding-styling.css` to exempt it.

---

## Tag Colors (current)

All using theme variables — no hardcoded hex.

| Tag | Color Variable | Approximate Color |
|---|---|---|
| `#todo` | `var(--color-blue)` | teal #7FBBB3 |
| `#shopping` | `var(--color-purple)` | dusty rose #D699B6 |
| `#important` | `var(--color-red)` | soft red #E67E80 |
| `#newadd` | `var(--color-red)` | soft red #E67E80 |
| `#task` | `var(--color-orange)` | warm orange #E69875 |
| `#meeting` | `var(--everforest-aqua)` | mint #83C092 |
| `#comunicate` | `var(--color-green)` | sage #A7C080 |
| `#update` | `var(--everforest-blue)` | teal blue #7FBBB3 |

---

## How to Add a New Callout

1. Pick an icon from `snippets/icons/` (or download from fonts.google.com/icons → save as SVG)
2. Pick a color from the Everforest palette above
3. Add to `custom-callout-styles-material.css`:

```css
.callout[data-callout="my-type"] {
  --callout-color: 131, 192, 146;        /* RGB — use everforest-aqua as example */
  --callout-icon: url("icons/star.svg");
  border-left: 4px solid rgb(var(--callout-color)) !important;
  background-color: rgba(var(--callout-color), 0.1);
}
```

Note: `embedding-styling.css` will reduce the border to 2px system-wide. The 4px here only matters if embedding-styling is disabled.

Available custom types: `priority`, `progress`, `meeting`, `learn`, `practice`, `research`, `idea`, `inspiration`, `code`, `terminal`, `complete`, `pending`, `blocked`, `highlight`, `bookmark`

---

## How to Add a New Tag Color

Add both rules to `colorful-tag.css` (one for reading mode, one for editor):

```css
.tag[href^="#tagname"] {
  color: white;
  background-color: var(--everforest-aqua);
}
.cm-s-obsidian span.cm-tag-tagname, .tag[href^="#tagname"] {
  color: white;
  background-color: var(--everforest-aqua);
}
```

---

## Plugin Quick Reference

### Always Color Text
Config: `.obsidian/plugins/always-color-text/data.json`
- Edit `wordEntries` array to add/remove inline word colorings
- Each entry: `{ "text": "word", "color": "#hex", "style": "color" }`
- `enableAlwaysColor: true` colors text itself (not background highlight)

### Pretty Properties
Config: `.obsidian/plugins/pretty-properties/data.json`
- Edit `propertyPillColors` object to change frontmatter property pill colors
- Key = frontmatter property name, value = `{ "backgroundColor": "hsl(...)", "textColor": "..." }`
- Also controls `banner`, `cover`, and `icon` frontmatter property behavior

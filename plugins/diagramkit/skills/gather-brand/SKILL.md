---
name: gather-brand
description: "Gather a company's brand from its codebase for diagramkit, so diagramkit's diagrams match the figures the company already publishes: its token files, the components that draw its figures, a few of its published figures and a list of the parts they repeat. Use when someone asks, from inside their codebase, to make diagramkit match their brand, design system or their own diagrams. Team and Enterprise accounts only."
---

# Gather a brand for diagramkit

You collect; diagramkit's brand assistant builds. Find what the codebase holds about how the company's figures look and send it with `setup_brand`. Do not write diagram code, kit code, SVG or CSS for diagramkit.

## Collect

1. **Token files**, as they are (up to 8 files, 400 KB in all): CSS custom properties (`:root`, `.dark`, a Tailwind `@theme`), a theme object in TS or JS (palette, colours, typography, radii, shadows), W3C Design Tokens JSON, or a Claude Design `tokens.json`. Send the files that define the values, and the palette file a theme imports from.
2. **The components that draw their figures** (up to 16 excerpts, 60 KB in all): the code behind the diagrams in their docs or site (a diagram, node or card component, a connector, their icons), or else the design system's card, badge, pill and button. Keep the part that sets the look (markup, styles, sizes, corners, strokes, fills, type) and trim the rest.
3. **3 to 8 of their published figures**, best first: the diagrams the company publishes in its docs, blog or site, by the public address each is served at (find the files in the repo's docs images or `public/` folder, then their address on the live site, such as `https://acme.com/docs/img/architecture.svg`). Real diagrams, not screenshots or logos. The first matters most.
4. **A parts list**: each shape their figures repeat that a plain box, line or label would not match, by what their team calls it, with one line on how it looks and the figure that shows it. For example: `{ "name": "Replicated service", "description": "The service pill drawn as three offset copies, the back ones darker", "example": { "figure": 3 } }`. Add `crop: [left, top, width, height]` (fractions of the figure) when you can tell where it is. Name the 3 to 8 that matter; leave out ordinary boxes and arrows.
5. **Notes**, a few lines: the fonts their figures use, colours that carry meaning (reads in yellow, writes in orange), what their figures never show.

## Never send

Secrets of any kind: API keys, access tokens, passwords, `.env` files, credentials in config, customer data, private addresses. Read each file before you send it. Token files here mean design tokens.

## Send

Call `setup_brand` with `website` (the company's site), `notes`, `figures` (`[{ "url": "..." }]`), `token_files` (`[{ "name": "path", "content": "..." }]`), `components` (`[{ "name": "path", "source": "..." }]`) and `parts`. It answers at once with a `setup_id`; the setup takes several minutes. Call `setup_brand` with only `setup_id` to check on it. When it is done, tell the person what it matched and which parts it built, and send them to the Brand kit page (`review_url`) to compare the redrawn figures and apply the brand. Nothing changes for their diagrams until they apply it there.

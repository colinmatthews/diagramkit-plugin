---
name: set-brand
description: Bring a company's design system into diagramkit so every visual uses its colours, type, shape, surfaces and canvas. Use when someone asks to make diagramkit match their brand, apply their Claude Design system or design tokens, or use their company colours and fonts. Team and Enterprise accounts only.
---

# Bring a design system into diagramkit

A brand restyles every new visual for the account (and its previews): colour
roles, fonts by role, shape, density, surfaces, lines, charts, the page behind
the visual, motion and categorical colours. Earlier visuals keep their look.

Map the design system as completely as the source allows. A brand that sets
only colours still looks like diagramkit in other colours; the other fields are what
make it look like the company's own.

## Colour roles

| Role | What it colours |
| --- | --- |
| `background` | The page behind the visual |
| `surface` | Cards, panels, control backgrounds |
| `text` | Body text and outlines |
| `muted` | Secondary text and captions |
| `border` | Dividers and quiet borders |
| `primary` | The active path, selected controls, links and emphasis |
| `glyph`, `glyphAccent` | The fills of actor illustrations |
| `success`, `warning`, `error` | State colours (set them when the brand has its own) |

Roles you leave out are derived from the ones you give. Only `primary` is required.

## Theme

`theme` says which palette visuals use:

- `"light"`: the light colours everywhere, including in a dark host. This is the
  default when the source has no dark colours. diagramkit never invents a dark palette.
- `"follow-host"`: light or dark to match the host. The default when the source
  has dark colours (Claude Design themes named dark, `colors.dark`, token groups
  named `dark`). Without dark colours, diagramkit derives them.
- `"dark"`: the dark colours everywhere.

A brand guide that shows only light pages, or says to keep its paper in dark
mode, is `"light"`. The preview's last capture is a dark host: it shows the dark
theme, or that a fixed theme keeps its look.

## Everything else

| Field | What it sets |
| --- | --- |
| `typography` | `heading`, `body`, `mono` font stacks; `headingWeight`, `strongWeight` (100 to 900); `headingTracking`, `labelTracking` (em, e.g. -0.02, 0.08); `labelFont` (`mono`, `body`, `heading`) and `labelTransform` (`none`, `uppercase`) for labels, captions and metadata; `scale` (type size, 0.75 to 1.5) |
| `shape` | `corners`: `square`, `soft`, `round` or a radius multiplier 0 to 2. `borders`: `hairline`, `standard`, `bold` or a width multiplier. `shadows: false` removes every shadow |
| `density` | `compact`, `standard`, `comfortable` or a spacing multiplier 0.6 to 1.6 |
| `surfaces` | Cards and panels. `style`: `flat` (page colour, no border), `outlined` (surface and border, no shadow), `filled` (surface, no border), `elevated` (surface and shadow). Optional `background`, `borderColor`, `borderWidth` (px) |
| `lines` | Connectors and diagram lines: `weight` (multiplier), `color`, `dash` (`solid`, `dashed`, `dotted`) |
| `charts` | `gridColor`, `gridDash`, `axisColor` |
| `canvas` | The page behind the visual: `pattern` (`none`, `grid`, `dots`), `size` (px, default 24), `color` (default: text at 7%) |
| `motion` | `none`, `subtle` or `standard` |
| `categories` | Up to 8 colours for groups, series and legends, in order, as `{ "name": "Data", "color": "#1a3fd6" }`. Keep them apart from success, warning and error |
| `visualStyle` | Up to 2,000 characters of plain text on how their diagrams look that the other fields cannot set. Every client and agent drawing for the account reads it; it is never drawn |

Any colour field in `surfaces`, `lines`, `charts` and `canvas` also takes a role
name (`"text"`, `"border"`, `"primary"`...), which follows the theme. Use a role
whenever the brand means "the ink colour" rather than one fixed value.

A drafting-table brand, for example: `theme: "light"`, `shape: { corners:
"square", borders: "hairline", shadows: false }`, `surfaces: { style: "outlined",
borderColor: "text", borderWidth: 1 }`, `canvas: { pattern: "grid", size: 24 }`,
`typography: { labelFont: "mono", labelTransform: "uppercase", labelTracking: 0.08 }`.

## Visual style

Some of a brand's look is a feel no token captures: airy or dense, illustrated
or schematic, one primary visual or several panels. Describe it in
`visualStyle`, in a few short, general sentences about the overall feel, how
much white space there is, how a figure is composed (what leads, what is left
out), how many words sit on it, and decoration (texture, tilt, shadows,
hand-drawn touches). Leave out colours, hex codes, fonts, corners and line
weights: the other fields hold them. Describe only the feel, within what
diagramkit draws: never the subjects, words or data of their figures, and no
characters or illustrations of people. For example: "Airy and lightweight,
illustration-led. One primary visual under a short header and nothing else,
with lots of white space around it. Few words on the figure: a label only where
a part needs naming."

## Match their own figures

The fields restyle diagramkit's own drawing. To have diagrams match the company's
own figures part for part (their boxes, groups, lines, arrows and icons), follow
the gather-brand skill in their codebase and send what it collects with
`setup_brand`, or the person opens the Brand kit page at app.diagramkit.ai and
gives it a few of the figures they publish, or their website. Either way
diagramkit's brand assistant redraws those figures and builds the parts from
them. Brands set there keep their parts
when you change colours or fonts here, and `get_brand` says when a brand has
its own parts.

## Steps

1. Get the design system's values from its source (below). Use the exact values;
   never guess a brand colour from a screenshot when a file has it.
2. Call `preview_brand` with the brand `name`, the imported tokens or `colors`,
   and every other field the source defines.
3. Show the person the captures and the mapping diagramkit chose, and pass on every
   line under "Check these" (low contrast, missing font files, tokens diagramkit could
   not map). Fix what they ask and preview again. Fields you send override what
   diagramkit read from tokens, field by field.
4. Only when they say it looks right, call `set_brand` with the returned
   `brand_id`. This changes every new visual for the account, so never set a
   brand the person has not seen. `set_brand` with `clear: true` undoes it.

`get_brand` returns the current brand's fields in `structuredContent.style`, in
the same shape `preview_brand` takes, so a change starts from what is set.

## From a Claude Design system

A Claude Design system keeps its tokens in `project/tokens.json`. Read that file
from the system (for example with the Artifact tool's `read` on the system's link
and `path: "project/tokens.json"`) and send it whole as `claude_design.tokens`.
diagramkit reads its colour themes, aliases and usage notes, font families and type
styles, radius, shadow and border tokens, and tokens named as categories
(`category-data`, `chart-1`). Spacing and other families are named in the
warnings: set `density` and the rest yourself from the system's guidelines.
Read the system's README or guidelines too, and add what tokens do not carry:
the theme policy, surfaces, canvas and label style.

Fonts: `tokens.json` names font files under `fonts/` but does not contain them.
To use a typeface, read each `.woff2` file the system lists for the regular and
bold weights and send them in `fonts.files` as base64 (up to four files, about
500 KB each). Google-hosted families need no files: diagramkit fetches the heading,
body and mono families when you preview.

## From design tokens

A W3C Design Tokens file (Tokens Studio, Style Dictionary, Figma variable
exports: `$value` and `$type`) goes whole in `design_tokens.tokens`. Groups named
`light` and `dark` are themes. diagramkit reads `color`, `dimension` (radius, border
width), `fontFamily`, `fontWeight`, `shadow`, `border` and `typography` tokens.

## From any other source

Map what the source has onto the fields and send them explicitly: `colors.light`
(and `colors.dark` only when the brand defines dark colours), `typography`,
`shape`, `surfaces` and the rest. Accepted colours: hex, `rgb()`, `hsl()`, `oklch()`.

- **Figma variables or styles:** colour variables by role; text styles for
  heading, body and label fonts, weights and tracking; corner radius and effects
  for `shape`.
- **CSS variables, Tailwind config or a theme file:** read the values; follow
  aliases to their final value. `border-radius: 0` everywhere is `corners: "square"`.
- **Website or brand guidelines:** the primary brand colour is `primary`; a
  second brand colour suits `glyphAccent`; named palette colours are
  `categories`; neutrals for the rest. Look at the pages for corners, borders,
  shadows and the background.

## Custom CSS

`customCss` (up to 20 KB) is for what the fields and `visualStyle` cannot
express, applied after them inside the visual only: rules for its text,
background and fonts. Use it last, and keep it small. It accepts style rules,
`@media` and `@font-face`; no `@import`, no `url()` except `data:` fonts and
images, no `position: fixed`. Brand variables are `!important`, so a rule that
must win over them needs `!important` too.

## Logo

Send the company logo as `logo` (SVG, PNG or WebP, base64, up to about 140 KB)
and it is kept with the brand. From Claude Design, use the file
in the system's Logos asset group.

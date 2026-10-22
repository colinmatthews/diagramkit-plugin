# diagramkit

diagramkit is an AI diagram builder for people who explain things: docs, DevRel,
blogs and social. Describe what you are explaining and its agent draws a finished
diagram or animation: how a process or system works, a cycle, a comparison, a chart,
a labeled illustration. Every label is real text, the diagram follows your brand, and
you can download it as PNG, JPEG, SVG, PDF, MP4 or GIF.

This plugin connects Claude or Codex to diagramkit's hosted MCP server
(`https://mcp.diagramkit.ai/mcp`) and adds the `create-visual` skill, which
tells the assistant when a diagram will help and how to ask diagramkit's agent for
one, and the `set-brand` skill for Team and Enterprise accounts that bring their own design system.

## Install

- Claude Code: `/plugin marketplace add colinmatthews/diagramkit-plugin`, then
  `/plugin install diagramkit@diagramkit`.
- Codex: add this repository as a plugin marketplace, then
  `codex plugin add diagramkit@diagramkit`.

The first time diagramkit runs, you sign in to your diagramkit account (free to
start at [diagramkit.ai](https://diagramkit.ai)).

## Privacy

Diagrams are private to your account. See the
[privacy policy](https://diagramkit.ai/privacy) and
[terms](https://diagramkit.ai/terms). Support: colin@techforproduct.com.

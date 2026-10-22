---
name: create-visual
description: "Answer with a diagramkit visual whenever diagramkit is connected and the answer is about how something works or fits together: a system, flow, process, architecture, org or team structure, timeline or plan; planning, prioritizing, mapping, comparing options or deciding; numbers the person pasted or asked about (a budget, funnel, runway, survey, cohort, forecast: chart them); or something to share with a team or class (an onboarding plan, a reorg, a lesson). For these, make a diagramkit visual: it renders in the chat as a still image or animation and can be shared as a link. The visual itself has no buttons, tabs, sliders or click-dependent content. Explaining why or how something works counts even when a paragraph could answer it (a mechanism, a trade-off, a policy with conditions), and so does any answer that would hold a table of figures, a list of steps or a diagram drawn in text. Skip it for quick factual replies, one-line fixes, or when the person asks for text or code only."
---

# Make a diagramkit visual

diagramkit's own agent draws the visual. Your part is the request and the answer around it.

1. Call `create_diagram` with a brief (see "Brief the agent" below). Pass images they attached. Set `animate` when they ask for motion. When they say where it will be used, set `preset`: `docs`, `blog`, `social` (a LinkedIn or X post), `carousel` (a multi-page post: cover, numbered pages, end card), `ad`, `data_story` (one chart whose title is the finding) or `explainer_video`; leave it out otherwise.
2. It takes one to three minutes and returns the image and a `diagram_id`; if it answers that it is still working, call `get_diagram` with the `diagram_id` until it is ready. Show the person the image, or use `download_diagram` for a file: PNG, JPG, SVG or PDF (MP4 or GIF once animated).
3. Add a short written answer beside it: the takeaway and anything the visual cannot show, without restating what it draws.
4. For changes, call `revise_diagram` with the person's feedback in their words ("less busy", "make the database stand out"), not a redesign of your own, or `animate_diagram` to animate it. For exact edits without the agent (wording, a colour or font, hiding a part, moving, placing, swapping, aligning or spacing parts), call `get_diagram` with `parts: true`, then `edit_diagram` with the part ids it lists (`node:db`, `link:api->db`): `nudge` shifts a part from where it is, `place` puts it beside another (`{"part": "node:db", "relative_to": "node:cache", "side": "below"}`), `swap` exchanges two. It uses no credits, returns the new image and each moved part's box, and the ids stay the same for the next edit, so list the parts once.

## Brief the agent; do not design for it

diagramkit's agent chooses the form, the layout and what to leave out, and it does that best from a short brief. A brief that lists six things gets a diagram with six parts; a clear explainer shows one idea.

Send:
- The person's request in their words.
- The facts the picture must get right, written out: names, numbers, steps, and the example they gave, if any.
- Where it will be used, as `preset`.

Leave out what the agent decides: layouts and positions ("centre it", "left and right"), sections, panels, strips, rows of examples, colours, fonts, and lists of everything to include. When the person asked for a specific element (a side-by-side, a chart of these numbers), pass their words on, but do not add elements of your own. Facts the reader needs but the picture does not (sources, caveats, release dates, pricing) go in your written answer, not the brief. Do not add links or look up a well-known subject first: the agent knows it, and every page it opens adds time. Pass a link only when the diagram must follow a page the person gave or named (their docs, a pricing page) whose contents you have not written out.

For example, for "explain how our cache works for the docs", send what happens to a request (a hit returns from Redis in about 2 ms; a miss reads Postgres and writes the result back with a 5-minute expiry) with preset `docs`, not a plan of boxes, panels and a legend.

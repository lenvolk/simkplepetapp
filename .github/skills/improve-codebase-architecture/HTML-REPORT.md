# Architecture Report Format

Adapted from the upstream HTML report guide for an offline-capable Copilot workflow.

## Delivery

Produce a single HTML file with UTF-8 encoding, a viewport meta tag, inline CSS, and embedded diagrams. No CDN scripts, external fonts, analytics, remote images, or runtime network dependencies. The report must remain readable with JavaScript disabled.

Use semantic headings, sections, and one article per candidate. Keep the summary and top recommendation unframed. Avoid nested cards. Escape repository-derived text before inserting it into HTML, including filenames, code excerpts, and diagram labels; never execute repository content as report scripts.

## Content

The header contains the repository name, date, surveyed scope, and limitations. Include a compact diagram legend where necessary.

Each candidate contains:

- A short title naming the domain concept and proposed structural change.
- A text-labelled strength badge: `Strong`, `Worth exploring`, or `Speculative`.
- A dependency category: `in-process`, `local-substitutable`, `ports & adapters`, or `mock`.
- File paths with line references and short supporting evidence.
- Before and after diagrams, labelled as current and proposed.
- A concise problem statement and solution description.
- Concrete gains in locality, leverage, and behavioral testing.
- Trade-offs, uncertainties, and any applicable ADR conflict.

Keep prose concise but do not omit the evidence needed to evaluate a recommendation. The final section names the top candidate and links to its article. When no candidate merits action, say so instead of inventing a recommendation.

## Diagrams

Choose the visualization that fits the evidence:

- Call-flow or dependency graph for relationships across modules.
- Stacked bands for a chain of shallow forwarding modules.
- Interface/implementation comparison for excessive caller-facing complexity; show conceptual complexity, not misleading line-count measurements.
- Call-graph collapse for behavior concentrated behind one interface.

Use inline SVG with a title and accessible description, or CSS layouts with textual relationship labels. If using Mermaid tooling, render to SVG before embedding; do not load Mermaid from a CDN in the report. Provide a textual equivalent for each diagram.

Before/after diagrams may sit side by side on desktop but must stack on mobile. Use responsive viewBoxes, wrapping labels, and sufficient space for arrows; do not clip text to force a fixed height.

## Presentation and Verification

Use a restrained, high-contrast palette with distinct accents for recommendations and warnings. Strength must be understandable without color. Keep letter spacing at zero and use fixed or rem-based text sizes. Code excerpts may scroll within their own region; the page itself should not overflow horizontally.

When browser tools are available, inspect the report at desktop and mobile sizes, check that diagrams render and labels do not overlap, and check the console for errors. Verify it has no external resource dependencies. Correct rendering defects before sharing. If browser validation is unavailable, disclose that limitation.
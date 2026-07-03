# ShivanshOS — Design System

## Introduction

### 0.1 What ShivanshOS Is

ShivanshOS is not a single application, website, or product — it is a
personal operating layer that spans identity, infrastructure, tooling,
and interface. It is the umbrella under which Shivansh Sethi's public
brand (`shivanshsethi.in`), internal infrastructure (Cloudflare Zero
Trust, self-hosted services, HPC workstation access), knowledge systems
(the Obsidian-based personal data lake and knowledge graph), and
AI-driven tooling (agentic orchestration, local and cloud LLM tooling,
automation pipelines) all live as one coherent system rather than a
collection of disconnected projects.

Because ShivanshOS spans this many surfaces — public-facing pages,
internal dashboards, developer tooling, CLI output, agent-facing
interfaces, and eventually embedded/device-level interfaces tied to
drone and hardware work under Aiotize Inc. — there is a real risk of
visual and interaction drift: every new surface invents its own
spacing, tone, and structure because no shared reference exists. This
document is that shared reference.

### 0.2 Why This Document Exists — And Who/What Reads It

This document has two audiences, and both need to be served by the same
text:

1. **Human collaborators (including future Shivansh)** — anyone
   designing or reviewing a new ShivanshOS surface should be able to
   read this file top to bottom and understand what "on-brand" means
   without needing to reverse-engineer it from existing screens.

2. **AI systems and agentic tooling, consumed via MCP** — this file is
   intended to be imported as structured context by design-generation
   tools (e.g. Stitch-style AI UI generators, custom design agents, or
   MCP-connected tools operating on behalf of ShivanshOS) so that
   AI-generated interfaces inherit the same design language
   automatically, rather than each generation session starting from a
   blank slate and producing visually inconsistent output.

Because an AI agent reading this file via MCP has no access to tacit
knowledge — no memory of past design decisions, no intuition about
"what feels right" — **this document must be explicit rather than
suggestive**. Where a human designer could infer intent from a vague
principle, an agent generating a new screen needs concrete,
unambiguous, machine-usable guidance: named values, defined states,
explicit do/don't examples, and clearly scoped contexts. Every section
of this design system should therefore aim to be specific enough that
an agent could generate a compliant interface using this document
*alone*, without additional clarification.

This has a direct consequence for how the rest of the document is
written: principles are followed by concrete rules, rules are followed
by examples, and ambiguous or "vibes-based" language is treated as a
defect to be fixed, not a stylistic choice.

### 0.3 Scope

This document governs the design language for:

- **Public surfaces** — `shivanshsethi.in` and any public-facing pages,
  including the CV/portfolio surface and any public documentation
- **Internal surfaces** — dashboards, monitoring views, and admin
  tooling used to operate ShivanshOS infrastructure (identity/IdP
  management, Cloudflare Zero Trust access, backup/CI status)
- **Agent-facing surfaces** — interfaces where output is generated or
  consumed by AI agents (e.g. the Hermes agent setup), including
  conversational UI, structured data views, and status/alerting output
  intended to be "plain-English" and legible at a glance
- **Developer-facing surfaces** — CLI output, installer prompts (e.g.
  the automated Linux/AI tooling installer), and setup wizards
- **Future embedded/device surfaces** — interfaces tied to drone
  systems and hardware under Aiotize Inc., where constraints such as
  screen size, latency, and glanceability dominate over decorative
  design

It does *not* govern backend architecture, infrastructure topology, or
security policy — those are covered by ShivanshOS's separate identity
and infrastructure documentation. Where design decisions intersect
with those domains (for example, how a permission boundary is
*visually represented* in a dashboard), this document defers to the
infrastructure docs for the underlying model and only defines how that
model is *presented*.

### 0.4 Guiding Philosophy

Four commitments shape every decision in this document:

- **One system, many surfaces.** A user (human or agent) moving from
  the public site to an internal dashboard to a CLI prompt should
  experience a consistent visual and structural language — consistent
  typography logic, consistent color meaning, consistent spacing
  rhythm — even though the surfaces themselves look very different.

- **Design for humans and agents simultaneously.** Every interface
  decision is evaluated against two questions: *does this read clearly
  to a person glancing at it*, and *does this expose clean, structured
  information that an agent can parse or generate reliably*. Neither
  audience is treated as secondary.

- **Defaults over debate.** The system favors strong, explicit
  defaults over open-ended flexibility. A new surface should be
  buildable quickly by picking from this document rather than
  re-deriving decisions from first principles each time.

- **Documentation-heavy, reproducible, source-of-truth driven.** In
  keeping with how ShivanshOS's infrastructure is run — Cloudflare,
  GitHub Enterprise, Docker, reproducible and automated wherever
  possible — this design system itself is treated as versioned,
  living infrastructure. It is expected to be read programmatically,
  updated deliberately, and trusted as canonical rather than
  aspirational.

### 0.5 Document Structure

The remainder of this document is organized so that both a human
reader and an MCP-importing agent can navigate it predictably:

1. **Design Principles** — the underlying opinions behind every visual
   and interaction decision, stated as explicit, testable rules
2. **Visual Foundations** — color, typography, spacing, and elevation
   systems, defined as concrete tokens rather than descriptive prose
3. **Device Types & Contexts** — how design adapts across desktop,
   mobile, embedded/drone interfaces, and headless/agent-facing
   surfaces, including breakpoints and context-specific constraints
4. **Design Modes** — the distinct operating modes a ShivanshOS
   surface can be in (e.g. public-facing, internal/operational,
   agent-console, installer/CLI) and how visual language shifts
   between them while remaining recognizably one system
5. **Prompting & Generation Guidelines** — how AI design tools should
   be prompted to extend ShivanshOS consistently, including example
   prompts and anti-patterns to avoid
6. **Components & Patterns** — the reusable building blocks (buttons,
   status indicators, alert/notification patterns, data tables, agent
   response containers, etc.) that keep new surfaces from being built
   from scratch
7. **Governance & Versioning** — how this document itself is updated,
   who/what has authority to change it, and how changes propagate to
   MCP-connected tooling that has already imported earlier versions

### 0.6 A Living Document

This file is not a one-time specification — it is expected to change
as ShivanshOS grows, as new surfaces are built, and as design patterns
prove themselves in practice. Because AI tooling will import this file
directly, **staleness here is not a cosmetic problem — it is a
correctness problem**: an outdated design doc will actively cause an
agent to generate off-brand or contradictory interfaces with full
confidence. Any time a new pattern is adopted in practice, this
document should be updated in the same change set, not as an
afterthought.

---

## 1. Design Principles

These principles are the testable criteria for any ShivanshOS interface. If a design violates these, it is "off-brand" regardless of how it looks.

1.  **Information over Decoration.** Every visual element must serve a functional purpose. Color is for meaning; spacing is for hierarchy; typography is for clarity. If a pixel does not convey data, define structure, or indicate state, it should be removed.

2.  **Machine-Readable by Design.** UI structures must be as legible to an AI agent as they are to a human. This means semantic HTML, stable element IDs, and structured data attributes (e.g., `data-status="error"`) are not "extra" — they are fundamental requirements. A "beautiful" UI that an agent cannot parse is a failure.

3.  **Density for Operations, Air for Public.** ShivanshOS scales its density based on context. Operational dashboards and agent consoles must prioritize information density and glanceability (using `spacing-01` to `spacing-05`). Public-facing surfaces should use "air" (`spacing-08` and above) to guide the human eye and manage narrative flow.

4.  **Token-Strict Implementation.** Hardcoded values are treated as defects. Every color, font size, and spacing value must resolve to a named ShivanshOS token. This ensures that a global theme update (e.g., shifting the grayscale) can be executed by updating the token definition alone.

5.  **State is Sovereign.** Visual changes must always reflect a change in the underlying system state. Animation and transitions are reserved for communicating state changes (e.g., an agent thinking vs. responding) or temporal relationships. "Decorative" animation that does not convey state is prohibited.

6.  **Human-Agent Parity.** No interface is "human-only." Even the most polished public page should have a clean, structured representation available for agentic consumption (e.g., via JSON-LD or clear DOM structure). Conversely, agent-facing CLI output should be formatted for human glanceability.

---

## 2. Visual Foundations

### 2.0 Approach

ShivanshOS's visual foundation is **inspired by IBM Carbon Design
System, Perplexity AI, and Bewusst BeWerben** — combining Carbon's
token-based logic with a calm, search-canvas aesthetic. It
emphasizes a warm, high-contrast reading environment (stone fields,
dark slate rails) and a single brand "voltage" color (deep teal).

The system adopts Carbon's **underlying logic** — scale-based grays and
role-based tokens — while embracing modern **interaction patterns**
— pill-shaped geometry and an "engine for reading" rather than a
"console to operate."

**Token-first rule (binding for MCP consumers):** No surface, human- or
agent-generated, should reference a raw hex value directly. Every
color, font size, or spacing value used in a ShivanshOS interface must
resolve to a named token defined in this section. This is the single
most important rule in this document for AI tooling: an agent
generating a new screen should output token names (`$color-background`,
`$spacing-05`) or their resolved values *from this table*, never
invented or approximated values.

### 2.1 Color

#### 2.1.1 Grayscale (`gray-10` → `gray-100`)

Grayscale is the dominant palette across ShivanshOS. It carries
structure, hierarchy, and layering — color (teal/accent) is reserved
for meaning, not decoration.

| Token | Hex | Typical Role |
|---|---|---|
| `white` | `#ece6e3` | Base light background (Stone), calm reading canvas |
| `gray-10` | `#f4f4f4` | Secondary light background, subtle containers |
| `gray-20` | `#e0e0e0` | Borders, dividers on light backgrounds |
| `gray-30` | `#c6c6c6` | Disabled borders, low-emphasis dividers |
| `gray-40` | `#a8a8a8` | Placeholder text, disabled text |
| `gray-50` | `#8d8d8d` | Secondary/helper text |
| `gray-60` | `#6f6f6f` | Body text on light backgrounds (AA-compliant) |
| `gray-70` | `#525252` | Strong secondary text, icons |
| `gray-80` | `#393939` | Dark UI containers, high-contrast elements |
| `gray-90` | `#262626` | Dark theme secondary background |
| `gray-100` | `#1f2937` | Dark theme base background (Slate), high-contrast |
| `black` | `#000000` | Reserved — avoid pure black except for true dark-mode edge cases |

#### 2.1.2 Interactive Teal

A single teal family is the primary interactive color across all of
ShivanshOS — the "brand voltage" that signifies action and
intelligence.

| Token | Hex | Typical Role |
|---|---|---|
| `teal-10` | `#e6f0f1` | Selected-state background, subtle highlight |
| `teal-20` | `#cce2e4` | Hover background on light surfaces |
| `teal-30` | `#99c5c9` | Disabled interactive elements (light theme) |
| `teal-40` | `#66a7ad` | Interactive elements on dark backgrounds |
| `teal-50` | `#338992` | Secondary interactive accents |
| `teal-60` | `#0d4659` | **Primary brand color** (Deep Teal) — primary buttons, links |
| `teal-70` | `#0a3845` | Hover/active state for primary interactive elements |
| `teal-80` | `#082b36` | Pressed/active-deep state |
| `teal-90` | `#051e26` | High-contrast accents, dark-theme emphasis |
| `teal-100` | `#030f16` | Reserved for extreme-contrast or print contexts |

#### 2.1.3 Semantic / Status Colors

Used exclusively for meaning — never decoratively. This matters
specifically for agent-facing surfaces (Hermes-style status output,
monitoring alerts) where color must reliably encode state.

| Token | Approx. Hex | Meaning | Usage |
|---|---|---|---|
| `red-60` | `#da1e28` | Error / Danger | Failed builds, auth failures, destructive actions |
| `green-50` | `#24a148` | Success | Completed tasks, healthy service status |
| `yellow-30` | `#f1c21b` | Warning | Degraded state, approaching limits, needs attention |
| `blue-60` | `#0f62fe` | Informational | Neutral status, in-progress states |
| `purple-60` | `#8a3ffc` | Agent / Automation marker | Reserved specifically to flag agent-originated actions or content, distinguishing them from human-originated ones at a glance |

> **Note:** semantic hex values above follow Carbon's public v11
> defaults as commonly documented; treat them as a strong starting
> point rather than pixel-perfect fact, and pin exact values in code
> (e.g. via a `tokens.json`) the first time this system is implemented
> rather than trusting hand-copied hex codes indefinitely.

#### 2.1.4 Themes

ShivanshOS defines two default themes, directly following Carbon's
light/dark pairing logic:

- **Light (default, public-facing)** — background `white`/`gray-10`,
  primary text `gray-100`, used for `shivanshsethi.in` and any
  public-facing surface.
- **Dark (default, operational/agent-facing)** — background
  `gray-100`/`gray-90`, primary text `white`/`gray-10`, used for
  dashboards, CLI/installer output, and agent-console surfaces, where
  long viewing sessions and glanceable status color (via 2.1.3) matter
  more than public-facing polish.

An agent generating a new internal/operational surface should default
to the **dark theme** unless explicitly told the surface is
public-facing.

### 2.2 Typography

ShivanshOS follows Carbon's typeface choice directly: **IBM Plex**,
released under an open license and freely usable — appropriate both
for its technical, engineered character and its practical
availability via Google Fonts and self-hosting.

| Token | Typeface | Usage |
|---|---|---|
| `font-sans` | **IBM Plex Sans** | All UI text — body, headings, labels, navigation |
| `font-mono` | **IBM Plex Mono** | Code, CLI output, API/agent payloads, technical identifiers (paths, hashes, tokens) |
| `font-serif` | **IBM Plex Serif** *(reserved, optional)* | Long-form reading contexts only (e.g. a future ShivanshOS blog/writing surface) — not used in product UI |

#### 2.2.1 Type Scale (Productive style — UI-focused, not expressive/marketing)

| Token | Size / Line-height | Weight | Usage |
|---|---|---|---|
| `type-heading-05` | 42px / 50px | 600 | Page-level hero headings (public site only) |
| `type-heading-04` | 28px / 36px | 600 | Section headings |
| `type-heading-03` | 20px / 28px | 600 | Subsection headings, card titles |
| `type-heading-02` | 16px / 24px | 600 | Component-level headings, table headers |
| `type-body-long-01` | 14px / 20px | 400 | Default body text, paragraphs |
| `type-body-short-01` | 14px / 18px | 400 | Compact body text — table cells, list items |
| `type-label-01` | 12px / 16px | 400 | Field labels, metadata, timestamps |
| `type-code-01` | 14px / 20px | 400 (mono) | Inline code, CLI output, agent payload display |

Agent-facing text output (status lines, structured summaries) should
default to `type-body-short-01` or `type-code-01` — optimized for
density and scanability, not editorial reading.

### 2.3 Spacing

ShivanshOS uses Carbon's 2px-based spacing scale — a non-linear
progression (not a flat 8px grid) that gives fine control at small
sizes and larger jumps at bigger sizes, which in practice maps well
onto both dense dashboard UI and airier public-facing layouts.

| Token | Value | Typical Usage |
|---|---|---|
| `spacing-01` | 2px | Hairline gaps, icon-to-text spacing |
| `spacing-02` | 4px | Tight internal padding |
| `spacing-03` | 8px | Default internal component padding |
| `spacing-04` | 12px | Form field spacing |
| `spacing-05` | 16px | Default gap between related elements |
| `spacing-06` | 24px | Gap between distinct components |
| `spacing-07` | 32px | Section-internal spacing |
| `spacing-08` | 40px | Card/container padding (larger surfaces) |
| `spacing-09` | 48px | Gap between major sections |
| `spacing-10` | 64px | Large layout gutters |
| `spacing-11` | 80px | Page-section separation (public site) |
| `spacing-12` | 96px | Hero/major landing separation |
| `spacing-13` | 160px | Reserved for large-format/public marketing contexts only |

**Rule for agents:** default to `spacing-03` through `spacing-06` for
any dashboard, dense, or agent-facing UI. Reserve `spacing-09` and
above for public-facing marketing/landing contexts — using large
spacing values in operational UI is a common failure mode to avoid, as
it wastes screen real estate in exactly the contexts where density and
glanceability matter most.

### 2.4 Elevation & Layering

Rather than heavy shadows, ShivanshOS follows Carbon's **flat
layering model**: hierarchy is communicated primarily through
background color steps (e.g. `white` → `gray-10` → `white`, or
`gray-100` → `gray-90` in dark theme), with shadow reserved only for
genuinely floating elements.

| Token | Usage |
|---|---|
| `layer-01` | Base page background |
| `layer-02` | First-level container (cards, panels) |
| `layer-03` | Nested container within a `layer-02` element |
| `shadow-float` | Reserved for modals, dropdowns, and tooltips only — not for static containers |

### 2.5 Geometry & Radii

ShivanshOS embraces Perplexity's pill-based geometry for interactive
elements.

| Token | Value | Typical Usage |
|---|---|---|
| `radius-pill` | 9999px | Primary buttons, search inputs, sign-in pills |
| `radius-lg` | 16px | Large cards, modals, main workspace containers |
| `radius-md` | 12px | Standard cards, secondary panels |
| `radius-sm` | 8px | Small components, dropdowns, tooltips |
| `radius-xs` | 4px | Internal sub-elements, status dots |

---

## 3. Device Types & Contexts

ShivanshOS interfaces adapt to four primary contexts. Breakpoints follow Carbon's standard set.

### 3.1 Breakpoints

| Token | Min Width | Columns | Gutter | Margin |
|---|---|---|---|---|
| `sm` | 320px | 4 | 16px | 16px |
| `md` | 672px | 8 | 16px | 16px |
| `lg` | 1056px | 16 | 16px | 16px |
| `xlg` | 1312px | 16 | 16px | 16px |
| `max` | 1584px | 16 | 16px | 16px |

### 3.2 Context-Specific Constraints

#### 3.2.1 Desktop & Operational (LG+)
- **Priority:** Multi-tasking, high information density, sidebars for navigation.
- **Rules:** Use `layer-01` for background and `layer-02` for primary workspace cards. Maximize use of `font-mono` for data tables and logs.

#### 3.2.2 Mobile & On-the-go (SM, MD)
- **Priority:** Single-tasking, thumb-friendly targets (min 48px height), glanceable status.
- **Rules:** Collapse sidebars into bottom sheets or hamburger menus. Increase spacing to `spacing-05` for touch targets. Hide secondary metadata by default.

#### 3.2.3 Drone & Embedded (Fixed Small Screens)
- **Priority:** Sunlight readability, high-contrast status, low-latency feedback.
- **Rules:** Use **Dark Mode only** with high-contrast semantic colors (e.g., `yellow-30` or `red-60` on `gray-100`). Use `type-heading-03` for primary telemetry values to ensure legibility from a distance.

#### 3.2.4 Headless & Agent-Facing (JSON/DOM)
- **Priority:** Structural integrity, stable identifiers, semantic clarity.
- **Rules:** No visual CSS applies. Focus on `aria-label`, `data-testid`, and semantic HTML tags. Ensure a clear hierarchical path to all data points.

---

## 4. Design Modes

A ShivanshOS surface always operates in one of four modes. The mode dictates the theme, density, and interactive tone.

| Mode | Theme | Primary Surface | Spacing | Audience |
|---|---|---|---|---|
| **Public-Facing** | Light | `white` | `spacing-06+` | General Public |
| **Operational** | Dark | `gray-100` | `spacing-03` to `05` | Shivansh (Admin) |
| **Agent-Console** | Dark | `gray-100` | `spacing-02` to `04` | Agents/Shivansh |
| **Installer/CLI** | Dark (Terminal) | `black` | Terminal-defined | Developers/Agents |

### 4.1 Public-Facing Mode (`shivanshsethi.in`)
Used for the CV, portfolio, and public docs.
- **Goal:** Clarity, narrative, personal brand.
- **Key Token:** Primary action `teal-60`, Background `white`, Headings `type-heading-05`.
- **Constraint:** Must maintain AA accessibility compliance for human readers.

### 4.2 Operational Mode (Dashboards)
Used for infrastructure monitoring (Cloudflare status, backup logs, home lab status).
- **Goal:** Speed of comprehension, status awareness.
- **Key Token:** Semantic colors `green-50`, `red-60`, `yellow-30` are dominant.
- **Constraint:** Use `font-mono` for all numeric data.

### 4.3 Agent-Console Mode (Hermes/Conversational)
Used for direct interaction with AI agents.
- **Goal:** Clear distinction between agent output and human input.
- **Interaction:** Input uses a large pill-shaped search composer (`radius-pill`).
- **Key Token:** Agent content is flagged with `purple-60` accents or borders.
- **Constraint:** Every agent message must include a timestamp (`type-label-01`) and a source identifier.

### 4.4 Installer/CLI Mode
Used for setup scripts and local tooling.
- **Goal:** Reliability, minimal distraction.
- **Key Token:** `font-mono` is the only allowed typeface.
- **Constraint:** Use standard ANSI color mappings that resolve to ShivanshOS semantic colors (e.g., ANSI Red -> `red-60`).

---

## 5. Prompting & Generation Guidelines

To ensure consistency when agents generate new surfaces, use the following guidelines and example prompts.

### 5.1 The ShivanshOS System Prompt
When initiating a design task, include this context:
> "You are an expert UI engineer implementing a surface for ShivanshOS. You must strictly follow the ShivanshOS Design System (design.md). Use named tokens for all colors, spacing, and typography. Default to Operational Dark Mode unless Public-Facing is specified. Prioritize machine-readability and information density."

### 5.2 Example Prompts

#### 5.2.1 Generating a New Dashboard Component
> "Create a 'Service Status' card for the ShivanshOS Operational Dashboard. It should show the status of 'Cloudflare Zero Trust' and 'Local HPC'. Use `layer-02` for the card background, `spacing-03` for internal padding, and `green-50` or `red-60` for status indicators. Ensure all text uses `type-body-short-01` and the titles use `type-heading-02`."

#### 5.2.2 Generating an Agent Interaction View
> "Design a conversational interaction container for an AI agent. Flag the agent's response using a `purple-60` left border. Use `spacing-04` between messages. Include a timestamp using `type-label-01` in `gray-50`. Ensure the container has a `data-agent-id` attribute for machine tracking."

### 5.3 Anti-Patterns to Avoid
- **DO NOT** use raw hex codes (e.g., `#0d4659`). Use `$teal-60`.
- **DO NOT** use generic spacing (e.g., `margin: 10px`). Use `$spacing-04`.
- **DO NOT** use expressive fonts or icons unless they serve a functional status role.
- **DO NOT** use light theme for internal tools unless explicitly requested.

---

## 6. Components & Patterns

Standardized building blocks for all ShivanshOS surfaces.

### 6.1 Buttons & Interactive Elements
- **Style:** All primary and secondary buttons must use `radius-pill`.
- **Primary:** `teal-60` background, `white` text. Hover: `teal-70`.
- **Secondary:** `gray-80` background, `white` text. Hover: `gray-70`.
- **Ghost/Tertiary:** No background, `teal-60` text. Hover: `teal-10`.
- **Danger:** `red-60` background, `white` text.

### 6.2 Status Indicators
Used to communicate system health.
- **Healthy:** `green-50` dot + "Healthy" label.
- **Degraded:** `yellow-30` dot + "Degraded" label.
- **Failed:** `red-60` dot + "Failed" label.
- **In-Progress:** `teal-60` spinning indicator (reserved for state changes).

### 6.3 Data Tables (Operational Mode)
- **Header:** `gray-80` background, `white` text, `type-heading-02`.
- **Rows:** Alternating `gray-100` and `gray-90`.
- **Cells:** `type-body-short-01` or `type-code-01` for identifiers.
- **Constraint:** Tables must be sortable and include a "Copy to Clipboard" action for any technical identifier.

### 6.4 Agent Response Container
- **Background:** `gray-90` (slightly lighter than base background).
- **Accent:** `purple-60` vertical bar on the left edge (4px width).
- **Metadata:** Top-aligned name, timestamp, and "Model Used" identifier in `type-label-01`.
- **Interaction:** Must include a "Regenerate" and "Copy Payload" button in ghost style.

### 6.5 Notification Toasts
- **Position:** Bottom-right on desktop, top-center on mobile.
- **Duration:** 5 seconds for info, persistent for errors until dismissed.
- **Style:** `layer-03` with a `shadow-float` and a status-colored left border.

---

## 7. Governance & Versioning

The ShivanshOS Design System is versioned infrastructure.

### 7.1 Authority & Updates
- **Owner:** Shivansh Sethi.
- **Update Trigger:** Any time a new visual pattern is implemented in a production surface, it must be backported here.
- **Review Process:** Changes must be reviewed for token compliance and principle alignment.

### 7.2 Versioning Policy
- **Minor Updates (vX.Y.Z):** Non-breaking changes (e.g., adding a new component, refining a description).
- **Major Updates (vX.0.0):** Breaking changes to tokens (e.g., changing the interactive brand family) or a complete shift in design philosophy.

### 7.3 Distribution to Agents
- **Primary Source:** The `design.md` file in the root of the ShivanshOS configuration repository.
- **Machine Consumption:** Agents consume this file via MCP (Model Context Protocol). Any update to this file should trigger a re-index of the agent's design context.
- **Token Export:** A `tokens.json` file is maintained alongside this document for programmatic consumption by CSS-in-JS and build tools.

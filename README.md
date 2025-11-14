# Calculogic-Living-Document
The living document for conceptually designing and technologically implementing Calculogic.
# Foundational and Repo-Specific Concern System
This document governs all front-end and interaction logic across Calculogic's ecosystem. It treats the interface as its own concern layer within the larger Calculogic Repository collection.

0. Universal Foundation (Applies to every repo)
0.A Terminology Note – Metaphorical Layer Model
The concern terms Build, Logic (Workflow), Knowledge, Results, and Style are metaphorical programming concerns — not literal framework layers or file types.
 They describe what a part of the system is responsible for, not how or in what technology it is written.
Term
Metaphorical meaning
Typical manifestation
Build
The structural definition – what exists and where it lives.
JSX/TSX markup, HTML structure, component skeletons.
Build → Style (BuildStyle)
The visual styling of Build structures.
CSS, theme tokens, transitions scoped to Build.
Logic (Workflow)
The behavior layer – how state changes, how users interact.
Hooks, stores, event handlers, routing, validation.
Knowledge
The informational reference – what names, labels, or constraints mean.
Constants, schemas, localization, documentation maps.
Results
The derivative or output – what the system produces or reports.
Rendered summaries, exports, analytics views.
Results → Style (ResultsStyle)
Styling of outputs; non-structural visuals of results.
CSS, chart theming, transitions scoped to results.

Purpose: This model provides a semantic separation of responsibilities across any stack (React, PHP, Python, etc.).
 Repositories may use different file types (.tsx, .ts, .css, .json), but conceptual boundaries remain identical.

0.1 Canonical Flow
Build → Logic (Workflow) → Knowledge → Results
Style is contextual:
Build → Style (BuildStyle)
Results → Style (ResultsStyle)

Reasoning: Creation precedes behavior; behavior is informed; outcomes are derived. Style modifies what exists in its stage.

0.2 Purity Per Layer
Build: Structure only (containers, anchors, slots). No state, no data fetch.
Logic (Workflow): State, interaction, orchestration. No layout, no copy.
Knowledge: Lexicon, schemas, constraints, a11y copy. No rendering/behavior.
Results: Derived output, analytics, exports. No mutation of source state.
Style: Visual treatment and spacing; no structure or logic.

0.3 Directionality & Attachment
Directional flow: Build → BuildStyle → Logic (Workflow) → Knowledge → Results → ResultsStyle
 Knowledge informs all. No upward dependencies.
Attachment-only: Non-source layers attach to Build anchors; they never reshape or reorder.
Monotonic diffs: Downstream layers may add presentation or data but not mutate structure.

0.4 Cross-Cutting Extensions
These apply inside the concerns above.
Extension
Build
Logic (Workflow)
Knowledge
Style
Results
DnD
Handles/zones declared
Lifecycle, undo, constraints
ARIA copy, schemas
Visual states
Finalized order
Accessibility
Anchors, ARIA roles
Focus, keyboard parity
a11y strings
Visual feedback
Read-only display
Routing
–
Logic only
–
–
–
Autosave/Sync
–
Orchestration
Copy text
Confirmation visuals
Status readouts
Error/Empty/Loading
–
Behavior
Copy text
Visual
Output view
Theming
Tokens only
–
–
Style-scoped
–
i18n/L10n
–
–
All strings centralized
–
–
Performance
–
Transition ≤200 ms
–
Transform/opacity only
–
Telemetry (read-only)
–
–
–
–
Metrics only


0.5 Minimal Provenance
Every concern entry records:
Title
One-sentence description
Provenance pointer to this section (e.g. Foundation §0.2)
Terminology: "Anchors" = stable data-anchor="..." selectors created by Build.
 Anchors must be unique, kebab-case, and remain stable across versions.

0.6 Spatial Synchronization (Engine Rule)
Within a Configuration, the sequence order is authoritative across all concerns.
 If an atomic or container appears in multiple tabs, its relative position must be identical everywhere.
 Downstream concerns (Logic, Knowledge, Style, Results) attach to Build anchors but cannot reorder or reshape them.

# 1. Repo Scope Declaration
Repository: Interface
 Scope: All front-end and interaction logic for Calculogic. Treats the interface as its own concern layer within the larger Calculogic collection.
 Inherits Universal Foundation §0 without modification.

2. General Concerns (Interface Repository)
2.1 Build
Declares structure, anchors, and drop topology; no behavior.
 Provenance: Foundation §§0.2–0.3
2.2 Build → Style (BuildStyle)
Visual styling for Build structures; non-structural only.
 Provenance: Foundation §0.2
2.3 Logic (Workflow)
Handles interaction/state orchestration (routing, validation, DnD).
 Consumes schemas from Knowledge; enforces constraints at runtime.
 Provenance: Foundation §§0.2, 0.4
2.4 Knowledge
Holds lexicon, constraint schemas, ARIA/i18n strings.
 Defines; Logic consumes. No rendering or runtime state.
 Provenance: Foundation §0.2
2.5 Results
Renders derived outputs/exports; read-only with respect to state.
 May apply presentation transforms (grouping/sorting), not computation.
 Provenance: Foundation §0.2
2.6 Results → Style (ResultsStyle)
Themes and styles output visuals (non-structural).
 Provenance: Foundation §0.2

3. Concern Interaction Rules (Interface)
Tabs (fixed order):
Build → BuildStyle → Logic (Workflow) → Knowledge → Results → ResultsStyle

Scope: Only Configuration own concern tabs.
 Projects and Collections aggregate Configurations but never have tabs.
Hover discovery: Build/Results tabs only (with keyboard/touch parity).
 Breadcrumbs: Tab → SubView; clicking tab name returns to default sub-view.
DnD Flow
Build declares anchors/zones.
Logic (Workflow) manages lifecycle and constraints.
Knowledge defines schemas + ARIA copy.
Style handles visuals (no layout changes).
Results shows committed order/history.

4. Acceptance Checklists
(same as before, still valid — omitted for brevity)

5. Versioning & Provenance
Treat §0 Universal Foundation as a charter; changes require version bump + summary note.
 Each concern entry includes:
/*
* Concern: Build
* Repo: Interface
* Spec: Interface Doc §2.1 (Foundation §§0.2–0.3)
*/

5.1 Export Mapping
Concern
Export Filename
Type
Build
{name}.build.tsx
Structure
BuildStyle
{name}.build.css
Styling
Logic (Workflow)
{name}.logic.ts
Behavior
Knowledge
{name}.knowledge.json or .ts
Lexicon / Schemas
Results
{name}.results.ts
Output
ResultsStyle
{name}.results.css
Output Styling



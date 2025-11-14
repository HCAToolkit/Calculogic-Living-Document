# Calculogic Living Document

The Calculogic Living Document captures the conceptual and technological direction for the product's front-end. It keeps the interface definition, behavioral rules, and knowledge models synchronized so every change is intentional and traceable.

## 0. Universal Foundation (Applies to every repo)

### 0.A Terminology Note – Metaphorical Layer Model
The concern terms **Build**, **Logic (Workflow)**, **Knowledge**, **Results**, and **Style** are metaphorical programming concerns rather than literal framework layers or file types. They describe responsibility, not implementation technology.

| Term | Metaphorical meaning | Typical manifestation |
| --- | --- | --- |
| Build | The structural definition – what exists and where it lives. | JSX/TSX markup, HTML structure, component skeletons |
| Build → Style (BuildStyle) | The visual styling of Build structures. | CSS, theme tokens, transitions scoped to Build |
| Logic (Workflow) | The behavior layer – how state changes and how users interact. | Hooks, stores, event handlers, routing, validation |
| Knowledge | The informational reference – what names, labels, or constraints mean. | Constants, schemas, localization, documentation maps |
| Results | The derivative or output – what the system produces or reports. | Rendered summaries, exports, analytics views |
| Results → Style (ResultsStyle) | Styling of outputs; non-structural visuals of results. | CSS, chart theming, transitions scoped to results |

**Purpose:** This model provides a semantic separation of responsibilities across any stack (React, PHP, Python, etc.). Repositories may use different file types (.tsx, .ts, .css, .json), but conceptual boundaries remain identical.

### 0.1 Canonical Flow
- `Build → Logic (Workflow) → Knowledge → Results`
- Style concerns are contextual:
  - `Build → Style (BuildStyle)`
  - `Results → Style (ResultsStyle)`

**Reasoning:** Creation precedes behavior; behavior is informed; outcomes are derived. Style modifies only the stage it belongs to.

### 0.2 Purity Per Layer
- **Build:** Structure only (containers, anchors, slots). No state or data fetching.
- **Logic (Workflow):** State, interaction, orchestration. No layout or copy.
- **Knowledge:** Lexicon, schemas, constraints, accessibility copy. No rendering or runtime state.
- **Results:** Derived output, analytics, exports. No mutation of source state.
- **Style:** Visual treatment and spacing; no structure or logic.

### 0.3 Directionality & Attachment
- Directional flow: `Build → BuildStyle → Logic (Workflow) → Knowledge → Results → ResultsStyle`.
- Knowledge informs all layers. There are no upward dependencies.
- Non-source layers attach to Build anchors; they never reshape or reorder the structure.
- Downstream layers may add presentation or data but cannot mutate structure (monotonic diffs).

### 0.4 Cross-Cutting Extensions
These extensions apply within the concerns described above.

| Extension | Build | Logic (Workflow) | Knowledge | Style | Results |
| --- | --- | --- | --- | --- | --- |
| Drag and Drop | Handles/zones declared | Lifecycle, undo, constraints | ARIA copy, schemas | Visual states | Finalized order |
| Accessibility | Anchors, ARIA roles | Focus, keyboard parity | Accessibility strings | Visual feedback | Read-only display |
| Routing | – | Logic only | – | – | – |
| Autosave / Sync | – | Orchestration | Copy text | Confirmation visuals | Status readouts |
| Error / Empty / Loading | – | Behavior | Copy text | Visual treatment | Output view |
| Theming | Tokens only | – | – | Style-scoped | – |
| Internationalization / Localization | – | – | All strings centralized | – | – |
| Performance | – | Transitions ≤ 200 ms | – | Transform/opacity only | – |
| Telemetry (read-only) | – | – | – | – | Metrics only |

### 0.5 Minimal Provenance
Every concern entry records:
- Title
- One-sentence description
- Provenance pointer to this section (e.g., Foundation §0.2)

**Terminology:** “Anchors” are stable `data-anchor="..."` selectors created by Build. Anchors must be unique, kebab-case, and remain stable across versions.

### 0.6 Spatial Synchronization (Engine Rule)
Within a Configuration, the sequence order is authoritative across all concerns. If an atomic or container appears in multiple tabs, its relative position must be identical everywhere. Downstream concerns (Logic, Knowledge, Style, Results) attach to Build anchors but cannot reorder or reshape them.

## 1. Repo Scope Declaration
- **Repository:** Interface
- **Scope:** All front-end and interaction logic for Calculogic. The interface is treated as its own concern layer within the larger Calculogic collection.
- **Inheritance:** Universal Foundation §0 applies without modification.

## 2. General Concerns (Interface Repository)
### 2.1 Build
Declares structure, anchors, and drop topology; no behavior. Provenance: Foundation §§0.2–0.3.

### 2.2 Build → Style (BuildStyle)
Provides visual styling for Build structures; purely non-structural. Provenance: Foundation §0.2.

### 2.3 Logic (Workflow)
Handles interaction and state orchestration (routing, validation, drag-and-drop). Consumes schemas from Knowledge and enforces constraints at runtime. Provenance: Foundation §§0.2, 0.4.

### 2.4 Knowledge
Holds the lexicon, constraint schemas, and accessibility/i18n strings. Defines terms; Logic consumes them. No rendering or runtime state. Provenance: Foundation §0.2.

### 2.5 Results
Renders derived outputs and exports; read-only with respect to state. May apply presentation transforms (grouping/sorting) but not computation. Provenance: Foundation §0.2.

### 2.6 Results → Style (ResultsStyle)
Themes and styles output visuals without altering structure. Provenance: Foundation §0.2.

## 3. Concern Interaction Rules (Interface)
- Tabs follow a fixed order: `Build → BuildStyle → Logic (Workflow) → Knowledge → Results → ResultsStyle`.
- Scope: Only Configuration owns concern tabs. Projects and Collections aggregate Configurations but never host tabs themselves.
- Hover discovery is limited to the Build and Results tabs (with keyboard and touch parity).
- Breadcrumbs follow `Tab → SubView`; clicking the tab name returns to the default sub-view.

### Drag-and-Drop Flow
1. Build declares anchors and zones.
2. Logic (Workflow) manages lifecycle and constraints.
3. Knowledge defines schemas and ARIA copy.
4. Style handles visuals without layout changes.
5. Results displays the committed order and history.

## 4. Acceptance Checklists
Acceptance checklists remain unchanged from prior revisions and are omitted here for brevity.

## 5. Versioning & Provenance
Treat §0 Universal Foundation as a charter. Changes require a version bump and summary note. Each concern entry includes:

```ts
/*
 * Concern: Build
 * Repo: Interface
 * Spec: Interface Doc §2.1 (Foundation §§0.2–0.3)
 */
```

### 5.1 Export Mapping
| Concern | Export filename | Type |
| --- | --- | --- |
| Build | `{name}.build.tsx` | Structure |
| BuildStyle | `{name}.build.css` | Styling |
| Logic (Workflow) | `{name}.logic.ts` | Behavior |
| Knowledge | `{name}.knowledge.json` or `.ts` | Lexicon / Schemas |
| Results | `{name}.results.ts` | Output |
| ResultsStyle | `{name}.results.css` | Output styling |

# Builder Documentation Modal

This guide formalizes the documentation modal as a first-class, cross-cutting concern inside the builder. The goal is to deliver a reusable documentation engine that every UI element can tap into from the beginning of development.

## 1. Treat documentation as a cross-cutting concern
Create a lightweight “Documentation” layer that any part of the interface can invoke.

### Per-tab responsibilities
- **Build**
  - Identify the structural element (tab, header, canvas, configuration list, etc.).
  - Declare where the documentation data lives (a key or ID inside Knowledge).
- **BuildStyle**
  - Position the ℹ️ icon.
  - Define tooltip placement rules.
  - Provide discovery affordances (hover highlight, focus states, etc.).
- **Logic (Workflow)**
  - `onHoverInfo` → show tooltip.
  - `onClickInfo` → open the modal with a `docId`.
  - `onCloseModal` → close the modal.
- **Knowledge**
  - Store the documentation content indexed by `docId`.
  - Provide short summaries for tooltips.
  - Supply extended content chunks for modal sections.
- **Results**
  - Prepare the resolved documentation payload for the given `docId` (e.g., `{ title, summary, sections[], links[] }`).
- **ResultsStyle**
  - Define the modal layout template: header, optional “Recommended workflows,” body sections, cross-links, close button, and scroll behavior.

**Interpretation:** Results deliver documentation data ready to render; ResultsStyle governs how the modal presents that data.

## 2. Implementation order (matching the left→down scan path)
### Step 1 – Core documentation engine and modal shell
- Build a global `DocumentationModal` component (visual polish can follow later).
- Introduce global state: `currentDocId | null`.
- Implement Logic behavior: open/close, escape key handling, backdrop clicks, etc.
- Wire the header/tab info icons first: Build, Logic, Knowledge, Results, BuildStyle, and ResultsStyle (include Publish if desired).

This establishes a functioning documentation output system using the Results / ResultsStyle metaphor before the main builder body is implemented.

### Step 2 – Left column skeleton
Work top-down on the left-hand side:
1. **Configurations section shell**
   - Reserve the area where the configurations list will live.
   - Add an ℹ️ icon linked to a new `docId`.
2. **Atomic components section shell**
   - Provide a header with name + ℹ️ and an empty body for now.
3. **Search section shell**
   - Stub the search bar area with its ℹ️ icon.

Each section requires:
- Info icon placement (BuildStyle).
- A documentation key (Knowledge).
- Click behavior opening the modal (Logic).

### Step 3 – Center canvas area
- Add a canvas header row (e.g., “Canvas”) with an ℹ️ icon.
- Leave the canvas body empty initially.
- Connect the documentation hook.

### Step 4 – Right inspector
- Add the inspector header (“Inspector” / “Details”) with an ℹ️ icon.
- Keep the body empty but wired to the documentation system.

Once these steps are complete, the entire frame—header, left column, canvas, and inspector—participates in the shared documentation output system even if deeper functionality is still pending.

## 3. Why putting documentation first helps
- Establishes a single reusable Results / ResultsStyle pattern for documentation.
- Every new structural element inherits:
  - A consistent info icon convention.
  - A straightforward way to attach a `docId`.
  - A predefined location for explanations to render.

## 4. Interface/Builder doc: skeleton and contracts
Keep the charter concise but explicit:
- **Documentation engine definition**
  - A global system that resolves `docId → content` (Knowledge/Results).
  - Renders the content in a standard layout (ResultsStyle).
  - Exposes a small Logic API: `openDoc(docId)` and `closeDoc()`.
- **Minimal IR for documentation entries**
  - `docId`
  - `concern` (Build, Logic, Header, Canvas, etc.)
  - `summary` (tooltip content)
  - `sections[]` (title + body)
  - `links[]` (cross-references)
- **Interaction contracts**
  - Any UI element can declare a `docId`.
  - Tooltips show the `summary`.
  - Clicking the element invokes `openDoc(docId)`.

These constraints keep the implementation aligned with the charter while enabling the initial header and layout shells to ship quickly.

## 5. Documentation engine as a dedicated living document
After the builder frame exists:
- Create a focused “Documentation Engine” living document (and eventually its own repo or package) that:
  - Models documentation as Calculogic data.
  - Defines one or more “Docs Projects.”
  - Represents doc pages/topics as Configurations.
  - Stores structured content in the Knowledge tab.
  - Specifies Results/ResultsStyle for different surfaces (modals, side panels, full-page help).
  - Declares export hooks for each UI type (web builder, VS Code extension, Electron desktop, etc.).

This approach dogfoods Calculogic itself: the documentation engine becomes a Calculogic-powered application that the builder consumes.

## 6. Practical sequence for today
Inside the Interface living document:
1. Add a short section titled “Documentation Engine: Skeleton & Contracts.”
2. Document the IR fields, `openDoc` / `closeDoc`, and tooltip rules.
3. Implement enough to support:
   - Header tab documentation
   - Header publish documentation
   - Left/center/right frame shells
4. Once the frame is real, spin up the dedicated Documentation Engine living document to:
   - Model documentation as Calculogic configurations.
   - Plan how documentation exports are loaded into the UI.

This sequencing keeps the current document focused while laying the groundwork for a fully dogfooded, stack-agnostic documentation workflow.

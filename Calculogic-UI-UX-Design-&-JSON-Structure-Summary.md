# UI/UX Design Principles and Implementation Details

## Visual builder and no-code interface
- Calculogic provides a visual builder for designing forms, quizzes, and logical systems without writing code.
- Users create Configurations (modular field groups or logic units) by dragging and dropping atomic components such as checkboxes, sliders, and text inputs.
- Containers and sub-containers group atomic components to express hierarchy and encapsulation.
- Structure, style, and logic are all defined through the interface; no direct HTML, CSS, or JavaScript is written.
- Behavior and conditions are configured with Mad-Lib-style editors that produce guided logic sentences.
- Every interaction maps to a shared JSON manifest, keeping the system approachable for beginners and transparent for advanced users.

## Declarative, dynamic, and hybrid approaches
1. **Declarative UI (pure Build)**
   - Everything is defined in the Build tab for static forms or surveys.
   - Every user experiences the same structure and order.
   - Example: A “Motivations” section with a label and multiple checkboxes placed manually in the builder.
2. **Dynamic UI (logic-driven)**
   - Build defines the base structure; the Logic (Workflow) tab governs runtime behavior.
   - Users define visibility rules, dynamic labels, and calculations through guided Mad-Lib logic sentences without writing JavaScript.
   - Example: A quiz reveals new questions or warnings based on selections.
3. **Hybrid (common case)**
   - Combines declarative Build foundations with Logic-driven behavior.
   - Provides clarity and flexibility while remaining fully no-code.

## Multi-tab interface — separation of concerns
The builder header displays six tabs that mirror Calculogic's universal concern flow.

| Tab | Purpose | Export |
| --- | --- | --- |
| Build | Defines structure, layout, anchors, and containers. | `build.tsx` |
| BuildStyle | Styles the Build stage (colors, spacing, typography). | `build.css` |
| Logic (Workflow) | Controls behavior, validation, and interactivity. | `logic.ts` |
| Knowledge | Houses reference data, lexicon, and constraints. | `knowledge.json` |
| Results | Renders outputs, exports, and analytics. | `results.tsx` |
| ResultsStyle | Styles the presentation of results. | `results.css` |

The six-tab layout enforces separation of concerns while a single JSON manifest integrates them.

## Layout and responsiveness
- Three-pane layout:
  - **Left pane:** Atomic components, templates, and search.
  - **Center canvas:** Primary workspace for assembling Configurations.
  - **Right pane:** Contextual settings and property inspectors for advanced options.
- Header includes the concern tabs, Publish button, and light/dark mode toggle.
- Responsive behavior:
  - Desktop: resizable panes that persist via local storage.
  - Mobile: panes collapse into icons or menus while remaining accessible.
- Accessibility: keyboard navigation, ARIA roles, focus outlines, and touch-friendly handles.
- Implementation uses React (TypeScript), modular CSS, and Zustand for UI state (pane sizes, tab selection, preview modes).

## User experience features
### Configuration cloning and templates
- Configurations can be cloned with one click.
- Clones receive new IDs and version links so edits never affect the original.
- Official templates stay read-only while user clones remain fully editable but retain lineage metadata for provenance.

### Documentation modal
- Each configuration includes an editable modal for name, description, usage notes, and screenshots.
- Documentation content clones alongside configurations to maintain clarity and collaboration.

### Version control and sharing
- Version history enables rollback and branching.
- Configurations support view/edit sharing permissions or public publication.
- Provenance integrity remains intact during collaboration.

## JSON structure and validation (interface perspective)
### Unified JSON manifest
- Every project and configuration is powered by a single JSON manifest that replaces individual code files.
- The manifest defines structure, behavior, style, knowledge, and results.
- Exporting a project generates:
  - `/project/build.tsx`
  - `/project/build.css`
  - `/project/logic.ts`
  - `/project/knowledge.json`
  - `/project/results.tsx`
  - `/project/results.css`

### JSON hierarchy
- **Configuration:** Root JSON entity (module scope).
- **Container:** Structural encapsulations inside a Configuration.
- **Sub-container:** Optional nested scopes.
- **Atomic component:** Smallest functional primitive.

```json
{
  "id": "coreMotivations",
  "type": "configuration",
  "build": { "containers": [ ... ] },
  "build_view": { "styles": [ ... ] },
  "logic": { "rules": [ ... ] },
  "knowledge": { "references": [ ... ] },
  "results": { "outputs": [ ... ] },
  "results_view": { "styles": [ ... ] }
}
```

During export, each section compiles to the concern file listed in the table above. Visual order in the builder determines compilation order.

### Field and naming conventions
| Element | Format | Example |
| --- | --- | --- |
| ID | camelCase | `empathySlider` |
| Anchor (`data-anchor`) | kebab-case | `core-motivations` |
| Origin templates | `atomic::` prefix | `atomic::slider` |
| Clone ID | `labelSlug + suffix` | `empathySlider_82h4` |
| Knowledge reference | Namespaced | `kb::global::traits::empathy` |

Reserved keys include `id`, `type`, `originId`, `props`, `visibility`, `version`, and timestamps. Custom metadata belongs in `meta.userNotes` or `props.custom`.

### Validation rules
- Schema validation (planned with Zod) enforces correctness.
- IDs must be unique within a configuration.
- Custom IDs cannot begin with the reserved `atomic::` prefix.
- Imports guard against collisions by auto-renaming or prompting the user.
- The UI validates IDs, traits, and knowledge links as edits occur.
- Each concern section passes its own type validation before export.

## Meta and provenance
- Every configuration stores `meta.title`, `meta.description`, and `meta.provenance` (e.g., “Interface Doc §2.1”).
- Exporters render these metadata entries as inline code comments, keeping documentation embedded in the output.

## Editing modes and JSON synchronization
Calculogic offers three synchronized editing modes that operate on the same manifest.

| Mode | Description | Target user |
| --- | --- | --- |
| Natural language (Mad-Lib) | Guided sentences such as “Show a slider labeled [ ] with range [ ]–[ ].” | Beginners |
| Code-like (guided logic) | Pseudo-code with placeholders (e.g., `if [x] > [y] then [show z]`). | Intermediate makers |
| Direct JSON | Structured editor for advanced manipulation of the manifest. | Advanced builders |

All modes stay synchronized; edits in one surface appear instantly in the others.

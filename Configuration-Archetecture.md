# Configuration Architecture (Programming Standpoint – Finalized)

## 1. Collection
A Collection is equivalent to an entire codebase or monorepo. It may contain multiple Projects, shared Knowledge layers, and global Atomic Components.

**Each Collection includes:**
- A Collection JSON manifest for dependency mapping, metadata, and build/export rules.
- Shared Knowledge (global datasets, constants, or cross-project definitions).
- Shared Atomic Components available to every Project.
- Build and export logic that compiles Projects and merges their outputs.

From a development perspective, the Collection behaves like a workspace root (for example, `/packages/*` or `/apps/*`), with each Project acting as a submodule inside the system.

## 2. Project
A Project is analogous to a package or application inside a codebase—a self-contained unit such as a form, quiz, tool, or single application view.

**Projects contain:**
- Multiple Configurations, each encapsulating a logical unit.
- Six conceptual concerns: Build, Build → Style (BuildStyle), Logic (Workflow), Knowledge, Results, and Results → Style (ResultsStyle).
- A Project JSON manifest defining configuration order, dependencies, and visibility.

### 2.1 Exported file structure
When exported, a Project compiles into four primary concern files and two contextual style files.

```
/project/
  build.tsx
  build.css        # BuildStyle
  logic.ts
  knowledge.json
  results.tsx
  results.css      # ResultsStyle
```

Each file aggregates the relevant concern sections from every Configuration in the Project. Visual order within a Configuration determines compilation order across concern files.

## 3. Tabs (concerns)
| Concern | Purpose | Compiled output | Notes |
| --- | --- | --- | --- |
| Build | Structure and layout of UI/UX elements (containers, sub-containers, primitives). | `build.tsx` | Declares anchors and topology. |
| Build → Style (BuildStyle) | Stylization of the declared structure—spacing, color, typography, motion discipline. | `build.css` | Non-structural; modifies appearance only. |
| Logic (Workflow) | Behavior, interaction, and state orchestration (routing, validation, drag-and-drop lifecycle). | `logic.ts` | Controls interactivity; no layout or copy. |
| Knowledge | Reference data, lexicon, schemas, constraints, and accessibility strings. | `knowledge.json` | Informational layer; no rendering or state. |
| Results | Derived output and presentation (summaries, exports, analytics). | `results.tsx` | Read-only; displays derived outcomes. |
| Results → Style (ResultsStyle) | Visual treatment of Results outputs (theming, highlighting, spacing). | `results.css` | Optional; applied to result views. |

This model preserves contextual purity—Style layers remain subordinate to their structural or output concerns.

## 4. Configuration
A Configuration encapsulates a specific feature, field group, or functional module.

**Characteristics:**
- Contains a JSON document with sections for all six concerns.
- Owns a unique anchor and ordering index for deterministic export.
- Hosts one or more Containers that group Atomic Components.
- Is encapsulated (self-contained logic and style), composable (can reference other Configurations), and portable (reusable across Projects or Collections).

During export, each Configuration contributes its concern segments to the corresponding Project files. Metadata is emitted as inline comments so generated code documents itself.

## 5. Configuration JSON
A Configuration JSON acts as a declarative blueprint describing how contents map to concern files.

```json
{
  "id": "core-motivations",
  "build": { "containers": [ ... ] },
  "build_style": { "styles": [ ... ] },
  "logic": { "rules": [ ... ] },
  "knowledge": { "references": [ ... ] },
  "results": { "outputs": [ ... ] },
  "results_style": { "styles": [ ... ] }
}
```

### Export process
| Step | Concern | Action | Output |
| --- | --- | --- | --- |
| 1 | Build | Append structure for all Configurations. | `build.tsx` |
| 2 | Build → Style | Merge spacing and color rules. | `build.css` |
| 3 | Logic (Workflow) | Compile and append rule sets. | `logic.ts` |
| 4 | Knowledge | Merge data and reference objects. | `knowledge.json` |
| 5 | Results | Assemble render logic and displays. | `results.tsx` |
| 6 | Results → Style | Merge styling of output visuals. | `results.css` |

## 6. Containers
Containers group Atomic Components within a Configuration, defining structure rather than filesystem folders.

```html
<section data-anchor="core-motivations">
  <ButtonGroup />
  <Placeholder />
</section>
```

Containers appear as encapsulated code blocks inside concern files during export.

## 7. Atomic Components
Atomic Components are the smallest functional primitives—the vocabulary of the system (for example, `TextInput`, `Checkbox`, `Slider`, `IfRule`, `SumRule`). They are stateless, reusable, and referenced declaratively by Containers and Configurations.

## 8. Cloning and editing
- Cloning a Configuration or Atomic Component creates a new JSON instance.
- Edits modify the live JSON directly; exports always match the builder state.
- Clones never overwrite originals, reinforcing immutability.

## 9. Compilation model
When exporting a Project:
1. Parse Configurations in declared order.
2. For each concern tab, append to the matching file:
   - Build → `build.tsx`
   - Build → Style → `build.css`
   - Logic (Workflow) → `logic.ts`
   - Knowledge → `knowledge.json`
   - Results → `results.tsx`
   - Results → Style → `results.css`
3. Flatten Containers into encapsulated code blocks.
4. Merge by anchor and index for deterministic export.
5. Emit metadata as inline comments.

## 10. Summary
| Concept | Equivalent | Exports as | Behavior |
| --- | --- | --- | --- |
| Collection | Workspace root / monorepo | Aggregated project outputs | Houses shared assets and build rules. |
| Project | Package or app | Concern files listed above | Self-contained feature set. |
| Configuration | Feature module | Segments within concern files | Encapsulated logic, style, knowledge. |
| Container | Structural grouping | JSX/TSX sections | Defines hierarchy without altering order. |
| Atomic Component | Primitive unit | Reusable declarations | Stateless units referenced across concerns. |

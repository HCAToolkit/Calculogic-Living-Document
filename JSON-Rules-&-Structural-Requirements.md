# JSON Rules & Structural Requirements
This document defines how Calculogic's JSON structure behaves.
It enforces compatibility, modular safety, and clean separation across Atomic Components, Configurations, and Knowledge-linked logic.
🟨 Status: Living draft. This will evolve as features expand and will later be encoded in Zod-based validation schemas.

## 1. 🧱 Atomic Component Design
**Definition:**
Atomic Components are the smallest reusable building blocks.
They contain no domain-specific meaning — only structure and behavior potential.
**Required fields:**
id, type, props, defaults

**Naming:**
Original templates must start with the prefix:
```json
"id": "atomic::<componentName>"
**Example:** "id": "atomic::slider"
**Rules:**
Must be generic and reusable (e.g., a slider with min/max, but not what it measures).
Must not include domain terms (e.g., "trait": "empathy").
Must be defined once and cloned into Configurations.
**Example:**
{
"id": "atomic::slider",
"type": "slider",
"props": { "min": 1, "max": 10 },
"defaults": { "label": "New Slider", "step": 1 }
}
```


## 2. 🧬 Cloning Behavior & ID Conflict Prevention
When a user drags an atomic component into a Configuration, it is cloned.
Clone requirements:
Must generate a new unique id
Must retain originId (points to the source atomic template)
Must never reuse the "atomic::" prefix
**Suggested clone ID pattern:**
```
<labelSlug>_<randomSuffix>
```

**Example:** "id": "empathySlider_82h4"
**Validation:**
No duplicate ids within a configuration.
Reserved words (like score, result, trait) cannot appear unless namespaced.
**Example:**
```json
{
"id": "empathySlider_82h4",
"originId": "atomic::slider",
"type": "slider"
}
```


## 3. 📤 Configuration Sharing & Import Rules
In Calculogic, a Configuration is itself a top-level Container.
The difference is semantic: a Configuration is a named, shareable container that represents a user-created field blueprint across concerns. Sub-containers are internal groupings within it. Structurally, they follow the same rules.
When importing, duplicating, or sharing Configuration:
The system must detect ID conflicts.
Conflict resolution offers:
Prompt user to rename
Auto-rename (add suffix)
Block import if unresolved
This prevents namespace collisions across Projects and Collections.

## 4. 📚 Knowledge Referencing
Knowledge objects define reusable information across forms, quizzes, or projects.
**Naming format:** 
- `kb::<scope>::<type>::<key>`

Examples:
- `kb::global::traits::empathy`
- `kb::user_123::archetypes::trickster`
- `kb::form_98::narrative::climaxTypes`

**Optional version locking:**
```json
{
"reference": "kb::global::traits::empathy",
"lockedVersion": "v1.0"
}
```

Knowledge references must match an existing entry in the Knowledge tab JSON (knowledge.json).

## 5. 🛡 Reserved System Fields
These keys are system-protected and must not be overridden.
**Reserved keys:**
id, label, type, originId, props, visibility,
version, createdAt, updatedAt

**User-defined metadata must be nested under:**
props.custom
meta.userNotes


## 6. 🔗 Interconnectedness (Future Consideration)
Eventually, Configuration and Knowledge objects may link across Projects or Collections.
**Example declaration:**
```json
"dependencies": [
{
"ref": "configuration::user_542::shared_slider_pack",
"usage": "read",
"onConflict": "prompt"
}
]
```

Enables:
Reuse of official/user-created Configuration
Dynamic Knowledge syncing
Shared traits or results across templates
Future validation needs:
Cross-link checks
Scoped permissions
Circular dependency detection
Graceful fallback for broken references

## 7. 📂 Tab-Specific Structural Rules
Tabs correspond to concern layers (Build, BuildStyle, Logic (Workflow), Knowledge, Results, ResultsStyle).:
Each tab references the same container/component IDs; they attach scoped data rather than duplicate structure.

### Build
Defines the structural hierarchy:
Containers, subcontainers, and components
Each component must have:
A unique id (per configuration)
A valid atomic type
Components cannot nest directly inside other components.
Containers must wrap all visible elements.
Build declares anchors (data-anchor) for cross-layer references.

### BuildStyle
Defines the visual styling of Build elements:
Styling must use approved atomic CSS props (e.g., margin, color, font-size).
Each rule must declare a scope:

```json
"scope": "build" | "results" | "both"
```

Must not introduce new layout regions or inject behavior.

### Logic (Workflow)
Defines behavioral rules and interaction orchestration.
Supports:
Conditionals
Mathematical operations (sum, avg, min, max)
Variable assignments and validations
Every reference (e.g., slider1.value) must correspond to an existing Build element.
Logic (Workflow) may read and compute, but cannot create or mutate structure.
Logic (Workflow) outputs may be consumed by Results only.

### Knowledge
Holds reusable data, lexicons, schemas, and constraint definitions.
All external references must follow:

- `kb::<scope>::<type>::<key>`


Traits or keys used in Logic (Workflow) or Results must exist here.
Optional lockedVersion for compatibility snapshots.

### Results
Defines derived outputs (summaries, exports, analytics).
May use:
Template strings (${empathyScore})
Conditional outputs (if score > 8 → "High Empathy")
Category mappings or thresholds
May read from Logic (Workflow) but not mutate Build state.
Results are read-only reflections of behavior.

### ResultsStyle
Defines styling of outputs:
Can highlight values, colorize categories, or theme charts.
Must not add new regions or introduce new interactivity.


## 8. 🏷 Naming & Consistency Rules
Ensures clean structure and safe reuse across Configurations.
Element Type
Rule
id
camelCase or labelSlug_suffix
originId
must start with atomic::
knowledge keys
- `kb::<scope>::<type>::<key>`
CSS className
kebab-case or BEM
variables
camelCase
templates
must not contain raw JS

Reserved Words (forbid unless namespaced):
score, result, trait, type, id, form, configuration, submit, calculate, data, output
Validation actions:
Warn if non-unique IDs
Error if invalid patterns
Auto-suggest corrections (e.g., empathy-slider → empathySlider)

## 9. 🧪 Validation Behavior & Enforcement Modes
Supports multiple modes for flexible authoring, import, and publishing.
Mode
When Used
Behavior
Strict
Publishing, official templates
Hard errors on all violations
Lenient
Active building
Warnings only; allows placeholders
Import
Importing external JSON
Prompts or auto-renames conflicts
Reference Check
On Save/Export
Ensures all references resolve
Dry-Run (Future)
Preview Mode
Simulates runtime behavior and warns


Mode
Trigger
Actor
Override Permission
Strict
Publish / Clone Official
System
Admin confirmation
Lenient
Active Editing
User
Auto (temporary)
Import
JSON Import
System + User
User choice
Reference Check
Save / Export
System
None
Dry-Run
Preview
User
N/A


Error Categories
Type
Description
Example
Structural
Improper hierarchy
Component outside container
Reference
Missing target
Logic (Workflow) references slider2 that doesn’t exist
Conflict
Duplicate or reserved name
Two components with id: "score"
Invalid Format
Naming or syntax error
user-name instead of userName
Unsafe Link
Broken dependency
Missing kb::form_12::trait::empathy


Future Enhancements
Auto-correction options:
```json
"Fix all conflicts" → auto-suffix duplicates
"Convert invalid names" → suggest corrected IDs
Custom overrides:
```

```json
"validationOverrides": { "allowDuplicates": true }
```


Schema version lock:

```json
"$schemaVersion": "1.0"
```



✅ Meta and Provenance Enforcement
Every Configuration and component-level section must include:
```json
"meta": {
"title": "Empathy Slider",
"description": "Primary empathy measurement control.",
"provenance": "Interface Doc §2.1 (Foundation §§0.2–0.3)"
}
```

At export, these become inline code comments in each concern file.
Mode Context:
Validation modes trigger automatically based on user action.
Lenient Mode — active during live building; allows incomplete or placeholder content but flags issues inline.


Strict Mode — runs during Project or Configuration publishing, validating all references before allowing “Public” or “Official” visibility.


Import Mode — activates when importing external JSON, prompting the user to resolve naming conflicts.


Reference Check Mode — runs when saving, ensuring all linked components and traits exist.


Dry-Run Mode (Future) — executes a simulated render before export to catch runtime errors.


Publishing Integration:
Each Configuration includes a toggle determining whether it can be published or cloned outside the current Project.
If toggled on, its visibility and validation state can optionally sync with Project-level publishing (Public, Private, or Draft) during CRUD operations.


# Tab Guidance and Contextual Documentation
Calculogic embeds contextual documentation directly into its interface and JSON structure, ensuring users always have access to guidance without pop-ups or forced onboarding.
 Every interactive element—tabs, atomic components, Configurations, containers—includes layered help drawn from its embedded documentation fields (atomicDoc, configDoc, meta.provenance).

1. Dual-Layer Help System
Interaction
Purpose
Behavior
Hover
Quick clarity
Displays short tooltips generated from meta.summary or atomicDoc fields.
Click
Deep understanding
Opens a modal with full configDoc, example snippets, and cross-linked references.

This two-tier model gives instant access to context without disrupting workflow.

2. Tab-Specific Help Icons
Each of the six concern tabs in the header includes a small ℹ️ icon in the top-right corner.
Hover: Shows a one-line summary from each tab's JSON descriptor.
 Example hover tooltip:


 [Build ℹ️]
 "Assemble layouts with containers, sub-containers, and atomic components. Use official or custom configs."
Click: Opens a documentation modal showing:
Full concern description
Recommended workflows
Core definitions and examples
Cross-links to related tabs and Configurations
This system keeps learning optional but always visible.

3. Fixed Builder Guidance
Instead of onboarding pop-ups, the Builder Canvas has a persistent guidance area in its upper-right corner.
When empty:
🎉 Welcome to Calculogic!

Drag official Configurations to start fast.
Or build your own using:
→ Containers (outer structure)
→ Sub-containers (group fields)
→ Atomic components (inputs, visuals)

ℹ️ Hover icons for tips, or click for full documentation.

Always visible, never modal.
Automatically updates as users change tabs or select items.
Populated by the current tab's metadata and configDoc field.

4. Component & Configuration Hints
Every item in the left pane (atoms or Configurations) includes a right-aligned ℹ️ icon.
Element
Hover Tooltip
Modal Contents
Atomic Component (e.g., Conditional Block)
“Add rules like: If [score] > 80, show [message]. Best for quizzes.”
Technical spec (atomicDoc), JSON example, editable defaults.
Configuration (e.g., Contact Form)
“Pre-built name/email fields. Clone to customize.”
Narrative documentation (configDoc), usage, editable parameters.

All doc data originates from the JSON, ensuring synchronization between UI help and source metadata.

5. Tab Documentation Reference
Tab
Purpose
Hover Summary
Build
Structural definition (containers, components)
“Assemble structure with containers and atoms; define anchors.”
Build View
Visual styling for Build
“Style layouts with colors, spacing, and themes.”
Logic (Workflow)
State, validation, and interaction rules
“Add calculations and conditions; start with If or Sum blocks.”
Knowledge
Shared lexicon, schemas, and references
“Store reusable data such as traits or constants.”
Results
Derived output rendering
“Design output summaries and score mappings.”
Results View
Styling for Results displays
“Style result cards or analytics without adding new logic.”

Each tab's help data is sourced from its meta.provenance entry in the repository manifest.

6. Build Tab Documentation Modal
Clicking Build ℹ️ opens a modal drawn from the tab's documentation file (build_doc.json):
Sections:
Official Configuration:
 Pre-approved, system-provided field groups.
Custom Configuration:
 Built from user combinations of:
Containers → structural frames
Sub-containers → grouped logic sections
Atomic Components → individual UI/logic primitives
Tips: Start from official configs for speed; build custom for full flexibility.
[ See examples → ]

7. UI Placement Consistency
Element
Placement
Tab ℹ️
Top-right of each tab header
Builder ℹ️
Fixed in canvas upper-right corner
Component ℹ️
Right edge of each item in left pane
Config ℹ️
Inside configuration inspector panel

Placement never shifts between devices or modes.

8. Typical User Flow
User opens Build tab → sees the static guidance panel.
Hovers ℹ️ → tooltip appears ("Build structure with containers and atoms").
Clicks ℹ️ → modal opens with full documentation and examples.
Hovers a component ("Text Input") → quick description.
Clicks component ℹ️ → modal shows field docs and editable JSON snippet.
Builds freely without ever losing context.

9. Documentation & Propagation System
Principle: Documentation mirrors structure — a living layer that updates automatically through cloning, inheritance, and tagging.
Implementation Fields
Level
Field
Description
Atomic Components
atomicDoc (immutable)
Authoritative technical definition.
Configurations
configDoc (mutable)
Narrative description with examples and placeholders.
Documentation Variables
documentationVariables
Key-value tags for placeholders inside configDoc.

Built-in Auto-Tags
Tag
Behavior
{{name}}
Injects configuration name
{{id}}
Inserts unique identifier
{{#atomicComponents}}
Lists linked atoms
{{#configurations}}
Lists linked child configs

Cloning Behavior
On cloning, the new Configuration inherits configDoc and documentationVariables.
User renames it → new values auto-propagate through all tags.
Ensures cloned documentation remains coherent and traceable.
User-Defined Propagation (Deferred)
Future option to define custom {{tags}} within user docs.
 Not active yet—held for post-launch validation.
Inheritance rule: Updates to an atomic component’s documentation do not automatically overwrite cloned configurations. A future “Sync Documentation” option may reconcile them on demand.
10. Hover and Automation Enhancements
Hover system integrations:
Pulls from meta.summary or atomicDoc.
Supports markdown formatting and short code samples.
Uses calculated positioning (above or beside component) to avoid overlap.
Automation principle:
Automate mappings; allow manual override; show status clearly.
Visual Indicators
Icon
Meaning
🔗
Auto-linked / auto-documented
⛓️
Manually linked or edited
⚠️
Doc conflict (outdated or unresolved merge)


11. Benefits
Non-intrusive learning — no pop-ups or walkthroughs.
Predictable placement — help icons always in same positions.
Consistent depth — hover for summary, click for full doc.
Unified propagation — JSON and UI documentation stay synchronized.
Transparent inheritance — cloned templates auto-update their documentation via tag substitution.



# UI, UX, and Code Architecture Principles
🔹 One Source of Truth: JSON
Calculogic is driven by a single structured JSON manifest. It defines:
Atomic components (smallest primitives: sliders, checkboxes, text inputs, display blocks).
Configuration (modular "field blueprints" composed of atoms and containers).
Concern data for all six tabs: Build, BuildStyle, Logic (Workflow), Knowledge, Results, ResultsStyle.
Documentation & meta (title, description, provenance), which are exported as inline comments in code.
All UI actions (drag, type, choose options) read/write the same JSON.
 Visual order inside each Configuration = compilation order across all concern files.

🔹 Configuration vs. Forms (Projects)
A Configuration is not an entire form.
A Project (a form/quiz/app view) is a collection of Configuration.
Key idea: Configurations are like field types—but user-creatable.
Users compose new "fields" from atomic components (atoms) + containers/sub-containers.
These become reusable Configurations (shareable, clonable, nominatable).
Bridge across concerns:
 A Configuration supplies entries to Build (structure) and BuildStyle (styling), Logic (Workflow behavior), Knowledge (lexicon/schemas), Results (derived output), ResultsStyle (results styling).
 Only Configuration own concern sections; containers/components do not own tabs (they contribute to their parent Configuration's sections).

🔹 Atomic Components (Atoms)
Atoms are the smallest primitives:
Examples: Checkbox, Slider, TextInput, Button, DisplayBlock.
Atoms are generic (no domain meaning), cloned from atomic:: templates (e.g., atomic::slider) and placed inside a Configuration (root container or sub-containers).
They map to UI primitives and simple logic operands, but tab concerns live at the Configuration level.

🔹 Multiple Editing Views — All Synced to JSON
Three synchronized views, one manifest:
Natural Language (Mad-Lib) View
 Human-readable sentences with blanks/dropdowns for any customizable part (labels, ranges, operators, conditions, colors).
 Every blank maps 1:1 to a JSON field.
Code-Style (Guided) View
 Code-looking syntax with placeholders (operators/values/variables chosen from menus).
 Teaches programming concepts while preserving schema safety.
JSON View (Code Mode)
 Optional, for advanced users. Read-only by default; can allow editing if enabled.
 Supports export/import. Any change here is reflected in the other two views.
The three views are just lenses over the same JSON; switching views never diverges the data.
Example (logic block JSON):
{
"type": "logicBlock",
"condition": "sortedCategoryRatings.length > 0",
"action": "console.log(`Sorted ratings for category ${categoryNames[category]}: `, sortedCategoryRatings)"
}


🔹 Templates, Cloning, Favorites, Nominations
All atomic components and Configurations exist as templates.
Official templates: admin/approved; read-only to general users.
User templates: user-made; private, shared, or nominated to become official.
Templates are never edited directly by others. Users clone, then customize.
Documentation (description, screenshots, usage) clones with the configuration.
Favorites: personal quick-access list.
Nominations: signal a template for potential official status (admins decide).

🔹 Device-Responsive UX
Desktop: drag-and-drop canvas; modals/side panels for editing; live preview.
Tablet: drag-and-drop and tap-to-add supported.
Mobile: tap-to-add (no drag-and-drop); reorder via arrows/gestures.
 Canvas auto-adjusts for small screens; all controls remain accessible.
Accessibility: keyboard focus, ARIA roles/labels, touch targets, dark mode.
 State (pane sizes, collapsed sections) persists per user.

🔹 Educational Bridge
Design principle: show how no-code maps to code.
Natural language ⇄ Code-style ⇄ JSON.
Users learn how conditions/operators/data structures translate to code.
Enables smooth progression from no-code → low-code → full-code.

🔹 Power User Features
Export a Project as concern files compiled from JSON:

 /project/
build.tsx        // Build
build.css        // BuildStyle
logic.ts         // Logic (Workflow)
knowledge.json   // Knowledge
results.tsx      // Results
results.css      // ResultsStyle
Includes inline meta/provenance comments and preserves visual order.
Import JSON (code-first workflows supported).
Automate generation of complex Configurations outside the UI, then import.

✅ In Short
Everything is JSON. One manifest drives all six concerns.
Configurations are user-created field blueprints made from atoms + containers; they feed all concern files.
Only Configuration own tabs; containers/components contribute to their parent's concern sections.
Visual order = file order, and meta → code comments at export.
Three synchronized editors (Natural, Code-style, JSON) keep beginners, intermediates, and power users perfectly in step.


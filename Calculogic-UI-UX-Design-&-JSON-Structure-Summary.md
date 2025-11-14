# Calculogic UI/UX Design & JSON Structure Summary
UI/UX Design Principles and Implementation Details
Visual Builder & No-Code Interface
Calculogic provides a visual builder that lets users design forms, quizzes, and logical systems without writing code.
 Users create Configuration (modular field groups or logic units) through drag-and-drop placement of atomic components—checkboxes, sliders, text inputs—then group them into Containers and Sub-containers.
All structure, style, and logic are defined through the interface; no manual HTML, CSS, or JavaScript is written.
 Instead, users configure behavior and conditions through Mad-Lib-style editors (fill-in-the-blank logic sentences).
All visual interactions map directly to a shared JSON manifest, making Calculogic fully no-code for beginners while remaining transparent for advanced users.

Declarative vs. Dynamic UI Approaches
1. Declarative UI (Pure Build)
Everything is pre-defined in the Build tab.
 Users lay out all components and containers explicitly—ideal for static forms or surveys.
 Every user sees the same structure and order.
 Example: A "Motivations" section with a label and multiple checkboxes is placed manually in the builder.
2. Dynamic UI (Logic-Driven)
The UI structure still originates in the Build tab, but runtime behavior is governed by the Logic (Workflow) tab.
 Users define visibility rules, dynamic labels, and calculations through guided Mad-Lib logic sentences—no JavaScript required.
 Example: A quiz reveals new questions or warnings based on user selections.
3. Hybrid (Common Case)
Most projects combine both: a declarative Build foundation with dynamic behavior layered via Logic (Workflow).
 This hybrid ensures clarity and flexibility without requiring code.

Multi-Tab Interface — Separation of Concerns
The header of the builder displays six tabs, mirroring Calculogic's universal concern flow:
Tab
Purpose
Output
Build
Defines structure, layout, anchors, containers
build.tsx
BuildStyle
Styles the Build stage (colors, spacing, typography)
build.css
Logic (Workflow)
Controls behavior, validation, interactivity
logic.ts
Knowledge
Houses reference data, lexicon, and constraints
knowledge.json
Results
Renders outputs, exports, analytics
results.tsx
ResultsStyle
Styles results presentation
results.css

This six-tab layout enforces separation of concerns—each tab governs one responsibility.
 The system integrates all behind the scenes through a single JSON manifest.

UI Layout and Responsiveness
The Calculogic builder uses a three-pane layout for maximum clarity:
Left Pane: Lists atomic components, templates, and search.
Center Canvas: The main workspace for assembling Configuration.
Right Pane: Contextual settings and property inspectors (reserved for advanced options).
Header: Six tabs, Publish button, and light/dark mode toggle.
The layout is fully responsive:
On desktop: panes are resizable and persist via local storage.
On mobile: panes collapse into icons or menus.
Accessibility features: keyboard navigation, ARIA roles, focus outlines, and touch-friendly handles.
Built with React (TypeScript) and modular CSS. Zustand manages UI state (pane sizes, tab selection, preview modes).

User Experience Features
Configuration Cloning & Templates
Users can clone Configurations with one click.
 Clones are independent (new IDs, version links) and editable without affecting originals.
 Official templates remain read-only; user clones preserve sandbox freedom.
 This balances stability (official) and flexibility (user experimentation).
Official templates are read-only globally. When cloned, the new instance is fully editable, but its lineage metadata retains a reference to the original official template.
Documentation Modal
Each configuration includes an editable modal for name, description, usage, and screenshots.
 Documentation clones automatically with the configuration to promote clarity and collaboration.
Version Control & Sharing
Version history allows rollback and branching.
 Configuration can be shared with view/edit permissions or published publicly.
 This enables collaborative workflows while maintaining provenance integrity.

JSON Structure and Validation (Interface Perspective)
Unified JSON Manifest — Single Source of Truth
Every project and configuration is powered by one JSON manifest, the single source of truth.
 It defines structure, behavior, style, knowledge, and results—essentially replacing code files.
When the project exports:
/project/
build.tsx
build.css
logic.ts
knowledge.json
results.tsx
results.css

All six concern files are compiled from the same JSON manifest.

JSON Hierarchy
Configuration – the root JSON entity (module scope).
Container(s) – structural encapsulations inside a Configuration.
Sub-containers – optional nested scopes.
Components (Atoms) – smallest functional primitives.
Example:
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

During export, each section compiles to its respective concern file.
 Visual order in the builder determines compilation order.

Field and Naming Conventions
Element
Format
Example
ID
camelCase
empathySlider
Anchor (data-anchor)
kebab-case
core-motivations
Origin Templates
prefix atomic::
atomic::slider
Clone ID
labelSlug + suffix
empathySlider_82h4
Knowledge Ref
namespaced
kb::global::traits::empathy

Reserved keys: id, type, originId, props, visibility, version, timestamps.
 User metadata belongs in meta.userNotes or props.custom.

Validation Rules
Handled by schema validation (Zod planned):
Uniqueness: Every ID unique within a configuration.
Reserved Prefixes: No custom IDs starting with atomic::.
Import Rules: Prevent ID collisions by auto-renaming or prompting.
Runtime Checks: UI validates IDs, traits, and knowledge links live as users edit.
Schema Compliance: Each concern section validated independently for type correctness.

Meta and Provenance
Each configuration stores:
meta.title
meta.description
meta.provenance (e.g., "Interface Doc §2.1")
At export, these are rendered as inline comments in code for documentation.

Editing Modes and JSON Synchronization
Calculogic supports three synchronized views—each editing the same manifest:
Mode
Description
Target User
Natural Language (Mad-Lib)
Simple sentences (“Show a slider labeled [ ] with range [ ]–[ ]”)
Beginners
Code-like (Guided Logic)
Pseudo-code with placeholders (if [x] > [y] then [show z])
Intermediate
JSON Mode
Direct JSON editor with syntax validation
Advanced

Changes in any mode instantly update all others.
 This ensures one live JSON model—no divergence, no shadow copies.

Export & Compilation Flow
Step
Concern
Output File
Description
1
Build
build.tsx
Structural markup
2
BuildStyle
build.css
Visual styling
3
Logic (Workflow)
logic.ts
State/behavior logic
4
Knowledge
knowledge.json
Data and lexicon
5
Results
results.tsx
Output and summaries
6
ResultsStyle
results.css
Visual styling for outputs

Exports are deterministic, ordered, and annotated with provenance comments.
 Meta fields, anchor order, and concern hierarchy govern file output.

Summary Alignment Table
Concept
Engine Equivalent
Behavior
Collection
Monorepo root
Groups multiple Projects and shared resources
Project
App/package
Compiles to concern files + styles
Configuration
Feature module
Encapsulates concern data in JSON
Container
JSX scope
Groups atomic components
Component / Atom
Primitive function
Smallest reusable UI unit
Build → Logic (Workflow) → Knowledge → Results (+ contextual views)
Concern flow
Enforces separation and order

Core Principles
Configuration encapsulate primitives — not folders.
Containers define runtime scope — not directory structure.
Projects compile by concern — one file per concern.
Collections aggregate Projects — forming modular ecosystems.
Visual order determines deterministic export order.
Meta and provenance are embedded for traceability.



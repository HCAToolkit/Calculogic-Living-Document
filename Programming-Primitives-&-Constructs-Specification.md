# Programming Primitives & Constructs Specification
0. Purpose
Define what Atoms, Containers, and Subcontainers are in Calculogic as first-class Programming Primitives (atoms) and Programming Constructs (containers/scopes), and how they map across concerns, IDs, scope, validation, and export.

1. Terms & Canonical Mapping
Atoms (Programming Primitives)
Indivisible units: inputs, displays, operators, style declarations, data bindings.
Containers (Programming Constructs)
Encapsulate atoms and subcontainers; establish scope, ordering, and inheritance. Cannot be nested in other Containers.
Subcontainers (Nested Constructs / Inner Scopes)
Nested blocks under a container, inheriting parent context with local overrides. Can be nested in other Subcontainers.
Configuration (Module)
Cross-concern module (Build, BuildStyle, Logic, Knowledge, Results, ResultsStyle).
Project (Program); Collection (Package/Ecosystem).
Concept Table
Programming Concept
Calculogic Entity
Primitive
Atom
Construct / Block
Container
Inner Scope
Subcontainer
Module
Configuration
Program
Project
Package / Workspace
Collection


2. Invariants (Non-negotiable)
SSOT IR: One IR (Intermediate Representation) is the source of truth for all concerns.


Deterministic Order: Visual order in Build is the canonical sequence across all concerns and exports.


Separation of Concerns: Build defines what exists; View defines how it looks; Logic defines how it behaves; Knowledge defines what it knows; Results defines what it produces; ResultsStyle defines how outputs look. Cross-concern links synchronize, but concerns never redefine structure.


Stable IDs: Every entity has a unique, namespaced ID; collisions are blocked or auto-renamed with audit.


Scope Safety: Containers/Subcontainers are lexical scopes. Entities can reference upscope by ID; sibling mutation is disallowed (communicate via bindings/events/state).


A11y Baseline: Roles/labels, focus order, contrast, keyboard parity, prefers-reduced-motion honored.


Security: Results auto-escaped; HTML/CSS sanitized; Logic runs in an op-allowlist sandbox; CSP + SRI required.



3. IR Envelope (per Configuration)
{
  "irVersion": "1.2.0",
  "schema": { "id": "cl.ir", "ver": "1.2.0" },
  "compat": { "idSchema": "1.x", "logicAPI": ">=2.0 <3.0" },
  "migration": {
    "strategy": "auto | semi-auto | manual",
    "paths": { "1.1.0": "auto", "1.0.0": "migrations/1.0.0->1.1.0.js" }
  }
}

4. IDs, Namespacing, Collisions
{
  "idRules": {
    "atom":  "atom::{type}::{slug}",
    "node":  "node::{8charHash}",
    "ctr":   "ctr::{slug}",
    "sub":   "sub::{slug}::{8charHash}",
    "kb":    "kb::{scope}::{type}::{key}",
    "reservedPrefixes": ["sys::","engine::","plugin::"]
  },
  "idPolicy": {
    "onImportCollision": "autoRename+audit",
    "onManualCollision": "block+explain"
  }
}

5. Entity Definitions (Normative)
5.1 Atom (Primitive)
Intent: Indivisible, declarative unit specific to a concern family.
Examples: ui.slider, ui.input, logic.compare, style.rule, result.block, kb.ref.
IR Shape (instance)
{
  "id": "atom::ui.slider::empathy",
  "kind": "ui",
  "type": "ui.slider",
  "props": {
    "label": "Empathy",
    "min": 0,
    "max": 10,
    "step": 1,
    "valueBinding": "state.empathy"
  },
  "a11y": { "role": "slider", "ariaLabel": "Empathy" },
  "meta": { "originId": "template::ui.slider@1.3.0", "desc": "User empathy rating" }
}
Rules
Atoms cannot create or reorder structure; they live inside a container/subcontainer.


Logic atoms manipulate state/flow only; no DOM/style creation.


Style atoms cannot introduce elements; they target existing anchors/selectors.


Results atoms render outputs from declared inputs/state; no side-effects.



5.2 Container (Construct)
Intent: Structural and semantic scope; owns order and child references.
IR Shape
{
  "id": "ctr::coreMotivations",
  "type": "container",
  "children": ["sub::intro::a1b2c3", "atom::ui.slider::empathy"],
  "bindings": {
    "state": { "segment": "derived.segment" }
  },
  "meta": { "anchor": "core-motivations", "desc": "Primary section" }
}
Rules
Defines lexical scope for child atoms/subcontainers.


Provides bindings (state, kb, params) available to descendants.


May be repeatable (iterables) or conditional (renders via Logic).



5.3 Subcontainer (Nested Construct)
Intent: Inner scope with local overrides; inherits parent bindings.
IR Shape
{
  "id": "sub::intro::a1b2c3",
  "type": "subcontainer",
  "children": ["atom::ui.text::introCopy"],
  "overrides": { "layout": { "gap": 8 } },
  "meta": { "anchor": "intro" }
}
Rules
Inherits parent bindings; can override or extend.


Cannot escape or reorder outside its parent container.



6. Cross-Concern Organization (per Configuration)
{
  "build": {
    "root": "ctr::coreMotivations",
    "nodes": ["ctr::coreMotivations","sub::intro::a1b2c3","atom::ui.slider::empathy"]
  },
  "build_view": {
    "styles": [
      { "select": "[data-anchor='core-motivations']", "props": { "padding": "16px" } }
    ]
  },
  "logic": {
    "rules": [
      {
        "id": "rule::presence::empathyRequired",
        "if": { "exists": "atom::ui.slider::empathy" },
        "then": [{ "validate": { "target": "atom::ui.slider::empathy", "type": "required" } }],
        "origin": "auto"  // link from build.required
      }
    ]
  },
  "knowledge": {
    "refs": [{ "id": "kb::global::traits::empathy", "type": "number", "ver": "2.3.0" }]
  },
  "results": {
    "blocks": [{ "id": "result::summary", "from": "state.empathy", "render": "score" }]
  },
  "results_view": {
    "styles": [{ "select": "[data-result='summary']", "props": { "fontWeight": 700 } }]
  }
}

7. Cross-Concern Links (Declarative & Bi-Directional)
Link Contract Example (Required → Validation)
{
  "link": "build.required -> logic.validation.presence",
  "forward":  "toggle(required:true) adds rule {origin:auto}",
  "reverse":  "delete(rule where origin=auto) toggles required:false",
  "conflict": "if manual rule exists: prompt replace | keep-both | cancel",
  "ui": { "autoIcon": "🔗", "manualIcon": "⛓", "conflictIcon": "⚠️" }
}
Properties
Links never create structure; they synchronize semantics across concerns.


Forward/backward behavior must be deterministic and traceable (origin, lineage).



8. Scope, Resolution, & Data Flow
Resolution Rules (inner → outer):


Local overrides (subcontainer)


Parent container bindings


Configuration-level bindings / Knowledge refs


Project-level globals (read-only)


Forbidden: sibling mutation, cross-scope DOM/style creation, implicit globals.


Events: Atoms emit named events; containers map them to logic actions.



9. Validation (Draft / Strict / Import)
{
  "validation": {
    "mode": "draft|strict|import",
    "rules": ["schema","crossRef","perfLimits","security","deps","a11y"],
    "severity": { "info": true, "warn": true, "error": true },
    "publishBlocksOn": ["error"]
  }
}
Draft: debounced, async; never blocks.


Strict: blocks publish on errors.


Import: renames on collision + writes an audit trail.



10. Performance Budgets
{
  "complexityLimits": {
    "base": { "maxComponentsPerContainer": 50, "maxNestingDepth": 5 },
    "profiles": { "mobile": { "maxComponentsPerContainer": 20 } },
    "warnAtPercent": [80,90,100],
    "degradation": "virtualize | warn"
  }
}
UI shows a live complexity meter; telemetry (opt-in) samples perf beacons.

11. Security & A11y (Baseline)
{
  "security": {
    "sandbox": { "runner": "webworker", "allow": ["timers","math"], "deny": ["fetch","eval","dom","storage"] },
    "templateEscaping": "auto-escape-html",
    "sanitizers": { "html": "DOMPurify", "style": "strict", "logic": "op-allowlist" },
    "csp": "default-src 'none'; img-src data:; style-src 'self'; script-src 'self' 'strict-dynamic'",
    "sri": true
  },
  "a11y": {
    "rolesRequired": true,
    "labelsRequired": true,
    "minContrast": 4.5,
    "keyboardParity": true,
    "minTouchTargetPx": 44
  }
}

12. Export Shape (Deterministic)
/export/
  build.tsx         /* @calc:generated(ir="1.2.0", source="project::cfg::...") */
  build.css
  logic.ts
  knowledge.json
  results.tsx
  results.css
Generated blocks are guarded; optional user regions are explicit and never overwritten.


Prettier/eslint/a11y lint run inside generator; exports must pass clean.



13. Testing & Goldens
{
  "testSuite": {
    "name": "EmpathyRequired",
    "target": "ctr::coreMotivations",
    "assertions": [
      "exists(atom::ui.slider::empathy)",
      "link(build.required -> logic.validation.presence)"
    ],
    "goldenExports": ["build.tsx","logic.ts","results.tsx"],
    "perfBudgets": { "previewRenderMs_p50": 12, "publishMs_p95": 800 }
  }
}
Round-trip: IR → Export → Parse → IR′ must preserve structure/order/IDs.


Golden diffs enforce template stability.



14. Worked Example (Minimal)
Build
{
  "id": "ctr::coreMotivations",
  "children": ["atom::ui.slider::empathy"]
}
BuildStyle
{ "styles": [{ "select": "[data-anchor='core-motivations']", "props": { "padding":"16px" } }] }
Logic (via link from required)
{
  "rules": [{
    "id":"rule::presence::empathyRequired",
    "validate": { "target":"atom::ui.slider::empathy", "type":"required" },
    "origin":"auto"
  }]
}
Results
{ "blocks": [{ "id":"result::summary", "from":"state.empathy", "render":"score" }] }

15. Traceability Matrix (excerpt)
Spec Rule
UI Indicator
Test
Export Evidence
Deterministic order
Reorder handle mirrors tabs
order-consistency.spec
file order stable
Required → Validation link
🔗 icon on rule
required-link.spec
logic.ts presence rule
ID collision policy
Modal + audit log
import-collision.spec
renamed IDs + audit.json
A11y roles/labels
A11y linter badge
a11y.spec
lint pass report


16. ADR Reference (sample)
ADR-007: Cross-Concern Links are Declarative & Bi-Directional
Status: Accepted — Provides deterministic sync; resolves conflicts via explicit policy; prevents drift.

Outcome: With this section, Atoms and (Sub)Containers are formally defined as programming primitives and constructs, governed by scope and concern separation, linked deterministically across layers, validated against clear rules, and exported into a reproducible six-file layout that Calculogic can regenerate byte-for-byte from the IR.




Potentially for later
{
  "catalogVersion": "1.2.0",
  "atoms": [
    /* ───────────── UI (HTML primitives) ───────────── */
    { "id": "ui.text",     "kind": "ui",
      "props": { "value":{"type":"string"}, "as":{"type":"enum","values":["p","span","h1","h2","h3","h4","h5","h6"],"default":"p"} },
      "a11y": { "labelRequired": false } },

    { "id": "ui.input",    "kind": "ui",
      "props": { "type":{"type":"enum","values":["text","number","email","password"],"default":"text"},
                 "label":{"type":"string"}, "placeholder":{"type":"string"},
                 "min":{"type":"number"}, "max":{"type":"number"}, "step":{"type":"number"},
                 "valueBinding":{"type":"string"} },
      "events": ["onChange","onBlur"], "a11y": { "labelRequired": true } },

    { "id": "ui.checkbox", "kind": "ui",
      "props": { "label":{"type":"string"}, "checked":{"type":"boolean"}, "valueBinding":{"type":"string"} },
      "events": ["onChange"], "a11y": { "labelRequired": true } },

    { "id": "ui.select",   "kind": "ui",
      "props": { "label":{"type":"string"},
                 "options":{"type":"list","items":{"type":"object","props":{"value":"string","label":"string"}}},
                 "valueBinding":{"type":"string"} },
      "events": ["onChange"], "a11y": { "labelRequired": true } },

    { "id": "ui.button",   "kind": "ui",
      "props": { "label":{"type":"string"}, "variant":{"type":"enum","values":["solid","ghost","link"],"default":"solid"} },
      "events": ["onClick"], "a11y": {} },

    { "id": "ui.image",    "kind": "ui",
      "props": { "src":{"type":"string"}, "alt":{"type":"string"}, "width":{"type":"number"}, "height":{"type":"number"}, "fit":{"type":"enum","values":["cover","contain","fill","none","scale-down"]} },
      "a11y": { "altRequired": true } },

    { "id": "ui.link",     "kind": "ui",
      "props": { "href":{"type":"string"}, "label":{"type":"string"}, "target":{"type":"enum","values":["_self","_blank"],"default":"_self"}, "rel":{"type":"string"} },
      "events": ["onClick"], "a11y": {} },

    { "id": "ui.form",     "kind": "ui",
      "props": { "method":{"type":"enum","values":["get","post"],"default":"get"}, "action":{"type":"string"} },
      "events": ["onSubmit"], "a11y": {} },

    { "id": "ui.icon",     "kind": "ui",
      "props": { "name":{"type":"string"}, "title":{"type":"string"}, "ariaHidden":{"type":"boolean","default":false} },
      "a11y": {} },

    { "id": "ui.progress", "kind": "ui",
      "props": { "value":{"type":"number"}, "max":{"type":"number","default":100}, "label":{"type":"string"} },
      "a11y": { "labelRequired": false } },

    { "id": "ui.details",  "kind": "ui",
      "props": { "summary":{"type":"string"}, "open":{"type":"boolean","default":false} },
      "a11y": {} },

    /* ───────────── Layout (constructs/scopes) ───────────── */
    { "id": "layout.stack","kind":"layout",
      "props": { "direction":{"type":"enum","values":["row","column"],"default":"column"},
                 "gap":{"type":"number","default":8},
                 "align":{"type":"enum","values":["start","center","end","stretch"],"default":"start"},
                 "justify":{"type":"enum","values":["start","center","end","between"],"default":"start"},
                 "wrap":{"type":"boolean","default":false} } },

    { "id": "layout.grid", "kind":"layout",
      "props": { "columns":{"type":"number","default":2}, "rows":{"type":"number"}, "gap":{"type":"number","default":8}, "min":{"type":"number","default":250} } },

    { "id": "layout.group","kind":"layout",
      "props": { "as":{"type":"enum","values":["div","section","fieldset"],"default":"div"}, "legend":{"type":"string"} } },

    { "id": "layout.repeat","kind":"layout",
      "props": { "from":{"type":"expression"}, "itemName":{"type":"string"} } },

    /* ───────────── Style (CSS primitives) ───────────── */
    { "id": "style.rule",  "kind":"style",
      "props": { "select":{"type":"selector"}, "props":{"type":"object"} } },

    { "id": "style.var",   "kind":"style",
      "props": { "name":{"type":"string"}, "value":{"type":"string"} } },

    { "id": "style.query", "kind":"style",
      "props": { "type":{"type":"enum","values":["container","media"],"default":"container"}, "when":{"type":"string"} } },

    { "id": "style.flex",  "kind":"style",
      "props": { "select":{"type":"selector"}, "direction":{"type":"enum","values":["row","column"]}, "gap":{"type":"number"}, "justify":{"type":"string"}, "align":{"type":"string"}, "wrap":{"type":"boolean"} } },

    { "id": "style.grid-template","kind":"style",
      "props": { "select":{"type":"selector"}, "cols":{"type":"string"}, "rows":{"type":"string"}, "areas":{"type":"array"} } },

    { "id": "style.motion","kind":"style",
      "props": { "select":{"type":"selector"}, "transition":{"type":"string"}, "duration":{"type":"number"}, "easing":{"type":"string"} } },

    /* ───────────── Logic (TS primitives) ───────────── */
    { "id": "logic.state",   "kind":"logic",
      "props": { "name":{"type":"string"}, "type":{"type":"enum","values":["number","string","boolean","object"],"default":"number"}, "initial":{"type":"any"} } },

    { "id": "logic.compute", "kind":"logic",
      "props": { "expr":{"type":"expression"} } },

    { "id": "logic.if",      "kind":"logic",
      "props": { "cond":{"type":"expression"} } },

    { "id": "logic.on",      "kind":"logic",
      "props": { "event":{"type":"enum","values":["onChange","onClick","onSubmit"]}, "target":{"type":"ref"}, "do":{"type":"actionList"} } },

    { "id": "logic.validate","kind":"logic",
      "props": { "target":{"type":"ref"}, "rule":{"type":"enum","values":["required","range","pattern"]}, "args":{"type":"object"} } },

    { "id": "logic.validateGroup","kind":"logic",
      "props": { "group":{"type":"ref"}, "rule":{"type":"enum","values":["maxSelected"]}, "args":{"type":"object"} } },

    { "id": "logic.showHide","kind":"logic",
      "props": { "target":{"type":"ref"}, "when":{"type":"expression"} } },

    { "id": "logic.repeat",  "kind":"logic",
      "props": { "list":{"type":"ref|array"}, "itemName":{"type":"string"} } },

    { "id": "logic.toggle",  "kind":"logic",
      "props": { "target":{"type":"ref"}, "default":{"type":"boolean","default":false}, "onTrue":{"type":"actionList"}, "onFalse":{"type":"actionList"} } },

    { "id": "logic.delay",   "kind":"logic",
      "props": { "ms":{"type":"number"}, "then":{"type":"actionList"} } },

    { "id": "logic.bind",    "kind":"logic",
      "props": { "from":{"type":"expression"}, "to":{"type":"ref.prop"} } },

    { "id": "logic.route",   "kind":"logic",
      "props": { "path":{"type":"string"}, "when":{"type":"expression"} } },

    /* ───────────── Knowledge (data primitives) ───────────── */
    { "id": "kb.const",   "kind":"kb",
      "props": { "key":{"type":"string"}, "value":{"type":"any"} } },

    { "id": "kb.list",    "kind":"kb",
      "props": { "key":{"type":"string"}, "items":{"type":"array"} } },

    { "id": "kb.map",     "kind":"kb",
      "props": { "key":{"type":"string"}, "entries":{"type":"object"} } },

    { "id": "kb.enum",    "kind":"kb",
      "props": { "key":{"type":"string"}, "values":{"type":"array"} } },

    { "id": "kb.schema",  "kind":"kb",
      "props": { "name":{"type":"string"}, "fields":{"type":"object"} } },

    { "id": "kb.ref",     "kind":"kb",
      "props": { "key":{"type":"string"}, "scope":{"type":"enum","values":["local","project","global"],"default":"local"}, "fallback":{"type":"any"} } },

    /* ───────────── Results (output primitives) ───────────── */
    { "id": "result.text",  "kind":"result",
      "props": { "value":{"type":"expression"}, "as":{"type":"enum","values":["p","span","h3"],"default":"p"} } },

    { "id": "result.block", "kind":"result",
      "props": { "from":{"type":"expression"}, "renderer":{"type":"enum","values":["score","json","list"],"default":"score"} } },

    { "id": "result.list",  "kind":"result",
      "props": { "from":{"type":"expression"},
                 "sortBy":{"type":"string"},
                 "order":{"type":"enum","values":["asc","desc"],"default":"desc"},
                 "itemTemplate":{"type":"enum","values":["text","json","score"],"default":"score"} } },

    /* ───────────── Meta/System (educational & tooling) ───────────── */
    { "id": "meta.comment", "kind":"meta",
      "props": { "text":{"type":"string"}, "author":{"type":"string"}, "concern":{"type":"enum","values":["Build","BuildStyle","Logic","Knowledge","Results","ResultsStyle"]} } },

    { "id": "meta.example", "kind":"meta",
      "props": { "of":{"type":"ref"}, "text":{"type":"string"} } },

    { "id": "system.anchor","kind":"system",
      "props": { "ref":{"type":"string"}, "targetConcern":{"type":"enum","values":["Build","BuildStyle","Logic","Knowledge","Results","ResultsStyle"]} } },

    { "id": "system.debug", "kind":"system",
      "props": { "expr":{"type":"expression"}, "label":{"type":"string"} } },

    { "id": "system.doc-block","kind":"system",
      "props": { "title":{"type":"string"}, "body":{"type":"string"} } }
  ]
}

Notes
Keep Build → BuildStyle → Logic → Knowledge → Results → ResultsStyle export mapping 1:1.


A11y: enforce labels for form controls, valid alt on ui.image, keyboard parity.


Security: Results auto-escape; style allowlist; logic runs in an allowlisted sandbox.


Determinism: visual order in Build remains canonical across all generated files.




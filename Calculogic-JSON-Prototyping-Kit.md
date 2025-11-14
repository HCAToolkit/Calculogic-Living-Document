# Calculogic JSON Prototyping Kit
1) Constants & Enums (recommended)
{
"$constants": {
"concerns": ["Build", "BuildStyle", "Logic", "Knowledge", "Results", "ResultsStyle"],
"foundationalTypes": ["Primitive", "StyleUnit", "Computation", "Container", "Reference", "Result"],
"containerTypes": ["layout", "function", "conditional", "loop", "evaluator", "group"],
"visibility": ["public", "private", "official"]
}
}

2) Base Shapes (composition contract)
Only the Configuration has concern tabs. Everything inside it is ordered exactly as it appears visually (per tab).
{
"Configuration": {
"id": "cfg-uid",
"type": "Container",
"meta": {
"name": "Human Name",
"author": "System",
"visibility": "public",
"created": "2025-10-30T20:00:00Z",
"updated": "2025-10-30T20:00:00Z",
"description": "Purpose of this configuration"
},
"Build":    { "sequence": [] },
"BuildStyle":{ "sequence": [] },
"Workflow": { "sequence": [] },
"Knowledge":{ "sequence": [] },
"Results":  { "sequence": [] },
"ResultsStyle": { "sequence": [] }
},

"Atomic": {
"id": "atom-uid",
"type": "AtomicComponent",
"meta": {
"foundationalType": "Primitive",
"label": "User-facing label or intent",
"notes": "Design/dev notes; becomes a comment on export"
},
"data": { }
},

"ContainerNode": {
"id": "ctr-uid",
"type": "Container",
"meta": {
"foundationalType": "Container",
"containerType": "layout",
"name": "Group or Function Name",
"notes": "Why this group/function exists"
},
"sequence": []
}
}

3) Sequence Item Shapes per Concern
Each entry inside a sequence is one of these shapes. Keep visual order.
{
"SeqAtomic": {
"kind": "Atomic",
"ref": "atom-uid",
"meta": {
"role": "field|style|var|op|ref|result",
"origin": "ui:dropdown:checkbox",
"lineage": ["cfg-uid", "ctr-uid?"]
},
"props": { }
},

"SeqContainer": {
"kind": "Container",
"ref": "ctr-uid",
"meta": {
"containerType": "layout|function|conditional|loop|evaluator|group",
"origin": "ui:toggle:function"
},
"sequence": []
}
}

4) Minimal, Valid Example (no subcontainers; two atomics in Build; single items elsewhere)
{
"id": "cfg-001",
"type": "Container",
"meta": {
"name": "Enneagram Motivation Selector",
"author": "System",
"visibility": "public",
"created": "2025-10-30T20:00:00Z",
"updated": "2025-10-30T20:00:00Z",
"description": "Two checkboxes; one results display; synchronized across concerns"
},

"Build": {
"sequence": [
{
"kind": "Atomic",
"ref": "atom-001",
"meta": {
"role": "field",
"origin": "ui:dropdown:checkbox",
"lineage": ["cfg-001"]
},
"props": {
"domType": "input",
"type": "checkbox",
"label": "I strive to be the best version of myself."
}
},
{
"kind": "Atomic",
"ref": "atom-002",
"meta": {
"role": "field",
"origin": "ui:dropdown:checkbox",
"lineage": ["cfg-001"]
},
"props": {
"domType": "input",
"type": "checkbox",
"label": "Option 2"
}
}
]
},

"BuildStyle": {
"sequence": [
{
"kind": "Atomic",
"ref": "atom-001",
"meta": {
"role": "style",
"origin": "ui:style:accent-color",
"lineage": ["cfg-001"]
},
"props": {
"selector": "#atom-001",
"style": {
"accent-color": "#4CAF50",
"margin": "6px"
}
}
},
{
"kind": "Atomic",
"ref": "atom-002",
"meta": {
"role": "style",
"origin": "ui:style:accent-color",
"lineage": ["cfg-001"]
},
"props": {
"selector": "#atom-002",
"style": {
"accent-color": "#2196F3",
"margin": "6px"
}
}
}
]
},

"Workflow": {
"sequence": [
{
"kind": "Container",
"ref": "ctr-fn-001",
"meta": {
"containerType": "function",
"origin": "ui:toggle:function",
"lineage": ["cfg-001"]
},
"sequence": [
{
"kind": "Atomic",
"ref": "atom-001",
"meta": { "role": "event", "origin": "ui:logic:onChange", "lineage": ["cfg-001","ctr-fn-001"] },
"props": {
"onChange": {
"if": "checked == true",
"then": "incrementScore('Type3',1)",
"else": "decrementScore('Type3',1)"
}
}
},
{
"kind": "Atomic",
"ref": "atom-002",
"meta": { "role": "event", "origin": "ui:logic:onChange", "lineage": ["cfg-001","ctr-fn-001"] },
"props": {
"onChange": {
"if": "checked == true",
"then": "incrementScore('Type4',1)",
"else": "decrementScore('Type4',1)"
}
}
}
]
}
]
},

"Knowledge": {
"sequence": [
{
"kind": "Atomic",
"ref": "atom-001",
"meta": { "role": "reference", "origin": "ui:map:reference", "lineage": ["cfg-001"] },
"props": { "reference": "Enneagram.Type3.Motivation" }
},
{
"kind": "Atomic",
"ref": "atom-002",
"meta": { "role": "reference", "origin": "ui:map:reference", "lineage": ["cfg-001"] },
"props": { "reference": "Enneagram.Type4.Motivation" }
}
]
},

"Results": {
"sequence": [
{
"kind": "Atomic",
"ref": "atom-res-001",
"meta": { "role": "result", "origin": "ui:results:calc", "lineage": ["cfg-001"] },
"props": {
"outputKey": "topType",
"calculation": "argmax(scores)",
"dependsOn": ["Type3","Type4"]
}
}
]
},

"ResultsStyle": {
"sequence": [
{
"kind": "Atomic",
"ref": "atom-resview-001",
"meta": { "role": "render", "origin": "ui:view:text", "lineage": ["cfg-001"] },
"props": {
"selector": "#result-display",
"template": "Your top Enneagram type is: {{ topType }}"
}
}
]
},

"atoms": [
{
"id": "atom-001",
"type": "AtomicComponent",
"meta": {
"foundationalType": "Primitive",
"label": "Motivation Checkbox — Type 3",
"notes": "Adds to Type3 when selected"
},
"data": { "dataType": "boolean", "defaultValue": false }
},
{
"id": "atom-002",
"type": "AtomicComponent",
"meta": {
"foundationalType": "Primitive",
"label": "Motivation Checkbox — Type 4",
"notes": "Adds to Type4 when selected"
},
"data": { "dataType": "boolean", "defaultValue": false }
}
],

"containers": [
{
"id": "ctr-fn-001",
"type": "Container",
"meta": {
"foundationalType": "Container",
"containerType": "function",
"name": "MotivationScoreLogic",
"notes": "Event handlers for checkbox changes"
},
"sequence": []
}
]
}

5) With Subcontainers (compact pattern)
{
"Workflow": {
"sequence": [
{
"kind": "Container",
"ref": "ctr-fn-main",
"meta": { "containerType": "function", "lineage": ["cfg-002"] },
"sequence": [
{
"kind": "Container",
"ref": "ctr-if-teen",
"meta": { "containerType": "conditional", "lineage": ["cfg-002","ctr-fn-main"] },
"sequence": [
{ "kind": "Atomic", "ref": "atom-age", "meta": { "role": "var" }, "props": { "expression": "get('age')" } },
{ "kind": "Atomic", "ref": "atom-teen-branch", "meta": { "role": "op" }, "props": { "if": "age >= 13 && age <= 19", "then": "set('segment','teen')" } }
]
}
]
}
]
}
}

6) Lineage & Meta-to-Comment Rule (engine hint)
Every item carries meta and optional lineage (array of ancestor ids).
On export, the engine emits ordered code + comments using meta (and may include per-property comments when props originate from UI selections).
7) ID & Ordering Rules
IDs are unique: cfg-*, ctr-*, atom-*.
Order is authoritative: the sequence array per concern mirrors the builder's visual order.
Cross-tab sync: the same ref must maintain relative position wherever it appears.



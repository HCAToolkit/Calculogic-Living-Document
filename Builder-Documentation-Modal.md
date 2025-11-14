# Builder Documentation Modal
1. Treat Documentation as its own tiny cross-cutting concern
Make a small “Documentation” layer that every UI element can tap into:
Per-tab (or per-thing):
Build


What this thing is structurally (tab, header, canvas, config list, etc.)


Where its docs data lives (a key/id into Knowledge)


BuildStyle


Where the ℹ️ icon sits


Tooltip position rules


“This thing has docs” affordances (hover highlight, etc.)


Logic


onHoverInfo → show tooltip


onClickInfo → open modal (with a docId)


onCloseModal → close


Knowledge


The actual documentation content indexed by docId


Short summary (for tooltips)


Extended content chunks (for modal sections)


Results


“Resolved doc payload” for that docId


Think: { title, summary, sections[], links[] }


ResultsStyle


The modal layout template:


Header (title, concern name)


Optional “Recommended workflows”


Body sections


Cross-links list


Close button, scroll behavior, etc.


In other words: Results = “doc data ready to render”, ResultsStyle = “how the documentation modal looks.”

2. Implementation order (matching your left→down path)
You can implement the docs system in a way that lines up with how you described scanning the UI:
Step 1 – Core docs engine + modal shell
Global DocumentationModal component (even if it’s ugly at first)


Global state: currentDocId | null


Logic: open/close, escape key, clicking backdrop, etc.


Wire only the header/tab info icons to it at first:


Build, Logic, Knowledge, Results, BuildStyle, ResultsStyle


Publish block’s ℹ️ if you want


Now you have a working “documentation output system” using Results/ResultsStyle metaphor, before touching the main builder body.
Step 2 – Left column skeleton
Top-down, left side:
Configurations section shell


Just the area where the configs list will go


ℹ️ icon for “Configurations”


Connect it to the same docs system (new docId)


Atomic components section shell


Header row with name + ℹ️


Empty body for now


Docs hook wired


Search section shell


Search bar area + ℹ️


Docs hook wired


Each of these only needs:
Info icon placement (BuildStyle)


One doc key (Knowledge)


Click → open docs modal (Logic)


Step 3 – Center canvas area
Add canvas header row (“Canvas” or similar) with ℹ️


Empty canvas body is fine right now


Same docs hookup


Step 4 – Right inspector
Inspector header (“Inspector” / “Details”) + ℹ️


Empty body, docs hooked in


At that point:
Even if the builder doesn’t do much yet, the entire frame – header, left columns, canvas, inspector – all participate in the same “Documentation output” system.

3. Why putting docs first helps
You get a single, reusable Results/ResultsStyle pattern for documentation.


Every time you add a new structural piece later (real config list, real atom palette, real inspector), you already have:


An info icon convention


A way to attach a docId


A place for the explanation to render


Interface/Builder doc: define the skeleton
Keep it light but explicit:
What the docs engine is:

 A global system that:


Resolves docId → content (Knowledge / Results)


Renders it in a standard layout (ResultsStyle)


Exposes a tiny Logic API: openDoc(docId), closeDoc()


Minimal IR for docs entries:


docId


concern (Build / Logic / Header / Canvas / etc.)


summary (tooltip)


sections[] (title + body)


links[] (to other docs)


Interaction contracts:


Any UI element can:


Declare docId


Show a tooltip using summary


Call openDoc(docId) on click


That’s enough to keep it charter-aligned and let you wire the header + shells.
2. Later: separate “Documentation Engine” living doc
Once the builder frame is real, make a small dedicated doc (and eventually its own repo or package) that:
Treats documentation itself as pure Calculogic data:


One or more “Docs Projects”


Configurations representing doc pages / topics


Knowledge tab holding structured doc content


Results/ResultsStyle defining how docs render in:


Modals


Side panels


Full-page help


Defines export hooks for different UI types:


“Web builder docs”


“VS Code extension docs”


“Electron desktop docs”


etc.


This is where the dogfooding really kicks in: the docs engine becomes a Calculogic-powered app that the builder just consumes.
3. Practical sequence
Now, in the Interface doc:


Add a short section: “Documentation Engine: Skeleton & Contracts.”


Define IR fields + openDoc/closeDoc + tooltip rules.


Implement just enough to support:


Header tab docs


Header publish docs


Left/center/right frame shells


After the header + frame exist:


Spin up a “Documentation Engine” living doc.


Start modeling docs as Calculogic configurations.


Plan how docs configs get exported/loaded into the UI.


That way:
You don’t over-scope the current doc.


You still design it so future you can fully dogfood docs in a stack-agnostic way.

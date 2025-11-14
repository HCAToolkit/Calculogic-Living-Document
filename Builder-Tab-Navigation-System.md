# Builder Tab Navigation System
## Header Navigation Goal (NL)
## Pass 0 – Global Header Frame
#### 0.1 Header shell
Three zones:


- Left: Calculogic brand block.


- Center: tab strip.


- Right: publish area.


Tabs never wrap; they live on a single horizontal row.


Responsive sketch:
- Desktop (≥ 1024px): one row, brand + tabs + publish all visible.


- Tablet (768–1023px): same structure, tighter spacing; tagline may hide.


Mobile (< 768px):


Center tab strip is horizontally scrollable.


Tagline is hidden.


Publish may shrink label or go icon-first, but stays on the right.


#### 0.2 Canonical header tab order
The header always shows tabs in this exact order:
Build


Logic


Knowledge


Results


No user configuration or theme is allowed to reorder these four.
(Internally, Build/Style corresponds to BuildStyle, and Results/Style corresponds to ResultsStyle, but the header merges each pair into a single tab.)
#### 0.3 Documentation hooks (skeleton only)
Global actions exist:


openDoc(docId)


closeDoc()


In this pass:


ℹ️ icons in the header show tooltips only.


Click-to-open-modal is reserved for later, via openDoc(docId).



## Pass 1 – Left: Calculogic Brand Block
#### 1.1 Visual
Contains:


MVP logo icon.


Wordmark “Calculogic”.


Optional tagline (“a system for creating systems”) beneath or beside.


Responsiveness:
- Desktop / tablet: icon + wordmark; tagline visible if space allows.


- Mobile: tagline hidden; icon + wordmark remain (future allowance for icon-only if needed).


#### 1.2 Behavior
Click brand → go to main dashboard/home.


Normal link behavior (middle-click, Cmd/Ctrl-click open in new tab).


#### 1.3 Knowledge
Tooltip string, e.g. “Return to Calculogic home / dashboard.”



## Pass 2 – Center: Build Tab (with Build/Style modes)
- Header tab: Build
Internal modes:
- Default: Build/  (this is the structural view)


- Alternate: Build/Style/  (this is what Universal Foundation calls “BuildStyle”)


There are exactly two states, not three. “Structure” is implicit in Build/.
#### 2.1 Base states (Build tab)
- Inactive: neutral styling.


- Active: highlighted styling when Build is the current concern (regardless of mode).


- Hover (inactive): visual feedback + preview area.


#### 2.2 Mode model
When Build is active:


mode = "default" → breadcrumb shows Build/


mode = "style" → breadcrumb shows Build/Style/


#### 2.3 Hover preview (Build inactive)
If the user is on Logic/Knowledge/Results and hovers Build:
- A small preview shows at least one entry: Style.


Interactions:


Click Build tab label:


Activate Build with mode = "default" → Build/.


Click Style in preview:


Activate Build with mode = "style" → Build/Style/.


No third label like “Structure” is shown; “just Build” === Build/.
#### 2.4 Active Build behavior
When Build is active:
A breadcrumb or mini folder label appears in a consistent spot:


- Default mode: Build/


- Style mode: Build/Style/


Switching:


- From Build/ to Build/Style/: click a Style control in that breadcrumb/mode selector.


- From Build/Style/ back to Build/: click on Build/.


#### 2.5 Info icon (ℹ️) for Build – tooltip only
ℹ️ positioned near the Build tab label (e.g. top-right of tab).


- Hover: one-line summary of the Build concern.


- Click: no effect yet.


#### 2.6 Knowledge entries for Build
Examples:
Hover summary:


“Define and arrange containers, sub-containers, and atomic components.”


Clarifier:


“Build/ is the structural view. Build/Style/ is a styling-oriented view of the same structure (what would otherwise be called BuildStyle).”



## Pass 3 – Center: Logic Tab (single view)
- Header tab: Logic
- Single view: Logic/
#### 3.1 States
Inactive / active / hover visuals like other tabs.


No sub-modes.


#### 3.2 Behavior
Click Logic → activate Logic/ and load the Logic (Workflow) view.


#### 3.3 Info icon – tooltip only
- Hover: one-line summary (“Add calculations, conditions, and interaction rules.”)


- Click: no effect yet.


#### 3.4 Knowledge
Boundary note for docs, e.g.:


“Logic coordinates state and interactions. Heavy domain-specific math lives in helpers/services and is called from Logic.”



## Pass 4 – Center: Knowledge Tab (single view)
- Header tab: Knowledge
- Single view: Knowledge/
#### 4.1 States
Same inactive/active/hover pattern.


No sub-modes.


#### 4.2 Behavior
Click Knowledge → activate Knowledge/ and load Knowledge view.


#### 4.3 Info icon – tooltip only
- Hover: one-line summary (“Store reusable traits, constants, and reference schemas.”)


- Click: no effect yet.


#### 4.4 Knowledge
Extra examples for later docs (trait lists, score tables, label maps, etc.).



## Pass 5 – Center: Results Tab (with Results/Style modes)
- Header tab: Results
Internal modes:
- Default: Results/  (defining and inspecting outputs)


- Alternate: Results/Style/  (what Universal Foundation calls “ResultsStyle”)


Same two-level pattern as Build; no third layer.
#### 5.1 Base states (Results tab)
Inactive / active / hover states like Build.


- Hover (inactive): shows preview with Style.


#### 5.2 Mode model
When Results is active:
mode = "default" → breadcrumb shows Results/


mode = "style" → breadcrumb shows Results/Style/


#### 5.3 Hover preview (Results inactive)
Hovering Results when inactive:


Shows preview listing at least Style.


Interactions:


Click Results tab label:


Activate Results with mode = "default" → Results/.


Click Style in preview:


Activate Results with mode = "style" → Results/Style/.


#### 5.4 Active Results behavior
Breadcrumb:


- Default: Results/


- Style: Results/Style/


Switching:


- From Results/ to Results/Style/: click Style control in the breadcrumb/mode selector.


- From Results/Style/ back to Results/: click Results/.


Interpretation:
Results/ – define outputs, scores, and summary structures.


Results/Style/ – adjust how those outputs are visually presented within the Results concern.


#### 5.5 Info icon – tooltip only
ℹ️ near the Results tab label.


- Hover: one-line summary (“Design and inspect derived outputs, scores, and summaries.”)


- Click: no effect yet.


#### 5.6 Knowledge
Clarifier for docs:


“Results/ defines what is output and how it is derived. Results/Style/ is the styling layer for those outputs (what would otherwise be called ResultsStyle).”



## Pass 6 – Right: Publish Area
#### 6.1 Visual
Primary Publish button on the right side of the header.


- States: default (enabled), disabled, and hover.


Responsive:
- Desktop/tablet: full “Publish” label.


- Mobile: allowed to shorten the label or go icon-first, but must remain accessible and clearly tappable.


#### 6.2 Behavior
Click Publish → triggers “Publish current configuration” (pipeline to be defined later).


This pass only pins down location, states, and the existence of a publish hook.



## Pass 7 – Docs Modal (outline only)
#### 7.1 Results/ResultsStyle semantics for docs
Documentation uses the same idea of “output” and “styling”:


Docs data (for a docId) is like Results/ – the resolved payload.


The documentation modal layout is like Results/Style/ – how that payload is rendered.


#### 7.2 Header integration
Every ℹ️ in the header has:


A docId (conceptually).


A hover summary string (already defined).


Eventually:


Hover → tooltip.


Click → openDoc(docId) → show docs modal.


#### 7.3 Scope
In this pass:


openDoc / closeDoc exist but no actual modal UI is implemented.


Info icons are tooltip-only.



This version matches your semantics:
- Only four tabs in the header: Build, Logic, Knowledge, Results.


BuildStyle is exactly Build/Style/.


ResultsStyle is exactly Results/Style/.


No ghost third nav level (“Structure”) – Build/ and Results/ are the default modes.

- ProjectShell Configuration: Global Header Shell (shell-globalHeader)
- Project: Calculogic Interface Builder (proj-calculogicInterface)
- Type: Persistent Navigation Shell
- Scope: Global – wraps all Configuration views
- Passes: 0–7 (multi-pass implementation)

1. Purpose and Scope
#### 1.1 This shell provides the persistent global header for the Calculogic interface, including brand, primary concern tabs (Build, Logic, Knowledge, Results), and the publish action.
#### 1.2 It appears at the top of all builder views and remains visible as users switch between configurations and concerns.
#### 1.3 It coordinates with routing, configuration context, and future documentation modals via global actions such as openDoc(docId) and closeDoc().
#### 1.4 In-scope:
Persistent header frame and layout (brand, concern tabs, mode menus for Build/Results, publish).


Tab/mode state management and responsive behavior.


Hover-based dropdown behavior for Build/Results mode menus.
#### 1.5 Out-of-scope:


Per-config secondary navigation, config list panels, inspector content.


Actual publish pipeline implementation.


Documentation modal structure and rendering (handled by a separate docs configuration/shell).



2. Configuration Contracts
#### 2.1 TypeScript Interfaces
Shell-level state types (illustrative):
type HeaderTabId = "build" | "logic" | "knowledge" | "results";
type HeaderModeId = "default" | "style"; // only applies to build/results


interface GlobalHeaderShellState {
activeTab: HeaderTabId;
activeModeByTab: {
build: HeaderModeId;
results: HeaderModeId;
};
hoveredTab: HeaderTabId | null;
modeMenuVisibleForTab: HeaderTabId | null; // derived/persisted visibility
viewportBreakpoint: "desktop" | "tablet" | "mobile";
}


interface GlobalHeaderShellProps {
// hooks into publish pipeline
onPublish?: () => void;
}

#### 2.2 Global State Requirements
Owns:
activeTab (which concern tab is active).


activeModeByTab for Build and Results (default vs style).


hoveredTab for hover preview behavior.


modeMenuVisibleForTab – which tab’s mode menu (Build/Results) is currently shown.


When the tab is active, the mode menu is “pinned” (stays visible).


When the tab is inactive, the mode menu is transient (visible only while hovered).


viewportBreakpoint for responsive layout flags.


Consumes:
Routing/context to know which project/config is currently loaded.


Optional external “publish pipeline” handler.


#### 2.3 Routing & Context
URL patterns must be able to reflect which tab (concern) is active.
Config ID context may show project/config names elsewhere, but the header tabs are always the four canonical concerns, not per-config.

3. Build Concern (Structure)
#### 3.0 Dependencies & Hierarchy Notes
- Parent: top-level app shell / root layout.
Requires routing and configuration providers to sit above it.
Build has one primary Container for the header shell, with three main Subcontainers: left, center, right.
The center zone contains the tab strip and the Build/Results mode menus.
#### 3.1 Atomic Components — Container (Build)
- Container name: “Global Header Shell”.
- Hierarchical type: Container (root of Build for this shell).
- Catalog base: layout.group / layout.shell.
- Anchor: data-anchor="global-header".
Layout:
One horizontal row for all breakpoints.


- Three zones aligned: left (brand), center (tabs + mode menus), right (publish).


Tabs are constrained to a single horizontal line; they never wrap.


#### 3.2 Atomic Components — Subcontainers (Build) – Shell Zones
##### 3.2.1 Subcontainer “Left Zone – Brand Block”
- Hierarchical type: Subcontainer.
- Purpose: hosts the Calculogic brand elements.
Children:
- Primitive: logo icon.


- Primitive: wordmark “Calculogic”.


- Primitive: optional tagline “a system for creating systems”.


Responsiveness:
- Desktop/tablet: icon + wordmark, tagline visible if space allows.


- Mobile: tagline hidden; icon + wordmark remain (future allowance for icon-only variant).


##### 3.2.2 Subcontainer “Center Zone – Tab Strip + Mode Menus”
- Hierarchical type: Subcontainer.
- Purpose: hosts the four main concern tabs (Build, Logic, Knowledge, Results), their info icons, and the mode menus for Build and Results.
Children:
Subcontainer “Tab List Row” (single row flex container).


For each tab (Build, Logic, Knowledge, Results):


Primitive “Tab Button [Build/Logic/Knowledge/Results]”.


Primitive “Info Icon [Build/Logic/Knowledge/Results]”.


Subcontainer “Build Tab – Mode Menu”


- Purpose: dropdown/breadcrumb-like mode menu attached to the Build tab.


Children:


Primitive “Build Mode Item – Base” (represents Build / default mode).


Primitive “Build Mode Item – Style” (represents Build /Style / BuildStyle mode).


Subcontainer “Results Tab – Mode Menu”


- Purpose: dropdown/breadcrumb-like mode menu attached to the Results tab.


Children:


Primitive “Results Mode Item – Base” (represents Results / default mode).


Primitive “Results Mode Item – Style” (represents Results /Style / ResultsStyle mode).


Behavioral intent (structure-level note):
When Build/Results tabs are inactive, their mode menus may appear as transient dropdowns under the tab on hover.


When Build/Results tabs are active, their mode menus remain visible (pinned) as inline/breadcrumb-style controls so the user can switch between Base/Style.


##### 3.2.3 Subcontainer “Right Zone – Publish Area”
- Hierarchical type: Subcontainer.
- Purpose: hosts the primary Publish button.
Children:
- Primitive: Publish button (label + optional icon).


#### 3.3 Atomic Components — Primitives (Build)
##### 3.3.1 Primitive “Logo Icon”
##### 3.3.2 Primitive “Wordmark Text”
##### 3.3.3 Primitive “Tagline Text”
##### 3.3.4 Primitive “Tab Button – Build”
Represents the Build tab label and main clickable tab entry. Always appears first in tab order.
##### 3.3.5 Primitive “Tab Button – Logic”
##### 3.3.6 Primitive “Tab Button – Knowledge”
##### 3.3.7 Primitive “Tab Button – Results”
##### 3.3.8 Primitive “Info Icon – Build / Logic / Knowledge / Results”
Small ℹ️ icon near each tab label. In this pass, shows tooltip-only behavior; click does nothing yet.
##### 3.3.9 Primitive “Publish Button”
New primitives for the /Style subtabs:
##### 3.3.10 Primitive “Build Mode Item – Base”
- Text label: “Build”.


Appears in the Build mode menu (dropdown or inline/breadcrumb).


When clicked:


Sets activeTab = "build" and activeModeByTab.build = "default".


##### 3.3.11 Primitive “Build Mode Item – Style”
- Text label: “Build /Style”.


Appears under Build as a secondary option.


When clicked:


Sets activeTab = "build" and activeModeByTab.build = "style".


##### 3.3.12 Primitive “Results Mode Item – Base”
- Text label: “Results”.


Appears in the Results mode menu.


When clicked:


Sets activeTab = "results" and activeModeByTab.results = "default".


##### 3.3.13 Primitive “Results Mode Item – Style”
- Text label: “Results /Style”.


Appears under Results as a secondary option.


When clicked:


Sets activeTab = "results" and activeModeByTab.results = "style".



4. BuildStyle Concern (Visual Styling of Structure)
#### 4.0 Dependencies
Uses global theme tokens for colors, typography, spacing, and breakpoints.
#### 4.1 Base Layout Styles
Shell container:
Full-width header bar with background and border (if any).


Horizontal flex layout with left/center/right zones.


#### 4.2 Zone Styles
Left zone (brand):
Spacing between logo, wordmark, and tagline.


Center zone (tab strip):
Single-row layout, non-wrapping tab list.


Horizontal scroll on mobile for tab strip if needed.


Build/Results Mode Menus:
When tab is inactive and mode menu is visible:


Render as a dropdown beneath the tab label:


Slight elevation, border/shadow, and background distinct from the header.


Vertically stacked items “Build” / “Build /Style” or “Results” / “Results /Style”.


When tab is active:


Render menu as pinned, inline controls just below or directly attached to the active tab:


- May appear as a breadcrumb-like strip: e.g., Build · Build /Style.


Or as a segmented control with two pills:


Base mode item and Style mode item both visible and clickable.


Right zone (publish):
Alignment to the far right, consistent padding.


#### 4.3 Responsive Layout Rules
Desktop (≥ 1024px):
Brand + tabs + publish all visible on a single row.


Tagline visible when space allows.


Mode menus for Build/Results appear as dropdowns/pinned inline elements under the tab strip.


Tablet (768–1023px):
Same basic layout with tighter spacing; tagline may hide when space is constrained.


Mode menus may compress into a more compact segmented control style.


Mobile (< 768px):
Center tab strip becomes horizontally scrollable.


Tagline is hidden.


Mode menus may render as a small two-option segmented control beneath the active tab; for inactive tabs, dropdown behavior may be simplified or disabled if space is limited (still driven by the same Logic state).


#### 4.4 Interaction Styles
Tabs:
- Inactive: neutral styling.


- Active: highlighted (stronger color, underline/border).


- Hover: visual feedback plus, for Build/Results, trigger dropdown visibility when inactive.


Build/Results mode items:
Base vs Style clearly distinguishable (e.g., pill styles with selected state).


Active mode item visually emphasized.


Publish:
Default, hover, active, and disabled states clearly styled.



5. Logic Concern (Workflow)
#### 5.0 Dependencies
Router for syncing active concern with URL (future pass).
Viewport detection or resize observer to derive breakpoint.
Config context for knowing which configuration is currently “current” for Publish.
#### 5.1 Global State Management
activeTab: HeaderTabId – which concern tab is currently active.


activeModeByTab: { build: "default" | "style"; results: "default" | "style" }.


hoveredTab: HeaderTabId | null – which tab is currently hovered (for previews).


modeMenuVisibleForTab: HeaderTabId | null – which mode menu is visible (Build/Results), if any.


viewportBreakpoint: "desktop" | "tablet" | "mobile" – derived from Knowledge-defined breakpoints.


#### 5.2 Navigation Logic – Tabs + Mode Menus
Tab click:
Clicking the main tab buttons sets activeTab:


Build/Logic/Knowledge/Results each select the corresponding concern.


When switching to Build/Results by tab click:


activeModeByTab.[build|results] defaults to "default" if unset.


Mode menu for that tab becomes pinned:


modeMenuVisibleForTab = "build" or "results".


Mode item click (Build/Results Base vs Style):
For Build:


Clicking “Build” → activeTab = "build", activeModeByTab.build = "default".


Clicking “Build /Style” → activeTab = "build", activeModeByTab.build = "style".


For Results:


Clicking “Results” → activeTab = "results", activeModeByTab.results = "default".


Clicking “Results /Style” → activeTab = "results", activeModeByTab.results = "style".


Hover behavior (transient dropdown when tab inactive):
On mouse enter over Build/Results tabs when they are not the active tab:


hoveredTab = "build" | "results".


modeMenuVisibleForTab is set to the hovered tab while hovered.


On mouse leave from the Build/Results tab area:


If that tab is not active:


hoveredTab = null.


modeMenuVisibleForTab is cleared (dropdown closes).


If that tab is active:


hoveredTab may be cleared, but modeMenuVisibleForTab remains set so the menu stays pinned.


This yields:
Dropdown-like menu when hovering a non-active Build/Results tab.


Pinned, persistent mode controls when the tab is active.


#### 5.3 Responsive Logic
Observe viewport width and map to "desktop" | "tablet" | "mobile" using Knowledge-defined thresholds.


Expose booleans such as isMobile, isTablet, isDesktop to Build/BuildStyle and conditional rendering logic, including:


Whether dropdown behavior is enabled on that breakpoint.


Whether mode menus compress into a segmented control/pill group.


#### 5.4 Shell-Specific Workflows
openDoc(docId) and closeDoc() exist as global actions:


Defined at shell level, but in this pass they are not yet wired to a rendered modal.


Future docs configuration/shell will subscribe to these actions to show/hide the documentation UI.


Publish:


Clicking the Publish button calls onPublish (or a future pipeline) with the “current configuration” context.


This pass only defines the hook and state; full pipeline is deferred.



6. Knowledge Concern (Reference Data)
#### 6.1 Shell Metadata – Tabs
Canonical tab definitions stored in a Knowledge map:
- Keys: "build", "logic", "knowledge", "results".


Each entry includes:


label (“Build”, “Logic”, “Knowledge”, “Results”).


order (1–4, fixed).


docId for the info icon (future).


hoverSummary string for tooltip.


Tab order is fixed and not user-configurable: Build, Logic, Knowledge, Results.
Mode metadata for Build/Results:
Mode labels:


Build:


- Base mode label: “Build”.


- Style mode label: “Build /Style”.


Results:


- Base mode label: “Results”.


- Style mode label: “Results /Style”.


Descriptions:


- Build (Base): “Define and arrange containers, sub-containers, and atomic components.”


- Build /Style: “Configure layout-affecting style for Build outputs (sizing, alignment, grouping).”


- Results (Base): “Inspect and shape derived outputs, scores, and summaries.”


- Results /Style: “Configure layout-affecting style for Results outputs (cards, grouping, highlight rules).”


#### 6.2 Build/Results Mode Metadata
Mode metadata:
Build:


Base:


label: "Build".


description: "Define and arrange containers, sub-containers, and atomic components."


Style:


label: "Build /Style".


description: "Configure layout-affecting style for Build outputs (grouping, alignment, sizing)."


Results:


Base:


label: "Results".


description: "Design and inspect derived outputs, scores, and summaries."


Style:


label: "Results /Style".


description: "Configure layout-affecting style for Results outputs (cards, grouping, highlight rules)."


Optional:
Per-mode docId and hoverSummary for the mode items themselves.


#### 6.3 Breakpoints
desktopMin = 1024, tabletMin = 768, etc.


Explanation of how width maps to "desktop" | "tablet" | "mobile".


#### 6.4 Brand Content
wordmark = "Calculogic".


tagline = "a system for creating systems".


- Brand hover tooltip: “Return to Calculogic home / dashboard.”


Info icon text (examples):
- Build: “Owns and arranges structure: containers, sub-containers, and atomic components.”


- Logic: “Defines calculations, conditions, validation rules, and interactions.”


- Knowledge: “Stores reusable traits, constants, and reference schemas.”


- Results: “Derived outputs and summaries produced from structure + logic + knowledge.”



7. Results Concern (Outputs)
#### 7.1 Navigation State Output (optional, dev)
Optional debug output structure:
currentActiveTab: HeaderTabId.


currentBuildMode: HeaderModeId.


currentResultsMode: HeaderModeId.


modeMenuVisibleForTab: HeaderTabId | null.


viewportBreakpoint: "desktop" | "tablet" | "mobile".


May be surfaced as a small dev-only panel anchored inside the header shell or a separate debug overlay.
#### 7.2 Accessibility Announcements
Result data for ARIA live-region messages:
On tab change:


“Switched to Build.”


“Switched to Logic.”


“Switched to Knowledge.”


“Switched to Results.”


On mode change:


“Switched to Build Style mode.”


“Switched to Build Base mode.”


“Switched to Results Style mode.”


“Switched to Results Base mode.”


Header-level ARIA outputs are derived from activeTab and activeModeByTab.
#### 7.3 Docs Modal Semantics (outline only)
Resolved documentation payload for a given docId is modeled as Results data, e.g.:


interface ResolvedDoc {
docId: string;
title: string;
summary: string;
sections: { id: string; title: string; body: string }[];
links: { label: string; href: string }[];
}

The actual documentation modal layout belongs to a separate docs configuration/shell.


In this header shell:


Results only defines the shape of resolved docs (for later consumption).


No modal UI is rendered in this pass.



8. ResultsStyle Concern (Output Styling)
#### 8.1 Debug Overlay Styles
Styling for any header-level debug panels (if enabled):


Small text blocks showing current active tab, Build/Results modes, breakpoint.


Low-contrast background, non-intrusive, optionally toggleable.


#### 8.2 Docs Modal Styling (future)
Reserved for basic documentation modal styling when the docs configuration is introduced:
Backdrop:


Full-screen semi-transparent overlay.


Panel:


Centered or side-docked panel, rounded corners, padding.


Typography:


Title, section headings, body copy consistent with global docs style.


Layout:


Scrollable content area; close button area.


Integration:


Styled relative to this shell only via shared theme tokens; structure and actual rendering defined elsewhere.



9. Assembly Pattern
#### 9.1 File Structure (implementation pattern)
/src/components/GlobalHeaderShell/
GlobalHeaderShell.build.tsx
GlobalHeaderShell.build.css
GlobalHeaderShell.logic.ts
GlobalHeaderShell.knowledge.ts
GlobalHeaderShell.results.tsx
GlobalHeaderShell.results.css
index.tsx

#### 9.2 Assembly Logic
GlobalHeaderShell.logic.ts


Exports hooks/utilities for:


activeTab, activeModeByTab, hoveredTab, modeMenuVisibleForTab, viewportBreakpoint.


Navigation handlers for tab clicks, mode menu clicks, hover in/out.


GlobalHeaderShell.build.tsx


Renders:


Left/center/right zones.


Tab List Row.


Build/Results Mode Menus (connected to logic state).


Brand and publish primitives.


GlobalHeaderShell.knowledge.ts


Central tab metadata, mode metadata, breakpoint constants, brand strings, and tooltips.


GlobalHeaderShell.results.tsx / .results.css


Optional debug output rendering (state panel).


Future ARIA announcements and docs payload mapping.


index.tsx


Composes all concerns into a GlobalHeaderShell component.


#### 9.3 Integration
The shell wraps all builder views and configurations.


Parent-level code provides:


onPublish.


Router/context providers.


Shell manages header-specific state and exposes it via:


Context, hooks, or props as needed for child components.



10. Implementation Passes
#### 10.1 Pass Mapping
## Pass 0 – Global Header Frame
Build:


Root container for header shell with three zones (left/center/right).


Tab List Row structure (placeholders only for tab labels).


BuildStyle:


Basic header bar styling, zones alignment, baseline spacing.


Logic:


Define initial state shape:


activeTab, activeModeByTab, hoveredTab, modeMenuVisibleForTab, viewportBreakpoint.


Knowledge:


Tab metadata (labels, order).


Breakpoint constants.



## Pass 1 – Left: Calculogic Brand Block
Build:


Place logo, wordmark, optional tagline primitives in left zone.


BuildStyle:


Responsive rules for brand spacing, tagline visibility.


Logic:


Brand click → navigate home/dashboard (standard link behavior).


Knowledge:


Brand strings and tooltip copy.



## Pass 2 – Center: Build Tab + Build Mode Menu
Build:


Add Build tab button + info icon in Tab List Row.


Add Build Tab – Mode Menu subcontainer with:


“Build Mode Item – Base”


“Build Mode Item – Style”


BuildStyle:


Styling for:


Active/inactive Build tab.


Build mode menu in dropdown form (inactive hover) and pinned/inline form (when Build active).


Logic:


Implement:


Tab click (Build) → activeTab = "build" (and default mode).


Mode item click:


Base vs Style for Build (update activeModeByTab.build).


Hover enter/leave behavior for Build tab:


Inactive → transient dropdown.


Active → pinned menu.


Knowledge:


Tooltip summary for Build tab.


Descriptive strings for Build Base vs Build /Style.



## Pass 3 – Center: Logic Tab
Build:


Logic tab button + info icon in Tab List Row.


BuildStyle:


Active/inactive styling; hover states.


Logic:


Tab click → activeTab = "logic".


Clear/adjust modeMenuVisibleForTab as necessary when leaving Build/Results.


Knowledge:


Tooltip and description for Logic.



## Pass 4 – Center: Knowledge Tab
Build:


Knowledge tab button + info icon in Tab List Row.


BuildStyle:


Active/inactive styling; hover states.


Logic:


Tab click → activeTab = "knowledge".


Clear/adjust modeMenuVisibleForTab as necessary.


Knowledge:


Tooltip and description for Knowledge.



## Pass 5 – Center: Results Tab + Results Mode Menu
Build:


Add Results tab button + info icon in Tab List Row.


Add Results Tab – Mode Menu subcontainer with:


“Results Mode Item – Base”


“Results Mode Item – Style”


BuildStyle:


Styling for:


Active/inactive Results tab.


Results mode menu as dropdown (inactive hover) and pinned inline (active).


Logic:


Tab click (Results) → activeTab = "results" (default mode).


Mode item click:


Base vs Style for Results (update activeModeByTab.results).


Hover enter/leave behavior for Results tab (same pattern as Build).


Knowledge:


Tooltip summary for Results tab.


Descriptive strings for Results Base vs Results /Style.



## Pass 6 – Right: Publish Area
Build:


Publish button primitive in right zone.


BuildStyle:


Responsive variations (full label vs compact/icon-first).


Logic:


Publish button calls onPublish hook with current config/project context.


Knowledge:


Tooltip/label text for publish behavior.



## Pass 7 – Docs Modal (outline only)
Logic:


Define openDoc(docId) and closeDoc() actions on the header shell.


Wire header info icons to call openDoc(docId) (no visible modal yet).


Knowledge:


Associate docId values with each header info icon (Build/Logic/Knowledge/Results, publish, and brand if desired).


Short summaries for tooltips.


Results / ResultsStyle:


Reserve the semantic mapping for docs payload (e.g., ResolvedDoc).


Reserve styling hooks for future docs modal.



#### 10.2 Export Checklist
- Tabs appear in fixed order: Build, Logic, Knowledge, Results.


Build/Style and Results/Style are available only via mode menus attached to their respective tabs (not separate tabs).


When Build/Results are inactive:


Hovering them shows a transient dropdown with Base and /Style options.


When Build/Results are active:


Their mode menus remain visible (pinned) and allow switching between Base and /Style.


Tab strip remains a single row at all breakpoints; mobile enables horizontal scrolling for overflow.


Brand click navigates home with standard link semantics.


Publish button:


Present and clearly tappable at all breakpoints.


Calls onPublish when clicked.


Hover and active states:


All tabs and mode items visually communicate focus/hover/active/disabled.


Accessibility:


ARIA live messages reflect tab and mode changes (including /Style changes).


Docs:


openDoc / closeDoc actions are defined and header info icons call openDoc(docId); actual modal UI is deferred to a separate docs configuration/shell.


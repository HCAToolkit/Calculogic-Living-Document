# Linking, Hierarchy, and Conflict Resolution
1. Principle of Manifestation Linking
Certain atomic properties in one concern naturally manifest as linked artifacts in other concerns.
 This maintains synchronization between Build, Logic (Workflow), Knowledge, and Results, ensuring deterministic relationships across all six tabs.
Conceptual Mapping:
Source Tab
Auto-Manifests As
Example
Build
Logic (Workflow) rule
required: true → creates “required validation” rule
Knowledge
Logic (Workflow) validator
trait.email.format = "email" → email format validator
Logic (Workflow)
Build property update
deleting a rule removes its related required property

Flow Diagram:
graph LR
A[Build Tab] -- required=true --> B[Auto-Create Required Rule]
B --> C[Logic (Workflow) Tab]
D[Logic (Workflow) Tab] -- Delete Rule --> E[Build Tab required=false]

Auto-manifested rules are always visible in the Logic (Workflow) tab and tagged as “auto-linked.”
Manual rules take precedence but do not delete auto-linked ones unless explicitly removed by the user.
Principle:
 Each manifestation link represents a bi-directional state reflection —
 the Build defines potential, Logic (Workflow) defines behavior, and both remain synchronized through the engine's internal manifest registry.

2. Link Management Rules
Case
System Behavior
User Experience
Auto-Creation
Enabling a property in Build or Knowledge silently generates the linked rule or reference in the target tab.
The linked element appears with a 🔗 icon and metadata noting its origin.
Manual Creation
User manually adds logic, style, or result entries.
Appears with a ⛓️ icon and “Unlinked” indicator until explicitly connected.
Duplicate Attempt
Adding a second instance of the same linkage triggers a modal.



⚠ "Component already has [rule]"
▸ Replace existing
▸ Add as exception
▸ Cancel
``` |
| **Deletion** | Removing a linked rule or style disables its source property. | Source checkbox toggles off automatically, preventing stale links. |

Conflict Priority:
When conflicting edits occur across tabs, the Build tab is authoritative for structure.
Logic (Workflow) and View tabs may override functional or visual behavior only when explicitly confirmed by the user.
If no Build data is present (e.g., for logic-only exports or external integrations), the Logic (Workflow) tab becomes the authoritative structural source, ensuring exportable logic and knowledge consistency.

This maintains **bidirectional integrity** while allowing **manual override** (an Interface §11 principle: "Silent Automation + Manual Escape Hatch").

---

## **3. Container Inheritance Hierarchy**

All structure within a Configuration follows a predictable **hierarchical model** that mirrors export order and logical containment.

**Diagram:**
```mermaid
graph TD
Root[Form Root Container]
Root --> ConfigA[Configuration A]
Root --> ConfigB[Configuration B]
ConfigA --> SubA[Sub-Container A]
ConfigA --> SubB[Sub-Container B]
SubA --> Atomic1[Text Input]
SubB --> Atomic2[Slider]

Behavioral Rules
Automatic Wrapping:
 A lone atomic dropped directly onto the root canvas auto-wraps into a generated sub-container.
 (Stored as "autoWrapped": true in Build JSON.)
Rule Inheritance:
Container-level rules apply to all child components.
Component-level definitions override inherited rules.
The UI visually marks inherited rules with an inherited-rule class.
JSON Representation:
{
"id": "container_personalInfo",
"inheritRules": true,
"children": [
{ "id": "nameInput", "overrides": ["required"] }
]
}


4. Conflict Resolution System
Every Project stores preferences controlling how rule or link conflicts are handled.
User Preferences Schema:
{
"ruleConflictResolution": "ask",      // [ask | auto-replace | allow-duplicates]
"containerAutoWrap": true,
"defaultRuleScope": "component"       // [component | sub-container | configuration]
}

Exception Handling Flow:
graph TB
A[Conflict Detected] --> B{User Preference}
B -->|ask| C[Show Resolution Modal]
B -->|auto-replace| D[Replace Existing]
B -->|allow-duplicates| E[Add With Warning]
C --> F[User Chooses Action]

Modal Example:
⚠ Duplicate Validation Rule
----------------------------------
Field "UserEmail" already has:
• Required Rule (added automatically)

[Replace Existing] [Add Anyway] [Cancel]
[ ] Remember this choice for all validations


5. Dual-Canvas Styling (Results-Aware View System)
Canvas: Build Canvas → input phase; Results Canvas → output phase.
Both follow the same structural scope rules but apply separate contextual style layers.
Canvas
Purpose
Example Rule
Build Canvas
Active form/quiz during input
[data-scope="build"] .text-input { width: 100% }
Results Canvas
Post-submission / analysis display
[data-scope="results"] .text-input { width: 80% }

View Rule Declaration Example:
{
"type": "width",
"target": "text-email",
"value": "100%",
"scope": "build"     // or "results"
}


6. Documentation Integration Points
To keep Calculogic's living documentation synchronized:
Section
Addition
§3.4 – Why This Works Without Raw Code
Add “Auto-Manifested Properties Across Tabs” — explaining Build/Logic (Workflow) reflection.
§5 – Example: Enneagram Test
Add note: “Required fields auto-generate validation rules via Manifestation Linking.”
§8 – Developer Documentation
Extend “Configuration Behavior” to describe rule inheritance hierarchy and override logic.
§11 – UI/UX Principles
Add new principle: “Silent Automation with Manual Escape Hatches.” — automate mappings, allow overrides, visually mark automation state.


7. Visual Design & Indicators
Status
Icon
Meaning
Auto-Linked
🔗
Created automatically by Manifestation Linking
Manually Linked
⛓️
Created by user and connected manually
Conflict Detected
⚠️
Duplicate or invalid rule; pending resolution

Container Hierarchy Viewer Example (plaintext):
[Form Root]
├─ [Configuration: Contact Form] (inherited-rules)
│  ├─ [Sub-Container: Personal Info]
│  │  ├─ [Text Input: Name] (overridden-rule)
│  │  └─ [Email Input]
│  └─ [Sub-Container: Preferences]
└─ [Configuration: Feedback]



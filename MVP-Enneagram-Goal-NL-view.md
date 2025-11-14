# MVP Enneagram Goal: NL view
Configuration 1: cfg-intro (Build-owned)
Meta
id: cfg-intro


type: Container


name: Intro Section


description: Intro title + explanation of the Enneagram test


Build (owns)
Root: IntroRoot (section.intro)


Atoms:


Title: H1 – “Welcome to my Holistic Character Assessment Toolkit: Enneagram!”


Heading: H2 – “Introduction:”


BodyText: P – intro paragraph


BuildStyle
Uses:


body base/reset


.container card


h1 centered


.intro p


Workflow
empty (no dynamic behavior)


Knowledge
References:


knowledge_introCopy (optional)


Results
empty


ResultsStyle
empty



Configuration 2: cfg-q-motivations (Build-owned)
Meta
id: cfg-q-motivations


type: Container


name: Question 1 – Motivations


Build (owns)
Root: Q1Root (section.question-container.motivations)


Prompt atom:


H2 – motivations question text


Sub-containers M1–M9:


each has:


Checkbox (name="motivations")


RatingDropdown (1–5)


Label (motivation sentence)


BuildStyle
Uses:


form standard


.question-container, .question


#enneagram-form p


input[type="checkbox"]


.rating-scale


label block


global button style


Workflow
State:


maxSelections = 4


selectedCount['Motivations'] = 0


Rules:


on checkbox change (group Motivations):


increment/decrement selectedCount['Motivations']


when checked, set its rating to 1


enforce max 4 (revert + alert)


log check / uncheck


on rating change (group Motivations):


if checkbox is checked, log select


use shared blueWords highlighter for Label text


Knowledge
Uses:


knowledge_questionsAndCores.Q1


knowledge_motivations.options


Results
empty


ResultsStyle
empty
Configurations 3–6: cfg-q-fears, cfg-q-desires, cfg-q-weaknesses, cfg-q-strengths (Build-owned)
Each follows the same structure as cfg-q-motivations, with only group/type specifics changed.

Configuration 3: cfg-q-fears
Meta
id: cfg-q-fears


type: Container


name: Question 2 – Fears


Build
Root: Q2Root (section.question-container.fears)


Prompt: H2 – fears question text


Sub-containers F1–F9:


checkbox, rating dropdown, label (fear sentences)


BuildStyle
Same as cfg-q-motivations (question styles)


Workflow
State:


maxSelections = 4


selectedCount['Fears'] = 0


Rules:


same pattern, but group "Fears"


Knowledge
knowledge_questionsAndCores.Q2


knowledge_fears.options


Results / ResultsStyle
empty



Configuration 4: cfg-q-desires
Same pattern, with:


Root: Q3Root (.desires)


group "Desires"


knowledge_questionsAndCores.Q3


knowledge_desires.options



Configuration 5: cfg-q-weaknesses
Same pattern, with:


Root: Q4Root (.weaknesses)


group "Weaknesses"


knowledge_questionsAndCores.Q4


knowledge_weaknesses.options



Configuration 6: cfg-q-strengths
Same pattern, with:


Root: Q5Root (.strengths)


group "Strengths"


knowledge_questionsAndCores.Q5


knowledge_strengths.options




Configuration 7: cfg-calculateButton (Build-owned)
Meta
id: cfg-calculateButton


type: Container


name: Calculate Button


description: Triggers evaluation and navigates to the Results page


Build (owns)
Root: CalculateRoot (section.actions)


Atoms:


Button – id=calculate-button, label="Calculate"


BuildStyle
Uses:


button.primary


Workflow
Rule:


On click 'calculate-button':


ratings = collectRatingsFrom([ cfg-q-motivations, cfg-q-fears, cfg-q-desires, cfg-q-weaknesses, cfg-q-strengths ])


engineOutput = Workflow.cfg-enneagramEngine.evaluate(ratings)


store(engineOutput, key='enneagramResults')


navigateTo(page_2)


Knowledge
empty


Results
empty


ResultsStyle
empty


Now: Workflow-owned configurations (logic only)
These come after the Build-owned ones.
Configuration 8: cfg-enneagramEngine (Workflow-owned)
Meta
id: cfg-enneagramEngine


type: Container


name: Enneagram Engine


Build
empty


BuildStyle
empty


Workflow (owns)
Constants:


categories = ['M','F','D','W','S']


categoryNames = { M:'Motivations', F:'Fears', … }


wingTypeAssociations = [...]


Functions:


isDirectWing(typeA, typeB)


areDirectWings(types[])


calculateDistance(a, b)


evaluate(ratings) → { enneagramTypeRatings, categoryBreakdowns, sortedJungianFunctions }


Knowledge
References:


knowledge_motivations / fears / desires / weaknesses / strengths


knowledge_enneagramTypes


knowledge_jungianFunctions


Results
empty


ResultsStyle
empty
Configuration 9: cfg-collapsibleBehavior (Workflow-owned, optional)
Meta
id: cfg-collapsibleBehavior


type: Container


name: Collapsible Toggles


description: Shared behavior for collapsible sections in results


Build
empty


BuildStyle
empty


Workflow (owns)
Rule:


On click .collapsible-toggle:


content = nextSibling('.collapsible-content')


toggle content.display between 'none' and 'block'


Knowledge
empty


Results
empty


ResultsStyle
empty



Configuration 10: knowledge_questionsAndCores (Knowledge-owned)
Meta
id: knowledge_questionsAndCores


type: Container


name: Questions and Cores


description: Q1–Q5 prompts for motivations, fears, desires, weaknesses, strengths


Build
empty


BuildStyle
empty


Workflow
empty


Knowledge (owns)
Sequence:


Q1: “Does your character relate to any of these core motivations? …”


Q2: “Does your character relate to any of these core fears? …”


Q3: “Does your character relate to any of these core desires? …”


Q4: “Does your character relate to any of these core weaknesses? …”


Q5: “Does your character relate to any of these core strengths? …”


Results
empty


ResultsStyle
empty



Configurations 11–17 (Knowledge-owned)
All follow the same meta-pattern: only Knowledge.sequence is populated.
Configuration 11: knowledge_motivations
Knowledge: M1–M9 motivation options (description, enneagram type + name, Jungian function + name)


Configuration 12: knowledge_fears
Knowledge: F1–F9 fear options


Configuration 13: knowledge_desires
Knowledge: D1–D9 desire options


Configuration 14: knowledge_weaknesses
Knowledge: W1–W9 weakness options


Configuration 15: knowledge_strengths
Knowledge: S1–S9 strength options


Configuration 16: knowledge_enneagramTypes
Knowledge: types 1–9 with:


name


description


core motivation/fear/desire/weakness/strength


Jungian function list


Configuration 17: knowledge_jungianFunctions + knowledge_introCopy
knowledge_jungianFunctions: function keys (Te, Ti, Fe, Fi, Se, Si, Ne, Ni) with name + description


knowledge_introCopy: intro title/paragraph (if you want intro text centralized)


All seven:
Build: empty


BuildStyle: empty


Workflow: empty


Knowledge: owns data


Results: empty


ResultsStyle: empty



Configuration 18: cfg-results_header (Results-owned)
Meta
id: cfg-results_header


type: Container


name: Results Header


description: Top-level heading for the Results page


Build
empty


BuildStyle
empty


Workflow
empty


Knowledge
empty


Results (owns)
Root: ResultsHeader (section.results-header)


Atoms:


H1: “Results”


ResultsStyle
Uses:


resultsHeader.h1


#resultsContainer.base (if shared)

Configuration 19: cfg-results_enneagramSummary (Results-owned)
Meta
id: cfg-results_enneagramSummary


type: Container


name: Enneagram Type Breakdown


Build
empty


BuildStyle
empty


Workflow
empty (binding is treated as part of Results wiring)


Knowledge
References:


knowledge_enneagramTypes


Results (owns)
Root: EnneagramSummaryRoot


Atoms:


H2: “Enneagram Type Breakdown”


Sub-containers:


DominantType: binds enneagramTypeRatings[0]


AuxType: binds [1]


TertType: binds [2]


InferiorType: binds [3]


ResultsStyle
Uses:


#result block


collapsible/details styles


Configuration 20: cfg-results_deepBreakdown (Results-owned)
Meta
id: cfg-results_deepBreakdown


type: Container


name: Deep Breakdown & Functions


description: Category-level Dominant/Aux/Tert/Inferior plus Jungian functions overview


Build
empty


BuildStyle
empty


Workflow
empty


Knowledge
References:


knowledge_motivations


knowledge_fears


knowledge_desires


knowledge_weaknesses


knowledge_strengths


knowledge_jungianFunctions


Results (owns)
Root: DeepBreakdownRoot (section#results-deep)


Blocks:


Category_Motivations – binds engineOutput.categoryBreakdowns['M']


Category_Fears – binds ['F']


Category_Desires – binds ['D']


Category_Weaknesses – binds ['W']


Category_Strengths – binds ['S']


JungianFunctionsOverview – binds engineOutput.sortedJungianFunctions


ResultsStyle
Uses:


#resultsContainer


.category h2


.details


details > summary


.collapsible


.collapsible-content


.content p



That’s everything you listed, reorganized into a pure prose Project-JSON:
Build-owned configs (1–7)


then Workflow-owned (8–9)


then Knowledge-owned (10–17)


then Results-owned (18–20)


Each config has all six concerns present in a fixed order, with only the owning concern(s) populated.


# MVP Enneagram Goal: JSON view
Concern key order: Build → BuildStyle → Workflow → Knowledge → Results → ResultsStyle.
I’ll show representative configs fully and indicate “same pattern” where it repeats.

Project: Enneagram Test (Conceptual Project JSON in Prose)

Configuration 1 – cfg-intro (Build-owned)
{
  "id": "cfg-intro",
  "type": "Container",
  "meta": {
    "name": "Intro Section",
    "author": "System",
    "visibility": "public",
    "description": "Intro title + explanation of the Enneagram test"
  },

  "Build": {
    "sequence": [
      "IntroRoot(section.intro)",
      "Title(H1: 'Welcome to my Holistic Character Assessment Toolkit: Enneagram!')",
      "Heading(H2: 'Introduction:')",
      "BodyText(P: 'The enneagram test helps you understand your character’s core motivations, fears…')"
    ]
  },

  "BuildStyle": {
    "sequence": [
      "use-style(body.base-reset)",
      "use-style(container.card)",
      "use-style(h1.centered)",
      "use-style(intro.bodyText)"
    ]
  },

  "Workflow": {
    "sequence": [
      // no dynamic behavior for intro
    ]
  },

  "Knowledge": {
    "sequence": [
      "ref(knowledge_introCopy)"   // optional: intro text lives in Knowledge too
    ]
  },

  "Results": {
    "sequence": []
  },

  "ResultsStyle": {
    "sequence": []
  }
}


Configuration 2 – cfg-q-motivations (Build-owned)
{
  "id": "cfg-q-motivations",
  "type": "Container",
  "meta": {
    "name": "Question 1 - Motivations",
    "author": "System",
    "visibility": "public",
    "description": "Motivations question: 9 options with checkbox + rating"
  },

  "Build": {
    "sequence": [
      "Q1Root(section.question-container.motivations)",
      "Prompt(H2: 'Does your character relate to any of these core motivations? Choose only 3 unless you can’t narrow it down.')",

      // Sub-container per option
      "SubContainer(M1, 'option-row', atoms: [
         Checkbox(name='motivations'),
         RatingDropdown(1..5),
         Label('Motivated to do what is right and just')
       ])",

      "SubContainer(M2, 'option-row', atoms: [
         Checkbox(name='motivations'),
         RatingDropdown(1..5),
         Label('Motivated to be loved and needed')
       ])",

      // … same pattern for M3..M9 …

      "SubContainer(M9, 'option-row', atoms: [
         Checkbox(name='motivations'),
         RatingDropdown(1..5),
         Label('Motivated to maintain inner peace and harmony')
       ])"
    ]
  },

  "BuildStyle": {
    "sequence": [
      "use-style(form.standard)",
      "use-style(.question-container)",
      "use-style(.question)",
      "use-style(#enneagram-form p)",
      "use-style(input[type='checkbox'])",
      "use-style(.rating-scale)",
      "use-style(label.block)",
      "use-style(button.primary)"  // shared button style on page 1
    ]
  },

  "Workflow": {
    "sequence": [
      "state(maxSelections = 4)",
      "state(selectedCount['Motivations'] = 0)",

      "rule(onCheckboxChange for group='Motivations': {
         if checked:
           selectedCount['Motivations']++,
           setAssociatedRating(1),
           logEvent('check', optionKey, 'Motivations')
         else:
           selectedCount['Motivations']--,
           logEvent('uncheck', optionKey, 'Motivations')

         if selectedCount['Motivations'] > maxSelections:
           showAlert('You can select only 4 options in this group.'),
           uncheck(),
           selectedCount['Motivations']--
       })",

      "rule(onRatingChange for group='Motivations': {
         if associatedCheckboxIsChecked:
           logEvent('select', optionKey, 'Motivations', ratingValue)
       })",

      "rule(renderLabelWithBlueWords: uses Workflow.blueWords to color specific tokens)"
    ]
  },

  "Knowledge": {
    "sequence": [
      "ref(knowledge_questionsAndCores.Q1Prompt)",
      "ref(knowledge_motivations.options)"  // M1..M9 data
    ]
  },

  "Results": {
    "sequence": []
  },

  "ResultsStyle": {
    "sequence": []
  }
}


Configurations 3–6 – cfg-q-fears, cfg-q-desires, cfg-q-weaknesses, cfg-q-strengths (Build-owned)
Each looks like cfg-q-motivations with:
Different prompt text


Different Knowledge reference (knowledge_fears, knowledge_desires, etc.)


selectedCount['Fears' | 'Desires' | 'Weaknesses' | 'Strengths']


same BuildStyle + Workflow pattern.


I’ll show one compactly; the others are “same pattern”.
{
  "id": "cfg-q-fears",
  "type": "Container",
  "meta": {
    "name": "Question 2 - Fears",
    "author": "System",
    "visibility": "public",
    "description": "Fears question: 9 options with checkbox + rating"
  },

  "Build":    { "sequence": [ /* Q2Root + 9 fear option-rows */ ] },
  "BuildStyle":{ "sequence": [ same styles as cfg-q-motivations ] },

  "Workflow": {
    "sequence": [
      "state(maxSelections = 4)",
      "state(selectedCount['Fears'] = 0)",
      "rule(onCheckboxChange group='Fears' …)",
      "rule(onRatingChange group='Fears' …)",
      "rule(renderLabelWithBlueWords shared)"
    ]
  },

  "Knowledge": {
    "sequence": [
      "ref(knowledge_questionsAndCores.Q2Prompt)",
      "ref(knowledge_fears.options)"
    ]
  },

  "Results":     { "sequence": [] },
  "ResultsStyle": { "sequence": [] }
}

cfg-q-desires, cfg-q-weaknesses, cfg-q-strengths follow the same meta-pattern.

Configuration 7 – cfg-calculateButton (Build-owned)
{
  "id": "cfg-calculateButton",
  "type": "Container",
  "meta": {
    "name": "Calculate Button",
    "author": "System",
    "visibility": "public",
    "description": "Triggers evaluation and navigates to the Results page"
  },

  "Build": {
    "sequence": [
      "CalculateRoot(section.actions)",
      "Button(id='calculate-button', label='Calculate')"
    ]
  },

  "BuildStyle": {
    "sequence": [
      "use-style(button.primary)"
    ]
  },

  "Workflow": {
    "sequence": [
      "rule(onClick 'calculate-button': {
         ratings = collectRatingsFrom([
           cfg-q-motivations,
           cfg-q-fears,
           cfg-q-desires,
           cfg-q-weaknesses,
           cfg-q-strengths
         ]),
         engineOutput = call(Workflow.cfg-enneagramEngine.evaluate, ratings),
         store(engineOutput, key='enneagramResults'),
         navigateTo(page_2)
       })"
    ]
  },

  "Knowledge": {
    "sequence": []
  },

  "Results":     { "sequence": [] },
  "ResultsStyle": { "sequence": [] }
}


Now: Workflow-owned configurations (logic only)
These come after the Build-owned ones.

Configuration 8 – cfg-enneagramEngine (Workflow-owned)
{
  "id": "cfg-enneagramEngine",
  "type": "Container",
  "meta": {
    "name": "Enneagram Engine",
    "author": "System",
    "visibility": "internal",
    "description": "Pure computation: scoring, sorting, wing logic"
  },

  "Build":     { "sequence": [] },
  "BuildStyle": { "sequence": [] },

  "Workflow": {
    "sequence": [
      "state(categories = ['M','F','D','W','S'])",
      "state(categoryNames = { M:'Motivations', F:'Fears', D:'Desires', W:'Weaknesses', S:'Strengths' })",

      "constant(wingTypeAssociations = [...])",

      "fn(isDirectWing(typeA, typeB))",
      "fn(areDirectWings(types[]))",
      "fn(calculateDistance(a, b))",

      "fn(evaluate(ratings): {
         // accumulate enneagramType(...) and jungianFunction(...),
         // compute per-category sorted lists,
         // compute enneagramTypeRatings sorted,
         // compute sortedJungianFunctions,
         // return { categoryBreakdowns, enneagramTypeRatings, sortedJungianFunctions }
       })"
    ]
  },

  "Knowledge": {
    "sequence": [
      "ref(knowledge_motivations)",
      "ref(knowledge_fears)",
      "ref(knowledge_desires)",
      "ref(knowledge_weaknesses)",
      "ref(knowledge_strengths)",
      "ref(knowledge_enneagramTypes)",
      "ref(knowledge_jungianFunctions)"
    ]
  },

  "Results":     { "sequence": [] },
  "ResultsStyle": { "sequence": [] }
}


Configuration 9 – cfg-collapsibleBehavior (Workflow-owned, optional)
{
  "id": "cfg-collapsibleBehavior",
  "type": "Container",
  "meta": {
    "name": "Collapsible Toggles",
    "author": "System",
    "visibility": "public",
    "description": "Shared behavior for collapsible sections in results"
  },

  "Build":     { "sequence": [] },
  "BuildStyle": { "sequence": [] },

  "Workflow": {
    "sequence": [
      "rule(onClick .collapsible-toggle: {
         content = nextSibling('.collapsible-content'),
         toggle(content.display between 'none' and 'block')
       })"
    ]
  },

  "Knowledge":  { "sequence": [] },
  "Results":    { "sequence": [] },
  "ResultsStyle":{ "sequence": [] }
}


Knowledge-owned configurations
After Workflow-owned configs.
(Only Knowledge has content; everything else empty.)

Configuration 10 – knowledge_questionsAndCores (Knowledge-owned)
{
  "id": "knowledge_questionsAndCores",
  "type": "Container",
  "meta": {
    "name": "Questions and Cores",
    "author": "System",
    "visibility": "internal",
    "description": "Q1–Q5 prompts for motivations, fears, desires, weaknesses, strengths"
  },

  "Build":     { "sequence": [] },
  "BuildStyle": { "sequence": [] },
  "Workflow":  { "sequence": [] },

  "Knowledge": {
    "sequence": [
      "Q1: 'Does your character relate to any of these core motivations? …'",
      "Q2: 'Does your character relate to any of these core fears? …'",
      "Q3: 'Does your character relate to any of these core desires? …'",
      "Q4: 'Does your character relate to any of these core weaknesses? …'",
      "Q5: 'Does your character relate to any of these core strengths? …'"
    ]
  },

  "Results":     { "sequence": [] },
  "ResultsStyle": { "sequence": [] }
}


Configuration 11 – knowledge_motivations (Knowledge-owned)
{
  "id": "knowledge_motivations",
  "type": "Container",
  "meta": {
    "name": "Motivations Options",
    "author": "System",
    "visibility": "internal",
    "description": "M1–M9 motivations mapping to Enneagram types and functions"
  },

  "Build":     { "sequence": [] },
  "BuildStyle": { "sequence": [] },
  "Workflow":  { "sequence": [] },

  "Knowledge": {
    "sequence": [
      "M1: { description: 'Motivated to do what is right and just', enneagramType:'1', … }",
      "M2: { description: 'Motivated to be loved and needed', enneagramType:'2', … }",
      // … M3..M9 …
    ]
  },

  "Results":     { "sequence": [] },
  "ResultsStyle": { "sequence": [] }
}

knowledge_fears, knowledge_desires, knowledge_weaknesses,
 knowledge_strengths, knowledge_enneagramTypes, knowledge_jungianFunctions, knowledge_introCopy all follow the same meta-pattern:
 only Knowledge.sequence is populated.

Results-owned configurations
These come last in the project.

Configuration 18 – cfg-results_header (Results-owned)
{
  "id": "cfg-results_header",
  "type": "Container",
  "meta": {
    "name": "Results Header",
    "author": "System",
    "visibility": "public",
    "description": "Top-level heading for the Results page"
  },

  "Build":      { "sequence": [] },
  "BuildStyle":  { "sequence": [] },
  "Workflow":   { "sequence": [] },
  "Knowledge":  { "sequence": [] },

  "Results": {
    "sequence": [
      "ResultsHeader(section.results-header)",
      "H1('Results')"
    ]
  },

  "ResultsStyle": {
    "sequence": [
      "use-style(resultsHeader.h1)",
      "use-style(#resultsContainer.base)"   // if shared
    ]
  }
}


Configuration 19 – cfg-results_enneagramSummary (Results-owned)
{
  "id": "cfg-results_enneagramSummary",
  "type": "Container",
  "meta": {
    "name": "Enneagram Type Breakdown",
    "author": "System",
    "visibility": "public",
    "description": "Dominant/Aux/Tertiary/Inferior Enneagram types summary"
  },

  "Build":      { "sequence": [] },
  "BuildStyle":  { "sequence": [] },
  "Workflow":   { "sequence": [] },

  "Knowledge": {
    "sequence": [
      "ref(knowledge_enneagramTypes)"
    ]
  },

  "Results": {
    "sequence": [
      "EnneagramSummaryRoot(section#enneagram-summary)",
      "H2('Enneagram Type Breakdown')",

      "DominantType(article, binds engineOutput.enneagramTypeRatings[0])",
      "AuxType(article, binds engineOutput.enneagramTypeRatings[1])",
      "TertType(article, binds engineOutput.enneagramTypeRatings[2])",
      "InferiorType(article, binds engineOutput.enneagramTypeRatings[3])"
    ]
  },

  "ResultsStyle": {
    "sequence": [
      "use-style(#result)",
      "use-style(details.summaryBlocks)",
      "use-style(collapsible.blocks)"
    ]
  }
}


Configuration 20 – cfg-results_deepBreakdown (Results-owned)
{
  "id": "cfg-results_deepBreakdown",
  "type": "Container",
  "meta": {
    "name": "Deep Breakdown & Functions",
    "author": "System",
    "visibility": "public",
    "description": "Category-level Dominant/Aux/Tert/Inferior plus Jungian functions overview"
  },

  "Build":      { "sequence": [] },
  "BuildStyle":  { "sequence": [] },
  "Workflow":   { "sequence": [] },

  "Knowledge": {
    "sequence": [
      "ref(knowledge_motivations)",
      "ref(knowledge_fears)",
      "ref(knowledge_desires)",
      "ref(knowledge_weaknesses)",
      "ref(knowledge_strengths)",
      "ref(knowledge_jungianFunctions)"
    ]
  },

  "Results": {
    "sequence": [
      "DeepBreakdownRoot(section#results-deep)",

      "Category_Motivations(block, binds engineOutput.categoryBreakdowns['M'])",
      "Category_Fears(block, binds engineOutput.categoryBreakdowns['F'])",
      "Category_Desires(block, binds engineOutput.categoryBreakdowns['D'])",
      "Category_Weaknesses(block, binds engineOutput.categoryBreakdowns['W'])",
      "Category_Strengths(block, binds engineOutput.categoryBreakdowns['S'])",

      "JungianFunctionsOverview(block, binds engineOutput.sortedJungianFunctions)"
    ]
  },

  "ResultsStyle": {
    "sequence": [
      "use-style(#resultsContainer)",
      "use-style(.category h2)",
      "use-style(.details)",
      "use-style(details > summary)",
      "use-style(.collapsible)",
      "use-style(.collapsible-content)",
      "use-style(.content p)"
    ]
  }
That’s the Enneagram Test project laid out in your Project JSON meta-pattern:
All Build-owned configs first (with Build + BuildStyle + their Workflow/Knowledge hooks).


Then Workflow-owned engine/behavior configs.


Then Knowledge-owned data configs.


Then Results-owned configs with Results + ResultsStyle filled, other concerns empty.




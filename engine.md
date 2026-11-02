# The Engine — How to build and use "The Classroom"

**FOR THE AI ONLY:** This file is not for the human to read. It contains everything "The Professor" needs to prepare dynamic explanations and exercise sets on a real screen. The user does not need to install, configure, or understand anything here. Keep it in your project folder.

You are "The Professor", and this is your engine. It tells you how to build and manage "The Classroom": a simple web interface that lives ONLY on the user's computer and renders the explanations and exercises you prepare. Read this entirely the first time you build the classroom, and consult it whenever you create a new piece or block.

> **Scope of this file:** This edition documents **Plan C** (no computer access — claude.ai artifacts, Gemini Web, ChatGPT, etc.) in full. If you are running in **Claude Code** or another environment that can execute commands on the user's machine, **Plan A / Plan B** are not specified here; obtain those setup and session-cycle instructions from the original `motor.md` (Spanish) or from a separate Plan A/B document before proceeding. Do not improvise a local server setup from this file.

> **Schema language:** all JSON keys, enum values, folder names, and record fields below are **in English**. If you are migrating from the older Spanish schema (`contenido/`, `respuestas/`, `tema`, `pieza`, `respuesta`, `acierto`…), the mapping is one-to-one and mechanical: see §4. The user's tracking files (`aprendizajes.md`, `fallos.md`, `repasos.md`) keep their Spanish names because they pre-date this schema and are the user's own data.

## 1. The Principles (Non-negotiable)

1. **The screen is a capability, not the product.** Conversation comes first. The classroom activates when a visual explanation or exercise set teaches better than a paragraph, or when the user requests it.

2. **The user never touches the terminal.** Checking for Node, installing, creating the project, starting it, opening the browser, fixing errors, and shutting it down: you do all of this. If something fails, you resolve it. Only ask the user if you absolutely need something only they can provide (e.g., permission to install). *(In Plan C there is no terminal to touch — this principle still applies to any tool-assisted setup you ever perform.)*

3. **Content is DATA.** Each explanation and exercise set is a JSON file with a list of blocks. Each block has a `code` (what type of block it is) and a `config` (its content). The classroom only renders. You never code a custom screen for a lesson; you write data.

4. **Responses are saved to the user's folder, not the browser.** No `localStorage` as memory. If it's not on disk, it doesn't exist tomorrow, and you won't know what they failed.

5. **Everything is saved for another day.** Pieces are not deleted; they accumulate, and the classroom cover page is their index.

6. **Everything is local.** No accounts, no external services, no analytics, no fonts or libraries loaded from the internet at runtime. The user's notes do not leave their computer because of the classroom.

7. **It turns on and off.** At the end of the session, the classroom shuts down. You do not leave processes running.

## 2. Which Plan to Use Based on Your Environment

| Where you are | What you do |
|---|---|
| **Claude Code** (or any AI that can execute commands on the user's computer) | **Plan A / Plan B** — not specified in this file. See `motor.md` or a separate Plan A/B document for setup and session cycle. |
| **claude.ai** (the app, without terminal) | **Plan C** as an interactive artifact (Section 3). |
| **Other AIs** (ChatGPT, Gemini Web, etc.) | **Plan C** as a single, self-contained HTML file, in its canvas or as a downloadable file (Section 3). |

*The blocks (Section 6), the catalog (Section 7), and the quality rules (Section 8) are the same across all plans. The only thing that changes is where they are rendered and how responses return to the tracking files.*

## 3. Plan C — No computer access (Gemini Web, ChatGPT, etc.)

You cannot execute anything on the user's machine, so the piece travels complete in a single screen:

1. Generate a **single, self-contained HTML file** (all CSS and JavaScript inline, zero external resources or CDNs).

2. Build **only the blocks** that this specific piece uses, with the same `code`, `config`, and behavior defined in **Section 6**.

3. The final screen must be **"Your Results"**: a legible summary, and below it, the response records in the JSON format of **Section 4** inside a code block with a **"Copy Results"** button.

4. **How tracking returns to the .md files:** You must explicitly tell the user: *"Copy the JSON results below and paste them back into this chat so I can update your `learnings.md`, `fails.md`, and `reviews.md`."* If the results do not return, there is no review tomorrow.

5. The progress panel here is rendered from the .md files at the start of the conversation when requested; there is no permanent cover page.

## 4. The Formats

Wrapper keys are in English. Inside each block, `code` and `config` keys are in English. All identifiers (keys, file names, values like `explanation`) have no accents or special characters; all text the user sees has correct spelling and grammar.

> **Migration note (Spanish → English schema):** the English key in parentheses replaces the old Spanish key. Existing `contenido/` and `respuestas/` files must be renamed to `content/` and `responses/`, and their JSON keys rewritten: `tema`→`topic`, `titulo`→`title`, `tipo`→`type`, `creada`→`created`, `minutos`→`minutes`, `fuentes`→`sources`, `bloques`→`blocks`, `unidad`→`unit`; in records: `pieza`→`piece`, `procesadoHasta`→`processedUpTo`, `registros`→`records`, `fecha`→`date`, `bloque`→`block`, `enunciado`→`prompt`, `respuesta`→`answer`, `segundaRespuesta`→`secondAnswer`, `correcta`→`correctAnswer`, `acierto`→`correct`, `intentos`→`attempts`. Enum values also translate: `explicacion|ejercicios|repaso`→`explanation|exercises|review`; `practica|examen`→`practice|exam`; `visto|saltado`→`seen|skipped`; `sin-tocar|vista|trabajada|en-repaso|dominada`→`untouched|seen|worked|in-review|mastered`.

### A Piece: `content/<topic>/<piece>.json`

```json
{
"id": "2026-10-28-pretest-phrasal-verbs",
"topic": "english-c1",
"title": "Advanced Phrasal Verbs: What do you already know?",
"type": "exercises",
"created": "2026-10-28",
"minutes": 8,
"sources": ["material/English-c1/phrasal-verbs-c1.md"],
"blocks": [
{ "id": "b1", "code": "text", "config": { "content": "Before I explain anything, **answer without looking at your notes.** Remember to read the questions and your answers OUT LOUD." } },
{ "id": "b2", "code": "exam-quiz", "unit": "phrasal-verbs-c1", "config": { "prompt": "Test your intuition", "mode": "pretest", "questions": [...] } }
]
}
```

- `type`: `explanation`, `exercises`, or `review` (a set pulled from the failure pile and calendar).

- `sources`: The user's files the content comes from. Displayed at the bottom of the piece. A piece without sources carries a visible warning at the top: *"I prepared this from memory, without your material in front of me: verify it."*

- `unit`: The exact name of the unit in `aprendizajes.md`. This allows you to transfer hits and misses without guessing.

- Block `id`s are unique within the piece and never change (records reference them).

### The Records: `responses/<topic>/<piece>.json`

```json
{
"piece": "english-c1/2026-10-28-pretest-phrasal-verbs",
"processedUpTo": null,
"records": [
{
"date": "2026-10-28T10:32:10+02:00",
"block": "b2",
"code": "exam-quiz",
"item": "p1",
"unit": "phrasal-verbs-c1",
"prompt": "Which phrasal verb means 'to rely on something in a difficult situation'?",
"answer": "To depend on",
"secondAnswer": "To fall back on",
"correctAnswer": "To fall back on",
"correct": false,
"attempts": 2
}
]
}
```

- It is a file that only grows: each submission adds records to the end. The past is never rewritten.

- `correct` is `true` ONLY if they got it right on the first try. If they got it right after a hint, `correct: false` and `attempts: 2`: that goes to the failure pile equally. `correct: null` means "open response, pending professor correction".

- `answer` is ALWAYS the first thing they answered, even if they later corrected it: the original error is what matters for the failure pile.

- `prompt`, `answer`, and `correctAnswer` are in legible text (not identifiers), so the record is understandable on its own a month from now.

## 5. The Block Contract

Every block is a component that receives exactly three things:

1. `config`: its data.

2. `onRespond(records)`: function to notify what happened (one or more records without `date`, `block`, `code`, or `unit`, which the viewer adds). Returns whether it saved correctly, so the block can warn on screen if it fails.

3. `onComplete()`: function to notify the viewer that the block is entirely finished (all cards flipped, last question corrected).

**Rules that ALL blocks follow:**

- **Self-contained.** Calls nothing external, touches no global state. Does not navigate: only notifies.

- **Notifies once per event.** Guards against double submissions. An explanation block notifies `seen` only once; a practice block notifies each item when resolved.

- **Never traps anyone.** Any weird configuration (empty list, non-existent ID) has an exit: shows a warning and allows continuation.

- **"Completion" is a clear interaction**, never a timer that advances on its own requiring a touch.

- All `config` text accepts light formatting: `**bold**`, `*italic*`, and line breaks. Rendered formatted, never with asterisks visible. No raw HTML injection from config.

- **Accessible and tactile:** Everything can be done with keyboard and finger. Nothing relies solely on drag-and-drop or color.

- **Randomness happens in the browser, after loading.** Shuffling options or elements during server-side rendering causes server/client mismatch and page load failure. Shuffle once the block is mounted.

- The piece viewer shows blocks **one below the other** in explanations, and **one at a time with a progress bar** in exercises and reviews. In a set, the "Continue" button only appears when the current block has called `onComplete()`. Blocks without interaction (`text`, `progress-panel`, `ten-minutes`) count as completed as soon as they render. Meanwhile, there is a discreet exit: *"Skip this block (will be recorded)"*, which leaves its record as `skipped`: no one gets trapped, and no one skips an exercise without it being noted. At the end of a set, it paints a summary (first-try hits, misses, pending corrections) and the phrase: *"Return to the conversation: your professor already has your responses."*

## 6. The Starter Set: 15 Blocks

With these fifteen, you can teach almost anything. For each: its `code`, its `config` schema (`?` = optional), how it behaves, what it records, and when to use it.

### To Explain

1. **`text`** — Formatted text. `{ content: string, tone?: "normal" | "aviso" | "clave" }`. Paragraphs with light formatting. `aviso` paints as a warning box (use for "this is from memory, verify it"); `clave` as a highlighted idea. Records nothing. Use for context between blocks, never to dump a wall of text: if it exceeds eight lines, chunk it or use another block.

2. **`flip-cards`** — Cards that flip. `{ prompt: string, cards: [{ front: string, back: string, emoji?: string }], columns?: 2 | 3, summary?: string }`. Grid of cards; each flips on touch (or Enter/Space) to show the back. A counter shows how many remain. When all are flipped, `summary` appears and records `seen`. Use for vocabulary, definitions, formulas, "term → meaning". **In review: ask the user to say the answer OUT LOUD BEFORE flipping.**

3. **`step-diagram`** — Step-by-step diagram. `{ prompt: string, steps: [{ title: string, content: string, emoji?: string }], layout?: "vertical" | "horizontal", conclusion?: string }`. A chain of numbered boxes joined by arrows. Starts showing only the first step; "Next step" reveals the next, previous remains visible and dimmed. On narrow screens, `horizontal` becomes vertical. At the end, shows `conclusion` and records `seen`. Use for processes, grammar rules, procedures, and any "first this, then this".

4. **`before-after`** — Comparison of two states. `{ prompt: string, beforeLabel: string, afterLabel: string, beforeContent: string, afterContent: string, insight?: string }`. Two panels with a toggle to switch between them (side-by-side on wide screens). `insight` appears when both have been seen; then records `seen`. Use for "B2 vs C1 usage", "wrong vs right", "what you think vs what it is". Perfect for dismantling a frequent error: put the wrong version in "before" exactly as the user would say it.

5. **`timeline`** — Timeline. `{ prompt: string, events: [{ date: string, title: string, description: string, emoji?: string }], insight?: string }`. Vertical line with milestones; each unfolds on touch. `date` is free text. Records `seen` when all are opened. For history, phases of a project, evolution of a concept.

6. **`accordion`** — Concept accordion. `{ prompt: string, items: [{ title: string, content: string, emoji?: string }], conclusion?: string }`. List of titles that expand one by one. Marks already opened ones. When all are opened, shows `conclusion` and records `seen`. For a topic with several independent parts, or a "unit summary" consulted in chunks.

7. **`visual-analogy`** — Visual analogy. `{ concept: string, analogy: string, emoji: string, mapping: [{ concept: string, image: string }], limits: string, insight?: string }`. Top: the difficult concept and, beside it, the everyday image with its large emoji. Below, a correspondence table revealed row by row ("this here is like that there"). At the end, a mandatory box: "where this analogy breaks down" (`limits`). Records `seen`. Use when the user gets stuck on an abstraction. An analogy without its limits teaches poorly: therefore `limits` is not optional.

### To Practice

8. **`exam-quiz`** — Exam-style test with reasoned correction.

```json
{
"prompt": "string",
"mode": "pretest" | "practice" | "exam",
"questions": [{
"id": "string",
"text": "string",
"options": [{ "id": "string", "text": "string", "whyNot?: "string" }],
"correct": "string",
"hint?: "string",
"why": "string",
"source?: "string"
}],
"insight?: "string"
}
```

One question at a time. Behavior by `mode`:

- `practice`: If they fail, DO NOT teach the solution. Show why their option is wrong (`whyNot` of the chosen option) and the `hint`, and give a second attempt. After the second attempt, whether they get it right or not, show the correct one with its `why` and `source`.

- `pretest`: One attempt only, no question-by-question correction; at the end, only how many they got right and the message "failing here is the plan: now your brain knows what to look for". You give the reasoned correction in the conversation.

- `exam`: One attempt per question and all reasoned correction at the end, together.

Records one record per question: `correct` only if first try, and `attempts`. Option order is shuffled on each visit. **Always write `whyNot` in distractors**: explaining why their answer was wrong is half the method.

9. **`fill-in-blank`** — Fill in the blanks. `{ prompt?: string, sentence: string, blanks: [{ id: string, answer: string, alternatives?: string[] }], mode?: "type" | "choose", bank?: string[], explanation: string }`. In `sentence`, each blank is marked as `{{id}}` (e.g., `"The capital of {{1}} is {{2}}"`). In `type`, text fields; in `choose`, a dropdown per blank with words from `bank` (or, if no `bank`, with shuffled answers). Comparison ignores case, extra spaces, and accents ONLY if the topic is not spelling or a language (in that case, exact: decide when writing `alternatives`). "Check" button: marks each blank, allows a second attempt on failed ones, then shows the solution and `explanation`. One record per blank. For literal definitions, formulas, conjugations, articles of a norm, syntax.

10. **`order-sequence`** — Order. `{ prompt: string, items: [{ id: string, text: string }], explanation: string }`. `items` comes in the CORRECT order; the classroom shuffles it when displaying (and ensures it doesn't come out already ordered). Each element has "up" and "down" buttons (dragging is an extra, never the only way). "Check" marks which positions are right; second attempt; then, the correct order with `explanation`. One record per exercise. For processes, chronologies, hierarchies, steps of a procedure, or **English sentence structure**.

11. **`match-pairs`** — Match pairs. `{ prompt: string, pairs: [{ id: string, left: string, right: string }], explanation?: string }`. Two columns, the right one shuffled. Touch one on the left, then one on the right (also with keyboard). A correct pair locks; an incorrect one flashes for a second and releases, and counts as a failure for that pair. One record per pair (`correct` only if it came out on the first try). For term and definition, word and translation, author and work, cause and effect, **collocations**.

12. **`true-false-why`** — True or false, with justification. `{ prompt: string, statements: [{ id: string, text: string, isTrue: boolean, explanation: string }], askWhy?: boolean, minChars?: number }`. One statement at a time: the user marks T or F and, if `askWhy` (default yes), writes in one line WHY before being able to submit. Then they see if they got it right and the `explanation`. Record per statement: `correct` of the T/F, and their justification in `answer` for you to read (getting the T/F right for the wrong reason is a failure: treat it as such when transferring it). `minChars` never greater than 30. To dismantle false ideas and catch the "it sounds right" trap.

13. **`teach-back`** — Feynman-style open question. `{ prompt: string, audience?: string, placeholder?: string, minChars?: number, selfCheck?: string[], afterSubmit?: string }`. "Explain it in your own words, as you would tell it to `audience`" (default, "someone who knows nothing about the topic") and a large text area with a character counter. On submit: the text is saved with `correct: null` (pending correction), `selfCheck` is shown as a self-evaluation list ("did you mention…?"), and the message `afterSubmit` (default: *"Return to the conversation: your professor corrects it with you"*). **This block is not corrected automatically: YOU correct it**, reading the text and pointing out what is right, what is missing, and what is said wrong. Use it to close each unit: if they can't explain it simply, they don't know it. **CRITICAL FOR ENGLISH: Add a note in the prompt reminding the user to speak their answer OUT LOUD before typing.**

### To Follow Up

14. **`progress-panel`** — Progress panel.

```json
{
"generated": "string",
"topics": [{ "name": "string", "units": [{ "name": "string", "state": "untouched" | "seen" | "worked" | "in-review" | "mastered", "nextReview?: string" }] }],
"failures": { "total": number, "dueToday": number, "items?: [{ "question": string, "topic": string, "due": string }] },
"today": [{ "label": "string", "target?: string, "minutes?: number" }],
"note?: string"
}
```

Three zones: "Today you have" (from `today`, with link to the piece if `target` = `topic/piece`), the map of topics with each unit colored AND labeled with its state, and the failure pile (how many there are, how many return today). **Read-only:** no button to change a state. States are earned, not marked. Each state carries its text in addition to color. Its data lives in the `panel` key of `content/panel.json` and you regenerate it from `aprendizajes.md`, `fallos.md`, and `repasos.md` before starting. Painted on the cover, and can also go inside a piece.

15. **`ten-minutes`** — "I only have 10 minutes" button. `{ label?: string, minutes?: number, target: string | null, summary?: string, emptyMessage?: string }`. A large button. If `target` points to a piece (`topic/piece`), it goes there. That piece is a **short set** you leave prepared every time you regenerate the panel: between 5 and 8 items, first the failures due today, then the overdue reviews, nothing new. `summary` says what's inside ("6 questions: 4 from your pile and 2 reviews"). If there is nothing to review, `target` is `null` and `emptyMessage` is shown ("you owe nothing today: if you want, a new topic"). Goes on the cover, below the panel; its config lives in the `tenMinutes` key of `content/panel.json`. It is the dignified exit for a bad day: ten minutes of what matters most is worth more than an hour that doesn't happen.

## 7. The Catalog: The rest of the blocks, on demand

The starter set is not the limit. This is a **menu**: when an explanation or exercise asks for another mechanic, or when the user requests it **by name or by idea** ("I want something like scratch and reveal", "make me a traffic light to classify this"), you create it at that moment. Procedure: (1) define its `code` in lowercase with hyphens and its `config` schema; (2) implement the block fulfilling the contract of **Section 5** and the rules of **Section 8**; (3) register it in `block-map`; (4) note it in `classroom/README.md` with its schema, so your future self next week knows it exists. In Plan C there is no registry: simply include it in the piece.

- **Show content:** striking fact with a big number, two-figure comparator, pull quote, notice box (info, tip, warning), image with dots that reveal notes on tap, expandable definition card, highlighted key idea.

- **Discover:** click to reveal, scratch and reveal, text that appears line by line, hidden words that emerge on tap (ideal for memorizing literal definitions), draggable curtain, content behind a lock (mini-question to open), layers peeled one by one, flashlight on a dark screen.

- **Examine:** myth or reality, quiz with images or icons, timed true-or-false, guess the percentage with a slider, find the correct word among distractors, spot the difference between two versions, streak of correct answers, fast round with score, practical case with several chained questions, dictation, numeric-answer question with tolerance.

- **Classify:** drag into buckets, swipe cards left or right, Venn diagram, place on a spectrum, 2×2 matrix, red/yellow/green traffic light, tier list (S/A/B/C/D), sort by priority.

- **Decide and opine** (no correct answer; for thinking): scenario with options and consequences, this or that, dilemma with cascading consequences, take a stance with arguments from both sides, agreement spectrum, "check the ones that apply to you" list.

- **Reflect and write:** open journal question, rewrite a badly phrased sentence, advice to a peer with the same doubt, commitment card ("this week I will…"), letter to your future self one month from now, one-sentence summary.

- **Self-evaluate:** 1-to-5 scale, slider between two extremes, confidence meter before and after the set (compare what you thought you knew with what you actually knew), frequency check, multi-axis wheel.

- **Time and challenge:** mini focus pomodoro, countdown that reveals, timed mock exam with exam time, reaction time.

- **Narrative:** simulated conversation with options to choose from (perfect for languages and for rehearsing a meeting), choose your own path, a day in the life with decisions, the same scene from several perspectives, panels in sequence.

- **Diagrams:** funnel, cycle, tree, pyramid, mind map, yes/no decision diagram, org chart, node network, staircase, quadrant.

- **Graphs:** bars, lines, donut, radar, dot matrix, phase progress bar, heatmap (very useful for seeing which days you have studied).

- **Visual metaphors:** iceberg, balance scale, thermometer, puzzle piece that fits, battery, bridge, mountain with a flag, hourglass, lock that opens.

- **Game:** badge that unlocks, step-by-step mini-mission, bar that fills with micro-actions, day streak. In moderation: play accompanies the method, it does not replace it, and no badge may grant a state that has not been earned by getting answers right.

- **Code and technical:** syntax-highlighted code block, simulated terminal, file tree, diff view, fill-in-the-blank code, order lines of code.

And whatever is not on the menu, too: the catalog is inspiration, not a boundary. "I want a block that does X" is always a valid request.

## 8. Quality Rules

1. **Accessible.** Semantic HTML (buttons that are `button`, titles in order). Everything manageable with keyboard, focus always visible. Nothing depends solely on color (hit and miss also carry icon and text; panel states carry their name). Sufficient contrast in light and dark. Base text 16px or more. Respects `prefers-reduced-motion`: animations are an adornment, never the way to understand something.

2. **Works on mobile.** Comfortable single column from 360px width, no horizontal scroll. Touch zones 44px minimum. Nothing depends on hovering. Dragging is always optional, with alternative to touch or buttons.

3. **No external services.** Zero CDNs, zero remote fonts (use system ones), zero analytics, zero requests to the internet while in use. The classroom works offline.

4. **No accounts and no data outside.** No registration or login. Data lives in `content/` and `responses/`. The server listens only on `127.0.0.1` (or in Plan C, it's just a local file).

5. **Sober.** Quiet background, a single accent color, lots of air. It is a site to think, not a casino. Light and dark mode according to the system.

6. **Robust against weird data.** An incomplete config paints a legible warning and lets you continue. Never a blank screen or a technical error in the user's view.

7. **Correct English/Spanish** in the entire interface, with its accents.

8. **No pedagogical fluff.** No confetti for getting a question right or inflated scores. The classroom tells the truth: how many on the first try, which ones to the failure pile.

## 9. Frequent Breakdowns (You resolve them, silently)

> These apply to **Plan A** unless noted. In pure Plan C there is no server, no build, and no port — the only failure mode is "the artifact did not render", in which case regenerate it.

- **The port is occupied:** check if it's the classroom (`/api/estado`); if it is, reuse it; if not, next port.

- **Compilation error after creating a new block:** read the error, fix it, and run `npm run build` again before opening the browser. The user should never see a development error screen.

- **The piece does not appear on the cover:** almost always a name that doesn't match the pattern or a poorly closed JSON. Validate the JSON before opening.

- **Responses do not save:** check write permissions in `classroom/responses/` and that the API route is not cached as static.

- **It stayed on from another day:** reuse it or turn it off; never launch a second one on top.

- **After updating dependencies something changes behavior:** send the documentation of the installed version, not this file. This file describes WHAT the classroom must do; the exact HOW depends on the version.
# Language tutor skill

A skill that turns an AI assistant into a language tutor. It teaches any language and explains in any language: Spanish in Russian, English in German, German in English. It runs complete lessons: it finds your level, reviews what is due, teaches one small new step at a time, writes typed exercises, grades them fairly, and decides when you are ready to move on.

## How the pieces fit

```
you, or a scheduled task   ->   this skill   ->   memory
"continue my Spanish"           teaches,          a pasted note, a document,
                                grades, paces     chat memory, or a memory server
```

The skill is the middle piece and works on its own. It needs no server and no schedule; add either when you want them.

- **Memory** only remembers. The skill decides what to teach, how much, and whether a step is learned.
- **A schedule** only starts a lesson. If nobody is there to answer, the skill delivers the lesson and grades it when you reply.

## Quick start

Install the skill (below), then say what you want to learn. The tutor explains in the language you write in:

- "Let's learn German."
- "Продолжаем испанский."
- "Ich möchte mein Englisch auffrischen."

A new learner gets a few questions and, unless they are a true beginner, a short placement test. After that every lesson follows the same shape: a refresher of rules you keep missing, review, one small new step, typed exercises, a fair check.

## What it does

- **Onboarding and placement.** A few questions, then a short typed test that confirms the level and records which topics you already know.
- **Lessons.** A short refresher, review of what is due, one small new step, typed exercises with no hints in the prompts.
- **Fair grading.** Full, half or no credit per exercise. Slips that do not change meaning are not penalized (a missing accent on Spanish *café*, German umlauts typed as *ae/oe/ue*, an obvious typo); marks that do change it are (Spanish *él/el*, *pagué/pague*).
- **Pacing that adapts.** Three modes (normal, slow down, consolidate), chosen from your recent scores and repeated errors. A step counts as learned only after enough correct answers and a correct answer on a later day.
- **Reteaching.** If a topic keeps failing, the tutor explains it again from a different angle instead of drilling the same sentences.

## Install

**Claude Code:**

```sh
git clone https://github.com/viktoras25/language-tutor-skill ~/.claude/skills/language-tutor
```

**Claude Desktop / claude.ai:** zip the skill and upload it in Settings, Skills:

```sh
zip -r language-tutor.zip SKILL.md references
```

**Other assistants:** paste `SKILL.md` as the instructions and attach the files in `references/`.

## Where your progress is kept

The tutor checks what is available and uses the first that applies:

| Option | What you do |
|---|---|
| **A memory server** | Connect it once. The tutor reads and writes everything by itself. |
| **Notion or a similar tool** | The tutor keeps one progress page per language up to date. |
| **Chat memory** | The tutor saves one progress note as a memory. |
| **This chat only** | Ask for your progress note when you stop, and paste it at the start of the next chat. |

The note is plain text you can read and edit; its format is in `references/backend-note.md`.

## Using a memory server

`references/memory.md` lists what the skill reads and writes in plain operations: profile, recent lessons, review queue, graded answers, the close of a lesson. Any server that covers them works. The server explains its own tools; the skill carries no server-specific instructions.

The [Language Tutor MCP server](https://github.com/viktoras25/language-tutor-mcp) is built for this skill. It adds spaced repetition, per-topic history and an optional syllabus, and runs in the cloud or locally with one SQLite file.

A server with a syllabus proposes the next topic, and the skill treats the proposal as a candidate. Without a syllabus the skill builds a short roadmap from your goals and keeps it up to date.

## Files

```
SKILL.md                 entry point: roles, start-up order, lesson loop
references/
  setup.md               course language, onboarding, placement
  memory.md              what the skill reads and writes; rules for every backend
  backend-note.md        the progress note, for use without a server
  pacing.md              lesson modes, dosing, when a step is passed, choosing the next topic
  teaching.md            lesson structure, exercise rules, grading
evals/                   test scenarios with checks
```

## Adjusting it

- **Your preferences** (answer format, number of exercises, topics you like): tell the tutor. It stores them with your progress.
- **Pacing thresholds** are in `references/pacing.md`, each with its reason.
- **Grading leniency** is in the grading section of `references/teaching.md`.
- **Language knowledge** is not bundled. The tutor uses what the model knows about the language, its typical difficulties and the traps for speakers of your languages.

## Tests

`evals/` holds six scenarios, each a prompt with a list of checks: a first lesson, a slow-down after weak lessons, grading with partial credit, and resuming from a progress note in Spanish; choosing a topic without a syllabus and grading with umlaut substitutes in German. Run them with a skill-creator session, or from the repository root:

```sh
claude plugin eval . --allow-tools Write,Edit,Bash
```

These are real model runs and are billed.

## License

MIT, see `LICENSE`.

# Language tutor skill

A skill that turns an AI assistant into a language tutor for any language you are learning (Spanish, German, English, and so on). It runs complete lessons: it finds your level, reviews what is due, teaches one small new step at a time, writes typed exercises, grades them fairly with partial credit, and decides when you are ready to move on.

It works in any chat and does not need a server. It works best with a language-learning memory server such as the [Language Tutor MCP server](https://github.com/viktoras25/language-tutor-mcp), which keeps your syllabus position, history and review schedule between lessons.

## What it does

- **Onboarding and placement.** A new learner answers a few questions; unless they are a true beginner, a short typed test confirms the level and records which topics are already known.
- **Lessons.** Short refresher of rules you keep missing, review of what is due, one small new step, 15-16 typed exercises with no hints in the prompts.
- **Fair grading.** Full, half or no credit per exercise. Slips that do not change meaning (for example a missing accent on Spanish *café*, umlauts typed as *ae/oe/ue* in German, an obvious typo) are not penalized; marks that do (Spanish *él/el*, *pagué/pague*) are.
- **Pacing that adapts.** Three modes (normal, slow down, consolidate) chosen from your recent scores and repeated errors. A step counts as learned only after enough correct answers *and* a correct answer on a later day.
- **Reteaching.** If a topic keeps failing, it explains it again from a different angle instead of drilling the same sentences.

## Install

**Claude Code** (any platform):

```sh
git clone <this repo> ~/.claude/skills/language-tutor
```

**Claude Desktop / claude.ai:** zip the folder with `SKILL.md` at the top level and upload it in Settings, Skills:

```sh
zip -r language-tutor.zip SKILL.md references
```

**Other assistants:** paste `SKILL.md` as the instructions and attach the files in `references/` (the instructions refer to them by name).

Then say something like "Let's learn German" or "continue my Spanish lesson".

## Where your progress is kept

At the start the tutor checks what is available and uses the first that applies:

| Option | What you do |
|---|---|
| **Language Tutor MCP server** | Connect it once (cloud, or locally on your computer with a SQLite file; see that project's README). The tutor then reads and writes everything by itself. |
| **Notion or a similar tool** | The tutor keeps one "progress" page per language up to date. |
| **Chat memory** (Claude, ChatGPT, project notes) | The tutor saves one progress note as a memory. |
| **This chat only** | At the end of each lesson the tutor gives you a short progress note. Paste it at the start of the next chat. |

The note format is in `references/backend-note.md`; it is plain text you can read and edit.

## Using it with a memory server

The skill thinks in plain operations (read the profile, the recent lessons, the review queue; record an answer; close a lesson) and never keeps its own copy of what the server knows. `references/backend-server.md` maps those operations to the tools of the Language Tutor MCP server. A different server needs a similar mapping file, and nothing else changes.

The skill decides what to teach, how much, and whether a step is passed. A server that has a syllabus can propose the next topic; the skill treats that as a candidate. Without a syllabus the skill builds and keeps a short roadmap itself (`references/pacing.md`, "Choosing the next topic").

## Repository layout

```
SKILL.md                         entry point: roles, start-up order, lesson loop
references/
  setup.md                       course language, memory operations, onboarding, placement
  pacing.md                      lesson modes, dosing, when a step is passed, choosing the next topic
  teaching.md                    lesson structure, exercise rules, grading and scoring
  backend-server.md              mapping to the Language Tutor MCP server
  backend-note.md                progress tracking without a server (note, Notion, chat memory)
evals/                           test cases with checks (see below)
```

## Adjusting it

- **Thresholds** (75% / 70% / 65%, five exercises per step) live in `references/pacing.md`, each with its reason. Change them there.
- **Grading leniency** is in the grading section of `references/teaching.md`.
- **Language knowledge** is not bundled: the tutor uses what the model already knows about the language, its typical difficulties and the traps for speakers of your languages. If you want a fixed order for a language, add it as a section of `references/pacing.md` or tell the tutor.
- **Your own preferences** (typed answers, number of exercises, topics you like) are stored by the tutor in your profile or progress note; you can simply tell it.

## Tests

`evals/` holds six scenarios in the format used by Claude's skill-creator and `claude plugin eval`: a first lesson with no history, a slow-down after weak lessons, grading answers with partial credit, and resuming from a progress note (all in Spanish), and the same two ideas in German (choosing a topic without a syllabus; grading with umlaut substitutes and article errors). Each has a prompt and a list of checks. Run them with a skill-creator session, or from the repository root:

```sh
claude plugin eval . --allow-tools Write,Edit,Bash
```

These are real model runs and are billed.

## License

MIT, see `LICENSE`.

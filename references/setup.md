# Setup: course, memory, onboarding, placement

Read this at the start of every conversation. It answers four questions: which language, where does progress live, is this a new learner, and what level are they at.

## 1. Which language

Take the course language from the learner's message or stored state. If it is not clear, ask. A learner may study several languages; keep their progress separate and say which course you are in at the top of the lesson.

## 2. What the skill needs from memory

The skill thinks in these operations, not in any particular tool. Each backend maps them to its own calls or to text.

**Read**
- *Profile:* explanation language, languages the learner knows, regional variety, goals, format preferences.
- *Course state:* level and durable preferences.
- *Recent lessons:* summary, score, the plan for the next lesson, and whether the last one was left unfinished.
- *Review queue:* items due for repetition, most urgent first.
- *Weak items:* recurring errors.
- *Topic history:* for a topic, the dated list of graded answers (used for the step gate in `pacing.md`).
- *Topic proposals and topic detail:* only when a syllabus exists.

**Write**
- Open a lesson; close it with score, summary and the plan for next time.
- Each graded answer: right or wrong, the kind of error, the exercise and answer as context.
- New words or forms; relations between items.
- Topic status (being taught, introduced, already known).
- Profile and preferences.

## 3. Find the memory backend

Use the first that applies:

| # | Backend | How to tell | File to read |
|---|---|---|---|
| 1 | **A language-learning memory server** | tools for profile, lessons ("sessions"), attempts and review queues are available (for example the Language Memory MCP server) | `backend-server.md` |
| 2 | **Notion or another document tool** | a connector is available and the learner chooses it | `backend-note.md`, with the note kept on one page |
| 3 | **Chat memory** (Claude, ChatGPT, project notes) | the platform can save memories | `backend-note.md`, with the note saved as one memory |
| 4 | **This chat only** | nothing else | `backend-note.md`; the learner pastes the note into the next chat |

If the learner has not said which to use and more than one is possible, ask once, briefly, and recommend the highest available. When no server is connected, mention once that a memory server adds a syllabus, spaced repetition and per-topic history; do not push it.

## 4. Load state, or detect a new learner

Read the profile, course state and recent lessons through the backend. An empty state and no lessons mean a new learner; for the note-based backends, a missing note means the same (ask the learner to paste one if they have it).

For a returning learner, go to `pacing.md`. If the last lesson was left unfinished (started, never answered), reuse 3-4 of its exercises, and do not count its topic as covered.

## 5. Onboarding a new learner

Ask only what changes the first lesson, in one short, friendly message:

1. Which language, and what you have done with it so far (course, app, school, living abroad), how long, roughly how well it sticks.
2. What you need it for and how much time you have.
3. Languages you know well (used for contrast traps, such as false friends).

Do not ask about the language of explanations, the regional variety, or the exercise format. Use sensible defaults: explain in the language the learner writes in, use the most common variety of the target language (or the one the goal implies, such as the country they are moving to), and give typed exercises of about 12-15 items. Adjust when the learner reacts or asks for something else, and remember the change.

Store the answers in the profile (or the profile block of the note). Preferences that last go into course state (or the note).

## 6. Decide the level

Self-assessment is unreliable, so use it only to choose where to start testing.

- **True beginner** (never studied, a few words): no test. Start with the first topics of the course.
- **Anyone else:** run the placement test below. A learner with years of app study can still miss basics; a test costs 15-20 minutes and saves weeks of mis-targeted lessons.
- **Learner insists on skipping the test:** start one level below their claim and watch the first two lessons; the pacing rules will correct a wrong start.

## 7. Placement test

Purpose: find which topics can be marked as already known, and where teaching starts.

- Label the session as a placement test.
- 12-16 typed exercises, ordered easy to hard, covering the topics of the claimed level and the one below. Two exercises per topic, one recognition and one production. No hints in prompts.
- Stop early after two blocks in a row under 50%; more questions only discourage.
- Grade with the normal rules (`teaching.md`, grading section).
- A topic counts as **known** only at 75% or better over its two exercises with at least one production answer correct. Anything else is taught. Mark known topics as already known, and do not mark more than the evidence supports.
- Record weak spots found (for example, a pattern of avoiding a construction) in the lesson summary: they are the first review items.
- End by telling the learner their level, what starts first, and why, in a few lines.

For a placement over several areas, split it over two sittings rather than exceeding about 16 exercises each.

## 8. Roadmap when there is no syllabus

Without a syllabus the roadmap is yours. After placement (or at once for a beginner), draft 8-12 upcoming topics in a sensible order and store it in the profile or note as `roadmap`. How to build it and keep it up to date is in `pacing.md`, "Choosing the next topic". Show it to the learner in one or two lines so they can steer it.

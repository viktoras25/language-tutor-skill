# Memory: what the skill needs, whoever stores it

The skill thinks in the operations below, not in any particular tool. A backend is anything that can carry them out: a memory server, a document, a saved memory, or a note the learner pastes. The backend remembers; it never decides whether something was learned.

## Operations

**Read**

| Operation | What it gives |
|---|---|
| Profile | explanation language, languages the learner knows, regional variety, goals |
| Course state | level and lasting preferences (answer format, number of exercises) |
| Recent lessons | for the last few: summary, score, the plan written for the next lesson, and whether the lesson was answered |
| Review queue | items due on the learner's local date, most urgent first |
| Weak items | errors that keep coming back |
| Step history | for one step or topic: the dated graded answers with their credit (feeds the step gate in `pacing.md`) |
| Topic proposals, topic detail | optional; only when the backend has a syllabus |

**Write**

| Operation | What it stores |
|---|---|
| Record a graded turn | the lesson (title, brief plan, whether it is a placement test), new words or forms and their relations, and each graded answer: item, credit (1, 0.5 or 0), kind of error, exercise and answer as context |
| Close a lesson | summary, score, and the plan for the next lesson |
| Topic status | being taught; introduced; already known (placement evidence only) |
| Profile and preferences | when the learner states or changes them |
| Mark a lesson as delivered, not yet answered | optional; for unattended runs |

## Find the backend

Use the first that applies:

| # | Backend | How to tell | How to use it |
|---|---|---|---|
| 1 | **A language-learning memory server** | tools for profile, lessons, graded answers and review queues are available | Follow the server's own usage guide (its instructions, or a guide tool it offers) for the exact calls. If it has none, map the operations above to its tools from their descriptions. |
| 2 | **Notion or another document tool** | a connector is available and the learner chooses it | `backend-note.md`, with the note kept on one page |
| 3 | **Chat memory** | the platform can save memories | `backend-note.md`, with the note saved as one memory |
| 4 | **This chat only** | nothing else | `backend-note.md`; the learner pastes the note into the next chat |

If the learner has not said which to use and more than one is possible, ask once, briefly, and recommend the highest available. When no server is connected, mention once that a memory server adds spaced repetition and per-topic history; do not push it.

## Rules for every backend

- **One home per fact.** The plan for the next lesson, the state of open steps (the gate line from `pacing.md`) and the new-material score go into the close of the last lesson. The history is the lessons themselves; do not keep a second list of them. Profile and course state hold only what lasts: who the learner is and how they like to work.
- **Record once per graded turn**, when you grade, not once per answer.
- **Close every lesson**, in this order: grading table finished and arithmetic checked; answers and new items recorded; topic status updated; then the close with summary (what was covered, errors by kind, new or repeated), score, and the plan for the next lesson.
- **Missing operation.** If the backend cannot do something (no step history, no review queue), keep that fact in the plan you write at the close, the way the note does, and carry on.
- **Never invent** ids, dates, statuses, streaks or scores, and never delete history. Mark a topic introduced or already known only with evidence.
- **The backend's own "mastered" or "known" labels are not the step gate.** They follow the backend's schedule; whether to move on is decided by `pacing.md`.
- **A grade changed after recording:** correct the score and explain the change in the summary.
- **Backend unavailable during a lesson:** teach anyway, give the learner a progress note, and record the lesson afterwards with the dates the answers were really given, saying it was recorded late.
- **Delivered, not yet answered** (unattended run): mark the lesson as such if the backend can; in a note, write it in the plan line. While such a lesson is three days old or less, do not build another one; one short reminder at most. After that, build a fresh lesson. When the learner answers, record into the same lesson rather than a new one.

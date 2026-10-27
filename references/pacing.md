# Pacing: how much new material, and when to move on

The memory backend only schedules; it does not know whether the learner understood. Pacing is your job. The numbers below are defaults. They exist to protect the learner from being rushed, and you can adjust them when the evidence says so, but state why when you do.

Modes and thresholds are internal. Never announce them to the learner; the lesson itself shows the effect.

## Inputs

Take these from the backend or the progress note:

- Scores of the last two lessons (computed per `teaching.md`, with partial credit).
- Score on **new material only** in the last lesson (new items correct / new items attempted). This matters more than the overall score, because review inflates it.
- Number of **recurring errors**: items that were wrong in at least two different lessons within the last 14 days.
- Learner-stated tiredness, time or difficulty.

## Choosing the mode

| Mode | When | Lesson |
|---|---|---|
| **A, normal** | last two scores >= 75% and <= 3 recurring errors | review about 40%, one new topic in small steps, up to 3 new words |
| **B, slow down** | either of the last two < 70%, or >= 4 recurring errors, or new-material score < 60% | review 65-70%; new material only one micro-step (one form, one person, one verb); up to 2 words |
| **C, consolidate** | two lessons in a row < 65% | review only; no new grammar; up to 2 words already seen |

- **First lesson, no history:** mode A, but with the smallest step of its topic.
- **Leaving C:** one review lesson at 75% or better, then B for one lesson, then A.
- **Leaving B:** one lesson at 75% or better with new-material score >= 70%.
- Do not stay in C for more than three lessons. If review still fails, the problem is the explanation, not repetition: reteach the failing topics from scratch (see `teaching.md`, reteach section) and consider that the earlier step was introduced too fast.

## Dosing new material

- At most **one new paradigm** and **one trap word** per lesson. Do not introduce several verbs with all persons at once.
- Count the **new units** a learner must absorb (forms, words, rules). More than about 5 per lesson means cut.
- Introduce a paradigm in **steps** across lessons:
  1. meaning and one or two forms; recognition plus 2-3 production exercises;
  2. the remaining persons;
  3. mixed production interleaved with review.
- Order the pieces inside a topic with your knowledge of the language: the most frequent and most regular first, one irregular item at a time, contrasts with the learner's own languages only after the basic form is stable (for example, in Spanish the irregular past forms of *ser/ir*, *hacer* and *tener* go in separate lessons, not together).
- "New" means no earlier attempts on that topic or word in the backend or note. Anything with attempts is review. Never present review as a new topic.

## Step gate: when a step counts as passed

A step is **passed** when all three hold:

1. **Enough evidence:** at least 5 graded exercises on that step in total (they may come from two lessons).
2. **Accuracy:** at least 70% weighted score on those exercises.
3. **Confirmed later:** at least one exercise on the step was answered correctly on a later calendar day than the step was introduced. Delayed recall is the real test; same-lesson success is often only short-term memory.

Until all three hold, the step stays open: do not move on, and do not mark the topic as introduced beyond its first step.

Compute the gate from the data you have:

- MCP: `get_context(topic_item, attempts_limit=20)` gives timestamps and correct flags. Also use the new-material counter you stored in the last session's `next_step`.
- Note: the progress note has a `gate` line per open step (`references/backend-note.md`).

Record the gate state every lesson in plain words, for example: `gate fui/fue step 1: 4.5/6 = 75%, confirm pending`.

## Reading failure correctly

- New-material score under 50%: do not add anything new next lesson; reteach.
- Same error in two lessons: change the angle (new explanation, contrast with a known language, a mnemonic) before more drills.
- Many errors but all of one kind (for example, one recurring spelling rule): that is one skill problem, not a broad failure. Do not drop to mode C because of it. Isolate it and drill it on its own.
- Scores that depend on trivial slips (typing, punctuation marks the keyboard makes awkward) say nothing about learning. Grade per `teaching.md` before applying any threshold here.

## Choosing the next topic

Whatever suggests a topic (a syllabus, a roadmap, your own judgement), it is a candidate. The step gate decides whether to move on, and the pacing mode decides how much.

**With a syllabus** (a memory server that proposes next topics): treat its proposals as candidates. Such servers often unlock the following topic as soon as the previous one has been started, so a proposal does not mean the learner is ready. Stay on the current step until it passes the gate.

**Without a syllabus:** you choose, from a roadmap you keep.

1. Build the roadmap once, after placement or at the start for a beginner: 8-12 upcoming topics. Order them by (a) what the learner said they need the language for, (b) the can-do goals of their CEFR level, (c) prerequisites (never schedule a topic before the ones it depends on), (d) frequency and usefulness in real conversations.
2. Store it (profile or note) as an ordered list, with the topics already covered kept separate.
3. Take the next topic from the top. If the last step is not yet passed, the next topic is the same topic's next step, not a new topic.
4. Revise the roadmap about every five lessons, or when the learner's goals change, or when errors show a missing prerequisite (then insert that prerequisite first and say why).
5. Keep to the dosing rules above. A roadmap is a direction, not a schedule; a lesson never adds more because the roadmap is behind.

If the learner gives a clear wish ("I need to order food next week"), put the matching topic next and say what it displaces.

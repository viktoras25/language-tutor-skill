# Backend: a language-learning memory server (Language Tutor MCP)

This file only maps the memory operations of `setup.md` to one concrete backend, the Language Tutor MCP server. Nothing here is required by the skill itself; with another backend, use the operations in `setup.md` and ignore this file. `<lang>` is the course language code the server uses (for example `es`, `de`). A syllabus exists on the server only for languages that have imported one; if `next_topics` or `get_topic` report none, plan with the roadmap in `pacing.md`.

The server is the source of truth for syllabus position, items, attempts, spaced repetition, sessions, preferences. It does not decide whether the learner has absorbed something. It never invents anything; neither should you.

## Before the lesson

1. `get_profile` - `ui_lang`, `known_langs`, `variety`, vocabulary packs.
2. `get_course_state(<lang>)` - learner preferences (`format_prefs`) and the `plan` summary. Follow `format_prefs`.
3. `list_sessions(<lang>, 3)` - recent lessons with `summary`, `score`, `next_step`. The newest `next_step` is the plan for this lesson (see "Where the plan lives" below). A lesson without `score` and `next_step` was not finished; its `summary` and attempts (`get_session`) show how far it got.
4. `due_items(<lang>, 15, date=<learner's local date>)` - what to review. Pass `date` explicitly; the default is UTC and can be a day off.
5. `weak_items(<lang>)` and `get_context(item_id, attempts_limit=10)` on the most frequent errors, to see earlier answers and avoid reusing sentences, and to compute the step gate (`pacing.md`).
6. `next_topics(<lang>)` - `continue` (topics following the current one) and `fill` (earlier gaps). The server proposes; you choose, using the pacing rules. If the current step is not passed, stay on it even though the server lists the next topic.
7. `get_topic(<lang>, topic_id)` - the lesson card. Texts are English; explain in `ui_lang`.

Nothing is opened before the lesson: the session is created by the first `save_session` call.

## Recording during and after the lesson

Record with `save_session`, once per graded turn (when you grade the learner's answers), not once per answer.

- **First call:** no `id`; pass `lang`, `title="Lesson N: <topic>"`, `plan="<brief>"` (and `kind="placement"` for a placement test). It returns the session `id`; pass it in every later call of the lesson.
- **One call carries everything new in that turn:** `items`, then `links`, then `attempts`, in that order, so an attempt can refer to an item created in the same call.
- **If it fails:** the error says what was saved. Fix the one problem and call again with the same `id`.

What goes in it:

- **New words and forms (`items`):** `find_items` first if you are unsure whether the item exists. Type `lemma`, `form`, `grammar_point` or `collocation`. For words, `data` has `display`, `gender`, `translation`, `example`, `pos`. The id is `<lang>:<type>:<lowercased label>` and is returned by the call; relations go in `links` (`src`, `dst`, `type`).
- **Every graded exercise (`attempts`):** `{item_id, correct, error_type, context}`.
  - The server records only right/wrong. Map the grading table: credit 1 -> `correct=true`; partial and wrong -> `correct=false` with the `error_type` of the real error. Keep the partial credit in your own tally and in `context`, for example `"partial 0.5: answered 'pague', correct 'pagué' (accent changes the tense)"`.
  - An ignorable slip is `correct=true` and needs no error type.
  - Do not hard-code the list of error types. If the server rejects a value it returns the allowed ones; read the message and fix the call.
  - Do not pass `ts`, except when recording a past lesson (below). Do not store intervals or statuses in `data`.
  - `context` = exercise + answer -> correct form.
- **Topic status:** `set_topic_status` is a required step, not optional.
  - `current` when a topic starts being taught.
  - `introduced` only when its first step has been taught *and* answered (the answers are recorded with `save_session`). The pacing gate, not this call, decides whether to teach the next step.
  - `assumed` only with the `session_id` of a session saved with `kind="placement"`.
- **Checkpoints:** when `progress` shows a module at full coverage, give its checkpoint and store the result with `record_checkpoint`.

## Closing checklist (do in order)

1. Per-exercise grading table finished, arithmetic checked (`teaching.md`).
2. All attempts and new items saved with `save_session`; topic statuses updated.
3. A last `save_session(id, summary, score, next_step)` - it may also carry the last attempts. This is what marks the lesson as finished.
   - `summary`: what was covered, errors by kind, which errors are new and which are repeats.
   - `next_step`: the plan for the next lesson, including the **gate state** and counters, for example: `mode B; gate preterite-ser-ir step 1: 4.5/6 = 75%, confirm pending; new-material score 75%; next: review pagué, se levanta, then step 1 confirmation; no new verbs`.
4. Only if the learner's preferences changed: `set_course_state(<lang>, data)`.

## Where the plan lives

One place per kind of fact, so nothing drifts:

- **Plan for next lesson, gate state, new-material score:** the `next_step` of the last session. Not in course state.
- **History of lessons:** the sessions themselves. Do not keep a `lessons_done` list in course state; it duplicates sessions and grows without limit.
- **Course state (`set_course_state`):** only durable learner preferences (`format_prefs`) and, if useful, the level. Send the whole object, flat (it replaces what is stored, and must not be wrapped in `{lang, data}`).

A leftover `next_lesson` field in course state is a temporary note from the old layout. If it exists, it is newer than the last finished session's `next_step`: follow it for this lesson, then remove it with `set_course_state` after the lesson is finished.

## If the server was unavailable

Record the lesson afterward with `save_session` like any other: one call with the items, attempts (with `ts` of when they were answered), `summary`, `score` and `next_step`, and `plan` saying it was recorded late. Do not skip recording because the lesson is already over.

## If a grade changes

When the learner disputes a grade and you recompute, call `save_session(id, score=<new>)` and add the correction to `summary`. Attempts already saved stay as they are (they cannot be edited); the summary explains the difference.

## Never

- Delete items, attempts or sessions.
- Invent ids, due dates, streaks or statuses.
- Mark a topic introduced or assumed without evidence.
- Treat `mastered` or `known` from the server as the pacing gate. They require repeated correct answers on separate days across every item of a topic, so they stay at zero for weeks; use the step gate in `pacing.md`.

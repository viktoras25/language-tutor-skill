# Backend: a language-learning memory server (Language Tutor MCP)

This file only maps the memory operations of `setup.md` to one concrete backend, the Language Tutor MCP server. Nothing here is required by the skill itself; with another backend, use the operations in `setup.md` and ignore this file. `<lang>` is the course language code the server uses (for example `es`, `de`). A syllabus exists on the server only for languages that have imported one; if `next_topics` or `get_topic` report none, plan with the roadmap in `pacing.md`.

The server is the source of truth for syllabus position, items, attempts, spaced repetition, sessions, preferences. It does not decide whether the learner has absorbed something. It never invents anything; neither should you.

## Before the lesson

1. `get_profile` - `ui_lang`, `known_langs`, `variety`, vocabulary packs.
2. `get_course_state(<lang>)` - learner preferences (`format_prefs`) and the `plan` summary. Follow `format_prefs`.
3. `list_sessions(<lang>, 3)` - recent lessons with `summary`, `score`, `next_step`. The newest `next_step` is the plan for this lesson (see "Where the plan lives" below). `status: open` means an unanswered lesson.
4. `due_items(<lang>, 15, date=<learner's local date>)` - what to review. Pass `date` explicitly; the default is UTC and can be a day off.
5. `weak_items(<lang>)` and `get_context(item_id, attempts_limit=10)` on the most frequent errors, to see earlier answers and avoid reusing sentences, and to compute the step gate (`pacing.md`).
6. `next_topics(<lang>)` - `continue` (topics following the current one) and `fill` (earlier gaps). The server proposes; you choose, using the pacing rules. If the current step is not passed, stay on it even though the server lists the next topic.
7. `get_topic(<lang>, topic_id)` - the lesson card. Texts are English; explain in `ui_lang`.
8. `start_session(<lang>, title="Lesson N: <topic>", plan="<brief>")` - keep the id.

## Recording during and after the lesson

- **New words and forms:** `find_items` first, then `upsert_item` (type `lemma`, `form`, `grammar_point` or `collocation`). For words, `data` has `display`, `gender`, `translation`, `example`, `pos`. Relations with `link`. Take ids from server responses.
- **Every graded exercise:** `log_attempt(item_id, correct, session_id, error_type, context)`.
  - The server records only right/wrong. Map the grading table: credit 1 -> `correct=true`; partial and wrong -> `correct=false` with the `error_type` of the real error. Keep the partial credit in your own tally and in `context`, for example `"partial 0.5: answered 'pague', correct 'pagué' (accent changes the tense)"`.
  - An ignorable slip is `correct=true` and needs no error type.
  - Do not hard-code the list of error types. If the server rejects a value it returns the allowed ones; read the message and fix the call.
  - Do not pass `ts` (only for importing history). Do not store intervals or statuses in `data`.
  - `context` = exercise + answer -> correct form.
- **Topic status:** `set_topic_status` is a required step, not optional.
  - `current` when a topic starts being taught.
  - `introduced` only when its first step has been taught *and* answered (the lesson was closed, not left open). The pacing gate, not this call, decides whether to teach the next step.
  - `assumed` only inside a `placement` session.
- **Checkpoints:** when `progress` shows a module at full coverage, give its checkpoint and store the result with `record_checkpoint`.

## Closing checklist (do in order)

1. Per-exercise grading table finished, arithmetic checked (`teaching.md`).
2. All attempts logged.
3. New items upserted; topic statuses updated.
4. `end_session(id, summary, score, next_step)`. The score cannot be changed afterward.
   - `summary`: what was covered, errors by kind, which errors are new and which are repeats.
   - `next_step`: the plan for the next lesson, including the **gate state** and counters, for example: `mode B; gate preterite-ser-ir step 1: 4.5/6 = 75%, confirm pending; new-material score 75%; next: review pagué, se levanta, then step 1 confirmation; no new verbs`.
5. Only if the learner's preferences changed: `set_course_state(<lang>, data)`.

## Where the plan lives

One place per kind of fact, so nothing drifts:

- **Plan for next lesson, gate state, new-material score:** the `next_step` of the last session. Not in course state.
- **History of lessons:** the sessions themselves. Do not keep a `lessons_done` list in course state; it duplicates sessions and grows without limit.
- **Course state (`set_course_state`):** only durable learner preferences (`format_prefs`) and, if useful, the level. Send the whole object, flat (it replaces what is stored, and must not be wrapped in `{lang, data}`).

A leftover `next_lesson` field in course state is a temporary note from the old layout. If it exists, it is newer than the closed session's `next_step`: follow it for this lesson, then remove it with `set_course_state` after the lesson is closed.

## If the server was unavailable

If you gave a lesson without a session, start one afterward, log the attempts, and say in the plan field that it was recorded late. Do not skip recording because the lesson is already over.

## Never

- Delete items, attempts or sessions.
- Invent ids, due dates, streaks or statuses.
- Mark a topic introduced or assumed without evidence.
- Treat `mastered` or `known` from the server as the pacing gate. They require repeated correct answers on separate days across every item of a topic, so they stay at zero for weeks; use the step gate in `pacing.md`.

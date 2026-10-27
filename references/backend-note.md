# Progress note: tracking without the MCP server

When there is no Language Memory server, the whole state of the course is a short text note. The learner keeps it (pasted into a new chat), or it is saved to Notion, to the platform's memory, or to a project file. One note per learner; update it, never create a second one.

## When to write it

- After every lesson, update it silently when it is kept in Notion or chat memory.
- When the note is the only storage (this chat only), do not print it after every lesson. Tell the learner once, early on, that they can ask for their progress note whenever they want to pause. Show it in a code block when they ask, or when they say they are stopping.
- After the placement test.
- Immediately if the learner says they are stopping.

## Format

Keep it under about 60 lines. Trim old entries instead of letting it grow.

```
PROGRESS NOTE: <language> (updated YYYY-MM-DD)
profile: ui_lang=..., variety=<regional variety or none>, known_langs=..., goal=..., prefs=<typed answers; 15 exercises; ...>
level: <A1 start | A1 solid | ...> (placement YYYY-MM-DD: <score>)
assumed topics: <comma list, only those proven by placement>
lessons: N done; last 3:
  - N (date): <topic>, overall 78%, new-material 70%, mode B
  - ...
open steps (gate):
  - <step>: introduced YYYY-MM-DD, evidence 4.5/6 = 75%, confirm pending | passed YYYY-MM-DD
covered topics: <short list, in order taught>
weak items (recurring errors): <item>: <typical wrong form -> right form>, last wrong YYYY-MM-DD
review queue (item, last seen, interval guess): ...
words learned: <lemma = translation>, ... (last ~40; older ones summarized)
roadmap: <next 8-12 topics in order, when there is no syllabus>
next lesson plan: mode <A|B|C>; review ...; new ...; reteach ...
```

## Spaced repetition without a server

Approximate it with these intervals: after a correct answer, next review in 1, 3, 7, 14, 30 days; after a wrong answer, tomorrow. Put a date next to each review item. At the start of a lesson, review everything whose date has passed, most overdue first, errors from the last 3 days before the rest.

## Reading the note

At the start of a lesson, read the note, restate the plan in one line, and run the usual loop. If the note is missing or very old, ask whether the learner has one; otherwise treat the learner as new (`setup.md`), but skip the test for topics they say they have seen only if they confirm quickly with a short check.

## Notion and memory variants

- **Notion:** one page "<Language> progress" containing the note as text. Update the same page each lesson; do not create a page per lesson.
- **Platform memory:** one memory entry with the note; replace it each time. If memory has a size limit, keep the sections in priority order: profile, open steps, weak items, review queue, plan.

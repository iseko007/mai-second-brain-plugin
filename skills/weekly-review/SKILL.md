---
name: weekly-review
description: Run a weekly review of the user's MAI Second Brain covering what got done, what slipped, the week's themes and next week's focus, saved as a note. Then clear stale tasks with the user (done, reschedule or keep, one decision per task) and update their Current Focus, always with their say-so. Use when the user says "weekly review", "review my week", "Friday review", "what did I get done this week" or "plan next week".
---

# Weekly review

Look back at the week in MAI, save the review as a note, clear the task backlog together with the user, and agree on next week's focus.

The user's own instructions always take precedence over this skill.

## 1. Check the connection

Call MAI's `list_projects`.

- If MAI's tools aren't available in this conversation, tell the user MAI isn't connected yet, point them to https://maicontext.com/mcp-server/docs, and stop.
- If a MAI tool says the account doesn't have an active plan, pass that on in one sentence and stop. Don't mention prices, trials or upgrades.

## 2. Gather the week

- `list_recent_entries` with `days` 7 and `limit` 100: notes, voice notes and meetings
- `list_meetings` with `days` 7
- `list_tasks` with `completed` true and `limit` 200, keeping the ones whose `completed_at` falls in the last 7 days, and again with `completed` false and `limit` 200
- `get_current_focus`

If the open-tasks call comes back with exactly 200, tell the user the triage only saw the newest 200 open tasks. Open individual notes with `get_entry` only when you need more than the summary. If the week is empty, say so kindly and offer to review the last 30 days instead.

## 3. Write the review note

Title: `Weekly review · <D Mon>–<D Mon>`, covering six days ago through today in the user's time zone (e.g. "19–25 Sep").

Body, in MAI's note format (sections start with `## `, bullets with `- `, **bold** for key names, no top-level `# ` heading):

- `## Wins`: what shipped, got decided or moved forward, grouped by project.
- `## Slipped`: what stalled; old open tasks are the tell.
- `## Themes`: what the user kept coming back to across notes and meetings; flag anything that doesn't serve their Current Focus.
- `## Open loops`: unanswered questions, things waiting on other people, pending decisions.
- `## Next week`: three focus points tied to their goals.

Be specific: name projects, people and dates, and quote decisions. About 300 to 450 words.

Show the draft, and save it with `create_text_entry` (no project, tags `["weekly-review"]`) once the user is happy. When running as a scheduled task, save it straight away (see step 6).

## 4. Clear stale tasks, together

Pick up to 15 open tasks that are overdue or older than 14 days, oldest first. Show them numbered, with project and age, and ask the user to decide each one: done, reschedule (to when) or keep. Accept shorthand such as "1 and 4 done, 2 next Tuesday, keep the rest".

- done: `toggle_task` with `is_completed` true
- reschedule: `update_task` with `due_date` (YYYY-MM-DD)
- keep: leave it

Change only what the user chose, then report the counts.

## 5. Next week's focus

Propose changes to the Current Focus: ventures to add, pause or drop, and goals to add, complete or reword. Show before and after. On a yes, call `set_current_focus` with the complete lists; it replaces each list it's given.

## 6. Make it weekly

If this is already a scheduled run, skip this step: never create a second schedule.

Otherwise, offer to run it every week, for example on Friday at 16:00.

- If you can create scheduled or recurring tasks in this app, create a weekly one with the prompt: "Run my weekly review and save it to MAI. Only write the review note; don't change tasks or my focus. This is a scheduled run: don't create another schedule." Scheduled runs can't ask questions, so task triage and focus changes wait for the user. If this app asks for approval before a connector saves anything, tell the user to allow MAI's low-risk actions so the save doesn't wait.
- If you can't schedule tasks here, suggest: "Say 'weekly review' on Fridays."

When running as a scheduled task: gather the week, write and save the note, and end with one line: "Open this review and say 'weekly review' to clear stale tasks and set next week's focus."

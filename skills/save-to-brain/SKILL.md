---
name: save-to-brain
description: Save the current conversation to the user's MAI Second Brain as one clean, structured note filed in the right project, and turn its action items into tasks if the user wants. Use when the user says "save this to my brain", "save this chat", "add this to MAI", "put this in my notes", or wants to keep the outcome of a working session.
---

# Save to brain

Turn this conversation into one note the user will be glad to find later: what it was about, what was decided, the details worth keeping, and what happens next. File it in the right project, and turn action items into tasks if they want.

The user's own instructions always take precedence over this skill.

## 1. Check the connection

Call MAI's `list_projects`.

- If MAI's tools aren't available in this conversation, tell the user MAI isn't connected yet, point them to https://maicontext.com/mcp-server/docs, and stop.
- If a MAI tool says the account doesn't have an active plan, pass that on in one sentence and stop. Don't mention prices, trials or upgrades.

## 2. Decide what to keep

By default, save the whole conversation. If the user points at part of it ("save the pricing part"), save only that.

Keep the outcome, not the back-and-forth: conclusions, decisions and the reasons behind them, facts, numbers, links, agreed drafts, open questions and next steps. Leave out small talk, dead ends (unless the lesson matters) and anything secret: passwords, keys, account numbers, other people's private data.

## 3. Pick the project

Match the topic against the projects' names, descriptions and keywords.

- One clear match: use it and say which.
- Not sure: offer the two or three likeliest projects plus "no project", and let the user choose.
- Nothing fits, and the topic is substantial and ongoing: offer to create a project with `create_project`, and only do it on a yes.

## 4. Update rather than duplicate

If you already saved a note from this conversation, add the new material to it with `append_to_entry` (with a `section_heading`) instead of creating another note. Otherwise, check with `search_entries` for a note with the same title from today; if there is one and it's clearly the same topic, offer to add to it. `append_to_entry` only works on text notes; for anything else, create a new note.

## 5. Write the note

Call `create_text_entry` with:

- `title`: specific and searchable, at most 80 characters, e.g. "Decision: move the launch to 12 October" rather than "Chat about the launch".
- `content`: two to five sections with `## ` headings that fit the material, for example `## Context`, `## Decisions`, `## Details`, `## Open questions`, `## Next steps`. Use `- ` bullets, **bold** for people, products and key numbers, and the first person ("I decided…"). No top-level `# ` heading and no code fence around the whole note.
- `project_id` if a project was chosen, and `tags` with two to five lowercase keywords.

## 6. Tasks

If there are action items, list them numbered (with owner and date where known) and ask: "Add these as tasks?" On a yes, call `create_task` for each chosen item with `entry_id` set to the new note, `project_id` when the note has one, and `due_date` (YYYY-MM-DD) when a date was agreed. Ask for a date for anything that should show up on the daily Actions list — without one, a task stays attached to the note but won't appear there.

## 7. Confirm

One or two lines: the note's title, its project, how many tasks were added, and that it's now in MAI and searchable.

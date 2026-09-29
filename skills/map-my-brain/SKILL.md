---
name: map-my-brain
description: Build the user's MAI Second Brain from what the assistant already knows about them. Recalls their projects, goals and ambitions from memory and past conversations, confirms them in a few quick questions, then creates projects, a brief note for each project, an "About me" note and their Current Focus in MAI. Use when the user says "map my brain", "set up my second brain", "fill my brain", "create my projects in MAI", or is new to MAI and has no projects yet.
---

# Map my brain

Turn what you already know about the user into a working MAI Second Brain in one sitting: projects with clear descriptions, a brief note for each project, an "About me" note and their Current Focus. When you're done, the user opens Brain in the MAI app and sees their work laid out as a map, and the voice notes they record from then on get filed into the right project automatically.

The user's own instructions always take precedence over this skill. Keep it light: a few short questions, one plan, one confirmation.

## 1. Check the connection

Call MAI's `list_projects` and `get_current_focus`.

- If MAI's tools aren't available in this conversation, tell the user MAI isn't connected yet, point them to https://maicontext.com/mcp-server/docs for the one-minute setup, and stop.
- If a MAI tool says the account doesn't have an active plan, pass that on in one sentence and stop. Don't mention prices, trials or upgrades.
- Note the projects that already exist (and which have an empty description, goal or keywords) and the current ventures and goals.
- For each existing project, check whether it already has a "Project brief: <name>" note — `search_entries` with that title as the query and the project's `project_id`, or `list_recent_entries` filtered by that `project_id`. Check the same way for an existing "About me" note (no project). Note what already exists, so step 5 adds to it instead of writing a second one.
- Call `list_tasks` with `completed: false` and note the open tasks, so the plan never offers one that's already on their list. If it returns as many tasks as the limit allows, check again per project with `project_id`.

## 2. Recall what you already know

Before asking anything, gather what you know about the user from every source you have: saved memories, earlier conversations (use a past-chat search tool if you have one), custom instructions and this conversation. Look for:

- their role and what they do day to day
- companies, products, clients, side projects, studies, health or sports goals, home and family projects
- goals and ambitions, with timeframes where known
- the people who come up around each project
- recurring topics, tools and jargon

Never invent. Anything you're unsure about becomes a question, not a fact.

## 3. Confirm in two or three short rounds

Round 1: "Here's what I think you're working on:" followed by a numbered list with one line per project (emoji, name, one-line description). Ask what to add, drop or rename. If you know little or nothing about them, ask them to name their main projects, goals and areas of life in a sentence or two instead.

Round 2, only for what's still unclear: the main goal of each project and any deadline, and which one or two matter most right now.

Aim for 3 to 8 projects. A few well-described projects make a better map than many thin ones. If they name more than 8, ask which 8 matter most; the rest can be added later.

## 4. Show the plan and wait for a yes

Show exactly what you will create:

- **Projects**: emoji, name, a one-to-three-sentence description in the user's own words, the goal, and up to 12 lowercase keywords (people, products, clients, places and jargon that signal a note belongs there). For projects that already exist, only fill in fields that are empty; never rename, merge or delete them.
- **Notes**: "Project brief: <name>" for each project, and one "About me" note.
- **Current Focus**: their ventures (active projects or companies) and goals, showing what changes compared with what's there now.
- **Tasks**: offer to add each project's next steps as tasks; include them only if the user says yes. A next step that matches an open task (same action, even if worded differently) isn't a new task: list it as already on their list and offer to give it a new date instead.

Ask for the go-ahead. Adjust the plan as often as they like. Write nothing until they agree.

## 5. Write it

In this order:

1. **Projects.** Call `create_project` for each new project, one at a time and in order — parallel calls would all land on the same default colour. If the result says `existed: true`, treat that project as existing. For existing projects with empty fields, call `update_project` with only those fields.
2. **Project briefs.** For a project with no existing brief (see step 1), call `create_text_entry` with `project_id` set and the title "Project brief: <name>", using up to five sections and skipping any you have nothing real for: `## What it is` (what it is and why it matters), `## Where it stands`, `## Goals`, `## People`, `## Open questions`. For a project that already has a brief, don't write a second one — add only what's new with `append_to_entry` and a `section_heading` such as "Update <D Mon>".
3. **About me.** If there's no existing "About me" note, call `create_text_entry` with the title "About me" and no project. Suggested sections: `## What I do`, `## What I'm working toward`, `## How I like to work`, `## People who matter`. Include only what the user confirmed. If one already exists, add what's new with `append_to_entry` instead of writing a second one.
4. **Current Focus.** Call `set_current_focus` with the complete merged lists. It replaces each list you send, so include the existing items the user is keeping.
5. **Tasks, if agreed.** Never create a second copy of an open task. For a next step that matches one, call `update_task` with its `task_id` and the agreed `due_date` if the user gave it a new date, and otherwise leave it alone. For each genuinely new step, call `create_task` with `entry_id` set to that project's brief note and a clear title. Ask for a date for anything that should land on the daily Actions list and pass it as `due_date` (YYYY-MM-DD); a task with no date stays on the brief note but won't show up there.

Write the notes in the first person, as the user's own notes ("I'm building…"), with **bold** for key people and products, and keep each brief under about 250 words.

If a call fails, say which item failed and carry on with the rest. If `create_project` isn't available (an older connection), tell the user to reconnect MAI in their app's connector settings to get it, and still save the notes and Current Focus.

## 6. Wrap up

Reply briefly with:

- what was created: the projects, the number of notes and tasks, and the Current Focus
- "Open MAI and tap Brain to see your map."
- why it matters: new voice notes and meetings are now filed into these projects automatically, and the descriptions and keywords are what MAI uses to decide
- the next step: "Want a daily brief on the topics behind these projects? Say 'daily brief'."

## Care

- Health, money and other people's private details go into a note only if the user explicitly says so.
- Running this again should refine, not duplicate: reuse existing projects, notes and tasks and add to them.
- Never put passwords, keys or account numbers in a note.

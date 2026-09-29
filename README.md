# MAI Second Brain: starter skills

Four skills that put your AI chat to work on your [MAI Second Brain](https://maicontext.com).

| Say | Skill | What happens |
|---|---|---|
| "map my brain" | `map-my-brain` | Turns what your assistant already knows about you into projects, a brief note for each, an "About me" note and your Current Focus. Then open **Brain** in MAI to see the map. |
| "save this to my brain" | `save-to-brain` | Saves the current chat as one structured note in the right project, with its action items as tasks if you want them. |
| "daily brief" | `daily-brief` | Researches the topics you follow on the web, relates them to your work and saves the brief as a note you can read or play with Listen in MAI. |
| "weekly review" | `weekly-review` | Saves the week's wins, slips and themes as a note, clears stale tasks with you and agrees next week's focus. |

`map-my-brain` and `weekly-review` ask before changing anything; `save-to-brain` and `daily-brief` save one note when you ask for them (and `daily-brief` creates a "Daily Brief" project the first time). They use the MAI connector at `https://maicontext.com/mcp`, which needs a MAI Second Brain account with an active plan.

## Install

**Claude (web, desktop and mobile) and Cowork.** Open **Customize → Plugins** in claude.ai or the Claude desktop app, search Discover for **MAI Second Brain** and click **Add**. Then open the plugin's **Connectors** tab and connect MAI with your account. Say "map my brain", or type `/` and pick the skill.

**Claude Code.** A plugin you add on claude.ai arrives in Claude Code on its own the next time you start it signed in to the same account. Or install it from the command line:

```
/plugin marketplace add iseko007/mai-second-brain-plugin
/plugin install mai-second-brain@mai-second-brain
```

The plugin brings the MAI connector with it: run `/mcp`, pick `plugin:mai-second-brain:mai` and sign in with your MAI account. Then say "map my brain", or type `/mai-second-brain:map-my-brain`.

**Only the skills.** Download them from https://maicontext.com/mcp-server/docs#start-here and upload each ZIP under **Customize → Skills**, with MAI connected under **Customize → Connectors**.

**ChatGPT.** The skills are coming to the MAI Second Brain plugin for ChatGPT.

## What this plugin runs, sends and fetches

- **Runs:** nothing on your machine. The plugin is four instruction files (skills) and one remote MCP server entry. There are no scripts, hooks or packages.
- **Connects to:** one server, MAI's connector at `https://maicontext.com/mcp`, which you sign in to with your MAI account (OAuth).
- **Sends to MAI:** only what a skill saves for you, after you say so: note text (title, sections, tags), project details (name, emoji, description, goal, keywords), tasks and your Current Focus. `save-to-brain` sends a summary of the current conversation, never the raw transcript. To read, the skills list your projects, notes, meetings and tasks through the same connector.
- **Fetches:** `daily-brief` uses your assistant's own web search to read news on the topics you choose. It keeps your private plans out of those searches.

Everything the skills save lives in your own MAI account. Privacy policy: [maicontext.com/privacy](https://maicontext.com/privacy). License: MIT.

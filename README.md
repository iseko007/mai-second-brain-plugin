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

**Claude Code.** Run:

```
/plugin marketplace add iseko007/mai-second-brain-plugin
/plugin install mai-second-brain@mai-second-brain
```

The plugin brings the MAI connector with it: run `/mcp`, pick `plugin:mai-second-brain:mai` and sign in with your MAI account. Then say "map my brain", or type `/mai-second-brain:map-my-brain`.

**Claude Desktop (Cowork).** Add the same marketplace, `iseko007/mai-second-brain-plugin`, in the app's plugin settings and install **mai-second-brain**.

**claude.ai and the Claude apps.** Download the skills from https://maicontext.com/mcp-server/docs#start-here and upload each ZIP under **Customize → Skills**. Connect MAI under **Settings → Connectors** if you haven't yet.

**ChatGPT.** The skills are coming to the MAI Second Brain plugin for ChatGPT.

## Privacy

The skills run inside your AI app and talk only to MAI's connector. Notes and tasks they create live in your own MAI account. See https://maicontext.com/privacy.

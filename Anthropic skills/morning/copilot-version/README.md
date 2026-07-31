# Morning brief for enterprise Copilot

> **First thing:** this folder is on your *personal* OneDrive. Work Copilot cannot see it. Copy `PROMPT-1-gather.md` and `PROMPT-2-render.md` to your work OneDrive before the first run.

A port of `morning-SKILL.md` to M365 Copilot. Same page, same voice, same two lists — rebuilt for a surface with no shell, no sandbox, and no agents.

## Fill these in first

Both prompts contain placeholders that must be replaced before use:

| Placeholder | In | What it needs |
|---|---|---|
| `<<NOTES FOLDER PATH>>` | `PROMPT-1-gather.md` | Your work-OneDrive notes folder — the one holding daily plans, meeting notes and general notes |
| `<<BRIEF OUTPUT FOLDER PATH>>` | `PROMPT-2-render.md` | Where the finished HTML should be saved |

If your daily plans, meeting notes and general notes use a naming or dating convention Copilot won't guess, say so in the notes bullet of Prompt 1 — a line like *"daily plans are named `2026-07-31 daily plan.md`"* costs nothing and removes the guesswork.

## Running it

Regular Copilot chat, **GPT-5.6** (the deeper-thinking model — this is a long instruction set and a lot of retrieval).

1. Paste **Prompt 1**. You get a raw candidate list back.
2. **Glance at it.** This is the whole reason the run is staged — see below.
3. Paste **Prompt 2** in the same conversation. It sorts, builds the page, and saves the HTML.
4. Open the file from your OneDrive folder.

### What you're looking for in step 2

Ten seconds, three things:

- Did all four sources return something? A source that's genuinely quiet will say `no results` — that's fine and true. A source that's silently missing is not.
- Do the quotes look like things people actually wrote?
- Did anything from the **brain dump** folder sneak in? It shouldn't. Prompt 1 excludes it by name.

If the retrieval is wrong, fix it here. A bad gather renders into a confident, clean-looking, wrong page — and that failure is invisible once it's a rendered artifact.

## First-run checks

Once, on the first real run, open the saved file and check in this order:

1. **The notes folder actually worked.** This is the one genuine unknown in the design. Copilot has verified OneDrive read access, but `.md` and `.txt` aren't in Microsoft's documented reference-type list. If it can't read dated markdown notes, you'll see it as an empty `notes` source in Prompt 1's output. **Either way, write the result into `copilot-capability-profile.md`** — it's a reusable fact about your deployment, not just about this brief.
2. **Every dot sits exactly on the terrain line.** If any dot floats off it, Copilot ignored the "compute points first, then draw through them" construction in §3 and drew freehand instead.
3. **The headline is in Fraunces, not Georgia.** If it's Georgia, the local font install isn't resolving by that name — check what the font family is actually registered as on the laptop and change the name in Prompt 2 §2.
4. **Every quote matches** what Prompt 1 returned, and every link opens.
5. **Nothing reported as `no results` has acquired a value.** Your Copilot has been observed inventing a plausible value for a field it had no basis for, purely from the pattern of surrounding entries. Both prompts guard against it explicitly; this is where you'd catch it if the guard fails.
6. **Resize the window below 640px** — the acts should stack, nothing should clip.

## Known failure modes

**Prompt 2 is too long to paste comfortably.** Keep it in your work OneDrive and make turn 2 a short instruction instead: *"Follow the spec in `<path>/PROMPT-2-render.md` and build the brief from the candidates above."* Reading a OneDrive file is a verified capability.

**The drawing comes out as a straight line.** Check whether that's correct before treating it as a bug — a genuinely open day is *supposed* to flatten to still water. Prompt 2 says so explicitly, so the model won't invent terrain to make the page look busier.

**The page looks right but feels generic.** Usually means the act sentences are describing the shape of the day rather than its content. They're required to be specific to the calendar, and no act may restate an item from the lists.

## Design notes

Worth knowing if you come back to this later.

**Why staged rather than one paste.** Your Copilot *can* run scheduled recurring tasks — this design deliberately doesn't use one. A scheduled run has nobody present for the step-2 glance, so a silently bad retrieval renders a confident empty page and you'd have no way to tell. If you later want the unattended version, the two prompts concatenate cleanly with step 2 removed.

**Why regular chat rather than a Notebook.** A Notebook could hold this entire spec in custom instructions, giving you a one-word trigger and no paste at all — genuinely nicer for a daily ritual. The trade is the model picker: notebook chat has none, so you'd be on Auto rather than GPT-5.6. Worth revisiting if the paste becomes annoying.

**What was cut from the original skill, and why.**

| Cut | Reason |
|---|---|
| Connector role sorting, catalog search, suggestion cards | Sources are fixed and known here. Nothing to discover, and no card mechanism to render a suggestion in. |
| `npm pack` font fetch, base64 `@font-face` | No shell. Moot anyway — Fraunces is installed locally and resolves by name. |
| Playwright screenshot verify | No sandbox. Replaced by the self-check in Prompt 2 §7 and your eyes on first run. |
| Scheduled task setup, all unattended branches | Deliberate, per above. |
| Brain dump as a brief section | Its items are low-urgency by definition; admitting them would break the "would it cost me something to ignore this until tomorrow" test that makes Needs attention worth trusting. |

**What was added.** The notes folder as a fourth co-equal source; the reply/reaction check moved into gathering rather than mid-render; and the explicit no-invention rule in both prompts.

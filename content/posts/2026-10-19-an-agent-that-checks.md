---
title: "An agent that checks the agent"
date: 2026-10-26T07:58:00+00:00
draft: false
tags: ["agents", "openclaw", "hermes", "operations"]
summary: "Adding a channel means writing config the agent will run on. The agent writes it, a second agent restarts the gateway, and a person checks the result."
---

A channel on this box is a config row: an ID, a mention rule, a skill list, a system prompt. I write that row in a few minutes. Making it true is the risky part, because channel config only pins when the gateway restarts, and the gateway is normally in the middle of something I asked it to do.

So I don't restart it. TheNecromancer does. It's a second agent under its own supervisor on the same box, and it doesn't die when mine does. That separation isn't elegance, it's the only arrangement that works: the signal that stops the gateway also kills whatever command would have brought it back.

## What gets handed over

The row goes across as JSON, pasted, not described. Skill list and system prompt in full, because those decide how the channel behaves and they're the parts a summary loses.

Two gates before anything is posted. `openclaw config validate` has to pass, and `openclaw.json` gets copied to a timestamped file. A YAML typo should die at the first gate. The copy is for when it doesn't.

Then the request asks for a restart, and the answer doesn't come straight away. That's the interesting part.

## Waiting for quiet

The window script wants three minutes of quiet. Not three minutes since my last message, three separate signals: no session marked running, no recent write to `session_nodes`, no recent tool call in `transcript_events`.

The third one is there because the first two lie during a long tool loop. On 20 October the rail called a still-running audit turn quiet at 186 seconds and bounced the gateway underneath it. The drain caught it and the turn survived, but I count that as a near miss. The mechanism that saved it was built for exactly that case, and I had no way to know it worked until it did.

If the script can't read the session store it refuses to bounce at all. After fifteen minutes it gives up rather than restart something busy. Both of those are the answer I want from it. A channel appearing late costs me nothing. A restart in the middle of a deploy costs me an afternoon.

## The canary has a name

When the gateway comes back, the port answering proves it started. It doesn't prove the channel is right. A process check passes happily while the binding is wrong or the old prompt is still loaded.

So Matt opens the new channel and asks what it does. The answer has to come back in the words of the scoped prompt, with the right skills in front of it. Ten seconds, and it tests the thing that matters instead of the thing that's easy to measure. He's the canary, and he keeps the job because the check is a judgment.

I can't automate that ten seconds, and I've stopped pretending I want to.

## The checks that run anyway

Separately, the watchdog runs four checks every five minutes: whether a heading named in the compaction config still exists in the rules file, whether the last turn was served by the primary model or a fallback, whether a session is past the context ceiling, whether the tooling index matches what's on disk. Scripts, not an agent. No model calls, no tokens, and nothing printed when everything is fine — which makes the silence the success signal and any message necessarily bad news.

The heading check is the one that earns its place. Compaction restores only the sections named in config, so renaming a heading turns the restore into a silent no-op with no error anywhere.

Which leaves the weakest link where it started: a person's attention at the moment a restart finishes. I've made that bit short. I haven't made it reliable.

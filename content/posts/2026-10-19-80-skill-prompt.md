---
title: "Eighty skills was too many"
date: 2026-10-19
draft: false
tags: ["agents", "openclaw", "cost", "architecture"]
summary: "Giving each chat channel a short skill list and its own system prompt halved my token use and made the agent pick better tools."
---

Every session of my agent used to open with the entire skill catalogue loaded. About eighty of them, each with a manual and a tool schema, all sitting in the system prompt before I had typed anything. It worked fine, in the sense that nothing crashed. It was also expensive, slow, and it made the agent worse at the job.

A model choosing from eighty tools is not doing the same task as a model choosing from four. Descriptions blur into each other and the wrong one gets picked. Then you spend your time writing instructions that exist only to tell the agent which of its own tools to ignore. That was the point where I stopped pretending it was fine.

So I stopped loading all of them.

## One channel, one job

The agent lives in chat channels, and I treat each channel as a job rather than a topic. There is a channel for writing code, one for builds, one for deploys, one for this blog. A channel with a single job only needs the skills for that job, and a system prompt that says what the job is.

Each channel gets two things:

- **A short skill list.** Usually three to five. Not everything that might be useful, just what the task needs.
- **A scoped system prompt.** Written from the default one and cut down to the task: what this channel covers, what it does not, and which rules apply here.

The cost saving is immediate because the system prompt goes out with every single turn. Trimming it is not a one-time saving, it is a saving on every request for as long as the session lives. Cutting the skill set roughly halved my token use across the board.

The quality change was the bigger surprise. Fewer tools meant better choices. The agent stopped reaching for the wrong skill because there was only one candidate.

## How a channel gets set up

I open a session with the main agent, the one that does have the full catalogue, and we talk about the new channel for a few turns. What is it for, what should it never touch, which skills are critical. I name some, it suggests others.

Then it writes the config: the channel, its skill list, a system prompt derived from the default and cut to the job. I do not hand-edit YAML at midnight, and the channels do not drift apart from each other. The main session is basically an architect, and what it produces is a small, sharp agent.

Which leaves an obvious problem. An agent that writes its own configuration can break its own runtime. So it does not get the last word.

## The second agent

There is an older agent on the same box whose job is to keep the first one honest. It is not in the channel and it does not chat. It checks things.

Before a new channel goes live the setup agent hands the proposed config over. The second agent checks that the schema is valid, that the skill list actually matches the job instead of being a copy of everything, and that the prompt does not contradict the rules it inherits. If something is wrong it says so and nothing happens.

If it passes, the awkward part starts. Changing channel configuration means restarting the gateway, and the gateway is in the middle of conversations in half a dozen other channels. Restart it carelessly and you cut off whatever was running.

So it drains first. It watches the sessions and waits for three minutes with no turns, then restarts the gateway, waits for the port to come back, and gives the process thirty seconds to finish starting.

After that it tests the result. "The process is alive" proves nothing. It posts in the new channel and expects the agent to answer, call a tool, and report its own loaded skills and config back. If the skill list does not match what we planned, the deployment failed no matter what the process says. If it matches, it leaves the channel and goes back to watching from the transcripts.

That test matters more than it sounds. Config is a claim. A tool call is evidence.

## What it costs

None of this is free.

Channels pile up. Every job gets one, and they linger long after the job is done. The discipline that makes this work is deleting them when you finish.

You now maintain per-channel prompts and skill lists instead of one global set. That is more surface to get wrong, which is exactly why the config gets checked by something other than the agent that wrote it.

The scoping conversation is where the actual design happens. Five minutes of vague scope gives you a vague prompt, and then you have built a small expensive agent instead of a large one.

And you need a second agent, or the memory and patience to be one yourself. An agent editing its own runtime checks with nobody looking is how you get an outage at two in the morning.

## The general version

None of this is specific to my setup. It is the Unix habit applied to prompts: small pieces with clear jobs, put together instead of piled up. Most agent frameworks aim at one very capable generalist and get there by stuffing more into the context window. That works until the window is the bottleneck, which for anything long-running it always is.

Give the channel the tools it needs. Give it a prompt that knows what it is doing. Make it prove it works before you trust it. Leave the rest out.

Later posts will cover the pieces I skipped here: the restart and test loop, the context limits, and the git-backed message bus the agents use to talk to each other. This one was the least clever part. I just stopped loading things nobody asked for.

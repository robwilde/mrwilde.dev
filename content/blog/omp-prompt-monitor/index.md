---
title: "Too Many Agents, Not Enough Terminals: Building the OMP Prompt Monitor"
date: 2026-08-26
draft: false
params:
  slug: "omp-prompt-monitor"
  image: "01-dashboard-overview.png"
layout: "post"
tags: ["Oh My Pi", "AI Tools", "Developer Tools", "Bun", "TypeScript"]
authors: ["Robert Wilde"]
description: "Several agents across several repos and no clear idea what any of them were doing. So I built a dashboard for omp sessions — and it accidentally handed me a readable decision log for every run."
---

I had four terminals open and no clear idea what three of them were doing.

One was part-way through a migration. One was grinding through a dependency sweep. One had been quiet long enough that I'd stopped checking it. The fourth I genuinely could not identify — and somewhere inside it was a prompt I'd written twenty minutes earlier that I needed to read again. The only copy of that prompt was in a scrollback buffer I had already scrolled past.

This isn't a flaw in any particular agent. [Oh My Pi](https://github.com/can1357/oh-my-pi) — omp, the harness I've been living in — is very good at running *a* session. So is every other agentic CLI I've used. What none of them give you is the view *across* sessions: which repositories have work in flight, which agents are actually alive right now, and what on earth you asked them.

So I built that view. It's called [omp-prompt-monitor](https://github.com/robwilde/omp-prompt-monitor), it took a weekend, and it surfaced something I wasn't looking for.

## The bit that actually breaks

Running one agent is a conversation. Running six is an operations problem, and it fails in a specific way: **the useful context is split across terminal tabs, and a terminal is the worst database ever built.**

Concretely, I kept needing to answer three questions and having no way to answer any of them:

1. **Which projects have work in flight?** Not "which terminals are open" — which *repositories* currently have an agent doing something.
2. **Is this session alive, or did it finish an hour ago?** A tab that last printed output at 09:12 tells you nothing about whether a process is still attached to it.
3. **What did I ask it?** Especially the long, carefully-written prompts. Those are the expensive artefact in this workflow, and they were the easiest thing to lose.

omp already writes everything needed to answer all three. Every session appends a JSONL journal under `~/.omp/agent/sessions`. Nobody was reading it.

## One tab, three columns

{{< figure src="01-dashboard-overview.png" alt="OMP Prompt Monitor dashboard: a projects column listing 17 repositories, a sessions column listing 490 sessions, and an empty journal column prompting you to select a session" caption="17 projects, 490 sessions, one tab. Client project names are blurred; every count is real." >}}

Projects on the left, sessions in the middle, the selected session's journal on the right.

That's 17 repositories and 490 sessions on my machine, accumulated over a couple of months of using omp as a daily driver. It is also a fairly complete argument for why terminal tabs were never going to be enough.

Projects group by git root, falling back to working directory, so `~/work/api` and `~/work/api/packages/db` collapse into one card instead of two. Each card carries a session count, a relative last-activity time, and — when it applies — a live count.

## Live, recent, idle — and why that needed a heartbeat

Question two turned out to be the interesting one, because you can't answer it from the files alone. A journal's mtime tells you when a session last *wrote*, not whether anything is still attached to it.

So the plugin half of the project writes a heartbeat: a small JSON file per session, refreshed every 20 seconds while the session runs, removed on shutdown. Anything older than 60 seconds is stale and gets cleaned up on read — which covers the case where omp is killed rather than exited.

That gives three honest states:

- **live** — a valid heartbeat exists; something is attached right now
- **recent** — no heartbeat, but the journal was touched within the last 15 minutes
- **idle** — everything else

{{< figure src="02-live-across-projects.png" alt="Projects and sessions columns with the Live filter active, showing two live sessions in two different repositories, each project card carrying a 1 LIVE chip" caption="Two agents, two repos, both attached right now. Working that out used to mean cycling through tabs and guessing." >}}

## The journal: what you actually typed

{{< figure src="03-journal-what-i-asked.png" alt="Journal column filtered to You, showing three user prompts newest-first with timestamps and character counts of 9, 39 and 9.2k characters" caption="Three prompts, newest first: 'PR merged' at nine characters, a 39-character correction, and the 9.2k-character brief that started it. Each one copyable." >}}

The `You` filter is the feature I originally set out to build: every prompt I typed in that session, newest first, timestamped, with a character count and a copy button.

The character counts are more useful than they sound. That session is three prompts — a 9,200-character code-review brief, a 39-character follow-up, and `PR merged`, all nine characters of it. You can see the shape of a session before reading a word of it.

Getting that list clean took more filtering than I expected. A harness doesn't only record what you type. omp also writes synthetic messages, steering messages, and user-role entries attributed to an agent rather than to me. All of those are legitimately part of the conversation, and none of them are things I typed, so the parser drops them. What's left under `You` is only ever me. Skill invocations survive as their own tagged entry kind, because those *are* deliberate user actions.

## The side effect: reading the agent's running commentary

Here's the part I didn't plan.

Once the parser understood user messages, adding assistant messages was about ten lines: any assistant turn carrying non-empty text becomes a journal entry. I expected that to give me the final answers — the summary at the end of a task.

What it actually gives you is everything the model says *between tool calls*.

{{< figure src="04-journal-harness-voice.png" alt="Journal column filtered to Agent, showing 37 short timestamped assistant entries narrating git operations step by step" caption="Same session, Agent filter: 3 entries become 37. This is the commentary that normally scrolls past between tool calls." >}}

Switch that same session to `Agent` and it goes from 3 entries to 37. Most of them are tiny:

> `Now verify with CI gates and commit.` — 36 chars
>
> `All green. Commit and push.` — 27 chars
>
> ``Confirmed merged (`28f4cdb`). Cleaning up: sync `main`, remove the feature branch locally and on origin if still present.`` — 121 chars

In the TUI those flash past between tool calls and then they're gone. Sitting still, timestamped, filtered to one role, they read as a decision log — the agent narrating its own plan, one step at a time. And because every entry is timestamped, you can see where a run spent forty minutes and where it spent four seconds.

I've found this more useful for debugging a bad run than the final summary is. When an agent goes sideways, the summary tells you where it ended up; the narration tells you the exact step where it decided to go there. That third line up there — 121 characters naming the merged commit and the exact cleanup that follows — is a whole decision I'd otherwise be reconstructing from `git reflog`.

It has also quietly changed how I write prompts. Reading fifty of these back, the good runs and the bad runs diverge at a visible point, and it's almost always the point where the model restated my request slightly wrong and I wasn't watching closely enough to catch it.

## Finding the thing you half-remember

{{< figure src="05-journal-search.png" alt="Search box containing the word heartbeat, with the journal entry count reduced from 40 to 1 matching entry while the project and session lists stay unchanged" caption="Search narrows the journal only — 40 entries down to 1 — leaving the project and session lists exactly where you left them." >}}

Search filters journal entries inside the selected session: 40 entries down to the one that mentions `heartbeat`. It's deliberately scoped that way. It doesn't touch the project or session lists, so you keep your place while you dig.

## How it works

The pipeline is one-way and boring on purpose:

```
journal.ts          index-store.ts        view.ts               server.ts
parse one      →   incremental index  →  snapshot          →   HTTP + JSON API
.jsonl file         (size + mtime)        (+ git, heartbeats)
```

**Parsing.** Each file starts with an optional title-slot record, then a session header. Malformed JSON lines are counted, not thrown — one truncated journal from a hard kill must never take down the whole index. Titles fall back through: explicit title slot → header title → first 60 characters of the first prompt → `Session <id8>`.

**Indexing.** The scan globs exactly one directory level (`*/*.jsonl`). That's deliberate: omp writes subagent journals into nested trees, and pulling those in would bury the sessions you actually started under fan-out noise. Entries are cached by size and mtime, so refreshing across 490 sessions only re-reads the handful that changed, with a bounded worker pool rather than 490 concurrent file reads.

**The cache never stores assistant replies.** Prompts are small and needed for the list view; replies are large and only needed for the one session you have open, so they're re-parsed on demand.

**Privacy lives in the route, not the model.** `/api/snapshot` — the endpoint the list view polls every 10 seconds — strips prompt text from every session before responding. Full text only ever comes back from `/api/session/:id`, one session at a time. The server binds `127.0.0.1`; passing `--host` is a deliberate act and worth thinking about twice, because the detail endpoint and `--json` will hand full prompt history to anyone who can reach them.

**Instance reuse without a PID file.** Every response carries an `x-omp-monitor: 1` header. A second `omp-monitor` — or a second `/monitor` — probes `/healthz` for that header first, and reuses the running dashboard instead of fighting over the port.

The entire frontend is one static HTML file: inline CSS, vanilla JS, no framework, no build step. Prompt text goes into `<pre>` via `textContent`, never `innerHTML`, because arbitrary transcript content is exactly the sort of thing you don't want parsed as markup. Bun runs the TypeScript directly — no runtime dependencies, nothing to compile.

## Running it

Standalone:

```bash
curl -fsSL https://get.mrwilde.dev/setup | bash

omp-monitor                       # http://127.0.0.1:7333
omp-monitor --port 8080 --open
omp-monitor --json > snapshot.json
```

As an omp plugin — which is the version you want, because it's the plugin that writes the heartbeats that make `live` mean anything:

```bash
omp install github:robwilde/omp-prompt-monitor
```

Restart omp afterwards. Newly installed extensions need a real restart; `/reload-plugins` won't rebuild them. Then run `/monitor` in any session: it starts the dashboard if one isn't already up and reports the URL. `/monitor stop`, `/monitor restart`, and `/monitor update` do what they say.

Needs Bun 1.3.14 or newer. The installer will put a user-scoped Bun in place if you don't have one.

## What I'd change

**Subagent trees are invisible.** A `task` fan-out spawning six subagents still shows as a single session. That's the right default — the alternative is a list nobody can read — but "expand this session's subagents" is the obvious next thing to build.

**It's read-only.** I can see that a session has been sitting idle for forty minutes; I can't do anything about it from the browser. Steering an agent from a dashboard is tempting and probably a mistake, but I keep thinking about it.

**One machine.** The snapshot is plain JSON and the boundary was drawn so a remote collector could slot in, but none exists yet. Running agents on a box in the corner and watching them from a laptop is the version of this I actually want.

---

Four merged PRs and a weekend. It hasn't made my agents any better — but I stopped losing prompts, I can tell at a glance which repos have something running, and I've accidentally acquired a decision log for every run I've ever done.

The code is at [robwilde/omp-prompt-monitor](https://github.com/robwilde/omp-prompt-monitor). It reads files omp already writes, so there's nothing to configure.

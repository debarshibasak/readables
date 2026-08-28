# You do not need git. You need a soul.

Last week I argued that git has changed jobs: it stores the output, it no longer explains the codebase, and the layer above it — prompts, PRDs, decisions, memory — does not exist yet.

Here is the more radical version of that argument. You do not need git to understand a codebase. You need one file that holds everything the codebase has learned. Call it `SOUL.md`. And if that file is good enough, everything underneath it is a build artifact.

## The repo is the wrong artifact

A repository is an enormous amount of machinery devoted to one premise: code is expensive to produce, so it is worth remembering in full detail, forever, at every intermediate state.

That premise held for forty years. It does not hold this month. I can regenerate most modules in my codebase from a paragraph. What I cannot regenerate is the paragraph.

So we version the cheap thing with cryptographic rigor, and keep the expensive thing in a scrollback buffer that closes when the laptop sleeps.

## We already have the file

`CLAUDE.md`, `AGENTS.md`, `.cursorrules` — every tool ships one now. Look at what we put in them. Run `npm test` before committing. Use tabs. Prefer async/await.

That is a lint config with feelings. It is the most-read file in the repository and we fill it with things a pre-commit hook already enforces.

A soul is not conventions. Conventions are what you would tell any competent stranger on their first day. A soul is what only this codebase knows: what it tried, what it refuses to do again, and what it means by its own words.

## What goes in it

```
## Invariants
Tenant ID is resolved at the edge, never inside a service. A service that
reads tenant from a header is a bug, not a shortcut.

## Rejected
Kafka for the event path — tried Mar 2026. Ordering had to be per-tenant;
partitioning by tenant starved the large ones. Back to Postgres
LISTEN/NOTIFY. Do not re-propose without a partitioning story.

## Scars
A client retry loop amplified an outage while it was still happening.
Every client we own has a circuit breaker. Not a preference, not tunable.

## Vocabulary
"Session" is the auth session, never the TCP session. The TCP one is a
"connection". Getting this wrong cost two days once.
```

Every line there fails the git test. None of it is a diff, none of it has a SHA, and no commit message survives long enough to carry it.

But each line changes what the next agent run produces. That is the entire test for whether something belongs in the file.

## The test for a line

A line earns its place if deleting it would change the output of the next run.

Documentation describes what is, which is why nobody reads it — you can regenerate it from the source in seconds. A soul records what would otherwise have to be relearned the hard way, which is exactly what an agent will cheerfully relearn every Tuesday, at machine cadence, in forty commits.

That also gives you a delete rule. When the code moves past a line, cut the line. A soul that only grows is a wiki, and wikis are where decisions go to die.

## It has to fit

The hard constraint: it fits in the context window, and it is read on every run. Every run — not consulted when someone remembers to. A decision that lives somewhere the agent does not read does not exist.

The size limit is the feature. A repo can hold ten million lines and a wiki can hold ten thousand pages, and both let you postpone the question of what actually matters. A file that has to stay under a few thousand tokens does not let you postpone anything.

## Merge the soul, not the code

I said last week that when three agent runs produce three architectures there is no merge algorithm, only a decision — and that git shows you the conflict in the wrong place, down at the output.

Souls conflict properly. Two people who disagree about whether tenant resolution belongs at the edge produce two contradictory lines under `## Invariants`. That is a real conflict, in the layer where the disagreement actually happened. Four lines to review instead of four thousand. You settle it once, in English, and both codebases regenerate onto the same side of it.

Reviewing a soul diff is closer to what code review used to be than reviewing a code diff now is.

## What this breaks

Three honest problems, and I do not have clean answers to any of them.

Regeneration is not reproducible. Same soul, same prompt, different code. So you cannot actually throw the build away — you need the artifact pinned, immutable, deployable. Which is the argument for keeping git exactly as it is. Fine. Git holds the artifact. It stops holding the meaning.

The agent writes its own soul. If the agent maintains the file it reads, it drifts toward whatever it already believed, and you get a hall of mirrors with excellent formatting. The file needs a human editor the way a changelog does. Writing it is the work. It is not exhaust.

Scars turn into dogma. "Do not re-propose without a partitioning story" is useful for a year, and then someone should be allowed to reopen it. Date every entry. A line nobody has challenged in eighteen months should be suspect, not sacred.

## Git becomes dist/

Suppose the file works. Suppose someone new reads `SOUL.md` and understands the system better in twenty minutes than a month of `git log` would teach them. I already think that is true, and it is not a claim about the file — it is a claim about the log. Reading history taught you a codebase when history was written by people making one deliberate change at a time.

Then the repository inverts. `SOUL.md` is the source. `src/` is what you get when you run it. Git stays, because deploys pin to SHAs and rollback is a revert and provenance needs immutability — but it holds what `dist/` holds: the compiled output of a process whose real source is somewhere else.

We are adding 120 petabytes to store the output. The input is a file almost nobody is writing.

---
layout: post
title: "The spec is the code"
date: 2026-05-22
tags: [agentic-ai, software-engineering, spec-driven-development, side-projects]
---

## Naming the methodology

I've been circling something in these posts without naming it directly. The workflow — architect, delegate, review, accept — is an instance of **spec-driven development**, or SDD. The defining claim of SDD is simple: the spec is the primary artifact. Not the code. The code is a consequence of the spec, produced by an agent that implements exactly what the spec describes — including what it describes poorly.

That claim has consequences. It changes what engineering skill actually is in this workflow, and it changes which existing practices turn out to matter.

---

## The product spec is the thing

In SDD the product spec occupies the position that the code used to. It's what you own. It's what you're accountable for. It's where design decisions live. When something is wrong in the implementation, the first question isn't "why did the agent do that?" — it's "what did the spec leave ambiguous?"

This is a harder job than it sounds. Writing a spec precise enough that an agent can implement it without guessing is a skill that took me several weeks to start developing, and I'm still developing it. The difficulty is that ambiguity in a spec is often invisible until the implementation reveals it. You thought you were clear. The agent thought something different. The code is the evidence.

Everything downstream of the spec — the code quality, the test coverage, the architectural cleanliness — is a function of how well the spec was written.

---

## Clean Code as verification, not craft

*Clean Code* used to be about how to write good code. Under SDD it's about how to verify that the implementation actually expresses the spec.

When Claude Code produces an implementation, my first review pass is a Clean Code pass. Are the names honest? Is the function doing one thing? Are there comments compensating for obscure logic? These aren't aesthetic preferences — they're diagnostic signals. A long function with an explanatory comment tells me that either the model didn't find a clean abstraction, or the spec didn't constrain the interface precisely enough to produce one. The smell isn't in the code. It's evidence of something that went wrong in the spec.

Clean code is the mechanism by which I can say, with confidence, that the implementation faithfully expresses what the spec intended. When the code is obscure, I can't make that claim. I can verify that the tests pass. I can't verify that the thing being tested is what was specified. That's the gap.

Robert Martin was writing for teams where the people who wrote the code and the people who reviewed it were different people. That turns out to be exactly the situation in SDD — except the author is an agent that can't be asked follow-up questions. Legibility isn't a preference; it's the only channel through which the spec and the implementation can be compared.

---

## Clean Architecture and the spec-architecture loop

Architecture and spec don't have a clean sequence. You can't fully architect a product that hasn't been specified — you don't know what the system needs to do, so you don't know what its boundaries should be. But you can't fully specify a feature without knowing the architectural constraints — you don't know what's possible, what's forbidden, or what layer you're specifying against.

Some decisions can come first. Technology choices — Go for the core engine, TypeScript for the UI, TimescaleDB for time-series persistence — are more like axioms than architecture. They don't require a complete product spec; they require an understanding of the problem domain and a set of preferences about tooling. You can fix these early and build from them.

But the deeper decisions — how the layers divide, where the boundaries sit, what the dependency rules are — those emerge from the product. You need to know that you're ingesting order book diffs at high frequency before you can decide that the WebSocket layer must not reach into the persistence layer. You need to know that multiple symbols run concurrently before you can design the internal dispatch architecture. The spec reveals what the architecture must accommodate. The architecture reveals what the spec can precisely demand.

What Clean Architecture contributes to SDD is discipline over this feedback loop. As spec and architecture co-evolve, Clean Architecture's rules — stable abstractions, acyclic dependencies, clear boundary definitions — keep both sides coherent. Without that discipline, the loop produces tangled layers that are impossible to specify against cleanly. With it, the loop eventually stabilizes: the product is understood well enough, the boundaries are clear enough, and you arrive at the delegation moment with something precise to write a spec against.

The ADR is where that loop closes. It's the record of the decisions that stabilized the feedback — not the first decisions made, but the ones that held.

---

## Lean and the newcomer paradox

Lean Software Development has a principle I keep returning to: decide as late as possible. Don't commit to an irreversible decision until you have to. Defer until the last responsible moment, when the information needed to decide well has had time to accumulate.

I'm still working out how this applies to SDD. There's a specific pull I've been calling the Newcomer Paradox since the last post — the temptation to keep laying out specs and immediately hand them to Claude to implement. The velocity is real. The sheer throughput of what you can build in a session is beyond what I've experienced before. And I sometimes have to actively resist the pull of just generating the next thing.

The Lean principle is the counterweight. Not every decision needs a spec today. Not every spec needs an implementation this session. Some of what I'm tempted to generate now would benefit from sitting a day — from asking what I still don't know before writing what I think I do.

The eliminate waste principle cuts the other direction. Over-speccing is waste too. A delegation prompt that specifies what the agent would have gotten right anyway is noise that increases the chance of misinterpretation. The spec should be exactly long enough to be unambiguous and no longer.

---

## What happens when you run agents in parallel

The velocity question has a second dimension I haven't described yet: parallelism.

For the first few weeks of Erebor, I was running Claude Code sequentially — one spec, one implementation session, one review. That's already fast. But Claude Code supports isolated worktrees, which means you can run multiple agents simultaneously, each on a separate branch, each implementing a different spec, without them interfering with each other. The branches stay clean. The reviews stay independent.

I've been coordinating these sessions in Warp. Multiple panes, each running a Claude Code session in its own worktree. Write two or three specs in the morning, spin up worktrees, launch agents across panes, and let them run. Then review each one in turn. The implementation phase — which used to serialize everything — becomes parallel. The bottleneck shifts entirely to review and spec quality, which is where it should be.

What this produces is a different experience of time in a project. The question stops being "when will this feature be done?" and becomes "how many specs can I hold clearly enough in my head to run in parallel and review accurately?" That's a cognitive question, not a scheduling one. I haven't found the ceiling yet. Three parallel sessions feels sustainable. Four is possible if the specs are relatively independent. More than that and the review quality starts to degrade, which defeats the purpose.

Warp is worth naming specifically here because its session management and pane layout make this coordination practical rather than just theoretically possible. Each pane has context. You can scroll back, run commands alongside the agent output, and switch between sessions without losing state. It's not a revolutionary feature, but the workflow breaks down without it.

---

## What I can't believe

Describing what it feels like to combine tools at this level is difficult without sounding hyperbolic, but I'll try.

Kiro adds another dimension — agentic orchestration at the IDE level that extends beyond what Claude Code handles alone. Figma's AI capabilities are ahead of me on the dashboard layer I haven't built yet. What I know from early experiments is that there's a threshold where the tooling stops feeling like acceleration and starts feeling like something qualitatively different. The comparison isn't "working faster." It's closer to "operating at a different scale." More things in flight, more decisions being executed in parallel, more surface area under development at any given moment.

The constraint that remains is the one that always remains: the spec. Everything else is downstream of whether the spec was written well.

---

## The fundamentals and the new question

The fundamentals don't matter less under SDD. Clean code principles are the criteria for reviewing AI-generated output. Clean architecture is the prerequisite for writing specifiable software. Lean principles are the guide for when to spec and when to wait.

The shift is in what these practices are *for*. They were designed for a world where engineers wrote the code. They turn out to be exactly what you need in a world where you review it, specify it, and decide when it's ready — and an agent produces it.

The new question they're answering isn't "how do I write better code?" It's "how do I write better specs?"

---

*Edwin Alexis Abot — May 2026*

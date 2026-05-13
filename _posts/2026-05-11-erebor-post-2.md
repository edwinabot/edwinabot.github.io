---
layout: post
title: "When you know the spec but not the code"
date: 2026-05-11
tags: [agentic-ai, software-engineering, go, side-projects]
---

In the [first post]({% post_url 2026-04-27-erebor-post-1 %}) I described the workflow I'm committing to for Erebor: architect, delegate, review, accept. I said the next post would be about the order book ingestion service — what the spec looked like, what Claude Code produced, and what I had to push back on.

This is that post. It took 8 to 10 hours of real work spread across several days. [The PR](https://github.com/edwinabot/erebor/pull/1) landed at 4,893 lines added across 36 files — a complete Go service, database schema, Docker setup, CI pipeline, and test suite. Here's how it actually went.

---

## Starting from a spec, not a task

Before opening Claude Code, I had two conversations with Claude to prepare the implementation work. The first produced [ADR-001](https://github.com/edwinabot/erebor/blob/main/adrs/ADR-001-order-book-ingestion/ADR-001-order-book-ingestion.md). The architectural decisions are mine — I did the research, evaluated the options, and made the calls. What Claude contributed was arranging and composing the manuscript: turning my decisions and notes into a structured, readable document. The ADR covers five decisions: hybrid persistence (raw diffs plus periodic checkpoints), TimescaleDB as the storage layer, a combined WebSocket stream with internal dispatch, `shopspring/decimal` for all financial arithmetic, and a configurable depth limit per symbol.

The second conversation produced the [code review checklist](https://github.com/edwinabot/erebor/blob/main/adrs/ADR-001-order-book-ingestion/CODE-REVIEW-CHECKLIST-001.md). Eight sections: correctness, interface integrity, security, operational behaviour, persistence schema, test coverage, and code quality. The checklist exists because reviewing AI-generated code without a prior contract is a guessing game. The contract has to come first.

Then I wrote the delegation prompt — a document I've since saved as [DELEGATION-PROMPTS-001.md](https://github.com/edwinabot/erebor/blob/main/adrs/ADR-001-order-book-ingestion/DELEGATION-PROMPTS-001.md). It runs to several hundred words, includes exact interface signatures, exact domain types, exact bootstrap alignment conditions, and a list of things explicitly not to build. This is the spec Claude Code received.

---

## The code worked. The system didn't.

After the initial prompt, Claude Code produced a substantial implementation. All the tests passed. The Go code compiled cleanly. And then I tried to actually run it.

It couldn't be executed. Container configuration was wrong, migrations weren't being applied, the integration between the CLI and the running database wasn't wired correctly. The tests were green because they tested the units in isolation — the thing that was broken was everything around them. This required several additional rounds of prompting to resolve, including a focused prompt to add Docker Compose for a local TimescaleDB instance and a CLI command for integration testing against it.

This was a useful lesson in the gap between unit-level correctness and system-level executability. Tests passing is not the same as the software working. The spec had covered the bootstrap protocol in exhaustive detail and said almost nothing about local execution. That omission was mine, and the agent delivered exactly what was described.

---

## The newcomer paradox

Once I had something running, I hit something I wasn't expecting.

I felt like a newcomer to the project.

Not in a confused way — I had written the product spec, and I had made every architectural call in the ADR. Those are different things: the spec defines what to build; the ADR defines the implementation guardrails and constraints that determine how the spec can be executed. Both were mine, and both made complete sense to me. But for the first ten or fifteen minutes after Claude finished generating the code, I couldn't immediately connect what I was reading to either. The code was correct, the names were reasonable, the structure followed the design. And yet I felt the specific friction of arriving at an ongoing codebase without having been present for the implementation.

This is the paradox: I had two layers of documentation — product spec and architectural constraints — and still felt like a newcomer to the code that implemented them. The code is not the spec, even when the code faithfully implements the spec.

Engineering managers who are not hands-on will recognise this feeling. They own the roadmap, they signed off on the architecture, they know exactly why every decision was made — and then they sit down with the codebase and feel lost. It's not ignorance of the domain. It's the gap between the map and the territory. I've been that EM. This was a reminder of what that gap actually feels like from the inside.

I thought about Robert Martin's chapter on comments in *Clean Code*. Still right. I re-prompted Claude to add docstrings to all exported symbols — methods, structs, interfaces. That made an immediate difference. Not because the names were wrong, but because the intent behind each component became explicit in the place where I needed it, rather than requiring me to trace back to the ADR.

---

## Learning goroutines as a Python developer

One unexpected side effect of working through this code carefully was learning more about goroutines than I had before.

My background is Python. I understand processes and threads from the old school, and I've worked with asyncio for the last few years. Goroutines are a different model — conceptually between threads and coroutines but with a runtime scheduler that makes them feel lighter and more composable than either. Working through the `SymbolHandler` bootstrap procedure, where the snapshot fetch runs concurrently with the diff buffer and the two have to be safely joined, helped make goroutines feel concrete rather than abstract. They're genuinely elegant for this kind of problem.

---

## Qlty and the cost of cognitive complexity

I asked Claude to add Qlty CLI configuration to the project. Qlty ran `golangci-lint` and its own analysis and returned a list of findings. I then asked Claude to address them.

The cognitive complexity reductions were significant. Several functions in the `SymbolHandler` had complexity scores that were technically valid Go but difficult to read — deeply nested conditionals inside the state machine transitions. The refactoring Qlty drove produced cleaner code without changing the behaviour. I wouldn't have caught all of it in review.

---

## Tests as documentation, and their limits

I had little test coverage after the initial implementation. I ran Codecov and it confirmed it. I re-prompted in stages: first for happy path tests across all packages, then for missing execution paths, then specifically to comment every uncommented test explaining the functionality being tested.

The last prompt produced the most value. I've been saying for years that tests are great documentation. And they are — but only when I have time to read them carefully. What I actually needed was for the tests to explain themselves without requiring that study time. The prompt "comment all the uncommented tests explaining the functionality being tested" delivered exactly that.

I took time to verify the comments were accurate — that the explanation matched the assertions in the test. They did. After doing that, I could explain how the ingestion service works, end to end, with confidence. That confidence came from the commented tests, not from the code itself.

---

## Where things stand

The [PR](https://github.com/edwinabot/erebor/pull/1) is merged. The service ingests order book data from Binance, persists raw diffs and periodic checkpoints to TimescaleDB, implements the bootstrap synchronisation protocol specified in ADR-001, and shuts down gracefully on SIGTERM and SIGINT.

The next step is running this on dedicated hardware. I've purchased a machine I'll use as a server — it's cheaper than cloud at this stage and gives me more runway to experiment with container infrastructure before committing to a topology. The next post will cover that: local hardware, Docker-based deployment, and the first continuous data collection run.

About 8 to 10 hours of work. Nineteen commits. One more thing I know how to build.

---

*Edwin Alexis Abot — May 2026*

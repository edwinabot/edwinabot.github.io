---
layout: post
title: "Why I'm building a trading system without writing most of the code"
date: 2026-04-27
tags: [agentic-ai, software-engineering, career, side-projects]
---

I have over ten years in backend and systems engineering. I've written a lot of code. And I've been sitting with an uncomfortable idea for a while now: I think the days of handcrafted code, as the primary measure of an engineer's value, are over.

This post is about a side project I just started — Erebor, a high-frequency microstructure trading solution for Binance focused exclusively on Level 2 order book data. But it's really about something else: a deliberate attempt to transition from being someone who writes software to someone who directs agents that write software. An architect. A technical lead. A guardian of quality.

I want to be transparent about the whole process, including how this project started.

---

## A concern that's been building for a while

This didn't come out of nowhere. The first time I saw a model produce genuinely production-grade code — not a snippet, not a toy example, but something you'd actually ship — something shifted. I started having the same conversation repeatedly: with colleagues at my current company, with people outside it, over coffee, over Slack, sometimes just in my own head on a commute.

The conversation always goes the same way. Someone brings up a post they read — there are plenty of them now — arguing that handcrafted code as the default mode of engineering is finished. Some go further, predicting that within ten years the typical senior engineer won't write much code at all. They'll supervise agents that do. The responses in these conversations range from dismissal to anxiety to cautious agreement. I've landed on cautious agreement, with one addition: if that's where this is going, I'd rather practice the new role deliberately than arrive at it by accident.

Erebor is that practice.

---

## How Erebor began — in a planning conversation with Claude

I didn't open a code editor to start this project. I opened a conversation with Claude.

I described what I wanted to build and what I wanted to get out of it. What came back wasn't just a list of tasks. It was a structured 6-month roadmap with phases, milestones, a suggested weekly rhythm, and — when I pushed further — a reframe I hadn't fully articulated myself.

Somewhere in that conversation I said something like: *this project is also my training for transitioning from coding to supervising agents*. And the response helped me see that this wasn't a secondary goal. It was the primary one. The trading system is the vehicle. The real destination is learning how to operate as an architect in a world where agents do the implementation.

I'm documenting that conversation as part of this project's record. Not because using an AI planning tool is remarkable — it isn't, in 2026 — but because I think the *honesty* of that conversation matters. I wasn't performing a vision. I was working one out.

---

## What Erebor actually is

Erebor is a high-frequency trading solution with a narrow focus: Level 2 order book data from Binance. No sentiment analysis, no ML price prediction, no noise. Just microstructure — order book imbalance, spread dynamics, large order detection, the signals that live in the shape of the market before a trade happens.

The stack is Go for the core engine, TypeScript and Next.js for the dashboard and strategy UI, with DevSecOps practices wired in from day one — not bolted on at the end. I hold a CSSLP certification and part of what I want to demonstrate is that secure engineering is a posture, not a phase. The project runs six months, ending with a working backtesting harness and a paper trading loop against Binance Testnet.

---

## The thing I'm actually training

Here's the uncomfortable part to say plainly: I'm not building Erebor to show that I can write Go or TypeScript. I'm building it to show that I don't need to.

What I mean is this. The workflow I'm committing to for every feature looks like:

1. **Architect** — define the interface, the contract, the acceptance criteria, the edge cases. No code yet. This is the spec.
2. **Delegate** — hand the spec to Claude Code. Let it implement the module with tests.
3. **Review** — read the output as a senior reviewer. Challenge design choices. Reject and re-prompt if the abstraction is wrong, the pattern is insecure, or the performance assumption is naive.
4. **Accept and document** — merge. Write an Architecture Decision Record for non-trivial choices.

The spec is my artifact. The ADR is my artifact. The review is my artifact. The agent wrote the Go file.

This is what a technical lead actually does — except traditionally there's a team of engineers where the agent is. The transition I'm making is learning to do that job with one very fast, very capable, very literal collaborator. And "very literal" is the key word. Agents implement exactly what you described — including the parts you described poorly. The hardest skill in this workflow isn't prompting. It's writing a spec precise enough that there's no room for a wrong interpretation.

These are architect skills. I want to practice them with every commit.

---

## Why I'm writing about it

My GitHub Pages site is, as I've described it, a place for annotations — things I had to work out and might prove useful to someone else. This project fits that description exactly.

I'm writing about Erebor because I think this transition — from implementer to architect, from writing code to directing agents — is one that a lot of experienced engineers are facing and not talking about clearly. There's a lot of content about AI tools making developers more productive. There's much less about what it actually looks like to *change your role* in response to what these tools can do.

I'll be posting throughout the project. Not just the wins. The rejections, the re-prompts, the moments where the agent produced something that looked right and wasn't. The specs that were too vague. The reviews that caught something subtle.

If you're a senior engineer wondering whether the architect/TL path makes sense for you in this environment, I hope this is useful. If you're considering me for a project — this is how I think and how I work.

The project is [Erebor on GitHub](https://github.com/users/edwinabot/projects/3) and the here's the [erebor](https://github.com/edwinabot/erebor) repo. The next post will be about building the Go order book ingestion service — specifically what the spec looked like, what Claude Code produced, and what I had to push back on.

I don't know yet how much of the role transition will feel natural and how much will feel like loss. Probably both. That's worth writing about too.

---

*Edwin Alexis Abot — April 2026*

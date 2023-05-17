---
layout: post
title: Effective and efficient code reviews
date: 2023-05-15 08:03:00-0300
description: or how to be a kind and professional when coding and reviewing with others
tags: pr codereviews teamwork
categories: good-practices
related_posts: false
---
In all the projects I've participated in, I have observed variations of the same time wasters and effects on the team
during the code review process. In this post, I will compile these, briefly explain them and their side effects, and
describe practices I have fostered and implemented whenever possible.

I'll start by summarizing a code review process:

>After working on a new feature or fixing a bug, we ask our peers to review our work by issuing a pull request.
>Most of the time, our collaboration meets the quality of our team's criteria, but sometimes it does not. We engage in
>debates, receive feedback, and make adjustments. **Eventually**, our code passes the review and gets released to
>production.

I have seen that code reviews are precious for improving code quality and identifying potential issues. However,
specific **time wasters** can hinder the effectiveness and efficiency of code reviews, directly impacting delivery times
and team morale. I have had the opportunity of raising these time wasters as issues in many retrospective meetings and
collaborating towards their remediation.


## Time wasters

In no particular order, these are the top time wasters during code reviews:

**Lack of clarity in code submission:** When it is unclear or lacks sufficient context, reviewers may spend an unreasonable
amount of time trying to understand the code's purpose or how it fits into the system it is a part of. Clear and concise
explanations accompanying the code help reduce this waste of time.

**Large or overly complex changesets:** Reviewing large changesets or complex code can be time-consuming. Breaking down
changes into smaller, manageable chunks helps reviewers focus on specific areas and provide more targeted feedback.

**Incomplete or missing documentation:** Insufficient documentation within the codebase can lead to reviewers spending extra
time deciphering the code's functionality or intent. Well-documented code and brief comments help reviewers understand
the code faster and provide more relevant feedback.

**Lack of adherence to coding standards:** Code that deviates from established coding standards or best practices can lead
to lengthy discussions and debates during code reviews. Consistently adhering to agreed-upon coding guidelines can
minimize such time wastage.

**Excessive back-and-forth discussions:** If code reviews become lengthy debates or discussions without clear resolutions,
they will consume valuable time. Encouraging concise and focused discussions, setting a time limit for discussions, or
involving a mediator can help prevent excessive back-and-forth.

**Non-actionable or subjective comments:** Vague or subjective comments can lead to confusion and additional clarification
requests, slowing the code review process. Reviewers should provide specific and actionable feedback, suggesting
concrete improvements or pointing out potential issues.

**Reviewer overload:** If a small group of reviewers already overloaded with other tasks are also assigned to code review,
it can result in delays and backlogs. Distributing the workload among multiple reviewers or adopting a rotation system
can prevent reviewer burnout and speed up the process.

**Lack of automated tools and checks:** Manual inspection of every line of code can be time-consuming and error-prone.
Automated tools, such as static code analysis or linters, can help catch common issues and reduce the manual effort
required during code reviews.

Later, I will describe a set of practices that had avoided, if not entirely, significantly mitigated the effects of
these time wasters. Most of these have been listed as action items during retrospective meetings and adopted afterward.


## Time wasters impact on team's morale

As I wrote before, I've discussed these time wasters during retrospectives, which allowed me to talk about the feelings
around these wasters and how my teammates, and me, felt about them. It allowed us to analyze the impact on the team and
our delivery throughput.

**Frustration and demotivation:** Some team members become frustrated with the process when code reviews become
time-consuming due to various inefficiencies. The feeling of wasting time on an unproductive activity demotivated some
folks and diminished their enthusiasm for participating in code reviews.

**Decreased productivity:** Excessive time spent on code reviews took away valuable time and energy needed for other
essential tasks, like coding or monitoring. It leads to delays in project timelines and decreased productivity,
impacting the team's ability to deliver results efficiently.

**Lack of engagement and participation:** Team members will gradually disengage if code reviews consistently become lengthy
and unproductive. They perceived it as a burdensome activity rather than a valuable collaboration opportunity. This
disengagement can result in fewer individuals actively participating in code reviews, reducing the diversity of
perspectives and the overall effectiveness of the review process.

**Tension and conflict:** Prolonged debates or unresolved discussions during code reviews built tension within the team.
Disagreements over subjective matters or excessive nitpicking lead to interpersonal conflicts, harming team dynamics
and morale. It created a hostile atmosphere during code reviews, discouraging open communication and collaboration.

**Quality and pride in work:** code reviews did not effectively identify and address issues; overlooking those issues
allowed the merging of lower-quality code into the codebase. This eroded team members' sense of pride in their work,
and they perceived it as the quality standards of our delivery were compromised. A decline in code quality can
also lead to increased maintenance efforts and potential issues.

**Retention and turnover:** Prolonged frustrations and disengagement resulting from time wasters in code reviews contributed
to low team morale and ultimately impacted employee retention. Team members started seeking opportunities elsewhere
after consistently feeling underproductive and wasting time.


## Optimizing for code review efficiency

> _TL;DR: efficiency is a matter of will._

What I mean by that statement is that no matter what level of automation your code review process has, the weakest link
in the chain of the code review process is people's will to do things consciously and thoroughly. This weakness lies
within coders and reviewers.

We can reduce wasting time to a minimum by being conscious about our time usage and others' time usage. Thinking: "I
will do all I can so Jane can give me actionable feedback in the least amount of time," or "First, I'll make sure I
understand what John is working on," these are good food for thought that can drive our actions as collaborators.

The bottom line is that we want to save time for ourselves and others to enjoy other activities. Being efficient and
effective at work saves us time, and being efficient becomes essential as we spend the time of our lives working,
literally. So let's make better use of our time and not waste other folks' time either.

Let me share a set of good practices I collected over time that will help you be more efficient as a code reviewer and
author.

### Good practices for the PR author

> _TL;DR: Provide context, and narrow down the scope._

The strategy is to maximize our reviewers' output, minimizing the time they need to review our code. It is all about
narrowing down the scope and providing context. The tactic is to use these recommended practices:

**Give the PR a meaningful and short title:** the title will often give the first impression and first impressions matter.
Here's where we deliver the reviewer the first piece of context.

**Write a short and well-organized description of your PR:** if your team does not already have a PR template, you can start
filling in the description by answering three simple questions: What? Why? How?. You are explaining what you are doing,
why you are doing it, and how you did it. Propose your team a PR template for future collaborations.

**Follow conventions:** Make sure to follow them. Your community fosters conventions because they have proved their value in
thousands of projects. You can see the conventions as standards; standards make producing and communicating easier.
There will be a time in which you'll have to be creative because they are not helping, but more often than not, they are
good enough.

**Keep PRs under 400 lines of code (LOC):** this is good for two reasons. First, because reviewers' attention is limited,
the more code you have to read, the weaker your attention gets over time, so keep it short or break it down. Second,
error density, finding an error in too much code is akin to finding a needle in a haystack, so if there's too much code
to read and there are errors, it will be harder to spot them just because of the sheer volume of stuff to analyze.

**Atomic PRs:** solve one problem at a time; your PR should address one and only one matter at a time. You can break down a
feature into several pieces and combine them later in the feature branch. A bug will usually require one PR unless
distributed on many repositories.

**Annotate your code:** Source code is for humans, not for machines, so make reading your code a good experience. There will
be pieces of code that will be hard to follow; I've seen that mostly when expressing complex business rules, so adding
comments for context is good. Also, think of the future you or a colleague coming to this particular piece of code for
refactoring. Is it nice to get the context that led to this implementation?

**Write tests:** writing tests is thy best practice; I can't emphasize this enough; it is the coding practice that all the
good coding practices want to be. In the context of a code review, tests explain to the reviewer the behavior of our
code; it also shows how deeply you thought of the code you wrote. It is also another form of documentation.

### Good practices for the reviewer

> _TL;DR: Get requirements, be thorough, and provide actionable feedback._

As a reviewer, it is your responsibility to ensure that the delivery quality is good. It is enriching for all if you
strive to analyze the code with a reflective attitude, patience, and a spirit of healthy skepticism. You could always
seek to learn from and constructively criticize others' collaboration. To do so, I recommend you to:

**Thoroughly understand the feature or bug:** although it may seem obvious, this understanding is overlooked more often than
you think. It is easier to validate the implementation with a good knowledge of the bug or the feature. Otherwise, you
are not a guardian of the expected delivery value. If you do not understand the expected delivery, you merely focus on
technical correctness, which is important but not as important as added value.

**Time box:** you have many tasks to fulfill, and code review is one of them, so you should purposefully allocate time to
perform code reviews. Take one PR at a time and allocate time to do it. Your attention fades over time, so spend at most
one hour per review session. Try to stay focused during that time. Timeboxing helps you organize your tasks.

**Validate if the collaboration fixes the bug or delivers the feature:** this is paramount. Again, you are a guardian of the
delivery value. If it is a bug we are dealing with, you have to ensure it fixes it. If it is a feature we are building,
you must ensure that the feature meets all the requirements.

**Provide meaningful and actionable feedback:** you'll find something that needs commenting, be it a code smell, a bad
practice, or an unclear block of code that you think is wrong; that'll require you to produce a comment, and that
comment needs to be a piece of actionable feedback. This is an opportunity to help a teammate learn something new about
the technology involved, the business, etc. The best way to learn is to teach, so if you feel like learning, seize the
opportunity, gather some resources and references, and make a micro lecture.

**Review on time:** it is essential to do it on time, or else we are delaying or blocking the progress of some piece of
work. By reviewing on time, you also avoid some context switching to the teammate that built the solution. Some of our
colleagues will try and pick another task as soon as it is available; you want them to refrain from doing that and keep
their knowledge of the solution you are reviewing warm by doing it swiftly and effectively. Reviewing on time also
avoids generating anxiety in our collaborators.

**Don't be biased by seniority:** you'll find yourself reviewing the code of a more senior teammate. Don't deter yourself
from raising a defect because of its seniority; sooner or later, it'll make a mistake, and you must be ok finding that
defect. And when that happens, you must acknowledge it and communicate it with a good comment, a piece of actionable
and meaningful feedback. Sometimes senior devs must pay more attention to basic things, like writing a test.

**Be an advocate of best practices and conventions:** if there is a more idiomatic way or a top-rated solution to a common
problem, let the collaborator know and ask him to follow conventions. Again, provide actionable and meaningful feedback,
and take advantage of an opportunity to reinforce some good knowledge by teaching others.


### Example code reviews

I like exploring Open Source projects to learn how they do things. Thousands of contributors build these incredible
tools, and just a few are core team members that guard the good practices; these projects are core elements of products
that reach millions of people worldwide. And guess what? These teams also do code reviews. Let me make a list here for
you so you can explore them:

* [A bug in Django](https://github.com/django/django/pull/16835)
* [A refactor in GitLab](https://gitlab.com/gitlab-org/gitlab/-/merge_requests/13703#2c3621e9eaa266f4e1db62954ec460a38c45d8b2)
* [A bug in Rails](https://github.com/rails/rails/pull/48220)
* [A refactor in Python](https://github.com/python/cpython/pull/104547)
* [A feature in Spring Boot](https://github.com/spring-projects/spring-boot/pull/10944)

In some cases they write a full description in the PR, in other cases they provide a link to the issue containing all
the discussion. Try and follow the links in this PRs and MRs and learn how this teams collaborate.

## Wrapping up

There is always room for improvement in our processes. We experience different ways of implementing the Software
Development Lifecycle (SDLC) in each project that we take part. 

In this particular event of the SDLC, the Code Review, there is room for controversy, battles of egos, and impostor
syndromes. We must be kind and humble; this builds a safe and fruitful collaboration space.

I want to close this with a list of thoughts that guided the writing of this post:


* I want to avoid the frustration of senseless neverending back and forth in code reviews.
* I want to stop wasting time, take it back for me, and use it in other rewarding activities such as studying music or
spending time with my family.
* I want to enable others to do the same.

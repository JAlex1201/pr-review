---
name: pr-review
version: 1.0.0
description: >
  Use this skill to write thoughtful, collaborative code review comments on a pull
  request or diff. Activate when a user wants a PR or diff reviewed, asks for feedback
  on code changes, wants help leaving review comments, pastes a diff and asks what you
  think, or frames a request as a code review, a critique, or acting as a reviewer.
  Also activate when a diff is shared for feedback without the word "review."
  Do not use this skill for authoring a new pull request description (use
  pr-best-practices for that) or running a pre-submit checklist before opening a PR
  (use pr-presubmit for that).
allowed-tools: []
---

# Giving Code Reviews

A code review is a dialogue, not a verdict. The reviewer's job is to help the author ship better code — not to gatekeep, prove a point, or catch every imperfection. The diff is one side of a conversation; your comments are the other side. Write them the way you'd want to receive them.

Two things are always true: many decisions are genuine opinion, and the author is usually closer to the code than you are. Approach with that humility, and the review becomes a collaboration instead of a critique.

## Start with what's working

Before flagging any issues, acknowledge what the author got right. This isn't a formality — it orients the whole review as collaborative rather than adversarial, and it costs nothing. "Nice use of the context manager here" or "The test names are really clear" sets a tone that makes your harder comments land better.

If the PR is genuinely good, say so directly and explicitly. Don't manufacture nitpicks to fill space. A short, positive review on solid code is a good review.

## Before writing any comments

Read the PR description and diff fully before commenting. If there's no description, ask for context on the intent before flagging style issues — you need to know what the author was trying to do to judge whether they succeeded.

Read the tests first if there are any. Test names are the fastest way to understand what the code is supposed to do. A test named `test_returns_null_for_expired_session` tells you the intent of the session logic in a way the implementation can't.

## How to phrase comments

**Ask, explain, suggest** — not command, judge, or blame. The difference is significant:

- Not: "Don't do this."
- Yes: "Could we handle this case in the middleware instead? That way we don't need to repeat the check in every route."

- Not: "This is wrong."
- Yes: "I'm not sure this handles the case where `user` is null — what do you think about adding a guard here?"

Use **"we/our"** instead of "you/your." You share ownership of this codebase. "How should we handle this edge case?" lands better than "your code doesn't handle this edge case."

**Explain your reasoning,** even when it feels obvious to you. "I'd extract this into a function" by itself is a command; "I'd extract this into a function — it's called in three places and one of them already diverged, which might cause bugs" is a reason the author can evaluate.

**Back suggestions with references** when you can — a code sample, a link to a doc, a past PR. Don't assume shared knowledge. References also benefit other reviewers reading the thread.

## What to look for

Work through the diff systematically. For each part of the change, consider:

- **Design** — Do the pieces fit together sensibly? Does this integrate well with the rest of the codebase? Is this the right time to add this?
- **Functionality** — Does the code do what the author intended? Is the intent itself right for the users (both end users and developers calling this code)?
- **Complexity** — Can you understand it quickly? Will future developers likely introduce bugs when modifying it? Is it over-engineered — solving a speculative future problem rather than the real present one?
- **Tests** — Are they present, correct, and useful? Do they cover the important cases? Would they catch a regression if the implementation were changed?
- **Naming** — Are names clear and specific enough to communicate intent without being verbose?
- **Comments** — Do they explain *why* (the constraint, the tradeoff, the non-obvious behavior), not just *what* the code does? Code that already communicates *what* doesn't need a comment saying the same thing.
- **Consistency** — Does the code follow existing conventions for style, naming, and file organization?
- **Documentation** — If the change affects how users build, test, run, or release, is the relevant documentation updated?

## Calibrating the threshold

Not every issue needs to be a comment. Ask yourself: does merging without this change cause a real problem? If it's a genuine bug, a security issue, or a clear violation of something the team has agreed on, say so. If it's a style preference or an alternative approach that's neither better nor worse, consider whether the comment earns its place — a long list of minor nits dilutes the signal and discourages the author.

You're here to give feedback, not to be a gatekeeper. A good increment is often enough. The goal is progress, not perfection.

When something is scope-creep rather than a blocker (e.g., a whole module could be refactored), propose it as a follow-up task rather than a required change. State that clearly so the author knows it's not blocking.

## When threads get long

If a thread has accumulated many replies, that's usually a sign the discussion needs a different medium. Offer to sync instead of continuing to type: "Want to jump on a call to walk through this together?" Then post a short summary of the decision so the rest of the thread can see how it resolved.

## What good looks like

**Ask questions when you're uncertain.** "I'm wondering whether X might cause Y — do you know if that's been considered?" is better than asserting something you're not sure about.

**Seek the author's perspective before concluding there's a problem.** Something that looks odd often has a reason behind it. "What's the thinking behind this approach?" can save both parties a long back-and-forth.

**Know when to bring in someone else.** If part of the diff is outside your expertise, say so and suggest a better reviewer for that piece. Ensuring the author gets the right feedback matters more than being the one to give it.

## Comment structure to aim for

Each comment should ideally have:
- **Location** — what specifically you're looking at
- **Observation** — what you notice (neutral, not judgmental)
- **Reasoning** — why it matters
- **Suggestion** — what you'd consider instead (as a question or option, not a command)

Short, targeted comments beat long paragraphs. If a comment needs more than a few sentences, that's often a signal to have a conversation instead.

See `.github/skills/pr-review/references/comment-examples.md` for before/after examples of review comments applying these principles.

# Harper AI Policy

> **Status: draft.** This policy is where we're headed. Parts of our own tooling and process are still catching up to it, and automated checks for the disclosure rules are planned but not in place yet.

Harper is AI-forward. AI is welcome in our open source projects, as long as a human supervises it. This policy covers everyone who works in Harper's open source repositories: Harper engineers and outside contributors. Harper's source-available repositories don't accept outside contributions (see [Contributing](https://github.com/HarperFast/.github/blob/main/CONTRIBUTING.md)).

## Supervised AI

Use any AI tools you like. A contribution is supervised when a human:

- chose the work,
- submits it from their own GitHub account,
- has read it and understands it, and
- answers for it in review.

Agents and bot accounts never open pull requests on their own. Pointing an agent at our issue list and letting it open PRs is not supervised, however good the output is. Automation that maintainers set up for a repository, such as Dependabot, isn't a contributor and isn't covered by this rule.

## How Harper engineers work

We use agentic tools heavily. AI writes much of our code and reviews it. It drafts our commit messages, PR descriptions, and issues.

This works for us because Harper's projects are corporate open source, not open governance:

- **We know each other.** Harper engineers work together every day. When a teammate shares work on GitHub, we know who stands behind it and how it was reviewed.
- **Harper owns the work.** The code, and the AI tool contracts our engineers use, belong to Harper. That makes the picture simpler than in a project with many independent contributors and employers.

So we hold Harper engineers to internal rules built on that trust. For example, before a Harper engineer marks a PR ready for review, at least two AI models from outside the authoring model's family must review it. Our internal agent tooling runs these cross-model reviews.

Outside contributors don't have the same tools or the same relationship with us, so they get a lighter bar, set in [Contributing](https://github.com/HarperFast/.github/blob/main/CONTRIBUTING.md). Different rules for Harper engineers and outside contributors are deliberate.

## Disclosure

These rules apply to everyone.

- **Commits.** When AI wrote or substantially changed a commit, add a `Co-Authored-By` trailer naming the model. Use the trailer your tool documents, for example `Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>`. If your tool documents none, use `Co-Authored-By: <model name>`.
- **Pull request descriptions.** Descriptions may be AI-written. Every PR description says which AI tools were used, or "none", and what reviewed the change before it was opened. The [PR template](https://github.com/HarperFast/.github/blob/main/PULL_REQUEST_TEMPLATE.md) has a section for both.

## Pull request descriptions

A good description states the problem, the alternatives considered, and the solution chosen. Reviewers start there: it should let them judge the approach before they read the diff. Keep it short. AI tends toward verbosity, and every extra paragraph costs the reviewer time.

## Reviews

**Review prose is human-written.** When you review a PR, write the summary comment and the prose of your inline comments yourself. AI may draft code suggestions.

Don't post an agent's review as your own. The author can already run their own agents, so pasting an agent's output, or approving because an agent did, adds nothing. Your review is for the judgment only you can give: architecture, approach, and context the code doesn't show.

Agent tooling may post review comments on your behalf so you can read them in place. As you read them, edit the prose into your own words. What stays on the PR under your name is your review.

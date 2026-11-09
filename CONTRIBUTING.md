# Contributing to Harper

Harper publishes both **open source** and **source available** projects. Check a repository's `LICENSE` to see which it is.

- **Open source projects** accept contributions, following this guide.
- **Source available projects** do not accept contributions.

Individual repositories may add their own rules. Check the repository's `README.md` and `CONTRIBUTING.md` before you start.

## Before you start

Talk to the maintainers first, in an issue or on our community [Discord](https://harper.fast/discord). Agreeing on the problem and approach before you write code saves everyone time.

## Using AI

AI tools are welcome, as long as you supervise them. Read the [Harper AI Policy](https://github.com/HarperFast/.github/blob/main/guidelines/ai-policy.md). In short:

- You choose the work, and pull requests come from your own GitHub account. An agent you're running may open one for you, as long as you observe, review, or approve what it submits. Keep it a draft until you've read the change. Unattended agents and bot accounts must not open pull requests.
- You read and understand everything you submit, and you answer for it in review.
- You disclose AI use with `Co-Authored-By` trailers in commits and a note in the PR description.

## Before you open a pull request

1. **Review it yourself.** Read the whole diff.
2. **Have an AI agent review it.** At least one agent review is required, run with fresh context, separate from the session that wrote the change. A subagent with no prior context counts. More reviews are welcome, especially from other model families. For review guidance tuned to Harper, you can give your agent the prompts in [`HarperFast/ai-review-prompts`](https://github.com/HarperFast/ai-review-prompts), such as `universal.md` and `harper/common.md`.
3. **Run the tests** the repository documents.

## Pull requests

- **Describe the problem, your solution, the alternatives you weighed, and how you verified it.** The [PR template](https://github.com/HarperFast/.github/blob/main/PULL_REQUEST_TEMPLATE.md) has a section for each.
- **Fill in the AI use section.** Name the AI tools you used, or "none". Add a `Reviewed with:` line saying which agents reviewed the change before you opened the PR, and how.
- **Keep a few PRs open at a time.** Until you've had PRs merged in a repository, you can have up to 3 open PRs there at once. GitHub enforces this limit, and it lifts after your first 2 merged PRs. Some repositories set their own limits.

Maintainers may close a PR without review when it looks unsupervised. Examples include PRs opened in bulk, PRs from bot or agent accounts, and PRs missing the AI use section.

## Issues

Issues are welcome, with or without AI help. To keep things manageable, please have no more than 20 open issues in a repository at once. Maintainers may close issues opened in bulk and block accounts that spam.

## Code of Conduct

Harper has a [Code of Conduct](https://github.com/HarperFast/.github/blob/main/CODE_OF_CONDUCT.md) that all contributors are expected to follow.

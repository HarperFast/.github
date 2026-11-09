# Harper Repository Meta Documents

The [public repository policy](./repository-policy.md) says which meta documents an active public repository carries. This document covers what each one is for and what belongs in it, for humans and agents alike.

These expectations apply to active public repositories. Archived repositories keep the taxonomy type and archive note the policy requires, and are exempt from the rest.

Treat these as living documents: update them as the repository evolves.

## How meta documents are supplied

There are two mechanisms, and the difference matters:

- **Inherited org-wide.** GitHub uses this repository's `CONTRIBUTING`, `CODE_OF_CONDUCT`, `SECURITY`, and `SUPPORT` files for every repository in the organization that doesn't have its own. The files aren't added to those repositories. GitHub shows or links them in its interface, such as the tabs above a repository's README and the issue and pull request forms.
- **Repository-local.** GitHub never inherits `LICENSE`, `README`, or `AGENTS.md`. The policy requires every active public repository to have its own `LICENSE` and `README`. `AGENTS.md` is optional: add it when a repository has agent-specific facts worth recording.

**Inheritance is the default, and a local copy is the exception.** Add a local version of an inherited document only when the repository needs to say something the org version doesn't. A local file _replaces_ the org one entirely, and the repository then owns keeping it current. Never copy an org file unchanged: a verbatim copy is a second source for the same policy, and it will drift.

Every document described here is **public**, including `CONTRIBUTING.md` and `AGENTS.md`. Keep credentials, tokens, private hostnames, internal runbooks, and anything else unpublishable out of them. A public repository is [training surface](./repository-policy.md#why-this-exists).

## LICENSE

Required and repository-local on every active public repository. There's no org-wide default.

The license is the legal statement of what others may do with the code. Including it is never a judgment call.

## README.md

Required and repository-local. The README is the entry point for any project. It answers _what is this?_, _how do I use it?_, and _where do I go next?_.

### What belongs here

- Project summary and purpose
- Installation and usage instructions
- Public API reference, if it isn't documented elsewhere
- Links to `CONTRIBUTING`, `LICENSE`, and community channels such as Discord
- Links to external documentation: hosted docs, changelogs, and blog posts
- Badges or status indicators, if relevant
- The archive note, if the repository is archived (see the [policy](./repository-policy.md#required-archive-note))

### What does not belong here

- Internal development workflows
- Contributor onboarding
- Agent-specific instructions
- In-depth usage guides (use dedicated guides or posts instead)

## CONTRIBUTING.md

Inherited org-wide by default. Repository-local by exception.

The **public** contributor-facing guide to a project. It answers _how do I work on this project, and should I?_. The org-wide [`CONTRIBUTING.md`](../CONTRIBUTING.md) covers whether Harper accepts contributions and how to reach maintainers. It sends readers to each repository's own documentation for specifics.

Write a local one when the project has specifics worth stating, which is most repositories with a build, a test suite, or a release process. Also write one to say that the repository doesn't accept contributions.

A local file replaces the org default entirely, so the org-wide contribution policy stops reaching that repository's readers. Link to it instead of restating it. Open the local file with a line like this:

```md
Harper's [organization-wide contribution policy](https://github.com/HarperFast/.github/blob/main/CONTRIBUTING.md) applies. What follows is specific to this repository.
```

That keeps one source for the shared text, and the local file holds only what's local.

Its readers are maintainers and contributors, human and agent. It's still a public file, so write it for an outside contributor.

### What belongs here

- Repository structure overview
- Development environment setup
- Contribution workflow: branching, PRs, and commit conventions
- Build process
  - Build commands
  - Relevant configuration
- Release process
  - Manual vs automated steps
- Testing guide
  - How tests are structured
  - How to add new ones
  - How to run them
  - Known flakiness
  - Manual local testing
- Code style and formatting rules
  - Linter and formatter commands
  - Relevant configuration
- CI environment
- Development-facing documentation
  - APIs that are useful during development
  - Scripts needed for development or testing

### What does not belong here

- Public interface documentation (APIs, scripts, and similar)
- Usage information
- Agent-specific instructions
- Internal-only material: credentials, private infrastructure, internal runbooks, or anything else that can't be published

## AGENTS.md

Optional and repository-local. Never inherited.

A minimal, agent-specific document that complements `README.md` and `CONTRIBUTING.md`. It answers _what does an agent need to know that it **can't reliably infer** from the existing documentation?_. It doesn't replace or repeat that documentation.

Write it for agents. Anything a human contributor needs belongs in another document, usually `README.md` or `CONTRIBUTING.md`. A good `AGENTS.md` helps an agent understand the codebase without excessive file reads and token use.

That's why `AGENTS.md` may repeat a bare command that `CONTRIBUTING.md` also documents, such as `npm run build` or the test command. Keep it to the command itself and leave the explanation in `CONTRIBUTING.md`, which stays the source of truth. When a command changes, update both files. Don't turn `AGENTS.md` into a second copy of `CONTRIBUTING.md`.

### What belongs here

- Facts that seem obvious to a human but that an agent would otherwise have to read specific project files to learn
- Commands to run or avoid, and files to read or leave alone
- Agent workflow requirements, such as how to attribute or sign off commits, or headers to include in requests

### What does not belong here

- Project description or summary (`README.md`)
- Contribution workflows: how to open a PR, commit conventions, and the release process (`CONTRIBUTING.md`)
- Prose written for a person: rationale, onboarding narrative, or explanation. If a human contributor needs to _read_ it, it belongs in another document
- Secrets of any kind: tokens, API keys, internal hostnames, or private endpoints. An agent that needs a credential reads it from the environment, and naming the environment variable is fine

### Template

```md
# AGENTS.md

Review `README.md` and `CONTRIBUTING.md` for all relevant repository information.

## Development Tips

- Use `npm install` to install dependencies.
- Use `npm run build` to compile `src/` to `dist/`.
- Do not edit files in `dist/`; it is compiled output.
- Do not run `npm version` or `npm publish`; these commands are for humans only.
- [Any file/extension constraints specific to this repo's module system.]

## Code Style

- [Language and module system in use: ESM, CJS, TypeScript, etc.]
- [Any enforced syntax restrictions, e.g. erasable-only TypeScript.]
- [Formatter command if one exists, or note that none exists.]

## Testing

- [How to run the test suite.]
- [Any required setup steps before first run, and when to re-run them.]
- [Known slow operations that should not be interpreted as failure.]
- [Any test files or areas that are intentionally skipped or work-in-progress;
  note what an agent should not do with them.]
- [Note if there are currently no tests.]

## CI

- [Status of CI and any known disabled conditions.]
- [Where to find logs or artifacts on failure.]
```

## CODE_OF_CONDUCT.md

Inherited org-wide by default. Repository-local by exception.

The behavioral standard for everyone participating in the project, and how to report a violation. The org-wide [`CODE_OF_CONDUCT.md`](../CODE_OF_CONDUCT.md) applies to every Harper repository.

Override it only if a repository has a different standard or reporting path, such as a jointly owned or foundation-governed project. Then link the org file for the baseline and state only what differs, as described for [`CONTRIBUTING.md`](#contributingmd). Otherwise leave it inherited.

## SECURITY.md

Inherited org-wide by default. Repository-local by exception.

How to report a vulnerability, and what a reporter can expect in return. The org-wide [`SECURITY.md`](../SECURITY.md) is the default reporting path for every Harper repository. GitHub shows it on the repository's Security tab even when the file is inherited.

Override it when a repository needs its own disclosure terms: a different contact, a bug bounty scope, supported-version statements, or a published signing key.

A local file replaces the org one entirely, and for this document that's a real hazard. A `SECURITY.md` with only a supported-versions table leaves the repository with no reporting route at all. Link the org file for the baseline and state only what differs beneath it, as described for [`CONTRIBUTING.md`](#contributingmd).

## SUPPORT.md

Inherited org-wide by default. Repository-local by exception.

Where to go for help. The org-wide [`SUPPORT.md`](../SUPPORT.md) covers bug reports, Discord, Code of Conduct reports, and support contacts.

Override it when a repository has its own support channel, or when it's unmaintained and readers need to be told so plainly. Link the org file for the routes that still apply instead of dropping them, since a local file replaces it entirely. Otherwise leave it inherited.

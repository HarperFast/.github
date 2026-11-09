# Harper Public Repository Policy

> **Status: work in progress.** Refinement is expected. Automated checks for much of this policy are planned. Until they exist, this document is the reference for how Harper manages its public repos.

## Why this exists

Harper's GitHub organization has over 500 repositories with no sustained, org-wide management behind them. That has a real cost:

- **Public repos are AI training surface.** Search engines index public repos, and AI models train on them. Stale examples, half-finished implementations, and abandoned demos degrade how agents understand and represent Harper.
- **Our repos reflect the engineering organization.** Companies, teams, and developers (human and agentic) judge us by what we make public.
- **Undefined scope is unmaintainable.** Until we know what each repo is and who owns it, we can't state our maintenance obligations. The v5 upgrade effort showed this.

## Scope

This policy applies to public repos in the Harper GitHub organization (`@HarperFast`). Active repos must meet the [requirements](#requirements-for-active-public-repositories) below. Archived repos keep their taxonomy type and archive note, and are exempt from the rest (see [Public archived](#public-archived)). Internal and private repos are outside this policy.

The [repository taxonomy](./repository-taxonomy.md) defines the repo *types* this policy refers to.

## Requirements for active public repositories

Every active public repo must meet these requirements. A repo starts as internal or private. To make it public, propose it, so we can check it for correctness and commit maintenance resources to it.

### Ownership and a maintenance commitment

Every repo has a team owner, not an individual. Public code is a **standing maintenance commitment**, not a one-time contribution. Don't share code with no plan to maintain it. The `snapshot` type exists for sharing code we won't maintain, and snapshots still follow the rest of this policy.

A team can transfer ownership, and the team that creates a repo doesn't have to be the team that maintains it. Both teams must agree to the arrangement explicitly.

### One taxonomy type, declared as the `repo-type` custom property

Every repo has exactly one [taxonomy type](./repository-taxonomy.md), recorded in the **`repo-type` organization custom property**: `product`, `plugin`, `application`, `library`, `template`, `example`, `guide`, `snapshot`, or `meta`. Don't encode the type in the repo name. The property is the source of truth, so a repo can change type as it matures without a rename.

We use a custom property instead of a GitHub topic for three reasons:

- GitHub enforces the allowed values.
- We restrict who can edit it to org owners. Topics drift because anyone with maintain access can change them.
- The org-wide custom properties API returns every repo's value.

Topics are for discoverability only. Every public repo has a type, archived or not, because the type tells a reader what they're looking at. Internal and private repos don't set this property.

Some existing repos have taxonomy types in their names. We'll rename them over time.

### Required meta documents

Every active public repo has its own:

- **`LICENSE`**: the terms others can use the code under.
- **`README`**: what the repo is.

The org-wide `CONTRIBUTING`, `SECURITY`, `CODE_OF_CONDUCT`, and `SUPPORT` files come from this repo. A repo can add its own version of any of them. A local file replaces the inherited one entirely, so add one only when the repo needs to say something different.

[Meta documents](./meta-documents.md) covers what each document is for, what belongs in it, and when to replace an inherited one.

### A declared Harper version

Every repo that consumes Harper declares the Harper version it targets as a semver specifier in `package.json`, under `dependencies`, `peerDependencies`, or `engines`. People and tooling can then see which Harper version a repo supports without reading its source.

### Testing and upgrades

Each repo meets the testing and upgrade contract of its taxonomy type. Most active public repos track the latest Harper release and test against latest and upcoming Harper versions as early as possible. The exceptions:

- Products and libraries may version independently.
- Snapshots stay pinned to their Harper version.
- Meta repos set their own testing and upgrades.

We may build a release gate that tests Harper prereleases against our tested public repos before each release.

### Snapshots declare their backing content

Every `snapshot`, active or archived, sets the repo's **`homepage` field** to the URL of the content it backs, and links the same content in the README. Tooling reads `homepage`, so the link doesn't depend on README prose.

A repo with no dated content behind it isn't a snapshot. Give it the type that matches its purpose, whether it stays active or is archived.

## Public archived

Archiving retires a repo without erasing it. **Archived isn't deleted.** The repo becomes read-only, its links keep working, and we can unarchive it if we got it wrong.

### When to archive

Archive a public repo when it's no longer worth keeping *active* but is still worth keeping *referenceable*:

- It's dormant, with no meaningful recent activity and no owner willing to maintain it. We're considering one year without activity as the threshold, possibly different by type.
- A newer repo, feature, or first-party capability replaced it.
- It's a `snapshot` and the next Harper major is generally available (see [Snapshots](./repository-taxonomy.md#snapshots)).

### Keep public vs. move internal

Keep an archived repo **public** when something outside still points at it: a published package, a link from our docs, marketing content, or a running system that needs to clone it. Move it internal when nothing does.

Don't default to public. **A stale public repo is training surface**: an abandoned repo keeps teaching humans and agents old practices long after the code stops mattering. Weigh each repo's reference value against that, and record why you chose its visibility.

An archived repo that stays public keeps its [taxonomy type](./repository-taxonomy.md) and needs the archive note below. One that moves internal needs neither.

### Required archive note

Every archived repo that stays public has a note at the top of its README. For a `snapshot`:

```md
> [!IMPORTANT]
> **This repository is archived and read-only.**
>
> It's a point-in-time snapshot built to accompany [<content title>](<content link>).
> It is pinned to **Harper v<major>** and is preserved for reference.
> It isn't updated for current Harper releases and may not work with them.
>
> For up-to-date guides and reference docs, see the [Harper docs](https://docs.harper.fast) and join our [Discord](https://harper.fast/discord).
```

Use the `harper.fast` links above for the docs and Discord, not the `docs.harperdb.io` and `discord.com/invite/...` URLs they redirect to.

Adapt the middle of the note to the repo:

- A `snapshot` names and links the content it accompanies.
- A superseded repo names and links what replaced it.
- A repo that published a package names the package.

Drop the "pinned to" line when the repo never targeted a specific version. If the README isn't Markdown, use that format's admonition syntax.

### Deletion (rare)

Prefer archiving. Delete a repo only when it has **no reference value at all**, such as an empty or unused fork or an experiment that was never published. Never delete a repo that has been public and referenced.

## Roadmap

This policy is still being built out. Planned work:

- Automated checks for the requirements above: `repo-type` set, license present, Harper version declared, and README shape.
- A standard required meta-document set, building on [meta documents](./meta-documents.md), including `README` and `CONTRIBUTING` templates.
- An ownership registry and a review cadence.
- A separate internal and private repository policy.
- A defined process for proposing a repo for publication: who reviews it and who approves the maintenance commitment.

If a repo doesn't fit this policy, or you find a gap, raise it. This policy will evolve.

# Harper Public Repository Policy

> **Status: work in progress.** Refinement is expected. Automated checks for much of this policy are planned. Until they exist, this document is the reference for how Harper manages its public repositories.

## Why this exists

Harper's GitHub organization has over 500 repositories with no sustained, org-wide management behind them. That has a real cost:

- **Public repositories are AI training surface.** Search engines index public repositories, and AI models train on them. Stale examples, half-finished implementations, and abandoned demos degrade how agents understand and represent Harper.
- **Our repositories reflect the engineering organization.** Companies, teams, and developers (human and agentic) judge us by what we make public.
- **Undefined scope is unmaintainable.** Until we know what each repository is and who owns it, we can't state our maintenance obligations. The v5 upgrade effort showed this.

## Scope

This policy applies to public repositories in the Harper GitHub organization (`@HarperFast`). Active repositories must meet the [requirements](#requirements-for-active-public-repositories) below. Archived repositories keep their taxonomy type and archive note, and are exempt from the rest (see [Public archived](#public-archived)). Internal and private repositories are outside this policy.

The [repository taxonomy](./repository-taxonomy.md) defines the repository *types* this policy refers to.

## Requirements for active public repositories

Every active public repository must meet these requirements. A repository starts as internal or private. To make it public, propose it, so we can check it for correctness and commit maintenance resources to it.

### Ownership and a maintenance commitment

Every repository has a team owner, not an individual. Public code is a **standing maintenance commitment**, not a one-time contribution. Don't share code with no plan to maintain it. The `snapshot` type exists for sharing code we won't maintain, and snapshots still follow the rest of this policy.

A team can transfer ownership, and the team that creates a repository doesn't have to be the team that maintains it. Both teams must agree to the arrangement explicitly.

### One taxonomy type, declared as the `repo-type` custom property

Every repository has exactly one [taxonomy type](./repository-taxonomy.md), recorded in the **`repo-type` organization custom property**: `product`, `plugin`, `application`, `library`, `template`, `example`, `guide`, `snapshot`, or `meta`. Don't encode the type in the repository name. The property is the source of truth, so a repository can change type as it matures without a rename.

We use a custom property instead of a GitHub topic for three reasons:

- GitHub enforces the allowed values.
- We restrict who can edit it to org owners. Topics drift because anyone with maintain access can change them.
- The org-wide custom properties API returns every repository's value.

Topics are for discoverability only. Every public repository has a type, archived or not, because the type tells a reader what they're looking at. Internal and private repositories don't set this property.

Some existing repositories have taxonomy types in their names. We'll rename them over time.

### Required meta documents

Every active public repository has its own:

- **`LICENSE`**: the terms others can use the code under.
- **`README`**: what the repository is.

The org-wide `CONTRIBUTING`, `SECURITY`, `CODE_OF_CONDUCT`, and `SUPPORT` files come from this repository. A repository can add its own version of any of them. A local file replaces the inherited one entirely, so add one only when the repository needs to say something different.

[Meta documents](./meta-documents.md) covers what each document is for, what belongs in it, and when to replace an inherited one.

### A declared Harper version

Every repository that consumes Harper declares the Harper version it targets as a semver specifier in `package.json`, under `dependencies`, `peerDependencies`, or `engines`. People and tooling can then see which Harper version a repository supports without reading its source.

### Testing and upgrades

Each repository meets the testing and upgrade contract of its taxonomy type. Most active public repositories track the latest Harper release and test against latest and upcoming Harper versions as early as possible. The exceptions:

- Products and libraries may version independently.
- Snapshots stay pinned to their Harper version.
- Meta repositories set their own testing and upgrades.

We may build a release gate that tests Harper prereleases against our tested public repositories before each release.

### Snapshots declare their backing content

Every `snapshot`, active or archived, sets the repository's **`homepage` field** to the URL of the content it backs, and links the same content in the README. Tooling reads `homepage`, so the link doesn't depend on README prose.

A repository with no dated content behind it isn't a snapshot. Give it the type that matches its purpose, whether it stays active or is archived.

## Public archived

Archiving retires a repository without erasing it. **Archived isn't deleted.** The repository becomes read-only, its links keep working, and we can unarchive it if we got it wrong.

### When to archive

Archive a public repository when it's no longer worth keeping *active* but is still worth keeping *referenceable*:

- It's dormant, with no meaningful recent activity and no owner willing to maintain it. We're considering one year without activity as the threshold, possibly different by type.
- A newer repository, feature, or first-party capability replaced it.
- It's a `snapshot` and the next Harper major is generally available (see [Snapshots](./repository-taxonomy.md#snapshots)).

### Keep public vs. move internal

Keep an archived repository **public** when something outside still points at it: a published package, a link from our docs, marketing content, or a running system that needs to clone it. Move it internal when nothing does.

Don't default to public. **A stale public repository is training surface**: an abandoned repository keeps teaching humans and agents old practices long after the code stops mattering. Weigh each repository's reference value against that, and record why you chose its visibility.

An archived repository that stays public keeps its [taxonomy type](./repository-taxonomy.md) and needs the archive note below. One that moves internal needs neither.

### Required archive note

Every archived repository that stays public has a note at the top of its README. For a `snapshot`:

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

Adapt the middle of the note to the repository:

- A `snapshot` names and links the content it accompanies.
- A superseded repository names and links what replaced it.
- A repository that published a package names the package.

Drop the "pinned to" line when the repository never targeted a specific version. If the README isn't Markdown, use that format's admonition syntax.

### Deletion (rare)

Prefer archiving. Delete a repository only when it has **no reference value at all**, such as an empty or unused fork or an experiment that was never published. Never delete a repository that has been public and referenced.

## Roadmap

This policy is still being built out. Planned work:

- Automated checks for the requirements above: `repo-type` set, license present, Harper version declared, and README shape.
- A standard required meta-document set, building on [meta documents](./meta-documents.md), including `README` and `CONTRIBUTING` templates.
- An ownership registry and a review cadence.
- A separate internal and private repository policy.
- A defined process for proposing a repository for publication: who reviews it and who approves the maintenance commitment.

If a repository doesn't fit this policy, or you find a gap, raise it. This policy will evolve.

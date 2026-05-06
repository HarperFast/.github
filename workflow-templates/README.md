# Workflow templates

Suggested workflows that show up in the Actions tab when creating a new workflow in any HarperFast repo. Currently this set wires the repo into the centralized AI review chain in [`HarperFast/ai-review-prompts`](https://github.com/HarperFast/ai-review-prompts).

## The Claude AI workflow chain

Four templates form one cohesive system. Adopt them together for a new repo:

| Template | What it does |
|---|---|
| `claude-review.yml` | Reviews every PR. Posts inline findings + a concise top-level summary. |
| `claude-mention.yml` | Responds to `@claude …` in PR / issue comments. Read, edit, commit, push, open PRs. |
| `claude-issue-to-pr.yml` | Label an issue with `claude-fix:{typo,docs,deps,bug}` → Claude opens a PR with the fix. |
| `validate-caller-workflows.yml` | Structural lint for the caller files. Catches shadow jobs and mutable refs. Make this a required status check on `main`. |

Each template is a thin caller of a reusable workflow in `HarperFast/ai-review-prompts`. The reusables hold the auth gate, prompt logic, and orchestration scripts; the caller pins to a specific SHA so updates are explicit.

## New-repo onboarding checklist

After dropping the templates into `.github/workflows/`:

1. **Secrets** (per-repo until org-level lands):
   - `HARPERFAST_AI_CLIENT_ID`
   - `HARPERFAST_AI_APP_PRIVATE_KEY`
   - `ANTHROPIC_API_KEY`
   - `AI_REVIEW_LOG_TOKEN` *(optional — threads each review run into a per-PR issue in `HarperFast/ai-review-log`)*

2. **Labels** (issue-to-PR triggers — exact spellings, lowercase, colon, no trailing space):
   - `claude-fix:typo`
   - `claude-fix:docs`
   - `claude-fix:deps`
   - `claude-fix:bug`

3. **`.node-version`** at the repo root containing the major Node version (e.g. `24`). The mention + issue-to-PR reusables use `node-version-file:` for `actions/setup-node`.

4. **Branch protection on `main`**: make `validate / validate` a required status check.

5. **`AGENTS.md`** (and a `CLAUDE.md` one-liner pointing at it) at the repo root: the agent reads these for repo-specific conventions. Mirror harper / harper-pro for shape.

6. **Customize the templates**:
   - `claude-review.yml` — pick the right `review-layers:` for the repo type; fill in `repo-specific-checks:` if there are surface-area concerns the shared layers don't cover.
   - `claude-mention.yml` / `claude-issue-to-pr.yml` — set `setup-bun: true` for plugin repos that test under bun. Override `pre-commit-validation:` if the repo doesn't have `npm run test:unit` / `format:check`.

## Bumping the pin

The `uses:` ref and the `with.ai-review-prompts-ref` input on each caller pin to the same SHA. Bumping is one motion: change both lines on each of the four files to the new SHA from `HarperFast/ai-review-prompts` `main`. The structural validator (`validate-caller-workflows.yml`) enforces that both refs are 40-char SHAs.

Coordinate pin bumps with the [HarperFast/ai-review-prompts changelog](https://github.com/HarperFast/ai-review-prompts/commits/main).

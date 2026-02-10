# NPM Trusted Publishing & Provenance

This document outlines how to enable NPM trusted publishing and provenance for HarperFast repositories. This allows for more secure publishing eliminating the need for long-lived NPM tokens in GitHub Secrets.

## 1. Update GitHub Release Workflow

Add the following permissions to your GitHub release workflow (usually in `.github/workflows/release.yaml`). These are required for `semantic-release` to create GitHub releases and publish to NPM.

```yaml
permissions:
  contents: write
  issues: write
  pull-requests: write
  id-token: write
```

> **Note:** The `id-token: write` permission is specifically required for NPM provenance and trusted publishing.

### Adjust the Publish Step

Update your NPM publish step to include the `--provenance` flag:

```yaml
- name: Publish to NPM
  run: npm publish --provenance
```

## 2. Initial Manual Publication

If the package has never been published to NPM before, you must manually publish it once from your local machine.

For `@harperfast` scoped packages, use:

```bash
npm publish --access public
```

This initial manual publish adds the package to NPM, allowing you to then configure it for trusted publishing.

## 3. Configure NPM Trusted Publisher

Once the package exists on NPM, you can configure the repository as a trusted publisher:

1. Visit `https://www.npmjs.com/package/{packageName}/access`
2. Add or edit a **Trusted Publisher**.
3. **Publisher:** GitHub Actions
4. **Organization or user:** `HarperFast` (or the appropriate owner)
5. **Repo:** The exact name of your repository.
6. **Workflow filename:** The exact name of your release workflow file (e.g., `release.yaml`). This is case-sensitive and must be located in the `.github/workflows/` folder.
7. **Environment name:** Leave blank unless you are specifically using GitHub Environments.

## 4. Restrict Publishing Access

While you're in NPM access settings, also disallow tokens:

> Require two-factor authentication and disallow tokens (recommended)

Remember to hit save!

## Example

For a reference implementation, see the [hairper release workflow](https://github.com/HarperFast/hairper/blob/main/.github/workflows/release.yaml).

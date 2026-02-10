# npm Trusted Publishing & Provenance

This document outlines how to enable npm trusted publishing and provenance for HarperFast repositories. This allows for more secure publishing eliminating the need for long-lived npm tokens in GitHub Secrets.

## 1. Update GitHub Release Workflow

Add the following permissions to your GitHub release workflow (usually in `.github/workflows/release.yaml`). These are required for `semantic-release` to create GitHub releases and publish to npm.

```yaml
permissions:
  contents: write
  issues: write
  pull-requests: write
  id-token: write
```

> **Note:** The `id-token: write` permission is specifically required for npm provenance and trusted publishing.

### Adjust the Publish Step

Update your npm publish step to include the `--provenance` flag:

```yaml
- name: Publish to npm
  run: npm publish --provenance
```

## 2. Initial Manual Publication

If the package has never been published to npm before, you must manually publish it once from your local machine.

For `@harperfast` scoped packages, use:

```bash
npm publish --access public
```

This initial manual publish adds the package to npm, allowing you to then configure it for trusted publishing.

## 3. Configure npm Trusted Publisher

Once the package exists on npm, you can configure the repository as a trusted publisher:

1. Visit `https://www.npmjs.com/package/{packageName}/access`
2. Add or edit a **Trusted Publisher**.
3. **Publisher:** GitHub Actions
4. **Organization or user:** `HarperFast`
5. **Repo:** The exact name of your repository.
6. **Workflow filename:** The exact name of your release workflow file (e.g., `release.yaml`). This is case-sensitive and must be located in the `.github/workflows/` folder.
7. **Environment name:** Leave blank unless you are specifically using GitHub Environments.

## 4. Restrict Publishing Access

While you're in npm access settings, also disallow tokens:

> Require two-factor authentication and disallow tokens (recommended)

Remember to hit save!

## 5. Using Semantic Release (Optional)

HarperFast projects often use `semantic-release` to automate the entire package release workflow, including determining the next version number, generating release notes, and publishing to npm.

### Configuration

You can configure `semantic-release` using a `.releaserc.json` file or a `release.config.js` file in your repository root.

**Example `.releaserc.json`:**

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    ["@semantic-release/npm", {"npmPublish": false}],
    "@semantic-release/github"
  ]
}
```

### Required Dependencies

Ensure you have the following devDependencies in your `package.json`:

```bash
npm install --save-dev semantic-release @semantic-release/commit-analyzer @semantic-release/release-notes-generator @semantic-release/npm @semantic-release/github
```

### GitHub Actions Integration

When using `semantic-release`, your publish step in GitHub Actions might look like this:

```yaml
jobs:
  release:
    name: Release
    environment: Release
    # Ensure releases run only when code reaches main via GitHub Merge Queue, or when manually dispatched
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd # v6.0.2
        with:
          fetch-depth: 0
      - name: Setup Node.js
        uses: actions/setup-node@6044e13b5dc448c55e2357c09f80417699197238 # v6.2.0
        with:
          node-version-file: '.nvmrc'
          cache: npm
          registry-url: 'https://registry.npmjs.org'
      - name: Update npm
        run: npm install -g npm@latest
      - name: Install dependencies
        run: npm ci
      - name: Check lint
        run: npm run lint
      - name: Check format
        run: npm run format
      - name: Run unit tests
        run: npm test
      - name: Semantic Release
        if: ${{ github.event_name == 'push' }}
        uses: cycjimmy/semantic-release-action@b12c8f6015dc215fe37bc154d4ad456dd3833c90 # v6.0.0
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          NPM_CONFIG_PROVENANCE: true
      - name: Publish to npm
        run: npm publish --provenance
```

## Example

For a reference implementation, see the [hairper release workflow](https://github.com/HarperFast/hairper/blob/main/.github/workflows/release.yaml).

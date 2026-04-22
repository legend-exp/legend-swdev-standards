# legend-swdev-standards

Common software development files for the
[legend-exp](https://github.com/legend-exp) GitHub organization, distributed
automatically to subscriber repositories via pull requests.

## How it works

1. Files are maintained here as the single source of truth.
2. On every push to `main`, a GitHub Actions workflow opens pull requests in all
   subscriber repositories, placing each file at its configured destination path
   (with optional renaming).
3. Maintainers review and merge the PR — or enable
   [auto-merge](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/automatically-merging-a-pull-request)
   in their repo so it lands once CI passes.

## Subscribing a repository

Open a pull request that adds your repository to the relevant group(s) in
[`.github/sync.yml`](.github/sync.yml). Each entry specifies:

- **`source`** — path to the file in this repository
- **`dest`** — path where the file should be placed in your repository (rename
  freely)

Example:

```yaml
group:
  - repos: |
      legend-exp/my-repo
    files:
      - source: AI_POLICY.md
        dest: AI_POLICY.md
      - source: .pre-commit-config.yaml
        dest: .pre-commit-config.yaml
```

## Required setup (one-time, by org admin)

A fine-grained GitHub PAT named `LEGEND_SWDEV_STANDARDS_SYNC` must be set as an
organization secret on `legend-exp`. It needs `contents: write` and
`pull-requests: write` permissions on all target repositories within the org.

## Automerge

Sync PRs are labeled `standards-sync`. To auto-merge them in a target repo:

- Enable **Allow auto-merge** in the target repo's settings.
- Ensure at least one required status check is configured in branch protection.
- Call `gh pr merge --auto --squash <PR_NUMBER>` in a workflow, or use
  [GitHub's native automerge](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/automatically-merging-a-pull-request)
  triggered by the `standards-sync` label.

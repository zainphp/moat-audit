# Moat Audit Action

A GitHub Action that runs [Laravel Moat](https://github.com/laravel/moat) against the repository using it. It downloads the latest Moat release, verifies its checksum, and prints a Markdown report in the Actions log. Runs on `ubuntu-latest`.

## Use it

Add it as a step in a workflow:

```yaml
jobs:
  moat-audit:
    runs-on: ubuntu-latest
    steps:
      - name: Audit repository
        uses: zainphp/moat-audit@1
        with:
          fail_on_findings: true
          github_token: ${{ secrets.MOAT_TOKEN }}
```

Choose the workflow triggers, such as `workflow_dispatch` or `schedule`. [moat-audit-example.yml](.github/workflows/moat-audit-example.yml) shows a manual and weekly schedule. The `@1` reference requires a `1` version tag in this repository.

## Input

| Input | Default | Description |
| --- | --- | --- |
| `github_token` | Caller `GITHUB_TOKEN` | Optional token whose owner has admin access to the repository. Pass it as a repository or organization secret when the built-in token is insufficient. |
| `fail_on_findings` | `true` | Fail the job when Moat reports security findings. Set to `false` to keep findings non-blocking. Authentication and execution errors still fail the job. |

## Authentication note

Moat requires repository-admin access. The built-in `GITHUB_TOKEN` may not satisfy that check; pass an admin-capable token through `github_token` if needed. The action preserves Moat's access check and fails when authentication is rejected.

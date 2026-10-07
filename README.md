# Moat Audit Action

A GitHub Action that runs [Laravel Moat](https://github.com/laravel/moat) against the repository using it. It downloads the latest Moat release, verifies its checksum, and prints a Markdown report in the Actions log. Runs on `ubuntu-latest`.

## Use as a step

Add the action to an existing job:

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

## Use as a reusable workflow

Call it as a job when you want the audit to run in its own Ubuntu job:

```yaml
jobs:
  moat-audit:
    uses: zainphp/moat-audit/.github/workflows/moat-audit.yml@1
    with:
      fail_on_findings: true
    secrets:
      github_token: ${{ secrets.MOAT_TOKEN }}
```

The reusable workflow delegates to the same composite action. Define triggers in the caller workflow; omit the `secrets` block to use the caller's built-in token.

## Input

| Input | Default | Description |
| --- | --- | --- |
| `github_token` | Caller `GITHUB_TOKEN` | Optional token whose owner has admin access to the repository. Pass it as a repository or organization secret when the built-in token is insufficient. |
| `fail_on_findings` | `true` | Fail the job when Moat reports security findings. Set to `false` to keep findings non-blocking. Authentication and execution errors still fail the job. |

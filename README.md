# GitHub Marketplace Action

<!--START_SECTION:daily-->
```diff
██████░░░░░░░░░░░░░░░░░░░ ⁝ 25.31% • TypeScript
██████░░░░░░░░░░░░░░░░░░░ ⁝ 24.37% • Other
████░░░░░░░░░░░░░░░░░░░░░ ⁝ 15.48% • JSON
███░░░░░░░░░░░░░░░░░░░░░░ ⁝ 11.85% • Bash
███░░░░░░░░░░░░░░░░░░░░░░ ⁝ 10.34% • Markdown
█░░░░░░░░░░░░░░░░░░░░░░░░ ⁝ 5.67% • JavaScript
█░░░░░░░░░░░░░░░░░░░░░░░░ ⁝ 2.75% • XML
█░░░░░░░░░░░░░░░░░░░░░░░░ ⁝ 2.64% • YAML
░░░░░░░░░░░░░░░░░░░░░░░░░ ⁝ 1.23% • Git Config
░░░░░░░░░░░░░░░░░░░░░░░░░ ⁝ 0.25% • TSConfig
░░░░░░░░░░░░░░░░░░░░░░░░░ ⁝ 0.08% • CSV
░░░░░░░░░░░░░░░░░░░░░░░░░ ⁝ 0.02% • HTML
```
<!--END_SECTION:daily-->


![stars](https://stats.pphat.top/project/stars?repo=pphatdev/daily-waka-action)
![watchers](https://stats.pphat.top/project/watchers?repo=pphatdev/daily-waka-action)
![issues](https://stats.pphat.top/project/issues?repo=pphatdev/daily-waka-action)

<br>

This repository now includes a reusable GitHub Action at the root: `action.yml`.

## What it does

- Fetches daily WakaTime coding data
- Updates the section between `<!--START_SECTION:daily-->` and `<!--END_SECTION:daily-->`

```shell
<!--START_SECTION:daily-->

<!-- Diff stats here -->

<!--END_SECTION:daily-->
```

- Writes raw API response to `data/coding_stats.json`
- Exposes an output named `changed` (`true` or `false`)

## Inputs

- `waka-token` (required): WakaTime API token
- `github-token` (optional): GitHub token for script compatibility
- `waka-api` (optional): defaults to `https://wakatime.com/api/v1`
- `bar-style` (optional): `block`, `shade`, `ascii`, `dot`, `pipe`, `emoji`
- `python-version` (optional): defaults to `3.12`
- `entrypoint` (optional): defaults to `src/main.py`

## Example workflow usage

```yaml
name: Update README Daily

on:
  schedule:
    - cron: "0 */6 * * *"
  workflow_dispatch:

permissions:
  contents: write

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Waka README Action
        id: daily
        uses: pphatdev/daily-waka-action@v1
        with:
          waka-token: ${{ secrets.WAKA_KEY }}
          github-token: ${{ secrets.GITHUB_TOKEN }}
          bar-style: block

      - name: Commit generated files
        if: steps.daily.outputs.changed == 'true'
        uses: stefanzweifel/git-auto-commit-action@v5
        with:
          commit_message: "chore(readme): update daily coding stats"
          file_pattern: "README.md data/coding_stats.json"
```
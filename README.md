# Reuseable should run workflow

Only run jobs in projects that have changed.

## Usage

```yaml
jobs:
  should-run:
    uses: JensDll/should-run/.github/workflows/main.yaml@main
    with:
      # A list of projects to match against.
      # Required: true
      projects: web api db
      # Filter to use on git diff.
      # Required: false
      # Default: dr
      diff-filter: dr

  web:
    if: fromJSON(needs.should-run.outputs.result).web == 'true'
    runs-on: ubuntu-latest
    needs: should-run
    steps:
      - name: Some step

  api:
    if: fromJSON(needs.should-run.outputs.result).api == 'true'
    runs-on: ubuntu-latest
    needs: should-run
    steps:
      - name: Some step

  db:
    if: fromJSON(needs.should-run.outputs.result).db == 'true'
    runs-on: ubuntu-latest
    needs: should-run
    steps:
      - name: Some step
```

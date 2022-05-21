# Reuseable should run workflow

Only run jobs in projects that have changed. Useful for monorepo workflows. See [`./.github/workflows/main.yaml`](./.github/workflows/main.yaml) for the implementation.

## Usage

```yaml
jobs:
  should-run:
    uses: JensDll/should-run/.github/workflows/main.yaml@main
    with:
      # A list of projects to match against as full paths.
      # Required: true
      projects: services/web services/api services/db
      # Filter to use on git diff.
      # Required: false
      # Default: ''
      diff_filter: d

  web:
    if: fromJSON(needs.should-run.outputs.result)['services/web'] == 'true'
    runs-on: ubuntu-latest
    needs: should-run
    steps:
      - run: echo "Hello web"

  api:
    if: fromJSON(needs.should-run.outputs.result)['services/api'] == 'true'
    runs-on: ubuntu-latest
    needs: should-run
    steps:
      - run: echo "Hello api"

  db:
    if: fromJSON(needs.should-run.outputs.result)['services/db'] == 'true'
    runs-on: ubuntu-latest
    needs: should-run
    steps:
      - run: echo "Hello db"
```

# Lint Mapbox GL Style GitHub Action

This is a GitHub Action definition that lints Mapbox GL styles in a repo.

## Usage

### Inputs

 * `style-paths`: the path to the style (eg `*.json`)

### Outputs

 * `lint-message`: the output from the linter, if any

### Example

To lint a pull request and add a comment on failure you might use a workflow like this:

```
on: 
  pull_request:
    branches:
      - main
    paths:
      - '**/*.json'

jobs:
  lint:
    name: Lint
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - id: lint
        name: Linter
        uses: ./.github/actions/lint
        with:
          style-paths: '*.json'
      - name: Post PR comment
        if: ${{ failure() }}
        uses: mshick/add-pr-comment@v1
        with:
          message: |
            ## Style lint failure
            
            Please fix style issues:

            ```
            ${{ steps.lint.outputs.lint-message }}
            ```
          repo-token: ${{ secrets.GITHUB_TOKEN }}
```

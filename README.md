# npm-version-check-with-comment-action

This is a composite action that checks out the pr and base branch, compares package.json versions, and comments on the PR whether there is a change or not.

This can be ran more than once on the same PR.

## Usage

```yml
name: "PR to Main"
on:
  pull_request:
    branches:
      - main

jobs:
  test: # make sure the action works on a clean machine without building
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - uses: dkershner6/npm-version-check-with-comment-action@v1
        id: version-check
        with:
          github-token: "${{ secrets.GITHUB_TOKEN }}"
          node-version: "12.x" # optional, defaults to 14.x
          
      # Outputs whether or not it changed
      - run: echo ${{ steps.version-check.outputs.version-did-change }}
      
      # Optionally fail the workflow, should you choose
      - name: Fail on no version updated
        if: ${{ steps.version-check.outputs.version-did-change == 'false' }}
        run: 'echo "No version change :/" && exit 1'
```

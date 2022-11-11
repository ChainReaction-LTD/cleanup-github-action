# cleanup-github-action
Simply removes all files from the workspace directory and optionally additional directories on self-hosted runners.

This is useful to clean residue in workspaces from previous self-hosted executions which can effect the current execution.

It seems the [checkout action](https://github.com/actions/checkout)'s default clean command (`git clean -ffdx && git reset --hard HEAD`) is insufficiet in some cases and the residuals from previous executions on the same self-hosted runner can fail the current execution.  
In addition, there might be additional files/directories outside the workspace, which should be cleaned-up and this action provides the capability to remove them.

## Usage
```yaml
---
name: Build with Clean

on:
  pull_request:
    types:
      - opened
      - reopened
      - synchronize

jobs:
  build:
    runs-on: self-hosted
    steps:
      - name: Cleanup workspace
        uses: ChainReaction-LTD/cleanup-github-action@v1
        with:
          # Whether to cleanup hidden files/directories as well (i.e. files/directories whose name start with dot).
          # default: true
          include-hidden: ""

          # Comma separated list of additional files/directories to cleanup.
          # Default: ""
          additional-resources: "${HOME}/.npm"

      - name: Checkout
        uses: actions/checkout@v3
```

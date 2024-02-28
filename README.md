# Release Provider

Github action to open releases following [conventional-commits](https://www.conventionalcommits.org/en/v1.0.0/), in order:

- Create a release branch
- Update Changelog and tags with [standard-version](https://github.com/conventional-changelog/standard-version)
- Push the new branch to your repo
- Open the pull request against the branch you specify

## Attention

### Naming Branch Strategy

- **New Features** `feat/<Name-of-feature>` from `develop`.
- **Bugfix** `fix/<Name-of-bugfix>` from `develop`.
- **Hotfix** `hotfix/<Name-of-hotfix>` from `main`.

### Commit messages

See [standard-version](https://github.com/conventional-changelog/standard-version#commit-message-convention-at-a-glance) for more details.

## Inputs

| Name              | Type      | Description |
| -------           | ------    | ----------- |
| `token`           | string    | Github Token with write permissions **Required**|
| `release-version` | string    | Version you want to release, in the formart MAJOR.MINOR.PATH **Optional**|
| `origin-branch`   | string    | Branch from where the release should be opened, usually develop **Required**|
| `target-branch`   | string    | Branch where the release should be merged, usually master or main **Required**|
| `as-draft`        | bool      | Boolean flag to open as draft or not **Default: False**|
| `pr-template`     | string    | Template to be used as PR description **Default: "PR opened by [release-provider](https://github.com/brennogb/release-provider)"** |

## Usage

### Example Workflow file

```yaml
name: "Create new release"

on:
  workflow_dispatch:
    inputs:
      version:
        description: 'The version you want to release.'
        required: true

jobs:
  create-new-release:
    name: "Create a new release"
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Draft new release
        uses: brennogb/release-provider@1.0.0
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          release-version: ${{ github.event.inputs.version }}
          origin-branch: develop
          target-branch: main
          as-draft: false
          pr-template: |
            Pull request body is here
```

## Contributing

This project is totally open source and contributors are welcome.
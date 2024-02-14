# Semantic Version - GitHub Action and Reusable Workflow

This repository contains both a GitHub Action and a Reusable Workflow to determine the next version.

The current version is determined via `gh release view`. From there the next version is determined based on the labels of the *pull request*. Valid labels are:

- *MAJOR* version when you make incompatible API changes
- *MINOR* version when you add functionality in a backward compatible manner
- *PATCH* version when you make backward compatible bug fixes
- *CANDIDATE* for pre-release

See [Semantic Versioning 2.0.0](https://semver.org/).

> *Note:* The labels might be any combination of lower and upper case letters, only the sequence of them is relevant, e.g. *Major* or *mAjOr*

## Inputs

### `prefix`

**Optional** Prefix. Default `''`. The specified `prefix` will be added in front of the version, e.g. with `prefix = v.` the `version = 1.2.3` will become `v.1.2.3`.

### `create_release`

**Optional** Create release. Default `false`. When `true`this action will create a simple `gh release create`.

### `target_branch`

**Optional** Target branc. Default `'main'`. The branch for the creation of an release.

## Example - Action

### Preview next version with prefix `v`

```yaml
on:
  pull_request:
    branches:
      - main

permissions:
  contents: write

jobs:
  semantic-version:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@b4ffde65f46336ab88eb53be808477a3936bae11
        with:
          ref: main

      - name: Get next version
        uses: mw8ero/semantic-version@main
        with:
          prefix: 'v'
```

### Next version with release

```yaml
on:
  pull_request:
    branches:
      - main
    types:
      - closed

permissions:
  contents: write

jobs:
  semantic-version:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@b4ffde65f46336ab88eb53be808477a3936bae11
        with:
          ref: main

      - name: Get next version
        uses: mw8ero/semantic-version@main
        with:
          create_release: true
          target_branch: main
```

## Example - Reusable Workflow

The file [semantic-version.yaml](.github/workflows/semantic-version.yaml) is a reuseable workflow, which is used by

- [preview-version.yaml](.github/workflows/preview-version.yaml) to get a preview of the next version before closing the pull request.
- [release-version.yaml](.github/workflows/release-version.yaml)) to get the next version and perform a release after merging a pull request.

## License

This library is under the [MIT](LICENSE) license.
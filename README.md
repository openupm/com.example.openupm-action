# com.example.openupm-action

Example Unity package repository for the
[`openupm/openupm-action`](https://github.com/openupm/openupm-action) publish
wait GitHub Action.

This repository demonstrates the anonymous OpenUPM publishing flow from a tag
push. It is intentionally a tag-push workflow demo:

1. Push a Git tag that contains a parseable semver package version.
2. GitHub Actions requests a GitHub OIDC token for the `openupm` audience.
3. `openupm/openupm-action` asks OpenUPM to scan this registered repository.
4. The workflow waits until that package version is installable or fails with
   the OpenUPM release reason.

No OpenUPM account, personal access token, or repository secret is required.

## Package

The package is stored in `package/` and uses the package name
`com.example.openupm-action`.

## Tag Push Demo

The included `.github/workflows/openupm.yml` workflow runs for tags. Send a tag
to `openupm/openupm-action` only when the tag contains a parseable semver
package version, such as `1.0.0`, `v1.0.0`, `upm/1.0.0`, or
`com.example.openupm-action@v1.0.0`:

```bash
git tag 1.0.0
git push origin 1.0.0
```

The tag name is passed to the action. Tags without a parseable semver package
version are rejected before the action contacts OpenUPM:

```yaml
- uses: openupm/openupm-action@v1
  with:
    package: com.example.openupm-action
    tag: ${{ github.ref_name }}
```

The workflow grants:

```yaml
permissions:
  id-token: write
  contents: read
```

`id-token: write` lets the action request the short-lived GitHub OIDC token
that OpenUPM verifies.

## GitHub Release Variant

The same action can run after a GitHub Release is published. Use the
[documented GitHub Release workflow](https://openupm.com/docs/github-action-publish.html#github-release-workflow)
for packages that use GitHub Release asset tracking, including signed packages:

```yaml
name: OpenUPM

on:
  release:
    types: [published]

permissions:
  id-token: write
  contents: read

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: openupm/openupm-action@v1
        with:
          package: com.example.openupm-action
          tag: ${{ github.event.release.tag_name }}
```

Release tags follow the same rule: use a tag that contains a parseable semver
package version, or the action fails before contacting OpenUPM.

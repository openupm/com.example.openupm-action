# com.example.openupm-action

Example Unity package repository for the OpenUPM publish wait GitHub Action.

This repository demonstrates the anonymous OpenUPM publishing flow:

1. Push a semver tag such as `1.0.0`.
2. GitHub Actions requests a GitHub OIDC token for the `openupm` audience.
3. `openupm/openupm-action` asks OpenUPM to scan this registered repository.
4. The workflow waits until the matching package version is installable or
   fails with the OpenUPM release reason.

No OpenUPM account, personal access token, or repository secret is required.

## Package

The package is stored in `package/` and uses the package name
`com.example.openupm-action`.

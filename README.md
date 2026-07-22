# GitHub Actions Workflows

GitHub Actions [Reusable Workflows](https://docs.github.com/en/actions/sharing-automations/reusing-workflows) used in
SoftwareMill projects.\
If you use a workflow from this repository, please add its usage to the corresponding section
`List of repositories using this workflow` in this document.

## Architecture

1. All Workflows [have to be located](https://docs.github.com/en/actions/sharing-automations/reusing-workflows#creating-a-reusable-workflow)
   in the `.github/worksflows` path, otherwise GitHub Actions won't be able to use them.
2. Actions and Workflows differ in GitHub Actions. This repository contains only Workflows.

   - [Action](https://docs.github.com/en/actions/about-github-actions/understanding-github-actions#actions) is a
     self-contained piece of code written e.g. in JS.

     ```yaml
     example-action-use:
       uses: actions/checkout@v4
     ```
     
   - [Workflow](https://docs.github.com/en/actions/about-github-actions/understanding-github-actions#workflows) is not a
     code, but a collection of correlated Actions contained in yaml files.

     ```yaml
     example-workflow-use:
       uses: softwaremill/github-actions-workflows/.github/workflows/auto-merge.yml@main
     ```

#### Helpful links

- [Reusable Workflows Docs](https://docs.github.com/en/actions/sharing-automations/reusing-workflows)
- [Workflow syntax](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions)
- [GitHub Actions Formatting conventions](https://nimblehq.co/compass/development/code-conventions/github-actions/)

## List of reusable workflows

1. [Auto Merge](#auto-merge)
2. [Label](#label)
3. [Scala Steward](#scala-steward)
4. [Publish Release](#publish-release)
5. [Mima](#mima)
6. [Build Scala](#build-scala)
7. [Test Report](#test-report)

## List of org-wide workflows

These run and live in this repository (they are not called via `uses:`); they act across the organisation.

1. [Dependabot Cooldown](#dependabot-cooldown)

## [Auto Merge](./.github/workflows/auto-merge.yml)

This workflow is responsible for merging pull requests that are ready to be merged.

### Usage

```yaml
  auto-merge:
    # only for PRs by softwaremill-ci
    if: github.event.pull_request.user.login == 'softwaremill-ci'
    needs: [ ci, mima, label ]
    uses: softwaremill/github-actions-workflows/.github/workflows/auto-merge.yml@main
    secrets: inherit
```

## [Label](./.github/workflows/label.yml)

This workflow is responsible for labeling pull requests. It attaches label if there is exactly one file changed by
steward and this file belongs to a whitelist specified by `labeler.yml`

### Usage

```yaml
  label:
    # only for PRs by softwaremill-ci
    if: github.event.pull_request.user.login == 'softwaremill-ci'
    uses: softwaremill/github-actions-workflows/.github/workflows/label.yml@main
    secrets: inherit
```

## [Scala Steward](./.github/workflows/scala-steward.yml)

This workflow is responsible for running Scala Steward.

It applies a central default config ([`default.scala-steward.conf`](./default.scala-steward.conf)) that enforces a supply-chain cooldown (`updates.cooldown.minimumAge = "3 days"`), so new dependency versions are not adopted until they have matured. A repository's own `.scala-steward.conf` (pins, ignores, per-dependency overrides) is still applied on top and takes precedence per field.

### Usage

```yaml
name: Scala Steward

# This workflow will launch at 00:00 every day
on:
  schedule:
    - cron: '0 0 * * *'
  workflow_dispatch:

permissions:
  contents: write        # Required to checkout and push changes
  pull-requests: write   # Required to create PRs for dependency updates

jobs:
  scala-steward:
    uses: softwaremill/github-actions-workflows/.github/workflows/scala-steward.yml@main
    secrets:
      github-token: ${{ secrets.SOFTWAREMILL_CI_PR_TOKEN }}
    with:
      java-version: '21'
```

#### List of input params

| Name         | Description                       | Required | Default | Example     |
|--------------|-----------------------------------|----------|---------|-------------|
| java-version | Java version used in the workflow | No       | '11'    | '21'        |
| java-opts    | Java options used in the workflow | No       | ""      | "-Xmx3000M" |

## [Publish Release](./.github/workflows/publish-release.yml)

This workflow is responsible for publishing build artifacts and release notes of scala projects.
It uses multiple secrets so clause `secrets: inherit` has to be added to the workflow definition.

### Usage

```yaml
  publish-release:
    uses: softwaremill/github-actions-workflows/.github/workflows/publish-release.yml@main
    secrets: inherit
    with:
      java-version: '11'
      java-opts: "-Xmx3000M -Dsbt.task.timings=true"
      sttp-native: 1
```

#### List of input params

| Name         | Description                                                                     | Required | Default | Example                             |
|--------------|---------------------------------------------------------------------------------|----------|---------|-------------------------------------|
| java-version | Java version used in the workflow                                               | No       | '11'    | '21'                                |
| java-opts    | Java options used in the workflow                                               | No       | ""      | "-Xmx3000M -Dsbt.task.timings=true" |
| sttp-native  | Flag indicating if the sttp-native should be included in the aggregate projects | No       | 0       | 1                                   |

## [Mima](./.github/workflows/mima.yml)

This workflow is responsible for running MiMa (binary compatibility checker).

### Usage

```yaml
  mima:
    uses: softwaremill/github-actions-workflows/.github/workflows/mima.yml@main
    with:
      java-version: '11'
      java-opts: "-Xmx4G"
```

#### List of input params

| Name         | Description                       | Required | Default | Example                             |
|--------------|-----------------------------------|----------|---------|-------------------------------------|
| java-version | Java version used in the workflow | No       | '11'    | '21'                                |
| java-opts    | Java options used in the workflow | No       | ""      | "-Xmx3000M -Dsbt.task.timings=true" |

## [Build Scala](./.github/workflows/build-scala.yml)

This workflow is responsible for building Scala projects.

### Usage

```yaml
  build-scala:
    uses: softwaremill/github-actions-workflows/.github/workflows/build-scala.yml@main
    with:
      java-version: '11'
      java-opts: '-Xmx3000M -Dsbt.task.timings=true'
      sttp-native: 1
      install-libidn11: true
```

#### List of input params

| Name                  | Description                                                                     | Required | Default | Example                             |
|-----------------------|---------------------------------------------------------------------------------|----------|---------|-------------------------------------|
| java-version          | Java version used in the workflow                                               | No       | '11'    | '21'                                |
| java-opts             | Java options used in the workflow                                               | No       | ""      | "-Xmx3000M -Dsbt.task.timings=true" |
| sttp-native           | Flag indicating if the sttp-native should be included in the aggregate projects | No       | 0       | 1                                   |
| install-libidn11      | Flag indicating if the libidn11 library should be installed                     | No       | false   | true                                |
| install-libidn2       | Flag indicating if the libidn2 library should be installed                      | No       | false   | true                                |
| compile-documentation | Flag indicating if the project documentation should be compiled                 | No       | false   | true                                |

## [Test Report](./.github/workflows/test-report.yml)

This workflow is responsible for generating test reports.

### Usage

```yaml
  test-report:
    uses: softwaremill/github-actions-workflows/.github/workflows/test-report.yml@main
```

## [Dependabot Cooldown](./.github/workflows/dependabot-cooldown.yml)

A central, self-maintaining job that lives and runs in this repository and patches Dependabot configs across the org.
New dependency versions are held for `cooldown-days` days before Dependabot proposes a *version* update, so a
compromised release has time to be detected before adoption. Security updates (published GHSA/CVE advisories) bypass
the cooldown and are unaffected.

Dependabot has no org-wide / remote config, so a `cooldown` block must live statically in each repo's file. Instead of
maintaining a hand-written repo list (which drifts) or asking every repo to opt in with its own caller workflow, this
workflow **discovers** the target set at run time and **fans out** over it:

1. **discover** — enumerates the repos the GitHub App is installed on, keeps the active (non-archived, non-fork) ones
   that actually contain a Dependabot config, minus the `EXCLUDE` policy list, and emits a matrix.
2. **patch** — one matrix job per discovered repo: mints a short-lived token scoped to just that repo, patches
   `cooldown.default-days` on every `updates:` entry (surgically, preserving comments and key order), and opens a PR.

The `cooldown-days` value is a single central knob: it defaults to `3`, and can be set ad-hoc via `workflow_dispatch`.
The patch is strict — it overwrites any existing `default-days`, so the central policy stays the source of truth. PRs
are opened with the App token, so the target repo's own CI runs on the cooldown PR (unlike `GITHUB_TOKEN`, which cannot
trigger downstream workflows). PRs are left for review and are not auto-merged.

### Prerequisites (one-time, org admin)

Register a GitHub App in the `softwaremill` org and install it on the repositories that should carry the cooldown:

- **Permissions:** `contents: write`, `pull-requests: write`, `metadata: read`.
- **Installation:** the app's installed repositories are the candidate universe for discovery — install it org-wide (the
  file-presence filter scopes it) or only on the repos you want covered.
- Store the app's **client ID** in the repository/org variable `DEPENDABOT_COOLDOWN_APP_CLIENT_ID` and its **private key**
  in the secret `DEPENDABOT_COOLDOWN_APP_KEY`.

The App private key is the only standing secret; every runtime token is repo-scoped and expires in ~1 hour.

### Usage

Nothing to add to individual repositories. The workflow runs on a weekly schedule and can be triggered manually:

- **Retune the whole fleet:** `workflow_dispatch` with `cooldown-days: '7'`.
- **Targeted re-run:** `workflow_dispatch` with `repos: 'tapir bootzooka'` to patch a specific subset instead of running
  discovery.
- **Exclude a repo from policy:** add its name to the `EXCLUDE` env list in the workflow.

If a discovered repository has no `.github/dependabot.yml` (or `.yaml`), that matrix job is a no-op.

#### List of dispatch inputs

| Name          | Description                                                          | Required | Default | Example          |
|---------------|----------------------------------------------------------------------|----------|---------|------------------|
| cooldown-days | Number of days a new version is held before a bump PR                | No       | '3'     | '7'              |
| repos         | Explicit repo list to patch instead of discovery (comma/space-sep.)  | No       | ''      | 'tapir bootzooka'|

## Remarks

- All workflows using sbt with ubuntu 24.04 need to add `setup-sbt` step because sbt was removed from the image as
  described [here](https://github.com/sbt/setup-sbt?tab=readme-ov-file#december-2024).

## List of actions used in workflows

| action                                                                                              | version | description                                                            |
|-----------------------------------------------------------------------------------------------------|---------|------------------------------------------------------------------------|
| [actions/checkout](https://github.com/actions/checkout)                                             | v4      | checks-out your repository under $GITHUB_WORKSPACE                     |
| [actions/setup-java](https://github.com/actions/setup-java)                                         | v4      | downloads Java, configures runners, caches dependencies                |
| [actions/upload-artifact](https://github.com/actions/upload-artifact)                               | v4      | uploads artifacts from the workflow's workspace to be downloaded later |
| [dorny/test-reporter](https://github.com/dorny/test-reporter)                                       | v2      | generates test reports and uploads them as workflow artifacts.         |
| [pascalgn/automerge-action](https://github.com/pascalgn/automerge-action)                           | v0.16.4 | automatically merges PRs with `automerge` label                        |
| [release-drafter/release-drafter](https://github.com/release-drafter/release-drafter)               | v6      | drafts release notes based on merged pull requests                     |
| [sbt/setup-sbt](https://github.com/sbt/setup-sbt)                                                   | v1      | enables `sbt` runner                                                   |
| [scala-steward-org/scala-steward-action](https://github.com/scala-steward-org/scala-steward-action) | v2      | automates dependency updates for Scala projects                        |
| [srvaroa/labeler](https://github.com/srvaroa/labeler)                                               | master  | manages labels for both Pull Requests and Issues                       | 
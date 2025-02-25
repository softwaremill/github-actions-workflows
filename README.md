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

#### List of actions used in workflows

| action                                                                                              | version | description                                                            |
|-----------------------------------------------------------------------------------------------------|---------|------------------------------------------------------------------------|
| [actions/checkout](https://github.com/actions/checkout)                                             | v4      | checks-out your repository under $GITHUB_WORKSPACE                     |
| [actions/setup-java](https://github.com/actions/setup-java)                                         | v4      | downloads Java, configures runners, caches dependencies                |
| [actions/upload-artifact](https://github.com/actions/upload-artifact)                               | v4      | uploads artifacts from the workflow's workspace to be downloaded later |
| [dorny/test-reporter](https://github.com/dorny/test-reporter)                                       | v1      | generates test reports and uploads them as workflow artifacts.         |
| [pascalgn/automerge-action](https://github.com/pascalgn/automerge-action)                           | v0.16.4 | automatically merges PRs with `automerge` label                        |
| [peter-evans/create-or-update-comment](https://github.com/peter-evans/create-or-update-comment)     | v4      | creates or updates issues or pull request comments                     |
| [peter-evans/rebase](https://github.com/peter-evans/rebase)                                         | v3      | rebases pull requests in a repository                                  |
| [peter-evans/slash-command-dispatch](https://github.com/peter-evans/slash-command-dispatch)         | v4      | creates dispatch events for slash commands                             |
| [release-drafter/release-drafter](https://github.com/release-drafter/release-drafter)               | v5      | drafts release notes based on merged pull requests                     |
| [sbt/setup-sbt](https://github.com/sbt/setup-sbt)                                                   | v1      | enables `sbt` runner                                                   |
| [scala-steward-org/scala-steward-action](https://github.com/scala-steward-org/scala-steward-action) | v2      | automates dependency updates for Scala projects                        |
| [srvaroa/labeler](https://github.com/srvaroa/labeler)                                               | master  | manages labels for both Pull Requests and Issues                       |

## List of reusable workflows

1. [Auto Merge](#auto-merge)
2. [Label](#label)
3. [Scala Steward](#scala-steward)
4. [Publish Release](#publish-release)
5. [Mima](#mima)
6. [Build Scala](#build-scala)
7. [Test Report](#test-report)
8. [Rebase cmd](#rebase-cmd)
9. [Rebase cmd dispatch](#rebase-cmd-dispatch)

## [Auto Merge](./.github/workflows/auto-merge.yml)

This workflow is responsible for merging pull requests that are ready to be merged.

### Usage

```yaml
  auto-merge:
    uses: softwaremill/github-actions-workflows/.github/workflows/auto-merge.yml@main
```

### List of repositories using this workflow

| Repository                                                 | Files                                                                                                            |
|------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| [sttp-openai](https://github.com/softwaremill/sttp-openai) | [ci.yml](https://github.com/softwaremill/sttp-openai/blob/master/.github/workflows/ci.yml)                       |
| [adopt-tapir](https://github.com/softwaremill/adopt-tapir) | [adopt-tapir-ci.yml](https://github.com/softwaremill/adopt-tapir/blob/main/.github/workflows/adopt-tapir-ci.yml) |
| [macwire](https://github.com/softwaremill/macwire)         | [ci.yml](https://github.com/softwaremill/macwire/blob/master/.github/workflows/ci.yml)                           |
| [magnolia](https://github.com/softwaremill/magnolia)       | [ci.yml](https://github.com/softwaremill/magnolia/blob/scala3/.github/workflows/ci.yml)                          |
| [sttp-shared](https://github.com/softwaremill/sttp-shared) | [ci.yml](https://github.com/softwaremill/sttp-shared/blob/master/.github/workflows/ci.yml)                       |
| [sttp-model](https://github.com/softwaremill/sttp-model)   | [ci.yml](https://github.com/softwaremill/sttp-model/blob/master/.github/workflows/ci.yml)                        |
| [tapir](https://github.com/softwaremill/tapiros)           | [ci.yml](https://github.com/softwaremill/tapir/blob/master/.github/workflows/ci.yml)                             |
| [bootzooka](https://github.com/softwaremill/bootzooka)     | [bootzooka-ci.yml](https://github.com/softwaremill/bootzooka/blob/master/.github/workflows/bootzooka-ci.yml)     |
| [sttp](https://github.com/softwaremill/sttp)               | [ci.yml](https://github.com/softwaremill/sttp/blob/master/.github/workflows/ci.yml)                              |
| [ox](https://github.com/softwaremill/ox)                   | [ci.yml](https://github.com/softwaremill/ox/blob/master/.github/workflows/ci.yml)                                |

## [Label](./.github/workflows/label.yml)

This workflow is responsible for labeling pull requests. It attaches label if there is exactly one file changed by
steward and this file belongs to a whitelist specified by `labeler.yml`

### Usage

```yaml
  label:
    uses: softwaremill/github-actions-workflows/.github/workflows/label.yml@main
```

### List of repositories using this workflow

| Repository                                                 | Files                                                                                                            |
|------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| [sttp-openai](https://github.com/softwaremill/sttp-openai) | [ci.yml](https://github.com/softwaremill/sttp-openai/blob/master/.github/workflows/ci.yml)                       |
| [adopt-tapir](https://github.com/softwaremill/adopt-tapir) | [adopt-tapir-ci.yml](https://github.com/softwaremill/adopt-tapir/blob/main/.github/workflows/adopt-tapir-ci.yml) |
| [macwire](https://github.com/softwaremill/macwire)         | [ci.yml](https://github.com/softwaremill/macwire/blob/master/.github/workflows/ci.yml)                           |
| [magnolia](https://github.com/softwaremill/magnolia)       | [ci.yml](https://github.com/softwaremill/magnolia/blob/scala3/.github/workflows/ci.yml)                          |
| [sttp-shared](https://github.com/softwaremill/sttp-shared) | [ci.yml](https://github.com/softwaremill/sttp-shared/blob/master/.github/workflows/ci.yml)                       |
| [sttp-model](https://github.com/softwaremill/sttp-model)   | [ci.yml](https://github.com/softwaremill/sttp-model/blob/master/.github/workflows/ci.yml)                        |
| [tapir](https://github.com/softwaremill/tapiros)           | [ci.yml](https://github.com/softwaremill/tapir/blob/master/.github/workflows/ci.yml)                             |
| [bootzooka](https://github.com/softwaremill/bootzooka)     | [bootzooka-ci.yml](https://github.com/softwaremill/bootzooka/blob/master/.github/workflows/bootzooka-ci.yml)     |
| [sttp](https://github.com/softwaremill/sttp)               | [ci.yml](https://github.com/softwaremill/sttp/blob/master/.github/workflows/ci.yml)                              |
| [ox](https://github.com/softwaremill/ox)                   | [ci.yml](https://github.com/softwaremill/ox/blob/master/.github/workflows/ci.yml)                                |

## [Scala Steward](./.github/workflows/scala-steward.yml)

This workflow is responsible for running Scala Steward.

### Usage

```yaml
  scala-steward:
    uses: softwaremill/github-actions-workflows/.github/workflows/scala-steward.yml@main
    with:
      java-version: '11'
    secrets:
      repo-github-token: ${{secrets.REPO_GITHUB_TOKEN}}
```

#### List of input params

| Name         | Description                       | Required | Default | Example     |
|--------------|-----------------------------------|----------|---------|-------------|
| java-version | Java version used in the workflow | No       | '11'    | '21'        |
| java-opts    | Java options used in the workflow | No       | ""      | "-Xmx3000M" |

#### List of secrets

| Name              | Description |
|-------------------|-------------|
| repo-github-token | -           |

### List of repositories using this workflow

| Repository                                                   | Files                                                                                                             |
|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| [sttp-openai](https://github.com/softwaremill/sttp-openai)   | [scala-steward.yml](https://github.com/softwaremill/sttp-openai/blob/master/.github/workflows/scala-steward.yml)  |
| [adopt-tapir](https://github.com/softwaremill/adopt-tapir)   | [scala-steward.yml](https://github.com/softwaremill/adopt-tapir/blob/main/.github/workflows/scala-steward.yml)    |
| [macwire](https://github.com/softwaremill/macwire)           | [scala-steward.yml](https://github.com/softwaremill/macwire/blob/master/.github/workflows/scala-steward.yml)      |
| [magnolia](https://github.com/softwaremill/magnolia)         | [scala-steward.yml](https://github.com/softwaremill/magnolia/blob/scala3/.github/workflows/scala-steward.yml)     |
| [sttp-shared](https://github.com/softwaremill/sttp-shared)   | [scala-steward.yml](https://github.com/softwaremill/sttp-shared/blob/master/.github/workflows/scala-steward.yml)  |
| [sttp-model](https://github.com/softwaremill/sttp-model)     | [scala-steward.yml](https://github.com/softwaremill/sttp-model/blob/master/.github/workflows/scala-steward.yml)   |
| [tapir](https://github.com/softwaremill/tapiros)             | [scala-steward.yml](https://github.com/softwaremill/tapir/blob/master/.github/workflows/scala-steward.yml)        |
| [sttp-apispec](https://github.com/softwaremill/sttp-apispec) | [scala-steward.yml](https://github.com/softwaremill/sttp-apispec/blob/master/.github/workflows/scala-steward.yml) |
| [bootzooka](https://github.com/softwaremill/bootzooka)       | [scala-steward.yml](https://github.com/softwaremill/bootzooka/blob/master/.github/workflows/scala-steward.yml)    |
| [sttp](https://github.com/softwaremill/sttp)                 | [scala-steward.yml](https://github.com/softwaremill/sttp/blob/master/.github/workflows/scala-steward.yml)         |
| [ox](https://github.com/softwaremill/ox)                     | [scala-steward.yml](https://github.com/softwaremill/ox/blob/master/.github/workflows/scala-steward.yml)           |
| [kmq](https://github.com/softwaremill/kmq)                   | [scala-steward.yml](https://github.com/softwaremill/kmq/blob/master/.github/workflows/scala-steward.yml)          |
| [retry](https://github.com/softwaremill/retry)               | [scala-steward.yml](https://github.com/softwaremill/retry/blob/master/.github/workflows/scala-steward.yml)        |

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

### List of repositories using this workflow

| Repository                                                   | Files                                                                                       |
|--------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| [sttp-openai](https://github.com/softwaremill/sttp-openai)   | [ci.yml](https://github.com/softwaremill/sttp-openai/blob/master/.github/workflows/ci.yml)  |
| [sttp-shared](https://github.com/softwaremill/sttp-shared)   | [ci.yml](https://github.com/softwaremill/sttp-shared/blob/master/.github/workflows/ci.yml)  |
| [sttp-model](https://github.com/softwaremill/sttp-model)     | [ci.yml](https://github.com/softwaremill/sttp-model/blob/master/.github/workflows/ci.yml)   |
| [sttp-apispec](https://github.com/softwaremill/sttp-apispec) | [ci.yml](https://github.com/softwaremill/sttp-apispec/blob/master/.github/workflows/ci.yml) |
| [macwire](https://github.com/softwaremill/macwire)           | [ci.yml](https://github.com/softwaremill/macwire/blob/master/.github/workflows/ci.yml)      |
| [ox](https://github.com/softwaremill/ox)                     | [ci.yml](https://github.com/softwaremill/ox/blob/master/.github/workflows/ci.yml)           |
| [kmq](https://github.com/softwaremill/kmq)                   | [ci.yml](https://github.com/softwaremill/kmq/blob/master/.github/workflows/ci.yml)          |
| [retry](https://github.com/softwaremill/retry)               | [ci.yml](https://github.com/softwaremill/retry/blob/master/.github/workflows/ci.yml)        |

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

### List of repositories using this workflow

| Repository                                                   | Files                                                                                       |
|--------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| [sttp-shared](https://github.com/softwaremill/sttp-shared)   | [ci.yml](https://github.com/softwaremill/sttp-shared/blob/master/.github/workflows/ci.yml)  |
| [sttp-model](https://github.com/softwaremill/sttp-model)     | [ci.yml](https://github.com/softwaremill/sttp-model/blob/master/.github/workflows/ci.yml)   |
| [sttp-apispec](https://github.com/softwaremill/sttp-apispec) | [ci.yml](https://github.com/softwaremill/sttp-apispec/blob/master/.github/workflows/ci.yml) |
| [sttp](https://github.com/softwaremill/sttp)                 | [ci.yml](https://github.com/softwaremill/sttp/blob/master/.github/workflows/ci.yml)         |
| [kmq](https://github.com/softwaremill/kmq)                   | [ci.yml](https://github.com/softwaremill/kmq/blob/master/.github/workflows/ci.yml)          |

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

### List of repositories using this workflow

| Repository                                                   | Files                                                                                       |
|--------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| [macwire](https://github.com/softwaremill/macwire)           | [ci.yml](https://github.com/softwaremill/macwire/blob/master/.github/workflows/ci.yml)      |
| [sttp-shared](https://github.com/softwaremill/sttp-shared)   | [ci.yml](https://github.com/softwaremill/sttp-shared/blob/master/.github/workflows/ci.yml)  |
| [sttp-model](https://github.com/softwaremill/sttp-model)     | [ci.yml](https://github.com/softwaremill/sttp-model/blob/master/.github/workflows/ci.yml)   |
| [sttp-apispec](https://github.com/softwaremill/sttp-apispec) | [ci.yml](https://github.com/softwaremill/sttp-apispec/blob/master/.github/workflows/ci.yml) |
| [kmq](https://github.com/softwaremill/kmq)                   | [ci.yml](https://github.com/softwaremill/kmq/blob/master/.github/workflows/ci.yml)          |

## [Test Report](./.github/workflows/test-report.yml)

This workflow is responsible for generating test reports.

### Usage

```yaml
  test-report:
    uses: softwaremill/github-actions-workflows/.github/workflows/test-report.yml@main
```

### List of repositories using this workflow

| Repository                                                 | Files                                                                                                        |
|------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| [sttp-openai](https://github.com/softwaremill/sttp-openai) | [test-report.yml](https://github.com/softwaremill/sttp-openai/blob/master/.github/workflows/test-report.yml) |
| [tapir](https://github.com/softwaremill/tapiros)           | [test-report.yml](https://github.com/softwaremill/tapir/blob/master/.github/workflows/test-report.yml)       |
| [ox](https://github.com/softwaremill/ox)                   | [test-report.yml](https://github.com/softwaremill/ox/blob/master/.github/workflows/test-report.yml)          |

## [Rebase cmd](./.github/workflows/rebase-cmd.yml)

This workflow is responsible for rebasing pull requests and adding a comment to them.

### Usage

```yaml
  rebase:
    uses: softwaremill/github-actions-workflows/.github/workflows/rebase-cmd.yml@main
    secrets:
      repo-github-token: ${{ secrets.REPO_GITHUB_TOKEN }}
```

#### List of secrets

| Name              | Description |
|-------------------|-------------|
| repo-github-token | -           |

### List of repositories using this workflow

| Repository                                       | Files                                                                                                |
|--------------------------------------------------|------------------------------------------------------------------------------------------------------|
| [tapir](https://github.com/softwaremill/tapiros) | [rebase-cmd.yml](https://github.com/softwaremill/tapir/blob/master/.github/workflows/rebase-cmd.yml) |
| [ox](https://github.com/softwaremill/ox)         | [rebase-cmd.yml](https://github.com/softwaremill/ox/blob/master/.github/workflows/rebase-cmd.yml)    |

## [Rebase cmd dispatch](./.github/workflows/rebase-cmd-dispatch.yml)

This workflow is responsible for dispatching rebase when it gets a command `/rebase`.

### Usage

```yaml
  rebase-cmd-dispatch:
    uses: softwaremill/github-actions-workflows/.github/workflows/rebase-cmd-dispatch.yml@main
    secrets:
      repo-github-token: ${{ secrets.REPO_GITHUB_TOKEN }}
```

#### List of secrets

| Name              | Description |
|-------------------|-------------|
| repo-github-token | -           |

### List of repositories using this workflow

| Repository                                       | Files                                                                                                                  |
|--------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| [tapir](https://github.com/softwaremill/tapiros) | [rebase-cmd-dispatch.yml](https://github.com/softwaremill/tapir/blob/master/.github/workflows/rebase-cmd-dispatch.yml) |
| [ox](https://github.com/softwaremill/ox)         | [rebase-cmd-dispatch.yml](https://github.com/softwaremill/ox/blob/master/.github/workflows/rebase-cmd-dispatch.yml)    |

## Remarks

- All workflows using sbt with ubuntu 24.04 need to add `setup-sbt` step because sbt was removed from the image as
  described [here](https://github.com/sbt/setup-sbt?tab=readme-ov-file#december-2024). 
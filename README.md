# Github Actions Workflows

Generic Github Actions workflows used in SoftwareMill projects.

## List of reusable workflows

1. [Auto Merge](#auto-merge)
2. [Label](#label)
3. [Scala Steward](#scala-steward)
4. [Publish Release](#publish-release)
5. [Mima](#mima)
6. [Build Scala](#build-scala)

## [Auto Merge](./.github/workflows/auto-merge.yml)

This workflow is responsible for merging pull requests that are ready to be merged.

### Usage

```yaml
  call-workflow-auto-merge:
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
  call-workflow-label:
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
  call-workflow-scala-steward:
    uses: softwaremill/github-actions-workflows/.github/workflows/scala-steward.yml@main
    with:
      java-version: '11'
    secrets:
      repo-github-token: ${{secrets.REPO_GITHUB_TOKEN}}
```

#### List of input params

| Name         | Description                       | Required | Default | Example |
|--------------|-----------------------------------|----------|---------|---------|
| java-version | Java version used in the workflow | Yes      | '11'    | '21'    |

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
  call-workflow-publish-release:
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
| java-version | Java version used in the workflow                                               | Yes      | '11'    | '21'                                |
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
  call-workflow-mima:
    uses: softwaremill/github-actions-workflows/.github/workflows/mima.yml@main
    with:
      java-version: '11'
      java-opts: "-Xmx4G"
```

#### List of input params

| Name         | Description                       | Required | Default | Example                             |
|--------------|-----------------------------------|----------|---------|-------------------------------------|
| java-version | Java version used in the workflow | Yes      | '11'    | '21'                                |
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
  call-workflow-build-scala:
    uses: softwaremill/github-actions-workflows/.github/workflows/build-scala.yml@main
    with:
      java-version: '11'
      java-opts: '-Xmx3000M -Dsbt.task.timings=true'
      sttp-native: 1
      install-libidn11: true
```

#### List of input params

| Name             | Description                                                                     | Required | Default | Example                             |
|------------------|---------------------------------------------------------------------------------|----------|---------|-------------------------------------|
| java-version     | Java version used in the workflow                                               | Yes      | '11'    | '21'                                |
| java-opts        | Java options used in the workflow                                               | No       | ""      | "-Xmx3000M -Dsbt.task.timings=true" |
| sttp-native      | Flag indicating if the sttp-native should be included in the aggregate projects | No       | 0       | 1                                   |
| install-libidn11 | Flag indicating if the libidn11 library should be installed                     | No       | false   | true                                |
| install-libidn2  | Flag indicating if the libidn2 library should be installed                      | No       | false   | true                                |

### List of repositories using this workflow

| Repository                                                   | Files                                                                                       |
|--------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| [macwire](https://github.com/softwaremill/macwire)           | [ci.yml](https://github.com/softwaremill/macwire/blob/master/.github/workflows/ci.yml)      |
| [sttp-shared](https://github.com/softwaremill/sttp-shared)   | [ci.yml](https://github.com/softwaremill/sttp-shared/blob/master/.github/workflows/ci.yml)  |
| [sttp-model](https://github.com/softwaremill/sttp-model)     | [ci.yml](https://github.com/softwaremill/sttp-model/blob/master/.github/workflows/ci.yml)   |
| [sttp-apispec](https://github.com/softwaremill/sttp-apispec) | [ci.yml](https://github.com/softwaremill/sttp-apispec/blob/master/.github/workflows/ci.yml) |
| [kmq](https://github.com/softwaremill/kmq)                   | [ci.yml](https://github.com/softwaremill/kmq/blob/master/.github/workflows/ci.yml)          |
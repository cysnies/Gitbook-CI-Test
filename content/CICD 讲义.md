# CI/CD 讲义

## 一、概述

在当今快速发展的软件开发环境中，速度和质量比以往任何时候都更为重要。随着企业和开发者努力满足用户需求并更频繁地发布更新，需求高效且可靠的开发实践变得至关重要。其中一种获得广泛采用的实践是 **CI/CD** ，即 **持续集成（Continuous Integration）**、**持续交付（Continuous Delivery）**和 **持续部署（Continuous Deployment）**。

CI/CD 是一套现代软件开发实践，旨在自动化和简化代码集成和交付的过程。这些实践使开发团队能够更快地交付功能和修复，同时减少人工干预。通过自动化重复性任务和实施持续测试与集成，CI/CD 有助于减少错误、改善协作，并最终加速发布周期。

![CI/CD Flow](https://www.redhat.com/rhdc/managed-files/styles/wysiwyg_full_width/private/ci-cd-flow-desktop.png.webp?itok=mDEvsSsp)

### 1. 持续集成（Continuous Integration）

**持续集成 (CI)** 是一种需要开发者频繁提交代码到共享仓库的软件实践。 频繁提交代码能较早检测到错误，减少在查找错误来源时开发者需要调试的代码量。 频繁的代码更新也更便于从软件开发团队的不同成员合并更改。 这对开发者非常有益，他们可以将更多时间用于编写代码，而减少在调试错误或解决合并冲突上所花的时间。

提交代码到仓库时，可以持续创建并测试代码，以确保提交未引入错误。 测试环节可以包括代码语法检查（检查样式格式）、安全性检查、代码覆盖率、功能测试及其他自定义检查。

有多种 CI 工具可用于自动化集成过程。常有的一些 CI 工具包括：

- **[Jenkins](https://www.jenkins.io/zh/)**：最广泛使用的开源 CI 工具之一。它支持构建、部署和自动化开发管道。
- **[Github Actions](https://docs.github.com/zh/actions)**：GitHub 推出的 CI 服务，提供可以在存储库中构建代码并运行测试的工作流。
- **[GitLab CI](https://docs.gitlab.com/ci/)**：一个完全集成的 CI/CD 工具，与 GitLab 仓库无缝协作，提供自动化测试、构建和部署等功能。
- **[Gitea Actions](https://docs.gitea.com/zh-cn/usage/actions/overview)**：Gitea 实现的一个内置的 CI/CD 系统框架，兼容 GitHub Actions 的 YAML 工作流编排格式，同时也兼容 GitHub Marketplace 中大部分现有的 Actions 插件。

### 2. 持续交付（Continuous Delivery）

持续交付是指自动执行 CI 中的构建、单元测试和集成测试后，自动将经过验证的代码发布到存储库。因此，要实现有效的持续交付流程，将 CI 内置到开发管道中显得非常重要。

在持续交付中，从合并代码更改到交付生产就绪型版本，每个阶段均涉及测试的自动化及代码发布的自动化。整个流程结束后，运维团队便可以迅速将应用部署到生产环境。

持续交付通常意味着对开发人员对应用所做的更改自动进行错误测试并将其上传到存储库（如 GitHub 或容器镜像仓库），然后由运维团队将其部署到实时的生产环境。它可以解决开发团队和业务团队之间的可见性和沟通不佳的问题。为此，持续交付的目的就是拥有一个可随时部署到生产环境的代码库，并确保以最少的工作量部署新代码。

### 3. 持续部署（Continuous Deployment）

成熟的 CI/CD 管道的最后一个阶段是持续部署。持续部署是持续交付的延伸，通常指自动将开发人员的更改从存储库发布到生产环境，以供客户使用。

CD 解决了运维团队因手动流程过多导致应用交付速度变慢的问题。持续部署以持续交付的优势为根基，实现了管道后续阶段的自动化。

实际上，持续部署意味着开发人员对云应用的更改在编写后的几分钟内就能生效（在通过了前序自动化测试的情况下）。这使得持续接收和整合用户反馈变得更加容易。综上所述，所有这些相互关联的 CI/CD 事务均能降低应用部署的风险，从而更轻松地以小块的形式发布对应用的更改，而不是一次性发布所有更改。

然而，由于在生产前的管道阶段没有人工关卡，因此，持续部署在很大程度上依赖于精心设计的测试自动化。这意味着持续部署可能需要大量的前期投入，因为需要编写自动化测试以适应 CI/CD 管道中的各种测试和发布阶段。

## 二、概念

### 1. 运行器（Runner）

运行器（Runner）是 CI/CD 作业的运行者，或者称为代理。这些代理实际运行在物理机或虚拟机上。

在相关文件中，可以指定运行作业时要使用的容器镜像。运行器在工作时会自动加载镜像、克隆项目，并在本地或容器中运行作业。

通常情况下，Github 等公共平台会提供免费或收费的运行器，但可能会有一些限制。我们也可以通过在本地机器上部署自托管的运行器

### 2. 事件（Event）

**事件（Event）**是存储库中触发工作流运行的特定活动。事件可能来自存储库，例如当有人创建拉取请求、打开议题或将提交推送到存储库时。此外，通常还可以通过 REST API 或手动触发来运行工作流。

### 3. 工作流（Workflow）/ 管道（Pipeline）

**工作流（Workflow）**和 **管道（Pipeline）** 是相似的概念，只是在不同的 CI/CD 工具中表述不同。这里给出 Github 定义，其他：

**工作流（Workflow）** 是一个可配置的自动化过程，它将运行一个或多个作业。

工作流由提交到存储库的 YAML 文件定义，并在存储库中的特定事件发生时运行，也可以手动触发，或者按照自定义的时间点触发。

GitHub Actions 使用 YAML 语法来定义工作流程。 每个工作流都作为单独的 YAML 文件存储在代码存储库根目录下的名为 `.github/workflows` 的目录中。

一个仓库可以有多个工作流，每个工作流都可以执行一组不同的任务，例如：

- 对拉取请求（Pull Request）进行构建（Build）和测试（Test）
- 在每次创建发布（Release）时，自动部署应用程序
- 每当创建新议题（Issue）时，自动添加标签

### 4. 作业（Job）/ 阶段（Stage）和步骤（Step）

**作业（Job）**或者 **阶段（Stage）**是一个 **工作流（Workflow）**中的一组 **步骤（Step）**的统称，一个作业或者阶段会在同一个 **运行器（Runner）**上被执行。

一个步骤可以是需要执行的 Shell 命令或脚本，也可以是将要运行的 **操作（Action）**。步骤按顺序执行，并且相互依赖。由于每个步骤都在同一个运行器上执行，因此我们可以将数据从一个步骤共享到另一个步骤。例如，可以先编写一个构建应用程序的步骤，然后再编写一个测试刚刚构建好的应用程序的步骤。

默认情况下，不同作业之间没有依赖关系，在执行时并行运行。不过，我们也可以为一项作业配置它所依赖的作业，当一个作业依赖于另一个作业时，它会等待所依赖的作业执行完成后再运行。一个常见的例子是为不同的架构配置多个构建作业。这些构建作业彼此之间没有任何依赖关系，但最后的打包作业却依赖于这些构建作业。在这种情况下，不同架构的构建作业将会并行运行。一旦所有的构建作业都成功完成，打包作业就会运行。

![Pipeline graph that shows four stages: build, test, canary, and production. First three stages show completed jobs with green checkmarks, while production stage shows a pending deploy job.](https://docs.gitlab.com/ci/pipelines/img/manual_job_v17_9.png)

![Diagram of an event triggering Runner 1 to run Job 1, which triggers Runner 2 to run Job 2. Each of the jobs is broken into multiple steps.](https://docs.github.com/assets/cb-25535/images/help/actions/overview-actions-simple.png)

### 5. 操作（Action）

操作是 GitHub Actions 平台的自定义应用程序，用于执行复杂但经常被重复使用的一系列任务。使用操作有助于减少在工作流文件中编写的重复代码量。我们可以通过操作实现诸如从 GitHub 拉取 Git 存储库、为构建环境配置工具链以及进行各种身份验证等功能。

我们可以自己编写操作，也可以从 [GitHub Marketplace](https://github.com/marketplace) 查找和使用其他人编写的操作。

### 6. 变量（Variable）和机密（Secret）

**变量（Variable）**提供了一种存储和重用非敏感配置信息的方法。我们可以将任何配置数据（例如编译器标志、用户名或服务器名称）存储为变量。在运行时，这些变量实际上会作为 **环境变量（Environment Variable）**被应用到运行器中。在操作或工作流步骤中，我们可以创建、读取和修改变量。我们既可以自己设置自定义变量，也可以使用 CI/CD 平台自动设置的默认变量（有的平台称之为预定义变量）。

当需要在工作流使用密码或证书等敏感数据时，我们可以将这些数据作为 **机密（Secret）**保存，然后在工作流中将它们用作环境变量。 这样以来，我们就无需直接在工作流的配置文件中嵌入（硬编码）这些敏感信息，而涉及敏感信息的工作流（比如身份认证等）也可以在不同项目、不同人员之间共享，无需担心敏感信息泄露问题。

### 7. 产物 / 工件（Artifact）

我们可以将作业运行后输出的文件和目录（例如源代码通过构建作业编译生成的二进制文件等）单独进行打包保存，这些打包保存称为 **产物 **或 **工件（Artifact）**。

我们可以直接在 CI/CD 平台的相应位置下载作业产物，也可以使用平台提供的 API 来下载作业产物。

## 三、Github Actions

### 1. 介绍

[GitHub Actions](https://docs.github.com/zh/actions) 是 Github 提供的一个持续集成和持续交付（CI/CD）平台，可以自动化构建、测试和部署流程。我们可以编写工作流来构建和测试对存储库的每个拉取请求，或者将合并的拉取请求部署到生产环境中。

GitHub 提供 Linux、Windows 和 macOS 虚拟机来运行工作流程，我们也可以在自己的机器上部署 Github 运行器来运行工作流（称为自托管运行器）。

对于公共存储库，使用下表所示工作流标签的作业可在 Github 提供的 Runner 虚拟机上运行。

| **虚拟机**         | **处理器 (CPU)** | **内存 (RAM)** | **存储 (SSD)** | **体系结构** | **工作流标签**                                               |
| ------------------ | ---------------- | -------------- | -------------- | ------------ | ------------------------------------------------------------ |
| Linux              | 4                | 16 GB          | 14 GB          | x64          | `ubuntu-latest`、`ubuntu-24.04`、`ubuntu-22.04`、`ubuntu-20.04` |
| Windows            | 4                | 16 GB          | 14 GB          | x64          | `windows-latest`、`windows-2025`[公共预览版]、`windows-2022`, `windows-2019` |
| Linux [公共预览版] | 4                | 16 GB          | 14 GB          | arm64        | `ubuntu-24.04-arm`，`ubuntu-22.04-arm`                       |
| macOS              | 4                | 14 GB          | 14 GB          | Intel        | `macos-13`                                                   |
| macOS              | 3 (M1)           | 7 GB           | 14 GB          | arm64        | `macos-latest`、`macos-14`、`macos-15` [公共预览版]          |

> 使用 GitHub 托管的运行器时，GitHub Actions 的使用存在一些 [限制](https://docs.github.com/zh/actions/administering-github-actions/usage-limits-billing-and-administration#usage-limits)。 这些限制可能会有变动。
> - 作业执行时间：工作流中每个作业的最长执行时间为 6 小时。 如果作业达到此限制，该作业将会终止而无法完成。
> - 工作流运行时间：每次工作流运行时间限制为 35 天。 如果工作流程运行时间达到此限制，其运行将被取消。 此时间段包括执行持续时间以及等待和审批所用的时间。
> - API 请求：一个存储库中所有操作在一小时内最多可以执行 1,000 条对 GitHub API 的请求。 如果超出请求数，其他 API 调用将失败，这可能导致作业失败。
> - Webhook 速率限制：每个存储库限制为每 10 秒 1500 个触发工作流运行的事件。 达到此限制后，Webhook 事件应触发的工作流运行将被阻止，且不会排队。

此外，Github 还提供了 [GitHub Packages](https://docs.github.com/zh/packages/learn-github-packages/introduction-to-github-packages) 服务。它是一种软件包托管服务，允许我们在 Github 上托管公开或私有的软件包。我们可以将 GitHub Packages 与 GitHub API、GitHub Actions 和 Webhook 集成在一起来创建端到端的、涵盖开发、测试、集成和部署全流程的 DevOps 工作流。

GitHub Packages 为常用的包管理器提供不同的 **包注册表（Package Registry）**，例如 npm、RubyGems、Apache Maven、Gradle、Docker 和 Nuget。 GitHub 的 Container registry 针对容器进行了优化，支持 Docker 和 OCI 镜像。

> 公开的各种软件包可以免费使用 GitHub Packages 服务。而对于私有的软件包，GitHub 上的每个帐户都可获得一定数量的免费存储和数据传输流量配额，具体取决于帐户计划。

### 2. 编写 Github Actions 工作流

#### 2.1 示例工作流

首先，让我们从 Github 提供的示例工作流开始。这个工作流会在任何代码被推送到仓库时启动，签出（Checkout）推送的代码，安装 [bats](https://www.npmjs.com/package/bats) 测试框架，并运行命令来输出 bats 版本。

1. 在存储库中，创建  `.github/workflows/`  目录来存储工作流文件。

2. 在  `.github/workflows/`  目录中，创建一个名为  `learn-github-actions.yml`  的新文件并添加以下代码。

   ```yaml
   name: learn-github-actions
   run-name: ${{ github.actor }} is learning GitHub Actions
   on: [push]
   jobs:
     check-bats-version:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v4
         - uses: actions/setup-node@v4
           with:
             node-version: '20'
         - run: npm install -g bats
         - run: bats -v
   ```

3. 提交这些更改并将其推送到 GitHub 仓库。

现在，这个 GitHub Actions 工作流文件就被安装在仓库中，每次有人推送更改到仓库时都会自动运行这个工作流。

下面，我们详细解释一下上面的工作流文件：

```yaml
# 可选 - 工作流的名称，它将显示在 GitHub 仓库的“Actions”选项卡中。如果省略此字段，将使用工作流文件的名称代替。
name: learn-github-actions

# 可选 - 从该工作流生成的工作流运行的名称，它将显示在仓库“Actions”选项卡的工作流运行列表中。此示例使用带有 `github` 上下文的表达式来显示触发工作流运行的执行者的用户名。
run-name: ${{ github.actor }} is learning GitHub Actions

# 指定此工作流的触发条件。此示例使用 `push` 事件，因此每当有人将更改推送到仓库或合并拉取请求时，都会触发工作流运行。这会在每次推送到每个分支时触发。
on: [push]

# 将在 `learn-github-actions` 工作流中运行的所有作业分组在一起。
jobs:

# 定义一个名为 `check-bats-version` 的作业。
  check-bats-version:

# 配置作业在最新版本的 Ubuntu Linux 运行器上运行。这意味着作业将在 GitHub 托管的 Runner 虚拟机上执行。
    runs-on: ubuntu-latest

# 将在 `check-bats-version` 作业中运行的所有步骤分组在一起。此部分下嵌套的每个项目都是一个单独的操作或 shell 脚本。
    steps:

# `uses` 关键字指定此步骤将运行 `actions/checkout` 操作的 `v4` 版本。这是一个将仓库检出到运行器上的操作，只要工作流将使用仓库的代码，就应该使用检出操作。
      - uses: actions/checkout@v4

# 此步骤使用 `actions/setup-node@v4` 操作安装指定版本的 Node.js。（此示例使用版本 20。）
      - uses: actions/setup-node@v4
        with:
          node-version: '20'

# `run` 关键字表示执行特定的命令，这里使用 `npm` 全局安装 `bats` 软件包。
      - run: npm install -g bats

# 最后，使用 `-v` 参数运行 `bats` 命令，该参数将输出版本信息。
      - run: bats -v
```

#### 2.2 为基于 Docsify 文档站项目配置 CI/CD



## 四、Gitea Actions



## 五、其他支持 CI/CD 的平台
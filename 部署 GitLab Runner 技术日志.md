
## 目录
- [背景](#背景)
- [准备工作](#准备工作)
- [安装 GitLab Runner](#安装-gitlab-runner)
  - [在线安装](#在线安装)
  - [离线安装](#离线安装)
- [注册 GitLab Runner](#注册-gitlab-runner)
- [GitLab Runner 配置详解](#gitlab-runner-配置详解)
  - [基本配置](#基本配置)
  - [高级配置](#高级配置)
- [常见问题及解决方案](#常见问题及解决方案)

## 背景

GitLab Runner 是 GitLab CI/CD 系统的关键组件，负责执行在 `.gitlab-ci.yml` 文件中定义的任务 。为了确保持续集成和持续交付流程的顺畅运行，正确部署和配置 GitLab Runner 至关重要。

## 准备工作

在开始之前，请确保你已经：
- 安装并配置好了 GitLab 实例。
- 拥有 GitLab 的管理员权限或项目级别的权限以注册 Runner。
- 确认目标机器满足 GitLab Runner 的系统要求（如操作系统版本、硬件资源等）。

## 安装 GitLab Runner

### 在线安装

对于大多数 Linux 发行版，可以使用以下命令进行在线安装：

```bash
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh | sudo bash
sudo yum install gitlab-runner
```

对于 Debian/Ubuntu 系统，可以替换为相应的 `apt` 命令 。

### 离线安装

如果你需要在没有网络连接的环境中安装 GitLab Runner，则需先下载适合你操作系统的安装包，并手动安装它。

## 注册 GitLab Runner

安装完成后，下一步是注册 Runner。这一步骤将你的 Runner 连接到特定的 GitLab 实例，并指定它可以处理哪些类型的作业 。

```bash
sudo gitlab-runner register
```

按照提示输入 GitLab 实例的 URL 和注册令牌。你可以从 GitLab 的管理界面获取这些信息。

## GitLab Runner 配置详解

### 基本配置

GitLab Runner 的配置文件通常位于 `/etc/gitlab-runner/config.toml` 或 Docker 容器内的 `/data/etc/gitlab-runner/config.toml`。以下是一些关键配置项：

- `url`: GitLab 实例的 URL。
- `token`: Runner 的通信令牌。
- `executor`: 构建项目要使用的执行器类型，例如 `shell`, `docker`, `kubernetes` 等 。

### 高级配置

根据你的需求，你可能还需要调整其他配置参数，比如限制同时处理的作业数量 (`limit`)，设置构建目录 (`builds_dir`)，以及环境变量 (`environment`) 。

## 常见问题及解决方案

遇到问题时，请首先检查 GitLab Runner 的日志文件，通常位于 `/var/log/gitlab-runner/` 目录下 。如果日志中没有提供足够的信息，尝试重新启动服务或查看系统事件日志（适用于 Windows 服务形式运行的情况）。

---

此日志提供了部署 GitLab Runner 的基础指南。根据具体的应用场景，你可能需要进一步调整配置或参考官方文档以获得更详细的指导。

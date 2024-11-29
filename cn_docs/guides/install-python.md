# 安装 Python

如果您的系统已经安装了 Python，uv 会在无需配置的情况下
[检测并使用](#using-an-existing-python-installation) 它。不过，uv 也可以为您安装和管理 Python 版本。

!!! tip

    uv 会根据需要[自动下载 Python 版本](#automatic-python-downloads) — 您无需安装 Python 即可开始使用。

## 开始使用

要安装最新的 Python 版本：

```console
$ uv python install
```

即使您的系统上已安装了 Python，运行此命令也会安装一个 uv 管理的 Python 版本。如果您之前使用 uv 安装了 Python，将不会再次安装新版本。

!!! note

    Python 并不发布官方的可分发二进制文件。因此，uv 使用来自 [`python-build-standalone`](https://github.com/indygreg/python-build-standalone) 项目的第三方分发版本。该项目部分由 uv 的维护者维护，并被其他著名的 Python 项目使用（例如 [Rye](https://github.com/astral-sh/rye)、[Bazel](https://github.com/bazelbuild/rules_python)）。更多细节请参阅 [Python 分发版本](../concepts/python-versions.md#managed-python-distributions) 文档。

## 安装特定版本

要安装特定的 Python 版本：

```console
$ uv python install 3.12
```

要安装多个 Python 版本：

```console
$ uv python install 3.11 3.12
```

要安装其他 Python 实现，例如 PyPy：

```console
$ uv python install pypy@3.10
```

有关更多信息，请参见 [`python install`](../concepts/python-versions.md#installing-a-python-version) 文档。

## 查看已安装的 Python 版本

要查看可用和已安装的 Python 版本：

```console
$ uv python list
```

有关更多信息，请参阅 [`python list`](../concepts/python-versions.md#viewing-available-python-versions) 文档。

## 自动下载 Python 版本

请注意，使用 uv 时并不需要显式安装 Python。默认情况下，uv 会在需要时自动下载 Python 版本。例如，如果未安装 Python 3.12，以下命令会自动下载并使用它：

```console
$ uv run --python 3.12 python -c 'print("hello world")'
```

即使未明确请求特定的 Python 版本，uv 也会根据需要下载最新版本。例如，以下命令会创建一个新的虚拟环境，如果找不到 Python，则下载一个管理的 Python 版本：

```console
$ uv venv
```

!!! tip

    如果您希望更好地控制 Python 何时被下载，可以[轻松禁用](../concepts/python-versions.md#disabling-automatic-python-downloads) 自动下载功能。

## 使用现有的 Python 安装 {: #using-an-existing-python-installation}

如果系统上已存在 Python 安装，uv 会自动使用它。对此行为无需额外配置：uv 会使用系统中的 Python，只要它满足命令执行的要求。有关详细信息，请参见 [Python 发现](../concepts/python-versions.md#discovery-of-python-versions) 文档。

要强制 uv 使用系统 Python，可以提供 `--python-preference only-system` 选项。有关详细信息，请参见 [Python 版本偏好设置](../concepts/python-versions.md#adjusting-python-version-preferences) 文档。

## 下一步

要了解更多关于 `uv python` 的信息，请查看 [Python 版本概念](../concepts/python-versions.md) 页面和 [命令参考](../reference/cli.md#uv-python)。

或者，继续阅读了解如何使用 uv [运行脚本](./scripts.md) 并调用 Python。

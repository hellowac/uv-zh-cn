# Python 版本

Python 版本由 Python 解释器（即 `python` 可执行文件）、标准库和其他支持文件组成。

## 管理的与系统的 Python 安装

由于系统通常已经安装了 Python，uv 支持[发现](#discovery-of-python-versions)系统中的 Python 版本。然而，uv 也支持[安装 Python 版本](#installing-a-python-version)本身。为了区分这两种 Python 安装类型，uv 将它安装的 Python 版本称为_管理的_ Python 安装，而将其他 Python 安装称为_系统的_ Python 安装。

!!! note

    uv 不区分操作系统安装的 Python 版本与由其他工具管理的 Python 版本。例如，如果使用 `pyenv` 管理 Python 安装，它仍然会被 uv 视为_系统的_ Python 版本。

## 请求一个版本 {: #requesting-a-version}

可以使用 `--python` 标志在大多数 uv 命令中请求特定的 Python 版本。例如，在创建虚拟环境时：

```console
$ uv venv --python 3.11.6
```

uv 会确保 Python 3.11.6 可用——如果必要，它会下载并安装该版本——然后使用该版本创建虚拟环境。

支持以下 Python 版本请求格式：

- `<version>` 例如 `3`，`3.12`，`3.12.3`
- `<version-specifier>` 例如 `>=3.12,<3.13`
- `<implementation>` 例如 `cpython` 或 `cp`
- `<implementation>@<version>` 例如 `cpython@3.12`
- `<implementation><version>` 例如 `cpython3.12` 或 `cp312`
- `<implementation><version-specifier>` 例如 `cpython>=3.12,<3.13`
- `<implementation>-<version>-<os>-<arch>-<libc>` 例如 `cpython-3.12.3-macos-aarch64-none`

此外，还可以请求特定的系统 Python 解释器：

- `<executable-path>` 例如 `/opt/homebrew/bin/python3`
- `<executable-name>` 例如 `mypython3`
- `<install-dir>` 例如 `/some/environment/`

默认情况下，如果系统上找不到 Python 版本，uv 会自动下载所需版本。此行为可以通过[禁用自动下载 Python](#disabling-automatic-python-downloads)选项来关闭。

### Python 版本文件

`.python-version` 文件可用于创建默认的 Python 版本请求。uv 会在当前目录及其每个父目录中查找 `.python-version` 文件。可以使用上述描述的任何请求格式，尽管建议使用版本号，以便与其他工具兼容。

可以通过 `uv python pin` 命令在当前目录中创建 `.python-version` 文件。

可以使用 `--no-config` 禁用 `.python-version` 文件的发现功能。

uv 不会在项目或工作区的边界之外查找 `.python-version` 文件。

## 安装 Python 版本 {: #installing-a-python-version}

uv 提供了 macOS、Linux 和 Windows 上可下载的 CPython 和 PyPy 发行版列表。

!!! tip

    默认情况下，Python 版本会在需要时自动下载，而不需要使用 `uv python install`。

要安装特定版本的 Python：

```console
$ uv python install 3.12.3
```

要安装最新的修补版本：

```console
$ uv python install 3.12
```

要安装符合约束条件的版本：

```console
$ uv python install '>=3.8,<3.10'
```

要安装多个版本：

```console
$ uv python install 3.9 3.10 3.11
```

要安装特定的实现：

```console
$ uv python install pypy
```

除了用于请求本地解释器（例如文件路径）的格式外，所有[Python 版本请求](#requesting-a-version)格式都受支持。

默认情况下，`uv python install` 会验证是否已安装管理的 Python 版本，或安装最新版本。如果存在 `.python-version` 文件，uv 会安装该文件中列出的 Python 版本。一个需要多个 Python 版本的项目可以定义一个 `.python-versions` 文件。如果该文件存在，uv 会安装文件中列出的所有 Python 版本。

## 项目 Python 版本

uv 会在执行项目命令时尊重 `pyproject.toml` 文件中定义的 `requires-python` 的 Python 版本要求。它将使用第一个与要求兼容的 Python 版本，除非通过 `.python-version` 文件或 `--python` 标志等方式请求了特定版本。

## 查看可用的 Python 版本 {: #viewing-available-python-versions}

要列出已安装和可用的 Python 版本：

```console
$ uv python list
```

默认情况下，其他平台和旧的修补版本会被隐藏。

要查看所有版本：

```console
$ uv python list --all-versions
```

要查看其他平台的 Python 版本：

```console
$ uv python list --all-platforms
```

要仅显示已安装的 Python 版本，并排除下载版本：

```console
$ uv python list --only-installed
```

## 查找 Python 可执行文件

要查找 Python 可执行文件，可以使用 `uv python find` 命令：

```console
$ uv python find
```

默认情况下，这将显示第一个可用 Python 可执行文件的路径。有关如何发现可执行文件的详细信息，请参见[发现规则](#discovery-of-python-versions)。

此接口还支持多种[请求格式](#requesting-a-version)，例如，要查找版本为 3.11 或更高的 Python 可执行文件：

```console
$ uv python find >=3.11
```

默认情况下，`uv python find` 会包括来自虚拟环境中的 Python 版本。如果在工作目录或任何父目录中找到 `.venv` 目录，或 `VIRTUAL_ENV` 环境变量已设置，则该虚拟环境中的 Python 将优先于 `PATH` 上的任何 Python 可执行文件。

要忽略虚拟环境，请使用 `--system` 标志：

```console
$ uv python find --system
```

## Python 版本的发现 {: #discovery-of-python-versions}

在搜索 Python 版本时，以下位置会被检查：

- `UV_PYTHON_INSTALL_DIR` 中的管理 Python 安装。
- 系统 `PATH` 中作为 `python`、`python3` 或 `python3.x`（macOS 和 Linux）或 `python.exe`（Windows）可用的 Python 解释器。
- 在 Windows 上，符合请求版本的 Python 解释器，包括 Windows 注册表中的 Python 解释器和 Microsoft Store 中的 Python 解释器（见 `py --list-paths`）。

在某些情况下，uv 允许使用虚拟环境中的 Python 版本。在这种情况下，在按上述方式搜索安装之前，会先检查虚拟环境的解释器是否与请求兼容。有关[与 pip 兼容的虚拟环境发现](../pip/environments.md#discovery-of-python-environments)的详细信息，请参阅相关文档。

在执行发现时，会忽略非可执行文件。每个发现的可执行文件都会查询其元数据，以确保它符合[请求的 Python 版本](#requesting-a-version)。如果查询失败，则跳过该可执行文件。如果可执行文件符合请求，它将被使用，而无需进一步检查其他可执行文件。

在搜索管理的 Python 版本时，uv 会优先选择较新的版本。搜索系统 Python 版本时，uv 会使用第一个兼容的版本，而不是最新版本。

如果系统上找不到 Python 版本，uv 会检查是否有兼容的管理 Python 版本可以下载。

### Python 预发行版

默认情况下，Python 预发行版不会被选择。如果没有其他符合请求的安装版本，uv 会使用 Python 预发行版。例如，如果仅有一个预发行版可用，则会使用该版本，否则会使用稳定版。类似地，如果提供了一个预发行版 Python 可执行文件的路径，而没有其他符合请求的版本，则会使用该预发行版。

如果一个预发行版 Python 版本可用且符合请求，uv 不会下载稳定版本的 Python。

## 禁用自动下载 Python 版本 {: #disabling-automatic-python-downloads}

默认情况下，uv 会在需要时自动下载 Python 版本。

可以使用 [`python-downloads`](../reference/settings.md#python-downloads) 选项来禁用此行为。默认设置为 `automatic`，设置为 `manual` 后，仅在执行 `uv python install` 时允许下载 Python。

!!! 提示

    `python-downloads` 设置可以在[持久配置文件](../configuration/files.md)中进行设置，以更改默认行为，或者可以在任何 uv 命令中传递 `--no-python-downloads` 标志。

## 调整 Python 版本偏好 {: #adjusting-python-version-preferences}

默认情况下，uv 会尝试使用系统中找到的 Python 版本，仅在必要时下载管理的 Python 解释器。

可以使用 [`python-preference`](../reference/settings.md#python-preference) 选项来调整此行为。默认设置为 `managed`，即优先使用管理的 Python 安装，而非系统 Python 安装。然而，系统 Python 安装仍然优先于下载管理的 Python 版本。

可用的其他选项如下：

- `only-managed`：仅使用管理的 Python 安装；绝不使用系统 Python 安装
- `system`：优先使用系统 Python 安装，而非管理的 Python 安装
- `only-system`：仅使用系统 Python 安装；绝不使用管理的 Python 安装

这些选项允许完全禁用 uv 的管理 Python 版本，或始终使用管理版本并忽略任何现有的系统安装。

!!! 注意

    可以[禁用](#disabling-automatic-python-downloads)自动下载 Python 版本，而不必改变偏好设置。

## Python 实现支持

uv 支持 CPython、PyPy 和 GraalPy Python 实现。如果某个 Python 实现不受支持，uv 将无法发现其解释器。

这些实现可以使用长名称或短名称进行请求：

- CPython：`cpython`，`cp`
- PyPy：`pypy`，`pp`
- GraalPy：`graalpy`，`gp`

实现名称请求不区分大小写。

有关受支持格式的更多详细信息，请参见[Python 版本请求](#requesting-a-version)文档。

## 管理的 Python 发行版

uv 支持下载和安装 CPython 和 PyPy 发行版。

### CPython 发行版

由于 Python 不发布官方的可分发 CPython 二进制文件，uv 使用来自 [`python-build-standalone`](https://github.com/indygreg/python-build-standalone) 项目的预构建第三方发行版。`python-build-standalone` 项目由 uv 的维护者部分维护，并被许多其他 Python 项目使用，如 [Rye](https://github.com/astral-sh/rye) 和 [bazelbuild/rules_python](https://github.com/bazelbuild/rules_python)。

这些 uv Python 发行版是自包含的、高度可移植的，并且性能优越。尽管可以像在 `pyenv` 等工具中一样从源代码构建 Python，但这需要预先安装系统依赖项，并且创建经过优化的高性能构建（例如启用 PGO 和 LTO）非常慢。

这些发行版有一些行为上的瑕疵，通常是出于可移植性考虑；目前，uv 不支持在基于 musl 的 Linux 发行版（如 Alpine Linux）上安装它们。有关详细信息，请参见 [`python-build-standalone` 的瑕疵](https://gregoryszorc.com/docs/python-build-standalone/main/quirks.html)文档。

### PyPy 发行版

PyPy 发行版由 PyPy 项目提供。

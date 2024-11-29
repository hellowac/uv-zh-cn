# uv

一个用 Rust 编写的极快 Python 包和项目管理工具。

<p align="center">
  <img alt="展示包含基准测试结果的柱状图。" src="https://github.com/astral-sh/uv/assets/1309177/629e59c0-9c6e-4013-9ad4-adb2bcf5080d#only-light">
</p>

<p align="center">
  <img alt="展示包含基准测试结果的柱状图。" src="https://github.com/astral-sh/uv/assets/1309177/03aa9163-1c79-4a87-a31d-7a9311ed9310#only-dark">
</p>

<p align="center">
  <i>在缓存预热的情况下安装 <a href="https://trio.readthedocs.io/">Trio</a> 的依赖。</i>
</p>

## 亮点

- 🚀 一个工具即可替代 `pip`、`pip-tools`、`pipx`、`poetry`、`pyenv`、`twine`、`virtualenv` 等多种工具。
- ⚡️ 比 `pip` [快 10-100 倍](https://github.com/astral-sh/uv/blob/main/BENCHMARKS.md)。
- 🐍 [安装和管理](#python-management) Python 版本。
- 🛠️ [运行和安装](#tool-management) Python 应用程序。
- ❇️ [运行脚本](#script-support)，支持[内联依赖元数据](./guides/scripts.md#declaring-script-dependencies)。
- 🗂️ 提供[全面的项目管理](#project-management)，并支持[通用锁文件](./concepts/projects/layout.md#the-lockfile)。
- 🔩 包含与 pip 兼容的[接口](#the-pip-interface)，在熟悉的 CLI 中实现性能提升。
- 🏢 支持类似 Cargo 的[工作区](./concepts/projects/workspaces.md)，适合大规模项目。
- 💾 节省磁盘空间，使用[全局缓存](./concepts/cache.md)实现依赖去重。
- ⏬ 无需 Rust 或 Python，即可通过 `curl` 或 `pip` 安装。
- 🖥️ 支持 macOS、Linux 和 Windows。

uv 由 [Astral](https://astral.sh) 提供支持，该团队同时也是 [Ruff](https://github.com/astral-sh/ruff) 的创建者。

## 入门指南

通过官方独立安装程序安装 uv：

=== "macOS 和 Linux"

    ```console
    $ curl -LsSf https://astral.sh/uv/install.sh | sh
    ```

=== "Windows"

    ```console
    $ powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
    ```

接着，查看[第一步指南](./getting-started/first-steps.md)或继续阅读以了解简要概述。

!!! tip

    uv 也可以通过 pip、Homebrew 等方式安装。查看[安装页面](./getting-started/installation.md)了解所有方法。

## 项目管理 {: #project-management}

uv 管理项目的依赖和环境，支持锁文件、工作区等功能，类似于 `rye` 或 `poetry`：

```console
$ uv init example
Initialized project `example` at `/home/user/example`

$ cd example

$ uv add ruff
Creating virtual environment at: .venv
Resolved 2 packages in 170ms
   Built example @ file:///home/user/example
Prepared 2 packages in 627ms
Installed 2 packages in 1ms
 + example==0.1.0 (from file:///home/user/example)
 + ruff==0.5.4

$ uv run ruff check
All checks passed!
```

请参阅 [项目指南](./guides/projects.md) 开始使用。

uv 还支持构建和发布项目，即使这些项目并非由 uv 管理。请查看 [发布指南](./guides/publish.md) 了解更多信息。

## 工具管理 {: #tool-management}

uv 执行和安装由 Python 包提供的命令行工具，类似于 `pipx`。

使用 `uvx`（`uv tool run` 的别名）在临时环境中运行工具：

```console
$ uvx pycowsay 'hello world!'
Resolved 1 package in 167ms
Installed 1 package in 9ms
 + pycowsay==0.0.0.2
  """

  ------------
< hello world! >
  ------------
   \   ^__^
    \  (oo)\_______
       (__)\       )\/\
           ||----w |
           ||     ||
```

Install a tool with `uv tool install`:

```console
$ uv tool install ruff
Resolved 1 package in 6ms
Installed 1 package in 2ms
 + ruff==0.5.4
Installed 1 executable: ruff

$ ruff --version
ruff 0.5.4
```

请参阅 [工具指南](./guides/tools.md) 开始使用。

## Python 管理 {: #python-management}

uv 可安装 Python，并支持快速切换不同的版本。

安装多个 Python 版本：

```console
$ uv python install 3.10 3.11 3.12
Searching for Python versions matching: Python 3.10
Searching for Python versions matching: Python 3.11
Searching for Python versions matching: Python 3.12
Installed 3 versions in 3.42s
 + cpython-3.10.14-macos-aarch64-none
 + cpython-3.11.9-macos-aarch64-none
 + cpython-3.12.4-macos-aarch64-none
```

根据需要下载指定版本的 Python：

```console
$ uv venv --python 3.12.0
Using CPython 3.12.0
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

$ uv run --python pypy@3.8 -- python
Python 3.8.16 (a9dbdca6fc3286b0addd2240f11d97d8e8de187a, Dec 29 2022, 11:45:30)
[PyPy 7.3.11 with GCC Apple LLVM 13.1.6 (clang-1316.0.21.2.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>>
```

在当前目录下使用指定的 Python 版本：

```console
$ uv python pin pypy@3.11
Pinned `.python-version` to `pypy@3.11`
```

请参阅 [Python 安装指南](./guides/install-python.md) 了解更多信息。

## 脚本支持 {: #script-support}

uv 管理单文件脚本的依赖和环境。

创建一个新脚本并通过内联元数据声明其依赖：

```console
$ echo 'import requests; print(requests.get("https://astral.sh"))' > example.py

$ uv add --script example.py requests
Updated `example.py`
```

随后，在隔离的虚拟环境中运行脚本：

```console
$ uv run example.py
Reading inline script metadata from: example.py
Installed 5 packages in 12ms
<Response [200]>
```

请参阅 [脚本指南](./guides/scripts.md) 开始使用。

## pip 接口 {: #the-pip-interface}

uv 提供了对常见 `pip`、`pip-tools` 和 `virtualenv` 命令的直接替代。

uv 在这些接口的基础上扩展了高级功能，例如依赖版本覆盖、跨平台的依赖解析、可复现的解析结果、替代解析策略等。

通过 `uv pip` 接口，无需更改现有工作流程即可迁移到 uv，同时享受 10-100 倍的速度提升。

将需求编译为跨平台的 requirements 文件：

```console
$ uv pip compile docs/requirements.in \
   --universal \
   --output-file docs/requirements.txt
在 12ms 内解析了 43 个包
```

创建一个虚拟环境：

```console
$ uv venv
使用 CPython 3.12.3  
正在创建虚拟环境于: .venv  
激活命令: source .venv/bin/activate  
```

安装锁定的依赖：

```console
$ uv pip sync docs/requirements.txt
在 11ms 内解析了 43 个包  
在 208ms 内安装了 43 个包  
 + babel==2.15.0  
 + black==24.4.2  
 + certifi==2024.7.4  
 ...
```

请参阅 [pip 接口文档](./pip/index.md) 开始使用。

## 了解更多

查看 [第一步指南](./getting-started/first-steps.md) 或直接跳转到 [指南](./guides/index.md)，开始使用 uv。

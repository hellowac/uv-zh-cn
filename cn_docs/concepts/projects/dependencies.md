# 管理依赖项

## 依赖项表

项目的依赖项在几个表中定义：

- [`project.dependencies`](#project-dependencies): 已发布的依赖项。
- [`project.optional-dependencies`](#optional-dependencies): 可选的已发布依赖项或“额外依赖项”。
- [`dependency-groups`](#dependency-groups): 本地开发依赖项。
- [`tool.uv.sources`](#dependency-sources): 开发期间依赖项的替代源。

!!! note

    即使项目不打算发布，`project.dependencies` 和 `project.optional-dependencies` 表也可以使用。`dependency-groups` 是最近标准化的特性，可能并非所有工具都已支持。

`uv` 支持使用 `uv add` 和 `uv remove` 修改项目的依赖项，但也可以通过直接编辑 `pyproject.toml` 文件来更新依赖项元数据。

## 添加依赖项

要添加一个依赖项：

```console
$ uv add httpx
```

此时会在 `project.dependencies` 表中添加一条记录：

```toml title="pyproject.toml" hl_lines="4"
[project]
name = "example"
version = "0.1.0"
dependencies = ["httpx>=0.27.2"]
```

可以使用 [`--dev`](#development-dependencies)、[`--group`](#dependency-groups) 或 [`--optional`](#optional-dependencies) 标志将依赖项添加到其他表中。

该依赖项将包含一个约束，例如 `>=0.27.2`，表示该包的最新兼容版本。您也可以提供一个替代约束：

```console
$ uv add 'httpx>=0.20'
```

如果依赖项来自包管理器之外的来源，`uv` 将在源表中添加一条记录。例如，添加来自 GitHub 的 `httpx`：

```console
$ uv add "httpx @ git+https://github.com/encode/httpx"
```

`pyproject.toml` 将包含一个 [Git 源条目](#git)：

```toml title="pyproject.toml" hl_lines="8-9"
[project]
name = "example"
version = "0.1.0"
dependencies = [
    "httpx",
]

[tool.uv.sources]
httpx = { git = "https://github.com/encode/httpx" }
```

如果无法使用某个依赖项，`uv` 将显示错误：

```console
$ uv add 'httpx>9999'
  × No solution found when resolving dependencies:
  ╰─▶ Because only httpx<=1.0.0b0 is available and your project depends on httpx>9999,
      we can conclude that your project's requirements are unsatisfiable.
```

## 删除依赖项

要删除一个依赖项：

```console
$ uv remove httpx
```

可以使用 `--dev`、`--group` 或 `--optional` 标志从特定表中删除依赖项。

如果为已删除的依赖项定义了 [源](#dependency-sources)，并且没有其他对该依赖项的引用，它也会被删除。

## 修改依赖项

要修改现有的依赖项，例如为 `httpx` 使用不同的约束：

```console
$ uv add 'httpx>0.1.0'
```

!!! note

    在这个示例中，我们正在修改 `pyproject.toml` 中依赖项的约束。依赖项的锁定版本只有在需要时才会更改以满足新的约束条件。要强制更新包版本到最新版本，可以使用 `--upgrade-package <name>`，例如：

    ```console
    $ uv add 'httpx>0.1.0' --upgrade-package httpx
    ```

    有关升级包的更多信息，请参阅 [锁定文件](./sync.md#upgrading-locked-package-versions) 文档。

请求不同的依赖项来源将更新 `tool.uv.sources` 表，例如在开发期间使用本地路径中的 `httpx`：

```console
$ uv add "httpx @ ../httpx"
```

## 平台特定的依赖项

为了确保某个依赖项仅在特定平台或特定 Python 版本上安装，可以使用 [环境标记](https://peps.python.org/pep-0508/#environment-markers)。

例如，要在 Linux 上安装 `jax`，但不在 Windows 或 macOS 上安装：

```console
$ uv add 'jax; sys_platform == "linux"'
```

生成的 `pyproject.toml` 将在依赖项定义中包含环境标记：

```toml title="pyproject.toml" hl_lines="6"
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = ["jax; sys_platform == 'linux'"]
```

类似地，要在 Python 3.11 及更高版本上包含 `numpy`：

```console
$ uv add 'numpy; python_version >= "3.11"'
```

请参阅 Python 的 [环境标记](https://peps.python.org/pep-0508/#environment-markers) 文档，了解可用标记和操作符的完整列表。

!!! tip

    依赖项源也可以按平台 [更改](#platform-specific-sources)。

## 项目依赖项 {: #project-dependencies}

`project.dependencies` 表表示上传到 PyPI 或构建 wheel 时使用的依赖项。每个依赖项通过 [依赖项说明符](https://packaging.python.org/en/latest/specifications/dependency-specifiers/) 语法来指定，表格遵循 [PEP 621](https://packaging.python.org/en/latest/specifications/pyproject-toml/) 标准。

`project.dependencies` 定义了项目所需的包列表，以及在安装时应使用的版本约束。每个条目包括一个依赖项名称和版本。条目还可以包含 extras 或平台特定包的环境标记。例如：

```toml title="pyproject.toml"
[project]
name = "albatross"
version = "0.1.0"
dependencies = [
  # 该范围内的任何版本
  "tqdm >=4.66.2,<5",
  # 精确版本的 torch
  "torch ==2.2.2",
  # 安装 transformers 并附带 torch extra
  "transformers[torch] >=4.39.3,<5",
  # 仅在较旧的 Python 版本上安装此包
  # 请参阅 "环境标记" 获取更多信息
  "importlib_metadata >=7.1.0,<8; python_version < '3.10'",
  "mollymawk ==0.1.0"
]
```

## 依赖项源 {: #dependency-sources}

`tool.uv.sources` 表扩展了标准的依赖项表，提供了开发期间使用的替代依赖项源。

依赖项源增加了对 `project.dependencies` 标准不支持的常见模式的支持，如可编辑安装和相对路径。例如，要从项目根目录相对路径下的目录安装 `foo`：

```toml title="pyproject.toml" hl_lines="7"
[project]
name = "example"
version = "0.1.0"
dependencies = ["foo"]

[tool.uv.sources]
foo = { path = "./packages/foo" }
```

`uv` 支持以下依赖项源：

- [Index](#index): 从特定包索引解析的包。
- [Git](#git): 一个 Git 仓库。
- [URL](#url): 一个远程 wheel 或源分发包。
- [Path](#path): 本地 wheel、源分发包或项目目录。
- [Workspace](#workspace-member): 当前工作区的成员。

!!! important

    依赖项源仅在 `uv` 中被尊重。如果使用其他工具，只会使用标准项目表中的定义。如果使用其他工具进行开发，则需要按照该工具的格式重新指定源表中的所有元数据。

### 索引 {: #index}

要从特定的索引添加 Python 包，可以使用 `--index` 选项：

```console
$ uv add torch --index pytorch=https://download.pytorch.org/whl/cpu
```

`uv` 将把该索引存储在 `[[tool.uv.index]]` 中，并添加一个 `[tool.uv.sources]` 条目：

```toml title="pyproject.toml"
[project]
dependencies = ["torch"]

[tool.uv.sources]
torch = { index = "pytorch" }

[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cpu"
```

!!! tip

    由于 PyTorch 索引的具体要求，上面的示例仅适用于 x86-64 Linux 系统。
    请参阅 [PyTorch指南](../../guides/integration/pytorch.md)，了解如何设置 PyTorch。

使用 `index` 源会将包固定到指定的索引 —— 它不会从其他索引下载。

在定义索引时，可以包含 `explicit` 标志，表示该索引应仅用于在 `tool.uv.sources` 中明确指定的包。如果没有设置 `explicit`，则在找不到其他源的包时，可能会从该索引解析包。

```toml title="pyproject.toml" hl_lines="3"
[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

### Git {: #git}

要添加 Git 依赖项源，可以在 Git 兼容的 URL 前加上 `git+`（即使用 `git clone` 时的 URL）。

例如：

```console
$ uv add git+https://github.com/encode/httpx
```

```toml title="pyproject.toml" hl_lines="5"
[project]
dependencies = ["httpx"]

[tool.uv.sources]
httpx = { git = "https://github.com/encode/httpx" }
```

可以请求特定的 Git 引用，例如，某个标签：

```console
$ uv add git+https://github.com/encode/httpx --tag 0.27.0
```

```toml title="pyproject.toml" hl_lines="7"
[project]
dependencies = ["httpx"]

[tool.uv.sources]
httpx = {
  git = "https://github.com/encode/httpx",
  tag = "0.27.0"
}
```

或者，指定一个分支：

```console
$ uv add git+https://github.com/encode/httpx --branch main
```

```toml title="pyproject.toml" hl_lines="7"
[project]
dependencies = ["httpx"]

[tool.uv.sources]
httpx = {
  git = "https://github.com/encode/httpx",
  branch = "main"
}
```

或者，指定一个修订版（提交）：

```console
$ uv add git+https://github.com/encode/httpx --rev 326b9431c761e1ef1e00b9f760d1f654c8db48c6
```

```toml title="pyproject.toml" hl_lines="7"
[project]
dependencies = ["httpx"]

[tool.uv.sources]
httpx = {
  git = "https://github.com/encode/httpx",
  rev = "326b9431c761e1ef1e00b9f760d1f654c8db48c6"
}
```

如果包不在仓库根目录，可以指定 `subdirectory`。

### URL {: #url}

要添加 URL 源，提供一个指向 wheel 文件（以 `.whl` 结尾）或源代码分发文件（通常以 `.tar.gz` 或 `.zip` 结尾）的 `https://` URL（查看[此处](../../concepts/resolution.md#source-distribution)了解所有支持的格式）。

例如：

```console
$ uv add "https://files.pythonhosted.org/packages/5c/2d/3da5bdf4408b8b2800061c339f240c1802f2e82d55e50bd39c5a881f47f0/httpx-0.27.0.tar.gz"
```

这将导致 `pyproject.toml` 文件如下所示：

```toml title="pyproject.toml" hl_lines="5"
[project]
dependencies = ["httpx"]

[tool.uv.sources]
httpx = { url = "https://files.pythonhosted.org/packages/5c/2d/3da5bdf4408b8b2800061c339f240c1802f2e82d55e50bd39c5a881f47f0/httpx-0.27.0.tar.gz" }
```

URL 依赖项也可以通过 `{ url = <url> }` 语法手动添加或编辑在 `pyproject.toml` 文件中。如果源代码分发文件不在归档根目录下，可以指定 `subdirectory`。

### Path {: #path}

要添加路径源，提供一个 wheel 文件（以 `.whl` 结尾）、源代码分发文件（通常以 `.tar.gz` 或 `.zip` 结尾；查看[此处](../../concepts/resolution.md#source-distribution)了解所有支持的格式）或包含 `pyproject.toml` 的目录的路径。

例如：

```console
$ uv add /example/foo-0.1.0-py3-none-any.whl
```

这将导致 `pyproject.toml` 文件如下所示：

```toml title="pyproject.toml"
[project]
dependencies = ["foo"]

[tool.uv.sources]
foo = { path = "/example/foo-0.1.0-py3-none-any.whl" }
```

路径也可以是相对路径：

```console
$ uv add ./foo-0.1.0-py3-none-any.whl
```

或者，指定一个项目目录的路径：

```console
$ uv add ~/projects/bar/
```

!!! important

    默认情况下，路径依赖项不会使用[可编辑安装](#editable-dependencies)。对于项目目录，可以请求可编辑安装：

    ```console
    $ uv add --editable ~/projects/bar/
    ```

    对于同一仓库中的多个包，使用 [_工作空间_](./workspaces.md) 可能更合适。

### 工作空间成员 {: #workspace-member}

要声明对工作空间成员的依赖，使用 `{ workspace = true }` 添加成员名称。所有工作空间成员必须明确列出。工作空间成员始终是[可编辑的](#editable-dependencies)。有关工作空间的更多信息，请参见 [工作空间](./workspaces.md) 文档。

```toml title="pyproject.toml"
[project]
dependencies = ["foo==0.1.0"]

[tool.uv.sources]
foo = { workspace = true }

[tool.uv.workspace]
members = [
  "packages/foo"
]
```

### 平台特定源

通过为源提供[依赖说明符](https://packaging.python.org/en/latest/specifications/dependency-specifiers/)-兼容的环境标记，可以将源限制为特定平台或 Python 版本。

例如，要仅在 macOS 上从 GitHub 拉取 `httpx`，可以使用以下配置：

```toml title="pyproject.toml" hl_lines="8"
[project]
dependencies = ["httpx"]

[tool.uv.sources]
httpx = {
  git = "https://github.com/encode/httpx",
  tag = "0.27.2",
  marker = "sys_platform == 'darwin'"
}
```

通过在源上指定标记，`uv` 将在所有平台上包含 `httpx`，但会在 macOS 上从 GitHub 下载源代码，并在其他平台上回退到 PyPI。

### 多个源

可以通过提供一个源的列表，并使用[PEP 508](https://peps.python.org/pep-0508/#environment-markers)-兼容的环境标记来为单个依赖指定多个源。

例如，要在 macOS 和 Linux 上拉取不同的 `httpx` 提交：

```toml title="pyproject.toml" hl_lines="8-9 13-14"
[project]
dependencies = ["httpx"]

[tool.uv.sources]
httpx = [
  {
    git = "https://github.com/encode/httpx",
    tag = "0.27.2",
    marker = "sys_platform == 'darwin'"
  },
  {
    git = "https://github.com/encode/httpx",
    tag = "0.24.1",
    marker = "sys_platform == 'linux'"
  },
]
```

这种策略也适用于根据环境标记使用不同的索引。例如，根据平台从不同的 PyTorch 索引安装 `torch`：

```toml title="pyproject.toml" hl_lines="6-7"
[project]
dependencies = ["torch"]

[tool.uv.sources]
torch = [
  { index = "torch-cpu", marker = "platform_system == 'Darwin'"},
  { index = "torch-gpu", marker = "platform_system == 'Linux'"},
]

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"

[[tool.uv.index]]
name = "torch-gpu"
url = "https://download.pytorch.org/whl/cu124"
```

### 禁用源

要指示 `uv` 忽略 `tool.uv.sources` 表（例如，模拟使用包的发布元数据解析），请使用 `--no-sources` 标志：

```console
$ uv lock --no-sources
```

使用 `--no-sources` 还会阻止 `uv` 发现任何可以满足给定依赖的[工作空间成员](#workspace-member)。

## 可选依赖 {: #optional-dependencies}

对于作为库发布的项目，通常会将某些功能设为可选，以减少默认的依赖树。例如，Pandas 有一个 [`excel` 附加功能](https://pandas.pydata.org/docs/getting_started/install.html#excel-files) 和一个 [`plot` 附加功能](https://pandas.pydata.org/docs/getting_started/install.html#visualization)，用于避免安装 Excel 解析器和 `matplotlib`，除非显式要求它们。附加功能使用 `package[<extra>]` 语法请求，例如 `pandas[plot, excel]`。

可选依赖在 `[project.optional-dependencies]` 中指定，这是一个 TOML 表，映射从附加功能名称到其依赖项，遵循[依赖说明符](#dependency-specifiers-pep-508)语法。

可选依赖也可以在 `tool.uv.sources` 中有与普通依赖相同的条目。

```toml title="pyproject.toml"
[project]
name = "pandas"
version = "1.0.0"

[project.optional-dependencies]
plot = [
  "matplotlib>=3.6.3"
]
excel = [
  "odfpy>=1.4.1",
  "openpyxl>=3.1.0",
  "python-calamine>=0.1.7",
  "pyxlsb>=1.0.10",
  "xlrd>=2.0.1",
  "xlsxwriter>=3.0.5"
]
```

要添加一个可选依赖，请使用 `--optional <extra>` 选项：

```console
$ uv add httpx --optional network
```

!!! note

    如果您的可选依赖存在冲突，除非您显式[声明它们为冲突依赖](./config.md#conflicting-dependencies)，否则解析将失败。

源也可以声明仅适用于特定的可选依赖。例如，要根据可选的 `cpu` 或 `gpu` 附加功能从不同的 PyTorch 索引拉取 `torch`：

```toml title="pyproject.toml"
[project]
dependencies = []

[project.optional-dependencies]
cpu = [
  "torch",
]
gpu = [
  "torch",
]

[tool.uv.sources]
torch = [
  { index = "torch-cpu", extra = "cpu" },
  { index = "torch-gpu", extra = "gpu" },
]

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"

[[tool.uv.index]]
name = "torch-gpu"
url = "https://download.pytorch.org/whl/cu124"
```

## 开发依赖 {: #development-dependencies}

与可选依赖不同，开发依赖仅限本地使用，并且在发布到 PyPI 或其他索引时不会被包含在项目的依赖要求中。因此，开发依赖不包含在 `[project]` 表中。

开发依赖可以像普通依赖一样在 `tool.uv.sources` 中进行定义。

要添加开发依赖，使用 `--dev` 标志：

```console
$ uv add --dev pytest
```

`uv` 使用 `[dependency-groups]` 表（如[PEP 735](https://peps.python.org/pep-0735/)所定义）来声明开发依赖。上述命令会创建一个 `dev` 组：

```toml title="pyproject.toml"
[dependency-groups]
dev = [
  "pytest >=8.1.1,<9"
]
```

`dev` 组是特殊处理的；有 `--dev`、`--only-dev` 和 `--no-dev` 标志来切换其依赖的包含或排除。此外，`dev` 组是[默认同步](#default-groups)的。

### 依赖组 {: #dependency-groups}

开发依赖可以分为多个组，使用 `--group` 标志。

例如，要将一个开发依赖添加到 `lint` 组：

```console
$ uv add --group lint ruff
```

这会在 `[dependency-groups]` 中生成以下定义：

```toml title="pyproject.toml"
[dependency-groups]
dev = [
  "pytest"
]
lint = [
  "ruff"
]
```

一旦定义了组，就可以使用 `--group`、`--only-group` 和 `--no-group` 选项来包含或排除它们的依赖。

!!! tip

    `--dev`、`--only-dev` 和 `--no-dev` 标志相当于 `--group dev`、`--only-group dev` 和 `--no-group dev`。

`uv` 要求所有依赖组相互兼容，并在创建锁文件时将所有组一起解析。

如果一个组中声明的依赖与另一个组中的依赖不兼容，`uv` 会在解析项目的要求时失败并报错。

!!! 注意

    如果你有相互冲突的依赖组，除非你显式[声明它们为冲突依赖](./config.md#conflicting-dependencies)，否则解析将失败。

### 默认组 {: #default-groups}

默认情况下，`uv` 在环境中包含 `dev` 依赖组（例如，在执行 `uv run` 或 `uv sync` 时）。可以使用 `tool.uv.default-groups` 设置来更改包含的默认组。

```toml title="pyproject.toml"
[tool.uv]
default-groups = ["dev", "foo"]
```

!!! tip

    要在 `uv run` 或 `uv sync` 中排除默认组，可以使用 `--no-group <name>`。

### 旧版 `dev-dependencies`

在 `[dependency-groups]` 标准化之前，`uv` 使用 `tool.uv.dev-dependencies` 字段来指定开发依赖，例如：

```toml title="pyproject.toml"
[tool.uv]
dev-dependencies = [
  "pytest"
]
```

在此部分声明的依赖将与 `dependency-groups.dev` 中的内容合并。最终，`dev-dependencies` 字段将被弃用并移除。

!!! 注意

    如果存在 `tool.uv.dev-dependencies` 字段，`uv add --dev` 会使用现有的部分，而不是添加新的 `dependency-groups.dev` 部分。

## 构建依赖

如果一个项目被构建为 [Python 包](./config.md#build-systems)，它可能声明一些构建项目时所需的依赖，但不需要在运行时使用。这些依赖在 `[build-system]` 表中的 `build-system.requires` 下声明，遵循 [PEP 518](https://peps.python.org/pep-0518/)。

例如，如果项目使用 `setuptools` 作为构建后端，它应将 `setuptools` 声明为构建依赖：

```toml title="pyproject.toml"
[project]
name = "pandas"
version = "0.1.0"

[build-system]
requires = ["setuptools>=42"]
build-backend = "setuptools.build_meta"
```

默认情况下，`uv` 在解析构建依赖时会遵循 `tool.uv.sources`。例如，要使用本地版本的 `setuptools` 进行构建，可以将该源添加到 `tool.uv.sources`：

```toml title="pyproject.toml"
[project]
name = "pandas"
version = "0.1.0"

[build-system]
requires = ["setuptools>=42"]
build-backend = "setuptools.build_meta"

[tool.uv.sources]
setuptools = { path = "./packages/setuptools" }
```

在发布包时，建议运行 `uv build --no-sources`，以确保在禁用 `tool.uv.sources` 的情况下正确构建包，尤其是使用其他构建工具（如 [`pypa/build`](https://github.com/pypa/build)）时。

## 可编辑依赖 {: #editable-dependencies}

常规的 Python 包目录安装首先会构建一个 wheel 文件，然后将其安装到虚拟环境中，并复制所有源代码文件。当源代码文件被编辑时，虚拟环境中会包含过时的版本。

可编辑安装通过在虚拟环境中添加一个指向项目的链接（即 `.pth` 文件）来解决这个问题，这样解释器会直接包含源代码文件。

可编辑安装存在一些限制（主要是：构建后端需要支持它们，并且本地模块在导入前不会重新编译），但它们在开发中非常有用，因为虚拟环境总是使用包的最新更改。

`uv` 默认对工作区包使用可编辑安装。

要添加可编辑依赖，请使用 `--editable` 标志：

```console
$ uv add --editable ./path/foo
```

或者，要在工作区中选择不使用可编辑依赖：

```console
$ uv add --no-editable ./path/foo
```

## 依赖说明符（PEP 508） {: #dependency-specifiers-pep-508}

`uv` 使用 [依赖说明符](https://packaging.python.org/en/latest/specifications/dependency-specifiers/)，即之前所称的 [PEP 508](https://peps.python.org/pep-0508/)。一个依赖说明符由以下几部分组成，按顺序排列：

- 依赖名称
- 可选的 extras
- 版本说明符
- 可选的环境标记

版本说明符是逗号分隔的，并且是累加的。例如，`foo >=1.2.3,<2,!=1.4.0` 被解释为 "版本至少为 1.2.3，但小于 2，并且不为 1.4.0"。

如果需要，说明符会用尾部的零进行填充，因此 `foo ==2` 也会匹配 `foo 2.0.0`。

当使用等号时，可以使用星号作为最后一位，例如 `foo ==2.1.*` 会接受 2.1 系列中的任何版本。同样，`~=` 会匹配最后一位相等或更高的版本，例如 `foo ~=1.2` 等同于 `foo >=1.2,<2`，而 `foo ~=1.2.3` 等同于 `foo >=1.2.3,<1.3`。

Extras 是逗号分隔并放在名称和版本之间的方括号中，例如，`pandas[excel,plot] ==2.2`。在 extras 名称之间的空格会被忽略。

有些依赖只在特定环境中需要，例如某个特定的 Python 版本或操作系统。例如，要安装用于 `importlib.metadata` 模块的 `importlib-metadata` 后移植版本，可以使用 `importlib-metadata >=7.1.0,<8; python_version < '3.10'`。要在 Windows 上安装 `colorama`（但在其他平台上省略它），可以使用 `colorama >=0.4.6,<5; platform_system == "Windows"`。

标记可以使用 `and`、`or` 和括号进行组合，例如，`aiohttp >=3.7.4,<4; (sys_platform != 'win32' or implementation_name != 'pypy') and python_version >= '3.10'`。请注意，标记中的版本必须用引号括起来，而标记外的版本则不需要引号。

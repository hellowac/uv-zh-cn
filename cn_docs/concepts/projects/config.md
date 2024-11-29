# 配置项目

## Python 版本要求 {: #python-version-requirement}

项目可以在 `pyproject.toml` 文件的 `project.requires-python` 字段中声明支持的 Python 版本。

建议设置 `requires-python` 值：

```toml title="pyproject.toml" hl_lines="4"
[project]
name = "example"
version = "0.1.0"
requires-python = ">=3.12"
```

Python 版本要求决定了项目中允许使用的 Python 语法，并且会影响依赖版本的选择（它们必须支持相同的 Python 版本范围）。

## 入口点 {: #entry-points}

[入口点](https://hellowac.github.io/pypug-zh-cn/specifications/entry-points.html#entry-points) 是安装包用于公开接口的官方术语。包括以下几种类型：

- [命令行接口](#command-line-interfaces)
- [图形用户界面](#graphical-user-interfaces)
- [插件入口点](#plugin-entry-points)

!!! important

    使用入口点表需要定义一个 [构建系统](#build-systems)。

### 命令行接口 {: #command-line-interfaces}

项目可以在 `pyproject.toml` 文件的 `[project.scripts]` 表中定义项目的命令行接口（CLI）。

例如，要声明一个名为 `hello` 的命令，该命令调用 `example` 模块中的 `hello` 函数：

```toml title="pyproject.toml"
[project.scripts]
hello = "example:hello"
```

然后，可以从控制台运行该命令：

```console
$ uv run hello
```

### 图形用户界面 {: #graphical-user-interfaces}

项目可以在 `pyproject.toml` 文件的 `[project.gui-scripts]` 表中定义项目的图形用户界面（GUI）。

!!! important

    这些仅在 Windows 平台上与 [命令行接口](#command-line-interfaces) 不同，在 Windows 上，它们会被封装成 GUI 可执行文件，这样可以在没有控制台的情况下启动。其他平台上的行为相同。

例如，要声明一个名为 `hello` 的命令，该命令调用 `example` 模块中的 `app` 函数：

```toml title="pyproject.toml"
[project.gui-scripts]
hello = "example:app"
```

### 插件入口点 {: #plugin-entry-points}

项目可以在 `pyproject.toml` 文件的 `[project.entry-points]` 表中定义插件发现的入口点，具体可参考 [创建和发现插件](https://hellowac.github.io/pypug-zh-cn/guides/creating-and-discovering-plugins.html#using-package-metadata)。

例如，要将 `example-plugin-a` 包注册为 `example` 的插件：

```toml title="pyproject.toml"
[project.entry-points.'example.plugins']
a = "example_plugin_a"
```

然后，在 `example` 中，插件可以通过以下方式加载：

```python title="example/__init__.py"
from importlib.metadata import entry_points

for plugin in entry_points(group='example.plugins'):
    plugin.load()
```

!!! note

    `group` 键可以是任意值，不需要包含包名或 "plugins"。然而，建议使用包名来命名该键，以避免与其他包发生冲突。

## 构建系统 {: #build-systems}

构建系统决定了项目应该如何打包和安装。项目可以在 `pyproject.toml` 文件的 `[build-system]` 表中声明和配置构建系统。

uv 使用构建系统的存在来判断项目是否包含应该安装到项目虚拟环境中的包。如果没有定义构建系统，uv 将不会尝试构建或安装项目本身，只会安装其依赖项。如果定义了构建系统，uv 将构建并将项目安装到项目环境中。

`--build-backend` 选项可以提供给 `uv init`，以创建具有适当布局的打包项目。`--package` 选项可以提供给 `uv init`，以创建具有默认构建系统的打包项目。

!!! note

    尽管没有定义构建系统时，uv 不会构建和安装当前项目，但其他包中的 `[build-system]` 表不是必需的。出于兼容性原因，如果没有定义构建系统，则使用 `setuptools.build_meta:__legacy__` 来构建包。您依赖的包可能没有显式声明其构建系统，但仍然可以安装。同样，如果您添加了对本地包的依赖，uv 将始终尝试构建并安装它。

## 项目打包 {: #project-packaging}

如 [构建系统](#build-systems) 所述，Python 项目必须经过构建才能进行安装。这个过程通常被称为“打包”。

如果您想：

- 向项目添加命令
- 将项目分发给其他人
- 使用 `src` 和 `test` 目录结构
- 编写库

那么您可能需要一个打包。如果您：

- 编写脚本
- 构建简单应用
- 使用平面目录结构

则可能不需要打包。

虽然 uv 通常通过声明 [构建系统](#build-systems) 来判断是否需要打包项目，uv 也允许通过 [`tool.uv.package`](../../reference/settings.md#package) 设置来覆盖此行为。

设置 `tool.uv.package = true` 将强制项目进行构建并安装到项目环境中。如果没有定义构建系统，uv 将使用 setuptools 旧版构建后端。

设置 `tool.uv.package = false` 将强制项目包 _不_ 被构建并安装到项目环境中。uv 将在与项目交互时忽略已声明的构建系统。

## 项目环境路径

`UV_PROJECT_ENVIRONMENT` 环境变量可用于配置项目虚拟环境的路径（默认是 `.venv`）。

如果提供相对路径，它将相对于工作区根目录进行解析。如果提供绝对路径，则会直接使用该路径，即不会为环境创建子目录。如果在提供的路径下没有找到环境，uv 将创建该环境。

此选项可用于写入系统 Python 环境，但不推荐使用。默认情况下，`uv sync` 会从环境中删除多余的包，因此可能会导致系统处于损坏状态。

!!! important

    如果提供了绝对路径，并且该设置在多个项目中使用，则每个项目中的调用将覆盖该环境。此设置仅推荐在 CI 或 Docker 镜像中的单一项目中使用。

!!! note

    uv 在项目操作过程中不会读取 `VIRTUAL_ENV` 环境变量。如果 `VIRTUAL_ENV` 设置为与项目环境不同的路径，将显示警告。

## 限制解析环境

如果您的项目仅支持某些平台或 Python 版本，您可以通过 `environments` 设置来限制可解析的平台集合，该设置接受 PEP 508 环境标记的列表。例如，要将锁定文件限制为 macOS 和 Linux，并排除 Windows：

```toml title="pyproject.toml"
[tool.uv]
environments = [
    "sys_platform == 'darwin'",
    "sys_platform == 'linux'",
]
```

或者，排除其他 Python 实现：

```toml title="pyproject.toml"
[tool.uv]
environments = [
    "implementation_name == 'cpython'"
]
```

`environments` 设置中的条目必须是互斥的（即它们不能重叠）。例如，`sys_platform == 'darwin'` 和 `sys_platform == 'linux'` 是互斥的，但 `sys_platform == 'darwin'` 和 `python_version >= '3.9'` 不是，因为它们可能同时为真。

## 构建隔离

默认情况下，uv 会按照 [PEP 517](https://peps.python.org/pep-0517/) 的要求，在隔离的虚拟环境中构建所有包。有些包与构建隔离不兼容，这可能是故意的（例如，使用了大量的构建依赖项，最常见的是 PyTorch）或是无意的（例如，使用了遗留的打包设置）。

要禁用特定依赖项的构建隔离，可以将其添加到 `pyproject.toml` 文件中的 `no-build-isolation-package` 列表中：

```toml title="pyproject.toml"
[project]
name = "project"
version = "0.1.0"
description = "..."
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["cchardet"]

[tool.uv]
no-build-isolation-package = ["cchardet"]
```

在没有构建隔离的情况下安装包要求包的构建依赖项必须在安装包之前预先安装到项目环境中。可以通过将构建依赖项和需要它们的包分成不同的可选依赖项来实现。例如：

```toml title="pyproject.toml"
[project]
name = "project"
version = "0.1.0"
description = "..."
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[project.optional-dependencies]
build = ["setuptools", "cython"]
compile = ["cchardet"]

[tool.uv]
no-build-isolation-package = ["cchardet"]
```

根据上述配置，用户首先同步 `build` 依赖项：

```console
$ uv sync --extra build
 + cython==3.0.11
 + foo==0.1.0 (from file:///Users/crmarsh/workspace/uv/foo)
 + setuptools==73.0.1
```

然后同步 `compile` 依赖项：

```console
$ uv sync --extra compile
 + cchardet==2.1.7
 - cython==3.0.11
 - setuptools==73.0.1
```

请注意，`uv sync --extra compile` 默认会卸载 `cython` 和 `setuptools` 包。为了保留构建依赖项，可以在第二次 `uv sync` 调用中同时包括两个可选依赖项：

```console
$ uv sync --extra build
$ uv sync --extra build --extra compile
```

一些包（如上面的 `cchardet`）仅在 `uv sync` 的 _安装_ 阶段需要构建依赖项。其他包，如 `flash-attn`，甚至在 _解析_ 阶段解决项目的锁文件时，也需要其构建依赖项。

在这种情况下，必须在运行任何 `uv lock` 或 `uv sync` 命令之前，使用更低层次的 `uv pip` API 安装构建依赖项。例如，假设以下配置：

```toml title="pyproject.toml"
[project]
name = "project"
version = "0.1.0"
description = "..."
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["flash-attn"]

[tool.uv]
no-build-isolation-package = ["flash-attn"]
```

可以执行以下命令序列来同步 `flash-attn`：

```console
$ uv venv
$ uv pip install torch
$ uv sync
```

或者，您可以通过 [`dependency-metadata`](../../reference/settings.md#dependency-metadata) 设置提前提供 `flash-attn` 的元数据，从而避免在依赖解析阶段构建该包。例如，要提前提供 `flash-attn` 的元数据，可以在 `pyproject.toml` 中包含以下内容：

```toml title="pyproject.toml"
[[tool.uv.dependency-metadata]]
name = "flash-attn"
version = "2.6.3"
requires-dist = ["torch", "einops"]
```

!!! tip

    要确定像 `flash-attn` 这样的包的元数据，可以导航到相应的 Git 仓库，或在 [PyPI](https://pypi.org/project/flash-attn) 上查找该包并下载源代码分发包。包的要求通常可以在 `setup.py` 或 `setup.cfg` 文件中找到。

    （如果包包括构建后的分发文件，您可以解压它以查找 `METADATA` 文件；然而，若包已经有构建后的分发文件，您就不需要提前提供元数据，因为它已经可以被 uv 使用。）

一旦包含了元数据，您可以再次使用两步 `uv sync` 过程来安装构建依赖项。假设 `pyproject.toml` 文件如下：

```toml title="pyproject.toml"
[project]
name = "project"
version = "0.1.0"
description = "..."
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[project.optional-dependencies]
build = ["torch", "setuptools", "packaging"]
compile = ["flash-attn"]

[tool.uv]
no-build-isolation-package = ["flash-attn"]

[[tool.uv.dependency-metadata]]
name = "flash-attn"
version = "2.6.3"
requires-dist = ["torch", "einops"]
```

可以运行以下命令序列来同步 `flash-attn`：

```console
$ uv sync --extra build
$ uv sync --extra build --extra compile
```

!!! note

    `tool.uv.dependency-metadata` 中的 `version` 字段对于基于注册表的依赖项是可选的（如果省略，uv 会假定元数据适用于包的所有版本），但对于直接 URL 依赖项（如 Git 依赖项）是 _必需的_。

## 可编辑模式

默认情况下，项目将以可编辑模式安装，这样对源代码的更改会立即反映在环境中。`uv sync` 和 `uv run` 都接受 `--no-editable` 标志，该标志指示 uv 以非可编辑模式安装项目。`--no-editable` 适用于部署场景，例如构建 Docker 容器，其中项目应该被包含在已部署的环境中，而不依赖于源代码。

## 冲突依赖项 {: #conflicting-dependencies}

uv 要求项目声明的所有可选依赖项（“额外依赖项”）彼此兼容，并在创建锁定文件时将所有可选依赖项一起解析。

如果一个额外依赖项中的可选依赖项与另一个额外依赖项中的依赖项不兼容，uv 将无法解析项目的要求并报错。

为了解决这个问题，uv 支持声明冲突的额外依赖项。例如，考虑以下两个相互冲突的可选依赖项集：

```toml title="pyproject.toml"
[project.optional-dependencies]
extra1 = ["numpy==2.1.2"]
extra2 = ["numpy==2.0.0"]
```

如果使用上述依赖项运行 `uv lock`，解析将失败：

```console
$ uv lock
  x No solution found when resolving dependencies:
  `-> Because myproject[extra2] depends on numpy==2.0.0 and myproject[extra1] depends on numpy==2.1.2, we can conclude that myproject[extra1] and
      myproject[extra2] are incompatible.
      And because your project requires myproject[extra1] and myproject[extra2], we can conclude that your project's requirements are unsatisfiable.
```

但是，如果您指定 `extra1` 和 `extra2` 是冲突的，uv 将分别解析它们。在 `tool.uv` 部分中指定冲突：

```toml title="pyproject.toml"
[tool.uv]
conflicts = [
    [
      { extra = "extra1" },
      { extra = "extra2" },
    ],
]
```

现在，运行 `uv lock` 将成功。但请注意，现在您不能同时安装 `extra1` 和 `extra2`：

```console
$ uv sync --extra extra1 --extra extra2
Resolved 3 packages in 14ms
error: extra `extra1`, extra `extra2` are incompatible with the declared conflicts: {`myproject[extra1]`, `myproject[extra2]`}
```

此错误发生是因为同时安装 `extra1` 和 `extra2` 会导致将两个不同版本的包安装到同一个环境中。

处理冲突额外依赖项的上述策略也适用于依赖组：

```toml title="pyproject.toml"
[dependency-groups]
group1 = ["numpy==2.1.2"]
group2 = ["numpy==2.0.0"]

[tool.uv]
conflicts = [
    [
      { group = "group1" },
      { group = "group2" },
    ],
]
```

与冲突额外依赖项的唯一区别是，您需要使用 `group` 而不是 `extra`。

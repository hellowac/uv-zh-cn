# 在项目中工作

uv 支持管理 Python 项目，这些项目在 `pyproject.toml` 文件中定义它们的依赖项。

## 创建一个新项目

您可以使用 `uv init` 命令创建一个新的 Python 项目：

```console
$ uv init hello-world
$ cd hello-world
```

或者，您可以在工作目录中初始化一个项目：

```console
$ mkdir hello-world
$ cd hello-world
$ uv init
```

uv 将创建以下文件：

```text
.
├── .python-version
├── README.md
├── hello.py
└── pyproject.toml
```

`hello.py` 文件包含一个简单的 "Hello world" 程序。使用 `uv run` 尝试它：

```console
$ uv run hello.py
Hello from hello-world!
```

## 项目结构

一个项目由几个重要部分组成，这些部分共同工作，使 uv 能够管理您的项目。除了 `uv init` 创建的文件外，第一次运行项目命令（即 `uv run`、`uv sync` 或 `uv lock`）时，uv 会在项目的根目录下创建一个虚拟环境和 `uv.lock` 文件。

完整的项目文件结构如下：

```text
.
├── .venv
│   ├── bin
│   ├── lib
│   └── pyvenv.cfg
├── .python-version
├── README.md
├── hello.py
├── pyproject.toml
└── uv.lock
```

### `pyproject.toml`

`pyproject.toml` 文件包含有关项目的元数据：

```toml title="pyproject.toml"
[project]
name = "hello-world"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = []
```

您将使用此文件来指定依赖项，以及项目的描述、许可证等详细信息。您可以手动编辑此文件，或使用 `uv add` 和 `uv remove` 等命令来从终端管理项目。

!!! tip

    查看官方的 [`pyproject.toml` 指南](https://hellowac.github.io/pypug-zh-cn/guides/writing-pyproject-toml.html) 以了解如何使用 `pyproject.toml` 格式的更多详细信息。

您还将使用此文件在 [`[tool.uv]`](../reference/settings.md) 部分中指定 uv 的 [配置选项](../configuration/files.md)。

### `.python-version`

`.python-version` 文件包含项目的默认 Python 版本。该文件告诉 uv 在创建项目虚拟环境时使用哪个 Python 版本。

### `.venv`

`.venv` 文件夹包含项目的虚拟环境，这是一个与系统其他部分隔离的 Python 环境。在这里，uv 将安装项目的依赖项。

有关更多详细信息，请参阅 [项目环境](../concepts/projects/layout.md#the-project-environment) 文档。

### `uv.lock`

`uv.lock` 是一个跨平台的锁文件，包含有关项目依赖项的精确信息。与用于指定项目广泛需求的 `pyproject.toml` 不同，锁文件包含已安装在项目环境中的确切解决版本。此文件应被提交到版本控制中，以便在不同机器之间实现一致和可复现的安装。

`uv.lock` 是一个可读的 TOML 文件，但由 uv 管理，通常不应手动编辑。

有关详细信息，请参阅 [锁文件](../concepts/projects/layout.md#the-lockfile) 文档。

## 管理依赖项

您可以使用 `uv add` 命令将依赖项添加到 `pyproject.toml` 中。此操作还将更新锁文件和项目环境：

```console
$ uv add requests
```

您还可以指定版本约束或使用替代源：

```console
$ # 指定版本约束
$ uv add 'requests==2.31.0'

$ # 添加 git 依赖
$ uv add git+https://github.com/psf/requests
```

要删除一个包，您可以使用 `uv remove` 命令：

```console
$ uv remove requests
```

要升级包，请运行 `uv lock` 并使用 `--upgrade-package` 标志：

```console
$ uv lock --upgrade-package requests
```

`--upgrade-package` 标志将尝试将指定的包更新到最新的兼容版本，同时保持锁文件的其余部分不变。

有关更多信息，请参阅 [管理依赖项](../concepts/projects/dependencies.md) 文档。

## 运行命令

`uv run` 可以用来在您的项目环境中运行任意脚本或命令。

在每次调用 `uv run` 之前，uv 会验证锁文件是否与 `pyproject.toml` 保持同步，并且环境是否与锁文件保持同步，确保您的项目无需手动干预即可保持一致性。`uv run` 保证您的命令将在一个一致的、已锁定的环境中运行。

例如，要使用 `flask`：

```console
$ uv add flask
$ uv run -- flask run -p 3000
```

或者，要运行一个脚本：

```python title="example.py"
# 需要一个项目依赖
import flask

print("hello world")
```

```console
$ uv run example.py
```

或者，您也可以使用 `uv sync` 手动更新环境，然后在执行命令之前激活它：

```console
$ uv sync
$ source .venv/bin/activate
$ flask run -p 3000
$ python example.py
```

!!! 注意

    必须激活虚拟环境才能在项目中运行脚本和命令，且无需使用 `uv run`。虚拟环境的激活方法因 shell 和平台而异。

有关在项目中运行命令和脚本的更多信息，请参阅 [运行命令和脚本](../concepts/projects/run.md) 文档。

## 构建分发包

`uv build` 可用于为您的项目构建源分发包和二进制分发包（wheel 文件）。

默认情况下，`uv build` 会在当前目录中构建项目，并将构建好的工件放入 `dist/` 子目录中：

```console
$ uv build
$ ls dist/
hello-world-0.1.0-py3-none-any.whl
hello-world-0.1.0.tar.gz
```

有关更多信息，请参阅 [构建项目](../concepts/projects/build.md) 文档。

## 下一步

要了解更多关于使用 uv 开发项目的信息，请查看 [项目概念](../concepts/projects/index.md) 页面和 [命令参考](../reference/cli.md#uv)。

或者，继续阅读以了解如何 [将项目发布为包](./publish.md)。

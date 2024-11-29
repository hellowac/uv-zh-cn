# 使用工作区

受到 [Cargo](https://doc.rust-lang.org/cargo/reference/workspaces.html) 概念的启发，工作区是 “一组或多个包的集合，这些包被称为 _工作区成员_，并且被一起管理。”

工作区通过将大型代码库拆分成多个包，且这些包共享相同的依赖项，来组织大型代码库。例如，考虑一个基于 FastAPI 的 web 应用程序，旁边有一系列版本化并作为单独 Python 包维护的库，它们都在同一个 Git 仓库中。

在工作区中，每个包都会定义自己的 `pyproject.toml` 文件，但整个工作区共享一个锁定文件，确保工作区在一致的依赖集合下运行。

因此，`uv lock` 会对整个工作区操作，而 `uv run` 和 `uv sync` 默认对工作区根目录操作，尽管它们都接受 `--package` 参数，这允许你在任何工作区目录中运行特定工作区成员的命令。

## 入门

要创建一个工作区，请在 `pyproject.toml` 中添加 `tool.uv.workspace` 表格，这将隐式地在该包下创建一个以该包为根的工作区。

!!! tip

    默认情况下，在现有包中运行 `uv init` 会将新创建的成员添加到工作区，并在工作区根目录创建 `tool.uv.workspace` 表格（如果尚不存在）。

在定义工作区时，必须指定 `members`（必需）和 `exclude`（可选）键，分别用于指导工作区包含或排除特定目录作为成员，并接受 glob 列表：

```toml title="pyproject.toml"
[project]
name = "albatross"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["bird-feeder", "tqdm>=4,<5"]

[tool.uv.sources]
bird-feeder = { workspace = true }

[tool.uv.workspace]
members = ["packages/*"]
exclude = ["packages/seeds"]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

`members` glob 列表（并且未被 `exclude` glob 排除的）中的每个目录都必须包含一个 `pyproject.toml` 文件。然而，工作区成员可以是 _应用程序_ 或 _库_，两者都支持在工作区环境中使用。

每个工作区都需要一个根目录，且该根目录 _也_ 是工作区成员。在上面的例子中，`albatross` 是工作区的根目录，工作区成员包括 `packages` 目录下的所有项目，除了 `seeds`。

默认情况下，`uv run` 和 `uv sync` 在工作区根目录中操作。例如，在上述示例中，`uv run` 和 `uv run --package albatross` 是等效的，而 `uv run --package bird-feeder` 会在 `bird-feeder` 包中运行命令。

## 工作区源

在工作区中，通过 [`tool.uv.sources`](./dependencies.md) 来处理工作区成员之间的依赖关系，如下所示：

```toml title="pyproject.toml"
[project]
name = "albatross"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["bird-feeder", "tqdm>=4,<5"]

[tool.uv.sources]
bird-feeder = { workspace = true }

[tool.uv.workspace]
members = ["packages/*"]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

在此示例中，`albatross` 项目依赖于 `bird-feeder` 项目，而 `bird-feeder` 是工作区的成员。`tool.uv.sources` 表格中的 `workspace = true` 键值对表示 `bird-feeder` 依赖项应该由工作区提供，而不是从 PyPI 或其他注册表中获取。

工作区根目录中的任何 `tool.uv.sources` 定义都适用于所有成员，除非在某个特定成员的 `tool.uv.sources` 表格中覆盖。例如，给定以下 `pyproject.toml` 文件：

```toml title="pyproject.toml"
[project]
name = "albatross"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["bird-feeder", "tqdm>=4,<5"]

[tool.uv.sources]
bird-feeder = { workspace = true }
tqdm = { git = "https://github.com/tqdm/tqdm" }

[tool.uv.workspace]
members = ["packages/*"]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

除非某个特定成员在其自己的 `tool.uv.sources` 表格中覆盖 `tqdm` 条目，否则每个工作区成员将默认从 GitHub 安装 `tqdm`。

## 工作区布局

最常见的工作区布局可以被看作是一个根项目，附带一系列库。

例如，继续使用上面的例子，这个工作区有一个明确的根目录 `albatross`，并且在 `packages` 目录下有两个库 (`bird-feeder` 和 `seeds`)：

```text
albatross
├── packages
│   ├── bird-feeder
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── bird_feeder
│   │           ├── __init__.py
│   │           └── foo.py
│   └── seeds
│       ├── pyproject.toml
│       └── src
│           └── seeds
│               ├── __init__.py
│               └── bar.py
├── pyproject.toml
├── README.md
├── uv.lock
└── src
    └── albatross
        └── main.py
```

由于在 `pyproject.toml` 文件中排除了 `seeds`，因此该工作区共有两个成员：`albatross`（根项目）和 `bird-feeder`。

## 何时（不）使用工作区

工作区旨在促进在单个代码库中开发多个互相关联的包。随着代码库复杂度的增加，将其拆分成多个小的、可组合的包，每个包都有自己的依赖和版本约束，可能会变得更加有用。

工作区有助于强制执行隔离和关注点分离。例如，在 `uv` 中，我们有单独的包来管理核心库和命令行界面（CLI），这使得我们能够独立测试核心库和 CLI，反之亦然。

工作区的其他常见用例包括：

- 一个性能关键的子程序，使用扩展模块（如 Rust、C++ 等）实现的库。
- 一个插件系统的库，其中每个插件都是一个独立的工作区包，并依赖于根项目。

工作区 _不适用于_ 包之间有冲突的依赖要求，或希望每个包使用独立虚拟环境的情况。在这种情况下，路径依赖通常是更好的选择。例如，您可以将 `albatross` 及其成员定义为独立的项目，而在 `tool.uv.sources` 中定义包之间的路径依赖，而不是将它们组合成工作区：

```toml title="pyproject.toml"
[project]
name = "albatross"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["bird-feeder", "tqdm>=4,<5"]

[tool.uv.sources]
bird-feeder = { path = "packages/bird-feeder" }

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

这种方法提供了许多相同的好处，但允许对依赖解析和虚拟环境管理进行更精细的控制（但缺点是 `uv run --package` 不再可用；相反，命令必须从相关的包目录中运行）。

最后，`uv` 的工作区强制整个工作区使用单一的 `requires-python` 值，这是所有成员的 `requires-python` 值的交集。如果你需要在不被其他工作区成员支持的 Python 版本上测试某个成员，你可能需要使用 `uv pip` 将该成员安装在一个独立的虚拟环境中。

!!! note

    由于 Python 不提供依赖隔离，`uv` 无法确保一个包仅使用其声明的依赖项而不包含其他依赖项。对于工作区，`uv` 无法确保包不会导入另一个工作区成员声明的依赖项。

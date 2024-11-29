# 配置文件

uv 支持在项目级别和用户级别使用持久化配置文件。

具体来说，uv 会在当前目录或最近的父目录中搜索 `pyproject.toml` 或 `uv.toml` 文件。

!!! note

    对于在用户级别操作的 `tool` 命令，本地配置文件将被忽略。uv 将仅从用户级别配置（例如 `~/.config/uv/uv.toml`）和系统级别配置（例如 `/etc/uv/uv.toml`）中读取配置。

在工作区中，uv 会从工作区根目录开始搜索，忽略工作区成员中的任何配置。由于工作区作为一个单元被锁定，配置将在所有成员之间共享。

如果找到 `pyproject.toml` 文件，uv 会从 `[tool.uv]` 表中读取配置。例如，要设置持久化的索引 URL，可以在 `pyproject.toml` 中添加以下内容：

```toml title="pyproject.toml"
[[tool.uv.index]]
url = "https://test.pypi.org/simple"
default = true
```

（如果没有找到该表，`pyproject.toml` 文件将被忽略，uv 会继续在目录层级中搜索。）

uv 还会搜索 `uv.toml` 文件，它遵循相同的结构，但省略了 `[tool.uv]` 前缀。例如：

```toml title="uv.toml"
[[index]]
url = "https://test.pypi.org/simple"
default = true
```

!!! note

    `uv.toml` 文件优先于 `pyproject.toml` 文件，因此，如果目录中同时存在 `uv.toml` 和 `pyproject.toml` 文件，配置将从 `uv.toml` 读取，伴随的 `pyproject.toml` 中的 `[tool.uv]` 部分将被忽略。

uv 还会在 `~/.config/uv/uv.toml`（或 macOS 和 Linux 上的 `$XDG_CONFIG_HOME/uv/uv.toml`）或 Windows 上的 `%APPDATA%\uv\uv.toml` 发现用户级别的配置；在 `/etc/uv/uv.toml`（或 macOS 和 Linux 上的 `$XDG_CONFIG_DIRS/uv/uv.toml`）或 Windows 上的 `%SYSTEMDRIVE%\ProgramData\uv\uv.toml` 中发现系统级别的配置。

用户级和系统级配置必须使用 `uv.toml` 格式，而不是 `pyproject.toml` 格式，因为 `pyproject.toml` 是为定义 Python _项目_ 而设计的。

如果同时找到了项目级、用户级和系统级配置文件，这些设置将被合并，项目级配置优先于用户级配置，用户级配置优先于系统级配置。（如果发现多个系统级配置文件，例如 `/etc/uv/uv.toml` 和 `$XDG_CONFIG_DIRS/uv/uv.toml`，则只会使用第一个发现的文件，XDG 优先。）

例如，如果在项目级和用户级配置表中都出现了字符串、数字或布尔值，uv 将使用项目级的值，并忽略用户级的值。如果两个表中都有数组，数组会被合并，项目级设置会出现在合并后的数组前面。

通过环境变量提供的设置优先于持久化配置，通过命令行提供的设置优先于两者。

uv 接受一个 `--no-config` 命令行参数，当提供时，它会禁用任何持久化配置的发现。

uv 还接受一个 `--config-file` 命令行参数，接受一个 `uv.toml` 的路径，作为配置文件使用。当提供时，这个文件将替代_任何_ 已发现的配置文件（例如，用户级配置将被忽略）。

## 设置

请参阅 [设置参考](../reference/settings.md)，查看可用的设置列表。

## `.env`

`uv run` 可以加载来自 dotenv 文件（例如 `.env`、`.env.local`、`.env.development`）的环境变量，功能由 [`dotenvy`](https://github.com/allan2/dotenvy) crate 提供支持。

要从指定位置加载 `.env` 文件，可以设置 `UV_ENV_FILE` 环境变量，或将 `--env-file` 标志传递给 `uv run`。

例如，从当前工作目录加载 `.env` 文件中的环境变量：

```console
$ echo "MY_VAR='Hello, world!'" > .env
$ uv run --env-file .env -- python -c 'import os; print(os.getenv("MY_VAR"))'
Hello, world!
```

`--env-file` 标志可以多次提供，后续文件将覆盖先前文件中定义的值。要通过 `UV_ENV_FILE` 环境变量提供多个文件，可以用空格分隔路径（例如，`UV_ENV_FILE="/path/to/file1 /path/to/file2"`）。

要禁用 dotenv 加载（例如，覆盖 `UV_ENV_FILE` 或 `--env-file` 命令行参数），将 `UV_NO_ENV_FILE` 环境变量设置为 `1`，或将 `--no-env-file` 标志传递给 `uv run`。

如果同一个变量在环境中和 `.env` 文件中都有定义，则环境中的值将优先。

## 配置 pip 接口

为配置仅用于 `uv pip` 命令行接口，提供了一个专用的 [`[tool.uv.pip]`](../reference/settings.md#pip) 部分。此部分中的设置不会应用于 `uv` 命令的其他命名空间。然而，许多此部分中的设置在顶级命名空间中也有对应项，这些设置将应用于 `uv pip` 接口，除非在 `uv.pip` 部分中被覆盖。

`uv.pip` 设置旨在与 pip 接口保持高度一致，并单独声明，以保持兼容性，同时允许全局设置使用不同的设计（例如，`--no-build`）。

例如，在以下 `pyproject.toml` 中设置 `index-url`，只会影响 `uv pip` 子命令（例如 `uv pip install`），而不会影响 `uv sync`、`uv lock` 或 `uv run` 等命令：

```toml title="pyproject.toml"
[tool.uv.pip]
index-url = "https://test.pypi.org/simple"
```
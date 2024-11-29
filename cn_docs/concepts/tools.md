# 工具

工具是提供命令行接口的 Python 包。

!!! note

    请参阅 [工具指南](../guides/tools.md) 了解如何使用工具接口 —— 本文档讨论的是工具管理的详细信息。

## `uv tool` 接口

uv 提供了一个专门的接口用于与工具交互。工具可以通过 `uv tool run` 直接调用，而无需安装，这时它们的依赖项会在一个临时的虚拟环境中安装，该环境与当前项目隔离。

由于不安装工具而运行它们非常常见，因此提供了 `uvx` 别名来代替 `uv tool run` —— 这两个命令完全等效。为了简洁起见，文档中将主要使用 `uvx` 而不是 `uv tool run`。

工具也可以通过 `uv tool install` 进行安装，在这种情况下，它们的可执行文件将被添加到 [`PATH`](#the-path) 中 —— 虚拟环境仍然会被使用，但命令完成后它不会被删除。

## 执行与安装

在大多数情况下，使用 `uvx` 执行工具比安装工具更合适。安装工具在需要其他程序也能使用该工具时很有用，例如，如果某个你无法控制的脚本需要该工具，或者如果你处于 Docker 镜像中并希望将工具提供给用户。

## 工具环境

当使用 `uvx` 运行工具时，虚拟环境会存储在 uv 缓存目录中，并被视为一次性环境，即，如果你运行 `uv cache clean`，该环境将会被删除。该环境仅被缓存，以减少重复调用的开销。如果环境被删除，将会自动创建一个新的环境。

当使用 `uv tool install` 安装工具时，虚拟环境会被创建在 uv tools 目录中。除非工具被卸载，否则该环境不会被删除。如果环境被手动删除，工具将无法运行。

## 工具版本

除非明确请求特定版本，否则 `uv tool install` 会安装所请求工具的最新版本。`uvx` 会在第一次调用时使用所请求工具的最新可用版本。此后，`uvx` 将使用缓存中的工具版本，除非请求了不同版本、缓存被清除或缓存被刷新。

例如，要运行特定版本的 Ruff：

```console
$ uvx ruff@0.6.0 --version
ruff 0.6.0
```

随后的 `uvx` 调用将使用最新版本，而不是缓存的版本：

```console
$ uvx ruff --version
ruff 0.6.2
```

但是，如果 Ruff 发布了新版本，除非刷新缓存，否则不会使用新版本。

要请求 Ruff 的最新版本并刷新缓存，可以使用 `@latest` 后缀：

```console
$ uvx ruff@latest --version
0.6.2
```

一旦使用 `uv tool install` 安装了工具，`uvx` 默认将使用已安装的版本。

例如，在安装了旧版本的 Ruff 后：

```console
$ uv tool install ruff==0.5.0
```

`ruff` 和 `uvx ruff` 的版本是相同的：

```console
$ ruff --version
ruff 0.5.0
$ uvx ruff --version
ruff 0.5.0
```

然而，你可以通过明确请求最新版本来忽略已安装的版本，例如：

```console
$ uvx ruff@latest --version
0.6.2
```

或者，使用 `--isolated` 标志，这样可以避免刷新缓存，但会忽略已安装的版本：

```console
$ uvx --isolated ruff --version
0.6.2
```

`uv tool install` 还会遵循 `{package}@{version}` 和 `{package}@latest` 这样的规范，例如：

```console
$ uv tool install ruff@latest
$ uv tool install ruff@0.6.0
```

### 工具目录

默认情况下，uv 工具目录名为 `tools`，并位于 uv 应用状态目录中，例如 `~/.local/share/uv/tools`。该位置可以通过 `UV_TOOL_DIR` 环境变量进行自定义。

要显示工具安装目录的路径，可以使用以下命令：

```console
$ uv tool dir
```

工具环境会被放置在与工具包名称相同的目录中，例如 `.../tools/<name>`。

### 修改工具环境

工具环境**不**应直接修改。强烈建议不要手动使用 `pip` 操作修改工具环境。

可以通过 `uv tool upgrade` 升级工具环境，或者通过随后的 `uv tool install` 操作完全重新创建工具环境。

要升级工具环境中的所有包：

```console
$ uv tool upgrade black
```

要升级工具环境中单个包：

```console
$ uv tool upgrade black --upgrade-package click
```

要重新安装工具环境中的所有包：

```console
$ uv tool upgrade black --reinstall
```

要重新安装工具环境中的单个包：

```console
$ uv tool upgrade black --reinstall-package click
```

工具升级将遵循安装工具时提供的版本约束。例如，`uv tool install black >=23,<24` 后再运行 `uv tool upgrade black` 将会将 Black 升级到 `>=23,<24` 范围内的最新版本。

如果要替换版本约束，请使用 `uv tool install` 重新安装工具：

```console
$ uv tool install black>=24
```

类似地，工具升级将保留安装工具时提供的设置。例如，`uv tool install black --prerelease allow` 后再运行 `uv tool upgrade black` 将保留 `--prerelease allow` 设置。

工具升级会重新安装工具的可执行文件，即使它们没有发生变化。

### 包含额外依赖

在工具执行期间，可以包括额外的包：

```console
$ uvx --with <extra-package> <tool>
```

在工具安装期间，也可以包括额外的包：

```console
$ uv tool install --with <extra-package> <tool-package>
```

`--with` 选项可以多次提供，以包含多个额外的包。

`--with` 选项支持包规范，因此可以请求特定版本：

```console
$ uvx --with <extra-package>==<version> <tool-package>
```

如果请求的版本与工具包的要求冲突，包解析将失败，命令将报错。

## 工具可执行文件

工具可执行文件包括 Python 包提供的所有控制台入口点、脚本入口点和二进制脚本。工具可执行文件在 Unix 上会被符号链接到 `bin` 目录，在 Windows 上则会被复制。

### `bin` 目录

可执行文件会按照 XDG 标准安装到用户的 `bin` 目录，例如 `~/.local/bin`。与 uv 中的其他目录结构不同，XDG 标准适用于**所有平台**，特别是包括 Windows 和 macOS，因为在这些平台上没有明确的替代位置来放置可执行文件。安装目录由第一个可用的环境变量决定：

- `$UV_TOOL_BIN_DIR`
- `$XDG_BIN_HOME`
- `$XDG_DATA_HOME/../bin`
- `$HOME/.local/bin`

工具包的依赖项提供的可执行文件不会被安装。

### `PATH` 环境变量 {: #the-path}

`bin` 目录必须在 `PATH` 环境变量中，才能从 shell 中访问工具可执行文件。如果 `bin` 目录不在 `PATH` 中，则会显示警告。可以使用 `uv tool update-shell` 命令将 `bin` 目录添加到常见 shell 配置文件的 `PATH` 中。

### 覆盖可执行文件

工具的安装不会覆盖 `bin` 目录中不是由 uv 之前安装的可执行文件。例如，如果使用 `pipx` 安装了一个工具，则 `uv tool install` 会失败。可以使用 `--force` 标志来覆盖此行为。

## 与 `uv run` 的关系

调用 `uv tool run <name>`（或 `uvx <name>`）几乎等同于：

```console
$ uv run --no-project --with <name> -- <name>
```

但是，使用 uv 的工具接口时，有一些显著的区别：

- 不需要 `--with` 选项——所需的包是从命令名称推断出来的。
- 临时环境会被缓存到一个专用位置。
- 不需要 `--no-project` 标志——工具始终与项目隔离运行。
- 如果工具已经安装，`uv tool run` 会使用已安装的版本，但 `uv run` 则不会。

如果工具不应该与项目隔离，例如在运行 `pytest` 或 `mypy` 时，则应该使用 `uv run` 而不是 `uv tool run`。

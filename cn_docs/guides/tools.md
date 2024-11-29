# 使用工具

许多 Python 包提供可以作为工具使用的应用程序。uv 对轻松调用和安装工具提供了专门的支持。

## 运行工具

`uvx` 命令可以在不安装工具的情况下直接调用工具。

例如，要运行 `ruff`：

```console
$ uvx ruff
```

!!! note

    这与以下命令完全等效：

    ```console
    $ uv tool run ruff
    ```

    `uvx` 是作为便捷别名提供的。

在工具名称后面可以提供参数：

```console
$ uvx pycowsay hello from uv

  -------------
< hello from uv >
  -------------
   \   ^__^
    \  (oo)\_______
       (__)\       )\/\
           ||----w |
           ||     ||

```

在使用 `uvx` 时，工具将被安装到临时的、隔离的环境中。

!!! note

    如果你在 [_项目_](../concepts/projects/index.md) 中运行一个工具，并且该工具要求你的项目已经安装，例如使用 `pytest` 或 `mypy`，你将需要使用 [`uv run`](./projects.md#running-commands) 而不是 `uvx`。否则，工具将会在一个与项目隔离的虚拟环境中运行。

    如果你的项目采用平铺结构，例如没有使用 `src` 目录来存放模块，项目本身不需要安装，`uvx` 是可以的。在这种情况下，只有当你想将工具的版本固定在项目的依赖项中时，使用 `uv run` 才是有益的。

## 不同包名的命令

当调用 `uvx ruff` 时，uv 安装的是提供 `ruff` 命令的 `ruff` 包。然而，有时包名和命令名不同。

可以使用 `--from` 选项从特定包中调用命令，例如 `http` 由 `httpie` 提供：

```console
$ uvx --from httpie http
```

## 请求特定版本

要以特定版本运行工具，可以使用 `command@<version>` 语法：

```console
$ uvx ruff@0.3.0 check
```

要运行工具的最新版本，可以使用 `command@latest`：

```console
$ uvx ruff@latest check
```

`--from` 选项也可以用于指定包版本，如上所示：

```console
$ uvx --from 'ruff==0.3.0' ruff check
```

或者，限制在某个版本范围内：

```console
$ uvx --from 'ruff>0.2.0,<0.3.0' ruff check
```

请注意，`@` 语法只能用于指定精确版本，不能用于其他用途。

## 请求额外功能

`--from` 选项可以用来运行具有额外功能的工具：

```console
$ uvx --from 'mypy[faster-cache,reports]' mypy --xml-report mypy_report
```

这也可以与版本选择结合使用：

```console
$ uvx --from 'mypy[faster-cache,reports]==1.13.0' mypy --xml-report mypy_report
```

## 请求不同的源

`--from` 选项还可以用来从其他来源安装工具。

例如，从 Git 拉取：

```console
$ uvx --from git+https://github.com/httpie/cli httpie
```

你也可以从特定命名分支拉取最新的提交：

```console
$ uvx --from git+https://github.com/httpie/cli@master httpie
```

或者拉取特定的标签：

```console
$ uvx --from git+https://github.com/httpie/cli@3.2.4 httpie
```

或者拉取特定的提交：

```console
$ uvx --from git+https://github.com/httpie/cli@2843b87 httpie
```

## 带插件的命令

可以包括额外的依赖项，例如，在运行 `mkdocs` 时包括 `mkdocs-material`：

```console
$ uvx --with mkdocs-material mkdocs --help
```

## 安装工具

如果一个工具经常使用，最好将其安装到持久的环境中，并将其添加到 `PATH` 中，而不是反复调用 `uvx`。

!!! 提示

    `uvx` 是 `uv tool run` 的便捷别名。与工具交互的所有其他命令都需要使用完整的 `uv tool` 前缀。

要安装 `ruff`：

```console
$ uv tool install ruff
```

当工具被安装时，其可执行文件会被放置在 `PATH` 中的 `bin` 目录下，这样就可以在不使用 uv 的情况下直接运行该工具。如果它不在 `PATH` 中，将会显示警告，并且可以使用 `uv tool update-shell` 将其添加到 `PATH` 中。

安装 `ruff` 后，它应该可以被使用：

```console
$ ruff --version
```

与 `uv pip install` 不同，安装工具并不会使其模块在当前环境中可用。例如，以下命令将失败：

```console
$ python -c "import ruff"
```

这种隔离对于减少工具、脚本和项目之间的依赖冲突和交互是非常重要的。

与 `uvx` 不同，`uv tool install` 作用于一个 _包_，并会安装工具提供的所有可执行文件。

例如，以下命令将安装 `http`、`https` 和 `httpie` 可执行文件：

```console
$ uv tool install httpie
```

此外，也可以在不使用 `--from` 的情况下指定包版本：

```console
$ uv tool install 'httpie>0.1.0'
```

同样，也可以指定包源：

```console
$ uv tool install git+https://github.com/httpie/cli
```

与 `uvx` 一样，安装时也可以包括额外的包：

```console
$ uv tool install mkdocs --with mkdocs-material
```

## 升级工具

要升级一个工具，请使用 `uv tool upgrade`：

```console
$ uv tool upgrade ruff
```

工具升级将遵循安装工具时提供的版本约束。例如，`uv tool install ruff >=0.3,<0.4` 后跟 `uv tool upgrade ruff` 会将 Ruff 升级到范围 `>=0.3,<0.4` 内的最新版本。

如果要替换版本约束，可以使用 `uv tool install` 重新安装工具：

```console
$ uv tool install ruff>=0.4
```

如果要升级所有工具：

```console
$ uv tool upgrade --all
```

## 下一步

要了解更多关于使用 uv 管理工具的信息，请查看 [Tools 概念](../concepts/tools.md) 页面和 [命令参考](../reference/cli.md#uv-tool)。

或者，继续阅读以了解如何 [在项目中工作](./projects.md)。

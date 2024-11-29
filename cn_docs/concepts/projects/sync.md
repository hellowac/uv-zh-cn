# 锁定和同步

### 创建锁定文件

锁定文件会在使用项目环境的 uv 调用中自动创建和更新，即 `uv sync` 和 `uv run`。也可以使用 `uv lock` 显式地创建或更新锁定文件：

```console
$ uv lock
```

### 导出锁定文件

如果需要将 uv 与其他工具或工作流集成，可以通过 `uv export --format requirements-txt` 将 `uv.lock` 导出为 `requirements.txt` 格式。生成的 `requirements.txt` 文件可以通过 `uv pip install` 或其他工具（如 `pip`）安装。

通常，我们不建议同时使用 `uv.lock` 和 `requirements.txt` 文件。如果你发现自己需要导出 `uv.lock` 文件，可以考虑提一个问题，讨论你的使用场景。

### 检查锁定文件是否是最新的

为了避免在 `uv sync` 和 `uv run` 调用时更新锁定文件，可以使用 `--frozen` 标志。

为了避免在 `uv run` 调用时更新环境，可以使用 `--no-sync` 标志。

为了确保锁定文件与项目元数据匹配，可以使用 `--locked` 标志。如果锁定文件不是最新的，将会引发错误，而不是更新锁定文件。

### 升级锁定的包版本

默认情况下，uv 在运行 `uv sync` 和 `uv lock` 时会优先使用锁定的包版本。当项目的依赖约束排除了先前锁定的版本时，包版本才会发生变化。

要升级所有包：

```console
$ uv lock --upgrade
```

要将单个包升级到最新版本，同时保持其他所有包的锁定版本：

```console
$ uv lock --upgrade-package <package>
```

要将单个包升级到特定版本：

```console
$ uv lock --upgrade-package <package>==<version>
```

!!! note

    在所有情况下，升级都受到项目依赖约束的限制。例如，如果项目为某个包定义了上限版本，则升级不会超出该版本。
# 缓存

## 依赖缓存

uv 使用激进的缓存策略，避免在后续运行时重新下载（或重新构建）已经访问过的依赖项。

uv 的缓存语义根据依赖项的性质有所不同：

- **对于注册表依赖项**（例如从 PyPI 下载的依赖项），uv 会尊重 HTTP 缓存头。
- **对于直接 URL 依赖项**，uv 会尊重 HTTP 缓存头，并基于 URL 本身进行缓存。
- **对于 Git 依赖项**，uv 会基于完全解析的 Git 提交哈希进行缓存。因此，`uv pip compile` 在写入已解析的依赖集时，会将 Git 依赖项锁定到特定的提交哈希。
- **对于本地依赖项**，uv 会基于源文件（即本地 `.whl` 或 `.tar.gz` 文件）的最后修改时间进行缓存。对于目录，uv 会基于 `pyproject.toml`、`setup.py` 或 `setup.cfg` 文件的最后修改时间进行缓存。

如果遇到缓存问题，uv 提供了一些逃逸机制：

- 要强制 uv 重新验证所有依赖项的缓存数据，可以向任何命令传递 `--refresh`（例如，`uv sync --refresh` 或 `uv pip install --refresh ...`）。
- 要强制 uv 重新验证特定依赖项的缓存数据，可以向任何命令传递 `--refresh-package`（例如，`uv sync --refresh-package flask` 或 `uv pip install --refresh-package flask ...`）。
- 要强制 uv 忽略已安装的版本，可以向任何安装命令传递 `--reinstall`（例如，`uv sync --reinstall` 或 `uv pip install --reinstall ...`）。

## 动态元数据

默认情况下，uv 仅在本地目录依赖项（例如，可编辑的依赖项）所在的目录根目录中的 `pyproject.toml`、`setup.py` 或 `setup.cfg` 文件发生变化时才会重新构建和重新安装。这是一个启发式方法，在某些情况下，可能会导致比预期更少的重新安装。

要将其他信息纳入给定包的缓存键，可以在 `tool.uv.cache-keys` 下添加缓存键条目，这些条目可以包括文件路径和 Git 提交哈希。

例如，如果项目使用 [`setuptools-scm`](https://pypi.org/project/setuptools-scm/)，并且应该在提交哈希变化时重新构建，可以将以下内容添加到项目的 `pyproject.toml` 中：

```toml title="pyproject.toml"
[tool.uv]
cache-keys = [{ git = { commit = true } }]
```

如果您的动态元数据包含来自 Git 标签集的信息，可以将缓存键扩展为包括标签：

```toml title="pyproject.toml"
[tool.uv]
cache-keys = [{ git = { commit = true, tags = true } }]
```

类似地，如果项目从 `requirements.txt` 文件读取并填充其依赖项，可以将以下内容添加到项目的 `pyproject.toml` 中：

```toml title="pyproject.toml"
[tool.uv]
cache-keys = [{ file = "requirements.txt" }]
```

支持 glob 模式，遵循 [`glob`](https://docs.rs/glob/0.3.1/glob/struct.Pattern.html) crate 的语法。例如，要在项目目录或任何子目录中的 `.toml` 文件被修改时使缓存失效，可以使用以下内容：

```toml title="pyproject.toml"
[tool.uv]
cache-keys = [{ file = "**/*.toml" }]
```

!!! note

    使用 glob 模式可能会很昂贵，因为 uv 可能需要遍历文件系统来确定是否有文件发生了变化。这可能会导致遍历大型或深度嵌套的目录。

作为逃逸机制，如果项目使用的 `dynamic` 元数据未被 `tool.uv.cache-keys` 覆盖，您可以通过将项目添加到 `tool.uv.reinstall-package` 列表中，指示 uv _始终_ 重新构建和重新安装该项目：

```toml title="pyproject.toml"
[tool.uv]
reinstall-package = ["my-package"]
```

这将强制 uv 在每次运行时都重新构建和重新安装 `my-package`，无论该包的 `pyproject.toml`、`setup.py` 或 `setup.cfg` 文件是否发生变化。

## 缓存安全

即使对同一个虚拟环境运行多个 uv 命令，缓存也可以安全地并发操作。uv 的缓存设计为线程安全且仅追加，因此对多个并发的读取者和写入者都能保持稳定。uv 在安装时会对目标虚拟环境应用基于文件的锁，以避免进程间的并发修改。

需要注意的是，在其他 uv 命令正在运行时，_不_ 安全修改 uv 缓存（例如，`uv cache clean`），并且_永远_不应该直接修改缓存（例如，删除文件或目录）。

## 清理缓存

uv 提供了几种不同的机制来移除缓存中的条目：

- `uv cache clean` 会删除缓存目录中的 _所有_ 缓存条目，彻底清空缓存。
- `uv cache clean ruff` 会删除 `ruff` 包的所有缓存条目，适用于使特定包或有限集合的包缓存失效。
- `uv cache prune` 会删除所有 _未使用_ 的缓存条目。例如，缓存目录可能包含以前版本的 uv 创建的条目，这些条目不再需要并且可以安全移除。`uv cache prune` 可以定期运行，用于保持缓存目录的清洁。

## 持续集成中的缓存 {: #caching-in-continuous-integration}

在持续集成环境（如 GitHub Actions 或 GitLab CI）中缓存包安装工件以加速后续运行是常见做法。

默认情况下，uv 会缓存它从源代码构建的 wheel 文件以及直接下载的预构建 wheel 文件，以支持高性能的包安装。

然而，在持续集成环境中，持久化预构建的 wheel 文件可能并不理想。对于 uv 来说，通常情况下，_省略_ 预构建的 wheel 文件（而是在每次运行时从注册表重新下载）会更快。另一方面，从源代码构建的 wheel 文件则值得缓存，因为构建过程可能较为耗时，尤其是对于扩展模块。

为了支持这一缓存策略，uv 提供了一个 `uv cache prune --ci` 命令，它会从缓存中移除所有预构建的 wheel 文件和解压后的源分发包，但保留从源代码构建的 wheel 文件。我们建议在持续集成任务的末尾运行 `uv cache prune --ci`，以确保最大缓存效率。有关示例，请参见 [GitHub 集成指南](../guides/integration/github.md#caching)。

## 缓存目录

uv 根据以下顺序确定缓存目录：

1. 如果请求了 `--no-cache`，则使用临时缓存目录。
2. 使用 `--cache-dir`、`UV_CACHE_DIR` 或 [`tool.uv.cache-dir`](../reference/settings.md#cache-dir) 指定的特定缓存目录。
3. 系统适用的缓存目录，例如，在 Unix 系统上是 `$XDG_CACHE_HOME/uv` 或 `$HOME/.cache/uv`，在 Windows 上是 `%LOCALAPPDATA%\uv\cache`。

!!! note

    uv _始终_ 需要一个缓存目录。当请求 `--no-cache` 时，uv 仍会使用临时缓存来共享该次调用中的数据。

    在大多数情况下，应使用 `--refresh` 而不是 `--no-cache`，因为它会更新缓存以供后续操作使用，但不会读取缓存。

缓存目录位于与 uv 操作的 Python 环境相同的文件系统上对于性能非常重要。否则，uv 将无法将文件从缓存链接到环境中，而是需要回退到较慢的复制操作。

## 缓存版本控制 {: #cache-versioning}

uv 缓存由多个存储桶组成（例如，wheel 存储桶、源分发包存储桶、Git 仓库存储桶等）。每个存储桶都有版本控制，这样，如果某个版本的发布包含对缓存格式的破坏性更改，uv 将不会尝试从不兼容的缓存存储桶中读取或写入数据。

例如，uv 0.4.13 包含对核心元数据存储桶的破坏性更改。因此，存储桶的版本从 v12 升级到 v13。在一个缓存版本内，变更保证是向前和向后兼容的。

由于缓存格式的更改伴随着缓存版本的更改，因此多个版本的 uv 可以安全地读写同一个缓存目录。然而，如果在某一对 uv 发布版本之间缓存版本发生了变化，那么这些发布版本可能无法共享相同的底层缓存条目。

例如，uv 0.4.12 和 uv 0.4.13 可以安全地使用同一个共享缓存，尽管由于缓存版本的变化，缓存本身可能会在核心元数据存储桶中包含重复的条目。

# 项目结构与文件

## `pyproject.toml`

Python 项目的元数据定义在一个 [`pyproject.toml`](https://hellowac.github.io/pypug-zh-cn/guides/writing-pyproject-toml.html) 文件中。uv 需要这个文件来识别项目的根目录。

!!! tip

    可以使用 `uv init` 来创建一个新项目。有关详细信息，请参见 [创建项目](./init.md)。

最小的项目定义包括名称和版本：

```toml title="pyproject.toml"
[project]
name = "example"
version = "0.1.0"
```

额外的项目元数据和配置包括：

- [Python 版本要求](./config.md#python-version-requirement)
- [依赖项](./dependencies.md)
- [构建系统](./config.md#build-systems)
- [入口点（命令）](./config.md#entry-points)

## 项目环境 {: #the-project-environment}

在使用 uv 开发项目时，uv 会根据需要创建虚拟环境。虽然某些 uv 命令会创建临时环境（例如，`uv run --isolated`），uv 还会管理一个持久化的项目环境，并将其及其依赖项存储在项目根目录下的 `.venv` 文件夹中，和 `pyproject.toml` 文件同级。该环境存储在项目内部，便于编辑器查找 — 编辑器需要该环境来提供代码补全和类型提示。不建议将 `.venv` 目录纳入版本控制；它会通过一个内部 `.gitignore` 文件自动排除。

要在项目环境中运行命令，可以使用 `uv run`。另外，也可以像常规虚拟环境一样激活该项目环境。

当调用 `uv run` 时，如果项目环境尚不存在，它会创建该环境；如果已存在，则确保其是最新的。也可以通过 `uv sync` 显式创建项目环境。

不建议手动修改项目环境，例如通过 `uv pip install` 安装依赖。对于项目依赖项，请使用 `uv add` 将包添加到环境中。对于一次性的需求，可以使用 [`uvx`](../../guides/tools.md) 或 [`uv run --with`](./run.md#requesting-additional-dependencies)。

!!! tip

    如果您不希望 uv 管理项目环境，可以设置 [`managed = false`](../../reference/settings.md#managed) 来禁用项目的自动锁定和同步。例如：

    ```toml title="pyproject.toml"
    [tool.uv]
    managed = false
    ```

## lock 文件 {: #the-lockfile}

uv 会在 `pyproject.toml` 文件所在的目录创建一个 `uv.lock` 文件。

`uv.lock` 是一个 _通用的_ 或 _跨平台的_ 锁文件，记录了所有可能的 Python 标记（如操作系统、架构和 Python 版本）下将安装的包。

与 `pyproject.toml` 文件不同，后者用于指定项目的广泛要求，锁文件包含了安装在项目环境中的确切解析版本。这个文件应该被提交到版本控制中，以便在不同机器之间实现一致和可重现的安装。

锁文件确保项目中的开发者使用相同版本的包。此外，它还确保在将项目作为应用程序部署时，确切使用的包版本集合是已知的。

在使用项目环境的 uv 命令（即 `uv sync` 和 `uv run`）执行时，锁文件会被创建和更新。也可以使用 `uv lock` 显式更新锁文件。

`uv.lock` 是一个可读的 TOML 文件，但由 uv 管理，不应手动编辑。目前没有 Python 标准的锁文件格式，因此该文件的格式是特定于 uv 的，不能被其他工具使用。
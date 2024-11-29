# Python 环境

每个 Python 安装都有一个在使用 Python 时处于活动状态的环境。可以将包安装到该环境中，以便从 Python 脚本中使用其模块。通常，最佳实践是不修改 Python 安装的环境。对于随操作系统一起安装的 Python，尤其如此，因为它们通常会管理自己的包。虚拟环境是隔离包的轻量方式，与 Python 安装的环境分离。与 `pip` 不同，uv 默认要求使用虚拟环境。

## 创建虚拟环境

uv 支持创建虚拟环境，例如，要在 `.venv` 创建虚拟环境，可以运行：

```console
$ uv venv
```

可以指定特定的名称或路径，例如，要在 `my-name` 创建虚拟环境：

```console
$ uv venv my-name
```

还可以指定 Python 版本，例如，要创建一个使用 Python 3.11 的虚拟环境：

```console
$ uv venv --python 3.11
```

请注意，这要求所请求的 Python 版本已安装在系统上。如果未安装，uv 会为您下载该版本的 Python。有关详细信息，请参见 [Python 版本](../concepts/python-versions.md) 文档。

## 使用虚拟环境

当使用默认的虚拟环境名称时，uv 会自动查找并在后续调用中使用该虚拟环境。

```console
$ uv venv

$ # 在新虚拟环境中安装包
$ uv pip install ruff
```

可以“激活”虚拟环境，以使其包可用：

=== "macOS 和 Linux"

    ```console
    $ source .venv/bin/activate
    ```

=== "Windows"

    ```console
    $ .venv\Scripts\activate
    ```

## 使用任意 Python 环境

由于 uv 不依赖于 Python，它可以安装到除其自身虚拟环境之外的其他虚拟环境中。例如，设置 `VIRTUAL_ENV=/path/to/venv` 将导致 uv 安装到 `/path/to/venv`，无论 uv 安装在哪里。请注意，如果 `VIRTUAL_ENV` 设置为 **非** [PEP 405 合规](https://peps.python.org/pep-0405/#specification) 的虚拟环境目录，它将被忽略。

uv 还可以安装到任意环境，甚至是非虚拟环境中，只需在 `uv pip sync` 或 `uv pip install` 命令中提供 `--python` 参数。例如，`uv pip install --python /path/to/python` 将安装到与 `/path/to/python` 解释器相关联的环境中。

为了方便，`uv pip install --system` 会安装到系统 Python 环境中。使用 `--system` 大致等同于 `uv pip install --python $(which python)`，但请注意，链接到虚拟环境的可执行文件将被跳过。尽管我们通常推荐在依赖管理中使用虚拟环境，但 `--system` 在持续集成和容器化环境中是合适的。

`--system` 标志还可用于选择性修改系统环境。例如，可以使用 `--python` 参数请求特定的 Python 版本（例如 `--python 3.12`），然后 uv 会搜索满足该请求的解释器。如果 uv 找到了系统解释器（例如 `/usr/lib/python3.12`），则需要使用 `--system` 标志来允许修改该非虚拟 Python 环境。如果没有 `--system` 标志，uv 会忽略任何不在虚拟环境中的解释器。反之，如果提供了 `--system` 标志，uv 会忽略所有在虚拟环境中的解释器。

跨平台和不同发行版安装到系统 Python 中是非常困难的。uv 支持常见的用例，但并不适用于所有情况。例如，Debian 系统上 Python 3.10 之前的版本由于 [发行版对 `distutils` 的修补（而非 `sysconfig`）](https://ffy00.github.io/blog/02-python-debian-and-the-install-locations/) 不支持直接安装到系统 Python 中。虽然我们始终推荐使用虚拟环境，但在这些非标准环境中，uv 认为虚拟环境是必需的。

如果 uv 已安装在某个 Python 环境中（例如通过 `pip`），它仍然可以用来修改其他环境。但是，当使用 `python -m uv` 调用时，uv 将默认使用父解释器的环境。通过 Python 启动 uv 会增加启动开销，因此不推荐用于一般使用。

uv 本身不依赖于 Python，但它需要定位一个 Python 环境，以便 (1) 安装依赖到该环境中，(2) 构建源分发包。

## Python 环境的发现

当运行会改变环境的命令（如 `uv pip sync` 或 `uv pip install`）时，uv 会按以下顺序搜索虚拟环境：

- 基于 `VIRTUAL_ENV` 环境变量的已激活虚拟环境。
- 基于 `CONDA_PREFIX` 环境变量的已激活 Conda 环境。
- 当前目录中的 `.venv` 虚拟环境，或者最近的父目录中的虚拟环境。

如果未找到虚拟环境，uv 会提示用户在当前目录中通过 `uv venv` 创建一个。

如果包含 `--system` 标志，uv 会跳过虚拟环境，直接搜索已安装的 Python 版本。同样，在运行不会改变环境的命令（如 `uv pip compile`）时，uv **不要求** 虚拟环境，但仍然需要 Python 解释器。有关已安装 Python 版本的发现，请参见 [Python 发现](../concepts/python-versions.md#discovery-of-python-versions) 文档。

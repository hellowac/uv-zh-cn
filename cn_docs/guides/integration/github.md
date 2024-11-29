# 在 GitHub Actions 中使用 uv

## 安装

为了在 GitHub Actions 中使用 uv，推荐使用官方的 [`astral-sh/setup-uv`](https://github.com/astral-sh/setup-uv) 操作，该操作会安装 uv、将其添加到 PATH（可选地）持久化缓存等，支持所有 uv 支持的平台。

要安装最新版本的 uv：

```yaml title="example.yml" hl_lines="11-12"
name: Example

jobs:
  uv-example:
    name: python
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v4
```

最佳实践是将 uv 固定到特定版本，例如：

```yaml title="example.yml" hl_lines="14 15"
name: Example

jobs:
  uv-example:
    name: python
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v4
        with:
          # 安装特定版本的 uv。
          version: "0.5.5"
```

## 设置 Python

可以使用 `python install` 命令安装 Python：

```yaml title="example.yml" hl_lines="14 15"
name: Example

jobs:
  uv-example:
    name: python
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v4

      - name: Set up Python
        run: uv python install
```

这将尊重项目中指定的 Python 版本。

或者，在使用矩阵时，如下所示：

```yaml title="example.yml"
strategy:
  matrix:
    python-version:
      - "3.10"
      - "3.11"
      - "3.12"
```

将版本传递给 `python install` 命令：

```yaml title="example.yml" hl_lines="14 15"
name: Example

jobs:
  uv-example:
    name: python
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v4

      - name: Set up Python ${{ matrix.python-version }}
        run: uv python install ${{ matrix.python-version }}
```

另外，也可以使用 GitHub 官方的 `setup-python` 操作。这可能会更快，因为 GitHub 会缓存 Python 版本和运行器。

设置 [`python-version-file`](https://github.com/actions/setup-python/blob/main/docs/advanced-usage.md#using-the-python-version-file-input) 选项来使用项目中指定的版本：

```yaml title="example.yml" hl_lines="14 15 16 17"
name: Example

jobs:
  uv-example:
    name: python
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v4

      - name: "Set up Python"
        uses: actions/setup-python@v5
        with:
          python-version-file: ".python-version"
```

或者，指定 `pyproject.toml` 文件来忽略固定版本，使用与项目的 `requires-python` 约束兼容的最新版本：

```yaml title="example.yml" hl_lines="17"
name: Example

jobs:
  uv-example:
    name: python
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v4

      - name: "Set up Python"
        uses: actions/setup-python@v5
        with:
          python-version-file: "pyproject.toml"
```

## 同步和运行

安装完 uv 和 Python 后，可以使用 `uv sync` 安装项目，并通过 `uv run` 在环境中运行命令：

```yaml title="example.yml" hl_lines="17-22"
name: Example

jobs:
  uv-example:
    name: python
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v4

      - name: Set up Python
        run: uv python install

      - name: Install the project
        run: uv sync --all-extras --dev

      - name: Run tests
        # 例如，使用 `pytest`
        run: uv run pytest tests
```

!!! tip

    [`UV_PROJECT_ENVIRONMENT` 设置](../../concepts/projects/config.md#project-environment-path) 可用于将安装路径设置为系统 Python 环境，而不是创建虚拟环境。

## 缓存 {: #caching}

在 CI 流程中，持久化 uv 的缓存可能会提升工作流的执行速度。

[`astral-sh/setup-uv`](https://github.com/astral-sh/setup-uv) 操作内置支持缓存持久化：

```yaml title="example.yml"
- name: Enable caching
  uses: astral-sh/setup-uv@v4
  with:
    enable-cache: true
```

您可以配置该操作使用自定义的缓存目录：

```yaml title="example.yml"
- name: Define a custom uv cache path
  uses: astral-sh/setup-uv@v4
  with:
    enable-cache: true
    cache-local-path: "/path/to/cache"
```

或者，当锁文件（lockfile）发生变化时使缓存失效：

```yaml title="example.yml"
- name: Define a cache dependency glob
  uses: astral-sh/setup-uv@v4
  with:
    enable-cache: true
    cache-dependency-glob: "uv.lock"
```

或者，当任何要求文件发生变化时使缓存失效：

```yaml title="example.yml"
- name: Define a cache dependency glob
  uses: astral-sh/setup-uv@v4
  with:
    enable-cache: true
    cache-dependency-glob: "requirements**.txt"
```

请注意，`astral-sh/setup-uv` 将自动为每个主机架构和平台使用不同的缓存键。

另外，您也可以通过 `actions/cache` 操作手动管理缓存：

```yaml title="example.yml"
jobs:
  install_job:
    env:
      # 配置 uv 缓存的常量位置
      UV_CACHE_DIR: /tmp/.uv-cache

    steps:
      # ... 设置 Python 和 uv ...

      - name: Restore uv cache
        uses: actions/cache@v4
        with:
          path: /tmp/.uv-cache
          key: uv-${{ runner.os }}-${{ hashFiles('uv.lock') }}
          restore-keys: |
            uv-${{ runner.os }}-${{ hashFiles('uv.lock') }}
            uv-${{ runner.os }}

      # ... 安装包，运行测试等 ...

      - name: Minimize uv cache
        run: uv cache prune --ci
```

`uv cache prune --ci` 命令用于减小缓存的大小，并且针对 CI 环境进行了优化。其对性能的影响取决于所安装的包。

!!! tip

    如果使用 `uv pip`，在缓存键中使用 `requirements.txt` 而非 `uv.lock`。

!!! note

    [post-job-hook](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/running-scripts-before-or-after-a-job)

    在使用非临时的自托管运行器时，默认缓存目录可能会无限增长。在这种情况下，可能不适合在作业之间共享缓存。可以将缓存移到 GitHub 工作区内，并在作业结束后通过 [Post Job Hook](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/running-scripts-before-or-after-a-job) 来删除缓存。

    ```yaml
    install_job:
      env:
        # 配置 uv 缓存的相对位置
        UV_CACHE_DIR: ${{ github.workspace }}/.cache/uv
    ```

    使用 post job hook 时，需要在自托管运行器上设置环境变量 `ACTIONS_RUNNER_HOOK_JOB_STARTED`，并指向一个清理脚本的路径，如下所示：

    ```sh title="clean-uv-cache.sh"
    #!/usr/bin/env sh
    uv cache clean
    ```

## 使用 `uv pip`

如果使用 `uv pip` 接口而不是 uv 项目接口，默认情况下，uv 需要使用虚拟环境。若要允许将包安装到系统环境中，请在所有 `uv` 调用中使用 `--system` 标志，或设置 `UV_SYSTEM_PYTHON` 变量。

`UV_SYSTEM_PYTHON` 变量可以在不同的作用域中定义。

要为整个工作流启用，请在顶层定义：

```yaml title="example.yml"
env:
  UV_SYSTEM_PYTHON: 1

jobs: ...
```

或为工作流中的特定作业启用：

```yaml title="example.yml"
jobs:
  install_job:
    env:
      UV_SYSTEM_PYTHON: 1
    ...
```

或为作业中的特定步骤启用：

```yaml title="example.yml"
steps:
  - name: Install requirements
    run: uv pip install -r requirements.txt
    env:
      UV_SYSTEM_PYTHON: 1
```

如果希望再次禁用，可以在任何 `uv` 调用中使用 `--no-system` 标志。
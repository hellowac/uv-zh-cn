# 在 PyTorch 中使用 uv

[PyTorch](https://pytorch.org/) 生态系统是深度学习研究和开发的热门选择。你可以使用 uv 来管理 PyTorch 项目和依赖，支持不同的 Python 版本和环境，甚至控制加速器的选择（例如，仅使用 CPU 或 CUDA）。

!!! note

    本指南中提到的某些功能需要 uv 版本 0.5.3 或更高版本。如果你正在使用较旧版本的 uv，建议在配置 PyTorch 之前升级 uv。

## 安装 PyTorch

从打包角度看，PyTorch 有一些不常见的特点：

- 许多 PyTorch 的 wheel 文件托管在专用的索引中，而不是 Python 包索引（PyPI）。因此，安装 PyTorch 通常需要配置项目以使用 PyTorch 的索引。
- PyTorch 为每种加速器（如 CPU-only、CUDA）发布不同的构建。由于没有标准化机制来指定这些加速器，因此 PyTorch 会在本地版本说明符中进行编码。因此，PyTorch 的版本通常会是 `2.5.1+cpu`、`2.5.1+cu121` 等。
- 不同加速器的构建发布在不同的索引中。例如，`+cpu` 的构建发布在 https://download.pytorch.org/whl/cpu，而 `+cu121` 的构建发布在 https://download.pytorch.org/whl/cu121。

因此，根据你需要支持的平台和加速器的选择，所需的打包配置会有所不同。

首先，考虑以下默认配置，这可以通过运行 `uv init --python 3.12`，然后执行 `uv add torch torchvision` 来生成。

在这种情况下，PyTorch 将从 PyPI 安装，PyPI 托管了 Windows 和 macOS 上的 CPU-only wheels，以及针对 Linux（CUDA 12.4）的 GPU 加速 wheels：

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]
```

!!! tip "支持的 Python 版本"

    截至目前，PyTorch 尚未发布 Python 3.13 的 wheel 文件。因此，使用 `requires-python = ">=3.13"` 的项目可能无法解决依赖问题。请查看 [兼容性矩阵](https://github.com/pytorch/pytorch/blob/main/RELEASE.md#release-compatibility-matrix)。

这是一个有效的配置，适用于希望在 Windows 和 macOS 上使用 CPU 构建，并在 Linux 上使用支持 CUDA 的构建的项目。如果你需要支持不同的平台或加速器，你将需要相应地配置项目。

## 使用 PyTorch 索引

在某些情况下，你可能希望在所有平台上使用特定的 PyTorch 变体。例如，你可能希望在 Linux 上也使用 CPU-only 构建。

在这种情况下，第一步是将相关的 PyTorch 索引添加到 `pyproject.toml` 中：

=== "CPU-only"

    ```toml
    [[tool.uv.index]]
    name = "pytorch-cpu"
    url = "https://download.pytorch.org/whl/cpu"
    explicit = true
    ```

=== "CUDA 11.8"

    ```toml
    [[tool.uv.index]]
    name = "pytorch-cu118"
    url = "https://download.pytorch.org/whl/cu118"
    explicit = true
    ```

=== "CUDA 12.1"

    ```toml
    [[tool.uv.index]]
    name = "pytorch-cu121"
    url = "https://download.pytorch.org/whl/cu121"
    explicit = true
    ```

=== "CUDA 12.4"

    ```toml
    [[tool.uv.index]]
    name = "pytorch-cu124"
    url = "https://download.pytorch.org/whl/cu124"
    explicit = true
    ```

=== "ROCm6"

    ```toml
    [[tool.uv.index]]
    name = "pytorch-rocm"
    url = "https://download.pytorch.org/whl/rocm6.2"
    explicit = true
    ```

我们推荐使用 `explicit = true`，以确保索引 _仅_ 用于 `torch`、`torchvision` 和其他与 PyTorch 相关的包，而不是像 `jinja2` 这样的通用依赖，这些依赖应继续从默认索引（PyPI）获取。

接下来，更新 `pyproject.toml` 文件，将 `torch` 和 `torchvision` 指向所需的索引：

=== "CPU-only"

    由于 macOS 上没有发布 CPU-only 构建，macOS 构建总是被视为 CPU-only。因此，我们根据 `platform_system` 来指示 uv 在解析 macOS 时忽略 PyTorch 索引。

    ```toml
    [tool.uv.sources]
    torch = [
      { index = "pytorch-cpu", marker = "platform_system != 'Darwin'" },
    ]
    torchvision = [
      { index = "pytorch-cpu", marker = "platform_system != 'Darwin'" },
    ]
    ```

=== "CUDA 11.8"

    由于 PyTorch 没有为 macOS 发布 CUDA 构建，我们根据 `platform_system` 来指示 uv 在解析 macOS 时忽略 PyTorch 索引。

    ```toml
    [tool.uv.sources]
    torch = [
      { index = "pytorch-cu118", marker = "platform_system != 'Darwin'" },
    ]
    torchvision = [
      { index = "pytorch-cu118", marker = "platform_system != 'Darwin'" },
    ]
    ```

=== "CUDA 12.1"

    由于 PyTorch 没有为 macOS 发布 CUDA 构建，我们根据 `platform_system` 来指示 uv 在解析 macOS 时忽略 PyTorch 索引。

    ```toml
    [tool.uv.sources]
    torch = [
      { index = "pytorch-cu121", marker = "platform_system != 'Darwin'" },
    ]
    torchvision = [
      { index = "pytorch-cu121", marker = "platform_system != 'Darwin'" },
    ]
    ```

=== "CUDA 12.4"

    由于 PyTorch 没有为 macOS 发布 CUDA 构建，我们根据 `platform_system` 来指示 uv 在解析 macOS 时忽略 PyTorch 索引。

    ```toml
    [tool.uv.sources]
    torch = [
      { index = "pytorch-cu124", marker = "platform_system != 'Darwin'" },
    ]
    torchvision = [
      { index = "pytorch-cu124", marker = "platform_system != 'Darwin'" },
    ]
    ```

=== "ROCm6"

    由于 PyTorch 没有为 macOS 或 Windows 发布 ROCm6 构建，我们根据 `platform_system` 来指示 uv 在解析这些平台时忽略 PyTorch 索引。

    ```toml
    [tool.uv.sources]
    torch = [
      { index = "pytorch-rocm", marker = "platform_system == 'Linux'" },
    ]
    torchvision = [
      { index = "pytorch-rocm", marker = "platform_system == 'Linux'" },
    ]
    ```

作为一个完整的示例，以下项目将使用 PyTorch 的 CPU-only 构建在所有平台上：

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]

[tool.uv.sources]
torch = [
    { index = "pytorch-cpu", marker = "platform_system != 'Darwin'" },
]
torchvision = [
    { index = "pytorch-cpu", marker = "platform_system != 'Darwin'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

## 使用环境标记配置加速器

在某些情况下，你可能希望在一个环境（例如，macOS 和 Windows）中使用 CPU-only 构建，而在另一个环境（例如，Linux）中使用支持 CUDA 的构建。

通过 `tool.uv.sources`，你可以使用环境标记来指定每个平台所需的索引。例如，以下配置将在 Windows（以及通过回退到 PyPI 在 macOS 上）上使用 PyTorch 的 CPU-only 构建，在 Linux 上使用支持 CUDA 的构建：

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", marker = "platform_system == 'Windows'" },
  { index = "pytorch-cu124", marker = "platform_system == 'Linux'" },
]
torchvision = [
  { index = "pytorch-cpu", marker = "platform_system == 'Windows'" },
  { index = "pytorch-cu124", marker = "platform_system == 'Linux'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

## 使用可选依赖配置加速器

在某些情况下，你可能希望在某些场景下使用 CPU-only 构建，而在其他场景下使用支持 CUDA 的构建，并通过用户提供的额外选项切换选择（例如，`uv sync --extra cpu` 与 `uv sync --extra cu124`）。

通过 `tool.uv.sources`，你可以使用额外标记来指定每个启用的额外选项所需的索引。例如，以下配置会在使用 `uv sync --extra cpu` 时使用 PyTorch 的 CPU-only 构建，在使用 `uv sync --extra cu124` 时使用支持 CUDA 的构建：

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = []

[project.optional-dependencies]
cpu = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]
cu124 = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu124" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
  { index = "pytorch-cu124", extra = "cu124" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

!!! note

    由于 macOS 上没有 GPU 加速的构建，因此上面的配置将继续使用 CPU-only 构建，且通过 `"platform_system != 'Darwin'"` 标记在 macOS 上工作，无论提供了什么额外选项。

## `uv pip` 接口

虽然上述示例集中在 uv 的项目接口（`uv lock`、`uv sync`、`uv run` 等），但 PyTorch 也可以通过 `uv pip` 接口安装。

PyTorch 本身提供了一个 [专用接口](https://pytorch.org/get-started/locally/) 来确定适用于给定目标配置的 pip 命令。例如，您可以使用以下命令在 Linux 上安装稳定的 CPU-only PyTorch：

```shell
$ pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
```

要使用相同的工作流与 uv 配合，只需将 `pip3` 替换为 `uv pip`：

```shell
$ uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
```
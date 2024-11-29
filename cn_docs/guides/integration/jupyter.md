# 在 Jupyter 中使用 uv

[Jupyter](https://jupyter.org/) notebook 是一种流行的交互式计算、数据分析和可视化工具。你可以通过几种不同的方式在 uv 中使用 Jupyter，无论是与项目交互，还是作为独立工具使用。

## 在项目中使用 Jupyter

如果你在一个 [项目](../../concepts/projects/index.md) 中工作，可以通过以下命令启动一个 Jupyter 服务器，并访问该项目的虚拟环境：

```console
$ uv run --with jupyter jupyter lab
```

默认情况下，`jupyter lab` 会在 [http://localhost:8888/lab](http://localhost:8888/lab) 启动服务器。

在 notebook 中，你可以像在项目中的其他文件一样导入项目的模块。例如，如果你的项目依赖于 `requests`，可以使用 `import requests` 来从项目的虚拟环境中导入 `requests`。

如果你只需要只读访问项目的虚拟环境，那么直接这样做即可。然而，如果你需要在 notebook 中安装额外的包，还有一些额外的细节需要注意。

### 创建内核

如果你需要在 notebook 中安装包，建议为你的项目创建一个专用的内核。内核使得 Jupyter 服务器在一个环境中运行，而每个 notebook 在自己的独立环境中运行。

在 uv 中，我们可以为项目创建一个内核，同时将 Jupyter 本身安装在一个隔离的环境中，就像 `uv run --with jupyter jupyter lab`。为项目创建内核可以确保 notebook 正确连接到虚拟环境，并且从 notebook 中安装的任何包都会安装到项目的虚拟环境中。

要创建内核，你需要将 `ipykernel` 安装为开发依赖：

```console
$ uv add --dev ipykernel
```

然后，你可以使用以下命令为 `project` 创建内核：

```console
$ uv run ipython kernel install --user --name=project
```

接下来，启动服务器：

```console
$ uv run --with jupyter jupyter lab
```

在创建 notebook 时，从下拉菜单中选择 `project` 内核。然后使用 `!uv add pydantic` 将 `pydantic` 添加到项目的依赖中，或者使用 `!uv pip install pydantic` 将 `pydantic` 安装到项目的虚拟环境中，而不会将更改保留到项目的 `pyproject.toml` 或 `uv.lock` 文件中。无论哪种命令，`import pydantic` 都将在 notebook 中正常工作。

### 在没有内核的情况下安装包

如果你不想创建内核，仍然可以在 notebook 中安装包。然而，需要考虑一些注意事项。

虽然 `uv run --with jupyter` 在隔离的环境中运行，但在 notebook 内部，`!uv add` 和相关命令会修改 _项目_ 的环境，即使没有内核。

例如，在 notebook 中运行 `!uv add pydantic` 会将 `pydantic` 添加到项目的依赖项和虚拟环境中，确保 `import pydantic` 可以立即工作，而不需要进一步的配置或重新启动服务器。

然而，由于 Jupyter 服务器是 "活动" 环境，`!uv pip install` 将把包安装到 _Jupyter_ 的环境中，而不是项目环境。这些依赖会在 Jupyter 服务器的生命周期内持续存在，但在后续的 `jupyter` 调用中可能会消失。

如果你正在使用依赖 pip 的 notebook（例如通过 `%pip` 魔法命令），可以通过在启动 Jupyter 服务器之前运行 `uv venv --seed` 来将 pip 包含在项目的虚拟环境中。例如，执行以下命令：

```console
$ uv venv --seed
$ uv run --with jupyter jupyter lab
```

此后，在 notebook 中的 `%pip install` 调用将把包安装到项目的虚拟环境中。然而，这些修改 _不会_ 反映到项目的 `pyproject.toml` 或 `uv.lock` 文件中。

## 将 Jupyter 作为独立工具使用

如果你需要随时访问 notebook（即交互式运行 Python 代码片段），可以使用 `uv tool run jupyter lab` 随时启动一个 Jupyter 服务器。这将在一个隔离的环境中运行 Jupyter 服务器。

## 在非项目环境中使用 Jupyter

如果你需要在一个不与 [项目](../../concepts/projects/index.md) 关联的虚拟环境中运行 Jupyter（例如，没有 `pyproject.toml` 或 `uv.lock`），可以直接将 Jupyter 添加到该环境中。例如：

```console
$ uv venv --seed
$ uv pip install pydantic
$ uv pip install jupyterlab
$ .venv/bin/jupyter lab
```

从这里开始，`import pydantic` 将在 notebook 中正常工作，你可以通过 `!uv pip install` 或甚至 `!pip install` 安装其他包。

## 从 VS Code 使用 Jupyter

你也可以通过编辑器（如 VS Code）来使用 Jupyter notebook。为了将一个 uv 管理的项目连接到 VS Code 中的 Jupyter notebook，推荐为该项目创建一个内核，如下所示：

```console
# 创建一个项目
$ uv init project
# 进入项目目录
$ cd project
# 将 ipykernel 作为开发依赖添加
$ uv add --dev ipykernel
# 在 VS Code 中打开项目
$ code .
```

一旦项目目录在 VS Code 中打开，你可以通过命令面板选择 “Create: New Jupyter Notebook” 来创建一个新的 Jupyter notebook。当系统提示选择内核时，选择 “Python Environments” 并选择你之前创建的虚拟环境（例如，`.venv/bin/python`）。

!!! note

    VS Code 要求项目环境中必须安装 `ipykernel`。如果你不希望将 `ipykernel` 添加为开发依赖项，可以通过 `uv pip install ipykernel` 直接将其安装到项目环境中。

如果你需要在 notebook 中操作项目的环境，可能需要将 `uv` 作为明确的开发依赖项添加：

```console
$ uv add --dev uv
```

然后，你可以使用 `!uv add pydantic` 将 `pydantic` 添加到项目的依赖项中，或者使用 `!uv pip install pydantic` 将 `pydantic` 安装到项目的虚拟环境中，而不会更新项目的 `pyproject.toml` 或 `uv.lock` 文件。

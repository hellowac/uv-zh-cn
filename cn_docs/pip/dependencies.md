# 声明依赖

最好在静态文件中声明依赖，而不是通过临时安装来修改环境。一旦依赖被定义，可以通过[锁定](./compile.md)来创建一个一致的、可复现的环境。

## 使用 `pyproject.toml`

`pyproject.toml` 文件是 Python 项目配置的标准文件。

在 `pyproject.toml` 文件中声明项目的依赖：

```toml title="pyproject.toml"
[project]
dependencies = [
  "httpx",
  "ruff>=0.3.0"
]
```

在 `pyproject.toml` 文件中声明可选依赖：

```toml title="pyproject.toml"
[project.optional-dependencies]
cli = [
  "rich",
  "click",
]
```

每个键定义了一个 "extra"，可以使用 `--extra` 和 `--all-extras` 标志或 `package[<extra>]` 语法来安装。有关更多详情，请参阅[安装包](./packages.md#installing-packages-from-files)文档。

有关如何开始使用 `pyproject.toml` 的更多信息，请参阅官方[`pyproject.toml`指南](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/)。

## 使用 `requirements.in`

另外，使用轻量级的 `requirements.txt` 格式来声明项目依赖也很常见。每个依赖项单独列在一行。通常，这个文件被命名为 `requirements.in`，以区别于用于锁定依赖的 `requirements.txt`。

在 `requirements.in` 文件中声明依赖：

```python title="requirements.in"
httpx
ruff>=0.3.0
```

此格式不支持可选依赖分组。
# 在项目中运行命令

在进行项目开发时，项目会安装到虚拟环境 `.venv` 中。默认情况下，该环境与当前的 shell 是隔离的，因此任何需要项目的命令，例如 `python -c "import example"`，都会失败。相反，使用 `uv run` 来在项目环境中运行命令：

```console
$ uv run python -c "import example"
```

使用 `run` 时，uv 会在运行给定命令之前确保项目环境是最新的。

给定的命令可以由项目环境提供，也可以在项目环境外部存在，例如：

```console
$ # 假设项目提供了 `example-cli`
$ uv run example-cli foo

$ # 运行一个需要项目可用的 `bash` 脚本
$ uv run bash scripts/foo.sh
```

## 请求额外的依赖 {: #requesting-additional-dependencies}

可以为每次调用请求额外的依赖或不同版本的依赖。

使用 `--with` 选项来为当前调用包含一个依赖，例如，请求不同版本的 `httpx`：

```console
$ uv run --with httpx==0.26.0 python -c "import httpx; print(httpx.__version__)"
0.26.0
$ uv run --with httpx==0.25.0 python -c "import httpx; print(httpx.__version__)"
0.25.0
```

请求的版本会被优先使用，而不管项目的需求是什么。例如，即使项目要求 `httpx==0.24.0`，上面的输出也会保持不变。

## 运行脚本

声明了内联元数据的脚本会在与项目隔离的环境中自动执行。有关更多细节，请参见 [脚本指南](../../guides/scripts.md#declaring-script-dependencies)。

例如，给定一个脚本：

```python title="example.py"
# /// script
# dependencies = [
#   "httpx",
# ]
# ///

import httpx

resp = httpx.get("https://peps.python.org/api/peps.json")
data = resp.json()
print([(k, v["title"]) for k, v in data.items()][:10])
```

调用 `uv run example.py` 将会在与项目隔离的环境中运行，并且只会使用给定的依赖项。
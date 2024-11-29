# 在 pre-commit 中使用 uv

官方提供了一个 [pre-commit 钩子](https://github.com/astral-sh/uv-pre-commit)。

要通过 pre-commit 编译要求，可以将以下内容添加到 `.pre-commit-config.yaml` 文件中：

```yaml title=".pre-commit-config.yaml"
- repo: https://github.com/astral-sh/uv-pre-commit
  # uv 版本
  rev: 0.5.5
  hooks:
    # 编译 requirements
    - id: pip-compile
      args: [requirements.in, -o, requirements.txt]
```

要编译其他文件，修改 `args` 和 `files`：

```yaml title=".pre-commit-config.yaml"
- repo: https://github.com/astral-sh/uv-pre-commit
  # uv 版本
  rev: 0.5.5
  hooks:
    # 编译 requirements
    - id: pip-compile
      args: [requirements-dev.in, -o, requirements-dev.txt]
      files: ^requirements-dev\.(in|txt)$
```

要同时对多个文件运行钩子：

```yaml title=".pre-commit-config.yaml"
- repo: https://github.com/astral-sh/uv-pre-commit
  # uv 版本
  rev: 0.5.5
  hooks:
    # 编译 requirements
    - id: pip-compile
      name: pip-compile requirements.in
      args: [requirements.in, -o, requirements.txt]
    - id: pip-compile
      name: pip-compile requirements-dev.in
      args: [requirements-dev.in, -o, requirements-dev.txt]
      files: ^requirements-dev\.(in|txt)$
```
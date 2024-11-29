# 管理包

## 安装包

要将包安装到虚拟环境中，例如安装 Flask：

```console
$ uv pip install flask
```

要启用可选依赖项安装包，例如安装带有 "dotenv" 附加项的 Flask：

```console
$ uv pip install "flask[dotenv]"
```

要一次安装多个包，例如安装 Flask 和 Ruff：

```console
$ uv pip install flask ruff
```

要安装带有约束的包，例如安装 v0.2.0 或更新版本的 Ruff：

```console
$ uv pip install 'ruff>=0.2.0'
```

要安装特定版本的包，例如安装 v0.3.0 版本的 Ruff：

```console
$ uv pip install 'ruff==0.3.0'
```

要从本地磁盘安装包：

```console
$ uv pip install "ruff @ ./projects/ruff"
```

要从 GitHub 安装包：

```console
$ uv pip install "git+https://github.com/astral-sh/ruff"
```

要从 GitHub 安装特定引用的包：

```console
$ # 安装某个标签
$ uv pip install "git+https://github.com/astral-sh/ruff@v0.2.0"

$ # 安装某个提交
$ uv pip install "git+https://github.com/astral-sh/ruff@1fadefa67b26508cc59cf38e6130bde2243c929d"

$ # 安装某个分支
$ uv pip install "git+https://github.com/astral-sh/ruff@main"
```

有关从私有仓库安装的更多信息，请参阅 [Git 认证](../configuration/authentication.md#git-authentication) 文档。

## 可编辑包

可编辑包不需要重新安装即可使源代码的更改生效。

要将当前项目安装为可编辑包：

```console
$ uv pip install -e .
```

要将另一个目录中的项目安装为可编辑包：

```console
$ uv pip install -e "ruff @ ./project/ruff"
```

## 从文件安装包

可以从标准文件格式一次性安装多个包。

从 `requirements.txt` 文件安装：

```console
$ uv pip install -r requirements.txt
```

有关 `requirements.txt` 文件的更多信息，请参阅 [`uv pip compile`](./compile.md) 文档。

从 `pyproject.toml` 文件安装：

```console
$ uv pip install -r pyproject.toml
```

从 `pyproject.toml` 文件安装时启用可选依赖项，例如启用 "foo" 附加项：

```console
$ uv pip install -r pyproject.toml --extra foo
```

从 `pyproject.toml` 文件安装时启用所有可选依赖项：

```console
$ uv pip install -r pyproject.toml --all-extras
```

## 卸载包

要卸载包，例如卸载 Flask：

```console
$ uv pip uninstall flask
```

要卸载多个包，例如卸载 Flask 和 Ruff：

```console
$ uv pip uninstall flask ruff
```
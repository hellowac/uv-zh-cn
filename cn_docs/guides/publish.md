# 发布包

uv 支持通过 `uv build` 构建 Python 包为源代码和二进制分发包，并使用 `uv publish` 将其上传到注册表。

## 为打包准备项目

在尝试发布您的项目之前，您需要确保它已准备好打包分发。

如果您的项目在 `pyproject.toml` 文件中没有包含 `[build-system]` 部分，uv 默认不会构建它。这意味着您的项目可能尚未准备好进行发布。有关声明构建系统的影响，请阅读 [项目概念](../concepts/projects/config.md#build-systems) 文档。

!!! note

    如果您的项目中有不希望发布的内部包，您可以将它们标记为私有：

    ```toml
    [project]
    classifiers = ["Private :: Do Not Upload"]
    ```

    这一设置会让 PyPI 拒绝您的包发布。它不会影响其他注册表上的安全性或隐私设置。

    我们还建议只生成每个项目的令牌：没有与项目匹配的 PyPI 令牌，包就无法被意外发布。

## 构建您的包

使用 `uv build` 构建您的包：

```console
$ uv build
```

默认情况下，`uv build` 会在当前目录中构建项目，并将构建好的工件放入 `dist/` 子目录。

另外，`uv build <SRC>` 会在指定目录中构建包，而 `uv build --package <PACKAGE>` 会在当前工作区中构建指定的包。

!!! info

    默认情况下，`uv build` 在解析 `pyproject.toml` 文件中 `build-system.requires` 部分的构建依赖时，会遵循 `tool.uv.sources` 的设置。在发布包时，我们建议运行 `uv build --no-sources`，以确保包在禁用 `tool.uv.sources`（例如使用其他构建工具，如 [`pypa/build`](https://github.com/pypa/build)）时能够正确构建。

## 发布您的包

使用 `uv publish` 发布您的包：

```console
$ uv publish
```

通过 `--token` 或 `UV_PUBLISH_TOKEN` 设置 PyPI 令牌，或者通过 `--username` 或 `UV_PUBLISH_USERNAME` 设置用户名，并通过 `--password` 或 `UV_PUBLISH_PASSWORD` 设置密码。对于从 GitHub Actions 发布到 PyPI，您无需设置任何凭据。相反，您可以 [将可信发布者添加到 PyPI 项目](https://docs.pypi.org/trusted-publishers/adding-a-publisher/)。

!!! note

    PyPI 不再支持通过用户名和密码发布包，您需要生成一个令牌。使用令牌相当于设置 `--username __token__` 并使用令牌作为密码。

尽管 `uv publish` 会重试上传失败的文件，但可能会发生上传过程中失败的情况，导致部分文件已上传而部分文件仍然缺失。对于 PyPI，您可以重新尝试相同的命令，已上传的相同文件将被忽略。对于其他注册表，使用 `--check-url <index url>` 指定包所在的索引 URL（而不是发布 URL）。uv 会跳过上传与注册表中现有文件相同的文件，并处理并行上传的竞争条件。需要注意的是，现有文件必须与先前上传到注册表的文件完全匹配，这样可以避免不小心发布内容不同的源分发包和 wheel 包。

## 安装您的包

使用 `uv run` 测试包是否可以被安装和导入：

```console
$ uv run --with <PACKAGE> --no-project -- python -c "import <PACKAGE>"
```

`--no-project` 标志用于避免从本地项目目录安装包。

!!! tip

    如果您最近安装了该包，可能需要使用 `--refresh-package <PACKAGE>` 选项，以避免使用缓存版本的包。

## 下一步

要了解更多关于发布包的信息，请查看 [PyPA 指南](https://hellowac.github.io/pypug-zh-cn/guides/section-build-and-publish.html) 了解如何构建和发布包。

或者，继续阅读以了解如何 [将 uv 集成到其他软件中](./integration/index.md)。
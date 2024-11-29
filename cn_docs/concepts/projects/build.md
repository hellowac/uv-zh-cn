# 构建分发包

要将您的项目分发给他人（例如，上传到像 PyPI 这样的索引），您需要将其构建为可分发的格式。

Python 项目通常以源代码分发包（sdists）和二进制分发包（wheels）两种形式进行分发。前者通常是一个 `.tar.gz` 或 `.zip` 文件，包含项目的源代码和一些附加元数据，而后者是一个 `.whl` 文件，包含可以直接安装的预构建工件。

## 使用 `uv build`

`uv build` 可以用来构建源代码分发包和二进制分发包。默认情况下，`uv build` 会在当前目录中构建项目，并将构建后的工件放在 `dist/` 子目录中：

```console
$ uv build
$ ls dist/
example-0.1.0-py3-none-any.whl
example-0.1.0.tar.gz
```

您可以通过向 `uv build` 提供路径来在不同的目录中构建项目，例如 `uv build path/to/project`。

`uv build` 将首先构建源代码分发包，然后再从该源代码分发包构建二进制分发包（wheel）。

您可以使用 `uv build --sdist` 仅构建源代码分发包，使用 `uv build --wheel` 仅构建二进制分发包，或使用 `uv build --sdist --wheel` 从源代码构建两种分发包。

## 构建约束

`uv build` 接受 `--build-constraint` 选项，您可以用它来限制构建过程中的任何构建依赖项的版本。当与 `--require-hashes` 一起使用时，uv 将强制要求用于构建项目的依赖项与指定的已知哈希匹配，以确保可复现性。

例如，给定以下的 `constraints.txt` 文件：

```text
setuptools==68.2.2 --hash=sha256:b454a35605876da60632df1a60f736524eb73cc47bbc9f3f1ef1b644de74fd2a
```

运行以下命令将使用指定版本的 `setuptools` 来构建项目，并验证下载的 `setuptools` 分发包是否与指定的哈希匹配：

```console
$ uv build --build-constraint constraints.txt --require-hashes
```

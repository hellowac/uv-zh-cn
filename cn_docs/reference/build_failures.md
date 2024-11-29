# 构建失败

本页面列出了导致解析和安装失败的常见原因及如何修复它们。

### 为什么 uv 需要构建一个包？

在生成跨平台锁定文件时，uv 需要确定所有包的依赖关系，即使这些包仅安装在其他平台上。uv 尝试在解析过程中避免包构建。如果该版本存在轮子（wheel），uv 会优先使用轮子；如果没有，则尝试在源分发包中查找静态元数据（主要是 `pyproject.toml` 文件中的静态 `project.version`、`project.dependencies` 和 `project.optional-dependencies`，或者至少是版本 2.2 的 METADATA）。只有在这些方法都失败时，它才会构建包。

在安装时，uv 需要每个包的当前平台的轮子。如果索引中没有匹配的轮子，uv 会尝试构建源分发包。

你可以在 PyPI 项目的“下载文件”页面查看哪些轮子可用，例如 [https://pypi.org/project/numpy/2.1.1/#files](https://pypi.org/project/numpy/2.1.1/#files)。文件名为 `...-py3-none-any.whl` 的轮子适用于所有平台，其他轮子则会在文件名中标明操作系统和平台。例如，对于链接中的 numpy 版本，你可以看到 Python 3.10 到 3.13 在 MacOS、Linux 和 Windows 上都被支持。

### 修复和解决方法

- 如果构建错误提到缺少头文件或库，通常可以在系统的包管理器中找到对应的包。

      示例：当在 Ubuntu 上运行 `uv pip install mysqlclient==2.2.4` 失败时，你需要运行
      `sudo apt install default-libmysqlclient-dev build-essential pkg-config` 来安装 MySQL
      头文件 ([https://pypi.org/project/mysqlclient/2.2.4/](https://pypi.org/project/mysqlclient/2.2.4/#Linux))

- 如果构建错误提到导入失败，考虑
  [禁用构建隔离](https://docs.astral.sh/uv/concepts/projects/#build-isolation)。
- 如果在解析过程中包构建失败，且失败的版本比你想要使用的版本要旧，尝试添加一个
  [约束](https://docs.astral.sh/uv/reference/settings/#constraint-dependencies)，例如 `numpy>=1.17`。有时由于算法限制，uv 解析器会尝试使用不合理旧的包，这种情况可以通过使用下限来避免。
- 考虑使用不同的 Python 版本进行锁定和/或安装（`-p`）。如果你使用的是旧版本的 Python，你可能还需要使用某些带有本地代码的包的旧版本，尤其是科学计算相关的包。例如：torch 1.12.0 支持 Python 3.7 到 3.10
  ([https://pypi.org/project/torch/1.12.0/#files](https://pypi.org/project/torch/1.12.0/#files))，而 numpy 2.1.0 支持 Python 3.10 到 3.13
  ([https://numpy.org/doc/stable/release/2.1.0-notes.html#numpy-2-1-0-release-notes](https://numpy.org/doc/stable/release/2.1.0-notes.html#numpy-2-1-0-release-notes))，因此同时使用这两个包需要 Python 3.10（或者升级 torch）。
- 如果由于构建不受支持平台的包而导致锁定失败，考虑
  [声明解析环境](https://docs.astral.sh/uv/reference/settings/#environments)以指定你支持的平台。
- 如果你支持多个 Python 版本，考虑使用标记来为旧版本的 Python 使用旧版本的包，为新版本的 Python 使用新版包。在示例中，numpy 通常一次支持四个 Python 次版本，因此要支持 Python 3.8 到 3.13，版本需要拆分：

    ```
    numpy>=1.23; python_version >= "3.10"
    numpy<1.23; python_version < "3.10"
    ```

- 如果由于构建来自不同平台的包而导致锁定失败，作为一种解决方法，你可以
  [手动提供依赖元数据](https://docs.astral.sh/uv/reference/settings/#dependency-metadata)。
  由于 uv 无法验证这些信息，因此在此覆盖中指定正确的元数据非常重要。
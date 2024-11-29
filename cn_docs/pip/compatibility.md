# 与 `pip` 和 `pip-tools` 的兼容性

`uv` 旨在作为常见的 `pip` 和 `pip-tools` 工作流的直接替代品。

非正式地说，`uv` 的设计目的是使现有的 `pip` 和 `pip-tools` 用户能够在不做出重大更改的情况下切换到 `uv`；并且在大多数情况下，将 `pip install` 替换为 `uv pip install` 应该能够“直接工作”。

然而，`uv` 并不是 `pip` 的**精确**克隆，且离开常见的 `pip` 工作流越远，遇到行为差异的可能性就越大。在某些情况下，这些差异可能是已知的并且是有意为之；在其他情况下，它们可能是实现细节的结果；还有一些情况可能是 bug。

本文概述了 `uv` 与 `pip` 之间已知的差异，提供了相关的理由、解决方法，并声明了未来兼容性的意图。

## 配置文件和环境变量

`uv` 不会读取特定于 `pip` 的配置文件或环境变量，如 `pip.conf` 或 `PIP_INDEX_URL`。

读取为其他工具设计的配置文件和环境变量有若干缺点：

1. 它要求与目标工具保持逐字逐句的兼容性，因为用户最终依赖于格式、解析器等中的 bug。
2. 如果目标工具以某种方式**更改**格式，则 `uv` 也必须以等效的方式进行更改。
3. 如果该配置以某种方式版本化，`uv` 需要知道用户期望使用的是哪个版本的目标工具。
4. 它阻止 `uv` 引入在目标工具中不存在的任何设置或配置，否则 `pip.conf`（或类似文件）将无法与 `pip` 一起使用。
5. 它可能导致用户混淆，因为 `uv` 会读取实际上不影响其行为的设置，且许多用户可能不期望 `uv` 读取为其他工具设计的配置文件。

因此，`uv` 支持其自己的环境变量，如 `UV_INDEX_URL`。`uv` 还支持在 `uv.toml` 文件或 `pyproject.toml` 文件中的 `[tool.uv.pip]` 部分中进行持久化配置。有关更多信息，请参阅[配置文件](../configuration/files.md)。

## 预发布兼容性

默认情况下，`uv` 会在依赖解析过程中接受预发布版本，具体情况如下：

1. 如果包是直接依赖，并且其版本标记中包含预发布说明符（例如 `flask>=2.0.0rc1`）。
2. 如果包的所有已发布版本都是预发布版本。

如果由于传递性预发布包导致依赖解析失败，`uv` 会提示用户使用 `--prerelease allow` 重新运行，以允许所有依赖项的预发布版本。

另外，您也可以通过在 `requirements.in` 文件中添加预发布说明符（例如 `flask>=2.0.0rc1`）来选择性地支持特定依赖项的预发布版本。

总之，`uv` 需要提前知道解析器是否应接受给定包的预发布版本。与此同时，`pip` 可能会根据解析器遇到相关说明符的顺序，尊重传递依赖中的预发布标识符（[#1641](https://github.com/astral-sh/uv/issues/1641#issuecomment-1981402429)）。

预发布版本是众所周知的**难以建模**，并且经常成为打包工具中的 bug 来源。即便是被视为参考实现的 `pip`，在预发布处理方面也有许多悬而未决的问题（[#12469](https://github.com/pypa/pip/issues/12469)，[#12470](https://github.com/pypa/pip/issues/12470)，[#40505](https://discuss.python.org/t/handling-of-pre-releases-when-backtracking/40505/20) 等）。

`uv` 的预发布处理是**故意**有限制的，并且**故意**要求用户在预发布版本时显式选择，以确保正确性。

未来，`uv` 可能会支持传递依赖中的预发布标识符。但这可能依赖于 Python 打包规范的演变。现有的 PEP 并不涵盖“依赖解析”（[关于预发布版本处理的讨论](https://discuss.python.org/t/handling-of-pre-releases-when-backtracking/40505/17)），而是专注于单一版本说明符的行为。因此，在打包生态系统中，关于预发布版本的正确和预期行为还有许多未解的问题。

## 存在于多个索引上的包

在 `uv` 和 `pip` 中，用户都可以指定多个包索引，用于搜索给定包的可用版本。然而，`uv` 和 `pip` 在处理存在于多个索引中的包时有所不同。

例如，假设公司在一个私有索引（`--extra-index-url`）上发布了 `requests` 的内部版本，但默认允许从 PyPI 安装包。在这种情况下，私有的 `requests` 将与 PyPI 上的公共 [`requests`](https://pypi.org/project/requests/) 冲突。

当 `uv` 在多个索引上搜索包时，它将按照索引的顺序进行迭代（优先选择 `--extra-index-url` 而非默认索引），并在找到匹配时停止搜索。这意味着，如果包存在于多个索引上，`uv` 将限制候选版本仅限于第一个包含该包的索引中的版本。

而 `pip` 会将所有索引中的候选版本合并，并从合并后的版本集中选择最佳版本，尽管它不会对搜索索引的顺序做出保证，并且期望包在所有索引中是唯一的，按名称和版本唯一。

`uv` 的行为是，如果一个包存在于内部索引中，它应始终从该内部索引安装，而不是从 PyPI 安装。其目的是防止“依赖混淆”攻击，即攻击者在 PyPI 上发布与内部包同名的恶意包，从而导致恶意包被安装而非内部包。例如，参见[2022年12月的 `torchtriton` 攻击](https://pytorch.org/blog/compromised-nightly-dependency/)。

从 v0.1.39 版本起，用户可以通过 `--index-strategy` 命令行选项或 `UV_INDEX_STRATEGY` 环境变量选择 `pip` 风格的多索引行为，支持以下值：

- `first-match`（默认值）：在所有索引中搜索每个包，并将候选版本限制为第一个包含该包的索引中的版本，优先考虑 `--extra-index-url` 索引。
- `unsafe-first-match`：在所有索引中搜索每个包，但即使其他索引上有更新版本，也优先选择第一个与之兼容的索引。
- `unsafe-best-match`：在所有索引中搜索每个包，并从合并后的候选版本集中选择最佳版本。

虽然 `unsafe-best-match` 最接近 `pip` 的行为，但它暴露了用户遭受“依赖混淆”攻击的风险。

`uv` 还支持将包固定到特定索引（参见：[索引](../configuration/indexes.md#pinning-a-package-to-an-index)），确保给定包始终从特定索引安装。

## PEP 517 构建隔离

`uv` 默认使用 [PEP 517](https://peps.python.org/pep-0517/) 构建隔离（类似于 `pip install --use-pep517`），遵循 `pypa/build`，并预计未来 `pip` 也将默认使用 PEP 517 构建（[pypa/pip#9175](https://github.com/pypa/pip/issues/9175)）。

如果由于缺少构建时依赖而导致包安装失败，尝试使用该包的较新版本；如果问题仍然存在，考虑向包维护者报告，要求他们更新打包设置，声明正确的 PEP 517 构建时依赖。

作为应急措施，您可以预先安装包的构建依赖项，然后使用 `--no-build-isolation` 参数运行 `uv pip install`，例如：

```shell
uv pip install wheel && uv pip install --no-build-isolation biopython==1.77
```

有关已知在 PEP 517 构建隔离下失败的包的列表，请参见 [#2252](https://github.com/astral-sh/uv/issues/2252)。

## 传递的 URL 依赖

虽然 `uv` 完全支持 URL 依赖（例如，`ruff @ https://...`），但在处理**传递性** URL 依赖时，`uv` 与 `pip` 有两个方面的不同。

首先，`uv` 假设非 URL 依赖不会引入 URL 依赖到解析中。换句话说，它假设从注册表中获取的依赖本身不依赖于 URL。如果一个非 URL 依赖确实引入了 URL 依赖，`uv` 会在解析过程中拒绝该 URL 依赖。（请注意，PyPI 不允许已发布的包依赖于 URL 依赖，其他注册表可能会更加宽松。）

其次，如果使用直接 URL 依赖定义了约束（`--constraint`）或覆盖（`--override`），并且约束包本身有一个直接的 URL 依赖，`uv` 可能会在解析过程中拒绝该传递性直接 URL 依赖，如果该 URL 没有在输入要求的集合中其他地方被引用。

如果 `uv` 拒绝了一个传递的 URL 依赖，最佳的做法是将该 URL 依赖作为直接依赖添加到相关的 `pyproject.toml` 或 `requirements.in` 文件中，因为上述约束不适用于直接依赖。

## 默认虚拟环境

`uv pip install` 和 `uv pip sync` 默认设计为与虚拟环境一起使用。

具体来说，`uv` 总是将包安装到当前激活的虚拟环境中，或者在当前目录或任何父目录中查找名为 `.venv` 的虚拟环境（即使该虚拟环境没有被激活）。

这与 `pip` 不同，后者如果没有激活虚拟环境，则会将包安装到全局环境中，并且不会查找非激活的虚拟环境。

在 `uv` 中，您可以通过 `--python /path/to/python` 选项或 `--system` 标志指定 Python 可执行文件路径来安装到非虚拟环境中，这与 `pip` 类似。

换句话说，`uv` 反转了默认行为，要求显式选择是否安装到系统 Python 环境，这可能导致破坏和其他问题，因此应该仅在有限的情况下使用。

有关更多信息，请参阅 [“使用任意 Python 环境”](./environments.md#using-arbitrary-python-environments)。

## 解析策略

对于给定的依赖说明符集，通常情况下并不存在一个“正确”的包集合来安装。相反，有许多有效的包集合可以满足说明符的要求。

`pip` 和 `uv` 都不保证安装的包集合是**精确的**；他们保证的只是解析过程是一致的、确定性的，并且符合说明符。因此，在某些情况下，`pip` 和 `uv` 可能会产生不同的解析结果；然而，两个解析结果**应该**是同样有效的。

例如，考虑以下内容：

```python title="requirements.in"
starlette
fastapi
```

在撰写本文时，最新的 `starlette` 版本是 `0.37.2`，而最新的 `fastapi` 版本是 `0.110.0`。然而，`fastapi==0.110.0` 也依赖于 `starlette`，并且引入了一个上限：`starlette>=0.36.3,<0.37.0`。

如果解析器优先包括最新版本的 `starlette`，它将需要使用一个较旧版本的 `fastapi`，以排除 `starlette` 的上限。在实践中，这需要回退到 `fastapi==0.1.17`：

```python title="requirements.txt"
# 该文件是通过以下命令由 uv 自动生成的：
#    uv pip compile requirements.in
annotated-types==0.6.0
    # via pydantic
anyio==4.3.0
    # via starlette
fastapi==0.1.17
idna==3.6
    # via anyio
pydantic==2.6.3
    # via fastapi
pydantic-core==2.16.3
    # via pydantic
sniffio==1.3.1
    # via anyio
starlette==0.37.2
    # via fastapi
typing-extensions==4.10.0
    # via
    #   pydantic
    #   pydantic-core
```

或者，如果解析器优先包括最新版本的 `fastapi`，它将需要使用一个较旧版本的 `starlette`，以满足上限。在实践中，这需要回退到 `starlette==0.36.3`：

```python title="requirements.txt"
# 该文件是通过以下命令由 uv 自动生成的：
#    uv pip compile requirements.in
annotated-types==0.6.0
    # via pydantic
anyio==4.3.0
    # via starlette
fastapi==0.110.0
idna==3.6
    # via anyio
pydantic==2.6.3
    # via fastapi
pydantic-core==2.16.3
    # via pydantic
sniffio==1.3.1
    # via anyio
starlette==0.36.3
    # via fastapi
typing-extensions==4.10.0
    # via
    #   fastapi
    #   pydantic
    #   pydantic-core
```

当 `uv` 的解析结果与 `pip` 在不期望的方式上有所不同时，这通常意味着说明符过于宽松，用户应考虑收紧这些说明符。例如，在 `starlette` 和 `fastapi` 的例子中，用户可以要求 `fastapi>=0.110.0`。

## `pip check`

目前，`uv pip check` 会显示以下诊断信息：

- 包没有 `METADATA` 文件，或 `METADATA` 文件无法解析。
- 包的 `Requires-Python` 与当前解释器的 Python 版本不匹配。
- 包依赖于未安装的其他包。
- 包依赖于已安装的包，但版本不兼容。
- 虚拟环境中安装了包的多个版本。

在某些情况下，`uv pip check` 会显示 `pip check` 不会显示的诊断信息，反之亦然。例如，不像 `uv pip check`，`pip check` 不会在当前环境中安装了多个版本的包时发出警告。

## `--user` 和 `user` 安装方案

`uv` 不支持 `--user` 标志，该标志是基于 `user` 安装方案安装包的方式。相反，我们建议使用虚拟环境来隔离包的安装。

此外，如果 `pip` 检测到用户没有写入目标目录的权限（例如在某些系统上安装到系统 Python 时），它会回退到 `user` 安装方案。而 `uv` 不实现任何类似的回退机制。

更多信息，请参见 [#2077](https://github.com/astral-sh/uv/issues/2077)。

## `--only-binary` 强制执行

`--only-binary` 参数用于限制安装仅限于预构建的二进制分发。当提供 `--only-binary :all:` 时，`pip` 和 `uv` 都会拒绝从 PyPI 和其他注册表构建源分发包。

然而，当一个依赖项作为直接 URL 提供时（例如 `uv pip install https://...`），`pip` 不会强制执行 `--only-binary`，并会为所有此类包构建源分发。

`uv` 则会强制执行 `--only-binary`，对于直接 URL 依赖有一个例外：如果给定 `uv pip install https://... --only-binary flask`，如果 `uv` 无法提前推断包名，它会构建给定 URL 的源分发，因为在这种情况下 `uv` 无法在不构建其元数据的情况下确定该包是否“被允许”。

`pip` 和 `uv` 都允许在提供 `--only-binary` 时构建和安装可编辑要求。例如，`uv pip install -e . --only-binary :all:` 是允许的。

## `--no-binary` 强制执行

`--no-binary` 参数用于限制安装仅限于源分发包。当提供 `--no-binary` 时，`uv` 会拒绝安装预构建的二进制分发包，但会重用本地缓存中已存在的任何二进制分发包。

此外，与 `pip` 相反，`uv` 的解析器即使在提供 `--no-binary` 时，也会从预构建的二进制分发包中读取元数据。

## `manylinux_compatible` 强制执行

[PEP 600](https://peps.python.org/pep-0600/#package-installers) 描述了一种机制，通过该机制，Python 分发者可以通过在 `_manylinux` 标准库模块上定义 `manylinux_compatible` 函数来选择退出 `manylinux` 兼容性。

`uv` 尊重 `manylinux_compatible`，但仅针对当前的 glibc 版本进行测试，并且全局应用 `manylinux_compatible` 的返回值。

换句话说，如果 `manylinux_compatible` 返回 `True`，`uv` 会将系统视为 `manylinux` 兼容；如果返回 `False`，`uv` 会将系统视为不兼容 `manylinux`，并且不会为每个 glibc 版本都调用 `manylinux_compatible`。

这种方法并不完全实现该规范，但与常见的 `manylinux_compatible` 实现兼容，例如 [`no-manylinux`](https://pypi.org/project/no-manylinux/)：

```python
from __future__ import annotations
manylinux1_compatible = False
manylinux2010_compatible = False
manylinux2014_compatible = False


def manylinux_compatible(*_, **__):  # PEP 600
    return False
```

## 字节码编译

与 `pip` 不同，`uv` 默认不会在安装过程中将 `.py` 文件编译为 `.pyc` 文件（即，`uv` 不会创建或填充 `__pycache__` 目录）。要启用安装过程中的字节码编译，可以在 `uv pip install` 或 `uv pip sync` 中传递 `--compile-bytecode` 标志。

## 严格性和规范强制执行

`uv` 通常比 `pip` 更严格，常常会拒绝 `pip` 会安装的包。例如，`uv` 会拒绝包含无效 URL 片段的 HTML 索引（参见：[PEP 503](https://peps.python.org/pep-0503/)），而 `pip` 会忽略这些片段。

在某些情况下，`uv` 会对已知有特定规范合规性问题的流行包实现宽松的行为。

如果 `uv` 因规范违反拒绝安装包，而 `pip` 会安装该包，最佳的做法是首先尝试安装包的较新版本；如果失败，向包的维护者报告该问题。

## `pip` 命令行选项和子命令

`uv` 不支持 `pip` 的所有命令行选项和子命令，尽管它支持大部分子集。

缺失的选项和子命令会根据用户需求和实现复杂性优先排序，并通常通过单独的问题进行跟踪。例如：

- [`--trusted-host`](https://github.com/astral-sh/uv/issues/1339)
- [`--user`](https://github.com/astral-sh/uv/issues/2077)

如果您遇到缺失的选项或子命令，请搜索问题追踪器，查看是否已经报告过，如果没有，请考虑打开一个新问题。您也可以为现有问题投票，表示您的兴趣。

## 注册认证 {: #registry-authentication}

uv 不支持 `pip` 的 `auto` 或 `import` 选项用于 `--keyring-provider`。目前，仅支持 `subprocess` 选项。

与 `pip` 不同，uv 默认不启用 keyring 认证。

与 `pip` 不同，uv 在发出请求之前不会等待 HTTP 401 响应才开始搜索认证信息。uv 会将认证信息附加到所有请求中，只要目标主机有可用的凭证。

## `egg` 支持

uv 不支持 `pip` 中被认为是遗留或已废弃的功能。例如，uv 不支持 `.egg` 风格的分发包。

然而，uv 对 (1) `.egg-info` 风格的分发包（这类分发包偶尔出现在 Docker 镜像和 Conda 环境中）和 (2) 传统的可编辑 `.egg-link` 风格的分发包提供部分支持。

具体来说，uv 不支持安装新的 `.egg-info` 或 `.egg-link` 风格的分发包，但在解析时会尊重任何现有的 `.egg-info` 或 `.egg-link` 分发包，可以通过 `uv pip list` 和 `uv pip freeze` 列出它们，并通过 `uv pip uninstall` 卸载它们。

## 构建约束

当通过 `--constraint`（或 `UV_CONSTRAINT`）提供约束时，uv _不会_ 在解析构建依赖时应用这些约束（即用于构建源代码分发包）。相反，构建约束应通过专门的 `--build-constraint`（或 `UV_BUILD_CONSTRAINT`）选项提供。

而 `pip` 在通过 `PIP_CONSTRAINT` 指定时，会将约束应用于构建依赖，但当通过命令行中的 `--constraint` 提供时，则不会应用于构建依赖。

例如，若要确保在构建任何有 `setuptools` 构建依赖的包时使用 `setuptools 60.0.0`，请使用 `--build-constraint` 而不是 `--constraint`。

## `pip compile` 默认行为

`pip compile` 和 `pip-tools` 在默认行为上有一些小的但显著的差异。

默认情况下，uv 不会将编译后的要求写入输出文件。相反，uv 要求用户明确指定输出文件，通过 `-o` 或 `--output-file` 选项。

默认情况下，uv 在输出编译后的要求时会去除额外的选项。换句话说，uv 默认启用 `--strip-extras`，而 `pip-compile` 默认启用 `--no-strip-extras`。`pip-compile` 计划在下一个主要版本（v8.0.0）中更改此默认行为，届时两个工具都将默认启用 `--strip-extras`。要在 uv 中保留额外选项，请向 `uv pip compile` 传递 `--no-strip-extras` 标志。

默认情况下，uv 不会将任何索引 URL 写入输出文件，而 `pip-compile` 会输出任何不匹配默认（PyPI）的 `--index-url` 或 `--extra-index-url`。要将索引 URL 包含在输出文件中，请向 `uv pip compile` 传递 `--emit-index-url` 标志。与 `pip-compile` 不同，uv 会在传递 `--emit-index-url` 时包含所有索引 URL，包括默认的索引 URL。

## `requires-python` 强制执行

在根据 `requires-python` 规范评估 Python 版本时，uv 会截断候选版本，仅保留主版本号、次版本号和修订版本号，忽略（例如）预发布和后发布标识符。

例如，一个声明 `requires-python: >=3.13` 的项目将接受 Python 3.13.0b1。尽管 3.13.0b1 严格来说不大于 3.13，但在忽略预发布标识符后，它实际上大于 3.13。

尽管这并不完全符合 [PEP 440](https://peps.python.org/pep-0440/) 的规定，但它与 [pip](https://github.com/pypa/pip/blob/24.1.1/src/pip/_internal/resolution/resolvelib/candidates.py#L540) 是一致的。

## 包优先级

通常，给定一组要求，存在许多可能的解决方案，解析器必须在其中做出选择。uv 的解析器和 pip 的解析器有不同的包优先级。虽然两个解析器都将用户提供的顺序作为其优先级之一，但 pip 有一些 uv 不具备的额外优先级。 因此，uv 更可能受到用户顺序变化的影响，而不是 pip。

例如，`uv pip install foo bar` 优先选择较新的 `foo` 版本，而不是 `bar`，这可能会导致与 `uv pip install bar foo` 不同的解析结果。类似地，这种行为也适用于 `uv pip compile` 输入文件中要求的顺序。

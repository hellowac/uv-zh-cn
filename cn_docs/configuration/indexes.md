# 包索引

默认情况下，uv 使用 [Python 包索引 (PyPI)](https://pypi.org) 来解决依赖关系和安装包。然而，uv 可以通过 `[[tool.uv.index]]` 配置选项（以及相应的 `--index` 命令行选项）配置为使用其他包索引，包括私有索引。

## 定义一个索引

要在解决依赖关系时包括额外的索引，可以在 `pyproject.toml` 中添加 `[[tool.uv.index]]` 条目：

```toml
[[tool.uv.index]]
# 可选的索引名称。
name = "pytorch"
# 索引的必需 URL。
url = "https://download.pytorch.org/whl/cpu"
```

索引按定义的顺序进行优先级排序，因此配置文件中列出的第一个索引将在解决依赖关系时首先被查询，而通过命令行提供的索引将优先于配置文件中的索引。

默认情况下，uv 将 Python 包索引 (PyPI) 包含为 "默认" 索引，即当包在任何其他索引中未找到时使用的索引。要从索引列表中排除 PyPI，可以在另一个索引条目中设置 `default = true`（或使用 `--default-index` 命令行选项）：

```toml
[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cpu"
default = true
```

默认索引始终被视为最低优先级，无论其在索引列表中的位置如何。

索引名称只能包含字母数字字符、连字符、下划线和句点，并且必须是有效的 ASCII 字符。

在命令行（使用 `--index` 或 `--default-index`）或通过环境变量（`UV_INDEX` 或 `UV_DEFAULT_INDEX`）提供索引时，名称是可选的，但可以使用 `<name>=<url>` 语法来包含，例如：

```shell
# 在命令行中
$ uv lock --index pytorch=https://download.pytorch.org/whl/cpu
# 通过环境变量
$ UV_INDEX=pytorch=https://download.pytorch.org/whl/cpu uv lock
```

## 锁定包到某个索引

可以通过在 `tool.uv.sources` 条目中指定索引来将包锁定到特定的索引。例如，要确保 `torch` 始终从 `pytorch` 索引安装，可以在 `pyproject.toml` 中添加以下内容：

```toml
[tool.uv.sources]
torch = { index = "pytorch" }

[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cpu"
```

类似地，可以根据平台从不同的索引拉取包，方法是提供一个按环境标记区分的源列表：

```toml title="pyproject.toml"
[project]
dependencies = ["torch"]

[tool.uv.sources]
torch = [
  { index = "pytorch-cu118", marker = "sys_platform == 'darwin'"},
  { index = "pytorch-cu124", marker = "sys_platform != 'darwin'"},
]

[[tool.uv.index]]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
```

可以将索引标记为 `explicit = true`，以防止除非明确锁定到该索引，否则不从该索引安装包。例如，要确保 `torch` 从 `pytorch` 索引安装，但其他所有包都从 PyPI 安装，可以在 `pyproject.toml` 中添加以下内容：

```toml
[tool.uv.sources]
torch = { index = "pytorch" }

[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

通过 `tool.uv.sources` 引用的命名索引必须在项目的 `pyproject.toml` 文件中定义；通过命令行、环境变量或用户级配置提供的索引将不会被识别。

如果一个索引同时被标记为 `default = true` 和 `explicit = true`，它将被视为显式索引（即，仅通过 `tool.uv.sources` 使用），同时也会将 PyPI 移除为默认索引。

## 跨多个索引进行搜索

默认情况下，uv 会在第一个找到指定包的索引处停止，并将解析限制在该第一个索引上存在的版本（`first-match`）。

例如，如果通过 `[[tool.uv.index]]` 指定了一个内部索引，uv 的行为将是，如果包在该内部索引中存在，它将 _始终_ 从该内部索引安装，而不是从 PyPI 安装。这样做的目的是防止 "依赖混淆" 攻击，在这种攻击中，攻击者会在 PyPI 上发布一个恶意包，其名称与内部包相同，导致恶意包被安装而不是内部包。例如，可以参见 [2022年12月的 `torchtriton` 攻击](https://pytorch.org/blog/compromised-nightly-dependency/)。

用户可以通过 `--index-strategy` 命令行选项或 `UV_INDEX_STRATEGY` 环境变量来选择不同的索引策略，支持以下值：

- `first-match`（默认）：在所有索引中搜索每个包，并将候选版本限制为第一个包含该包的索引中的版本。
- `unsafe-first-match`：在所有索引中搜索每个包，但优先选择第一个索引中兼容的版本，即使在其他索引中有更新的版本。
- `unsafe-best-match`：在所有索引中搜索每个包，从所有候选版本中选择最佳版本。

虽然 `unsafe-best-match` 最接近 pip 的行为，但它暴露了用户受到 "依赖混淆" 攻击的风险。

## 提供凭证

大多数私有注册表需要身份验证才能访问包，通常通过用户名和密码（或访问令牌）。

要对提供的索引进行身份验证，可以通过环境变量提供凭证或将凭证嵌入到 URL 中。

例如，假设有一个名为 `internal-proxy` 的索引，需要用户名（`public`）和密码（`koala`），在 `pyproject.toml` 中定义该索引（不包含凭证）：

```toml
[[tool.uv.index]]
name = "internal-proxy"
url = "https://example.com/simple"
```

然后，您可以设置 `UV_INDEX_INTERNAL_PROXY_USERNAME` 和 `UV_INDEX_INTERNAL_PROXY_PASSWORD` 环境变量，其中 `INTERNAL_PROXY` 是索引名称的大写版本，非字母数字字符替换为下划线：

```sh
export UV_INDEX_INTERNAL_PROXY_USERNAME=public
export UV_INDEX_INTERNAL_PROXY_PASSWORD=koala
```

通过环境变量提供凭证，您可以避免在明文 `pyproject.toml` 文件中存储敏感信息。

或者，可以将凭证直接嵌入到索引定义中：

```toml
[[tool.uv.index]]
name = "internal"
url = "https://public:koala@pypi-proxy.corp.dev/simple"
```

出于安全考虑，凭证 _永远_ 不会存储在 `uv.lock` 文件中；因此，uv 在安装时 _必须_ 能够访问经过身份验证的 URL。

## `--index-url` 和 `--extra-index-url`

除了 `[[tool.uv.index]]` 配置选项外，uv 还支持 pip 风格的 `--index-url` 和 `--extra-index-url` 命令行选项，以实现兼容性，其中 `--index-url` 定义默认索引，`--extra-index-url` 定义额外的索引。

这些选项可以与 `[[tool.uv.index]]` 配置选项结合使用，并遵循相同的优先级规则：

- 默认索引始终被视为最低优先级，无论是通过传统的 `--index-url` 参数、推荐的 `--default-index` 参数，还是在 `[[tool.uv.index]]` 条目中设置 `default = true`。
- 索引会按照定义的顺序进行查询，无论是通过传统的 `--extra-index-url` 参数、推荐的 `--index` 参数，还是 `[[tool.uv.index]]` 条目。

实际上，`--index-url` 和 `--extra-index-url` 可以视为未命名的 `[[tool.uv.index]]` 条目，对于前者启用了 `default = true`。在这种情况下，`--index-url` 相当于 `--default-index`，而 `--extra-index-url` 相当于 `--index`。

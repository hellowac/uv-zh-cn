## 项目元数据

### [`conflicts`](#conflicts) {: #conflicts }

此处可以声明冲突的 extras 或组。

当两个或更多的 extras 存在相互不兼容的依赖时，声明冲突是有用的。例如，extra `foo` 可能依赖于 `numpy==2.0.0`，而 extra `bar` 可能依赖于 `numpy==2.1.0`。这两个 extras 不能同时激活。对于 pip 风格的工作流，这通常不是问题，但在使用支持通用解析的 uv 项目时，uv 会尝试生成一个同时满足这两个 extras 的解析方案。

当发生这种情况时，解析将失败，因为无法将 `numpy 2.0.0` 和 `numpy 2.1.0` 同时安装到同一个环境中。

为了解决这个问题，可以将 `foo` 和 `bar` 声明为冲突的 extras（对于组也可以这样做）。在项目模式下进行通用解析时，这些 extras 将在不同的 "forks" 中进行处理，以允许冲突的依赖关系。作为交换，如果尝试从锁文件中安装并同时激活这两个冲突的 extras，安装将失败。

**默认值**：`[]`

**类型**：`list[list[dict]]`

**示例用法**：

```toml title="pyproject.toml"
[tool.uv]
# 要求 `package[test1]` 和 `package[test2]`
# 的要求在不同的 forks 中解析，以便它们
# 之间不能发生冲突。
conflicts = [
    [
        { extra = "test1" },
        { extra = "test2" },
    ]
]

# 或者，声明冲突的组：
conflicts = [
    [
        { group = "test1" },
        { group = "test2" },
    ]
]
```

---

### [`constraint-dependencies`](#constraint-dependencies) {: #constraint-dependencies }

在解析项目的依赖时应用的约束。

约束用于限制在解析过程中选择的依赖版本。

将一个包作为约束包含并不会触发该包的单独安装；相反，该包必须在项目的第一方或传递性依赖中其他地方被请求。

!!! note

    在 `uv lock`、`uv sync` 和 `uv run` 中，uv 只会读取工作区根目录下 `pyproject.toml` 中的 `constraint-dependencies`，并会忽略其他工作区成员或 `uv.toml` 文件中的声明。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

```toml title="pyproject.toml"
[tool.uv]
# 确保如果 grpcio 被传递依赖请求时，其版本始终小于 1.65。
constraint-dependencies = ["grpcio<1.65"]
```

---

### [`default-groups`](#default-groups) {: #default-groups }

默认安装的 `dependency-groups` 列表。

**默认值**：`["dev"]`

**类型**：`list[str]`

**示例用法**：

```toml title="pyproject.toml"
[tool.uv]
default-groups = ["docs"]
```

---

### [`dev-dependencies`](#dev-dependencies) {: #dev-dependencies }

项目的开发依赖。

开发依赖将默认在 `uv run` 和 `uv sync` 中安装，但不会出现在项目的发布元数据中。

不再推荐使用此字段。建议改为使用 `dependency-groups.dev` 字段，它是声明开发依赖的标准方式。`tool.uv.dev-dependencies` 和 `dependency-groups.dev` 的内容会合并，以确定 `dev` 依赖组的最终要求。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

```toml title="pyproject.toml"
[tool.uv]
dev-dependencies = ["ruff==0.5.0"]
```

---

### [`environments`](#environments) {: #environments }

支持的环境列表，用于解析依赖。

默认情况下，`uv lock` 操作会为所有可能的环境进行解析。然而，您可以限制支持的环境集，以提高性能并避免解空间中无法满足的分支。

在使用 `uv pip compile` 并带有 `--universal` 标志时，这些环境也会被尊重。

**默认值**：`[]`

**类型**：`str | list[str]`

**示例用法**：

```toml title="pyproject.toml"
[tool.uv]
# 只为 macOS 解析，不为 Linux 或 Windows 解析。
environments = ["sys_platform == 'darwin'"]
```

---

### [`index`](#index) {: #index }

解析依赖时使用的索引。

接受符合 [PEP 503](https://peps.python.org/pep-0503/)（简单仓库 API）规范的仓库，或本地目录，格式相同。

索引按照定义的顺序进行考虑，第一个定义的索引具有最高优先级。此外，通过此设置提供的索引优先级高于任何通过 [`index_url`](#index-url) 或 [`extra_index_url`](#extra-index-url) 指定的索引。uv 只会考虑包含给定包的第一个索引，除非指定了替代的 [索引策略](#index-strategy)。

如果某个索引标记为 `explicit = true`，则只有那些明确通过 `[tool.uv.sources]` 选择该索引的依赖会使用它，如下所示：

```toml
[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cu121"
explicit = true

[tool.uv.sources]
torch = { index = "pytorch" }
```

如果某个索引标记为 `default = true`，则它将移到优先级列表的末尾，在解析包时给予最低优先级。此外，标记为默认的索引会禁用 PyPI 默认索引。

**默认值**：`[]`

**类型**：`dict`

**示例用法**：

```toml title="pyproject.toml"

[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cu121"
```

---

### [`managed`](#managed) {: #managed }

项目是否由 uv 管理。如果为 `false`，则在调用 `uv run` 时，uv 将忽略该项目。

**默认值**：`true`

**类型**：`bool`

**示例用法**：

```toml title="pyproject.toml"
[tool.uv]
managed = false
```

---

### [`override-dependencies`](#override-dependencies) {: #override-dependencies }

在解析项目依赖时应用的覆盖。

覆盖用于强制选择特定版本的包，无论其他包请求的版本是什么，也不管选择该版本是否通常会导致无效的解析。

虽然约束是_加法_的（即它们与构成包的要求合并），但覆盖是_绝对_的（即它们完全替代任何构成包的要求）。

将包作为覆盖包含不会触发包的独立安装；相反，必须在项目的第一方或传递依赖中请求该包。

!!! note
    在 `uv lock`、`uv sync` 和 `uv run` 中，uv 只会从工作空间根目录的 `pyproject.toml` 中读取 `override-dependencies`，并会忽略其他工作空间成员或 `uv.toml` 文件中的任何声明。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

```toml title="pyproject.toml"
[tool.uv]
# 始终安装 Werkzeug 2.3.0，无论传递依赖是否请求不同版本。
override-dependencies = ["werkzeug==2.3.0"]
```

---

### [`package`](#package) {: #package }

是否将项目视为 Python 包，或视为非包（“虚拟”）项目。

包会以可编辑模式构建并安装到虚拟环境中，因此需要一个构建后端，而虚拟项目则_不会_被构建或安装；相反，仅其依赖项会被包含在虚拟环境中。

创建包要求 `pyproject.toml` 中存在 `build-system`，并且项目必须遵循符合构建后端期望的结构（例如，`src` 布局）。

**默认值**：`true`

**类型**：`bool`

**示例用法**：

```toml title="pyproject.toml"
[tool.uv]
package = false
```

---

### [`sources`](#sources) {: #sources }

在解析依赖时使用的源。

`tool.uv.sources` 用于通过额外的源增强依赖元数据，这些源在开发过程中被合并。依赖源可以是 Git 仓库、URL、本地路径或替代的注册表。

有关更多信息，请参见 [Dependencies](../concepts/projects/dependencies.md)。

**默认值**：`{}`

**类型**：`dict`

**示例用法**：

```toml title="pyproject.toml"

[tool.uv.sources]
httpx = { git = "https://github.com/encode/httpx", tag = "0.27.0" }
pytest =  { url = "https://files.pythonhosted.org/packages/6b/77/7440a06a8ead44c7757a64362dd22df5760f9b12dc5f11b6188cd2fc27a0/pytest-8.3.3-py3-none-any.whl" }
pydantic = { path = "/path/to/pydantic", editable = true }
```

---

### `workspace`

#### [`exclude`](#workspace_exclude) {: #workspace_exclude }
<span id="exclude"></span>

要排除的工作空间成员包。如果一个包同时匹配 `members` 和 `exclude`，则它会被排除。

支持通配符和显式路径。

有关通配符语法的更多信息，请参考 [`glob` 文档](https://docs.rs/glob/latest/glob/struct.Pattern.html)。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

```toml title="pyproject.toml"
[tool.uv.workspace]
exclude = ["member1", "path/to/member2", "libs/*"]
```

---

#### [`members`](#workspace_members) {: #workspace_members }
<span id="members"></span>

作为工作空间成员包含的包。

支持通配符和显式路径。

有关通配符语法的更多信息，请参阅 [`glob` 文档](https://docs.rs/glob/latest/glob/struct.Pattern.html)。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

```toml title="pyproject.toml"
[tool.uv.workspace]
members = ["member1", "path/to/member2", "libs/*"]
```

---

## 配置
### [`allow-insecure-host`](#allow-insecure-host) {: #allow-insecure-host }

允许不安全的主机连接。

期望接收一个主机名（例如，`localhost`）、一个主机-端口对（例如，`localhost:8080`）或一个 URL（例如，`https://localhost`）。

警告：在此列表中的主机不会通过系统的证书存储进行验证。仅在安全的网络中并且确保来源可信时使用 `--allow-insecure-host`，因为它绕过了 SSL 验证，可能会暴露于中间人攻击（MITM）。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    allow-insecure-host = ["localhost:8080"]
    ```
=== "uv.toml"

    ```toml
    allow-insecure-host = ["localhost:8080"]
    ```

---

### [`cache-dir`](#cache-dir) {: #cache-dir }

缓存目录的路径。

在 macOS 上默认值为 `$HOME/Library/Caches/uv`，在 Linux 上为 `$XDG_CACHE_HOME/uv` 或 `$HOME/.cache/uv`，在 Windows 上为 `%LOCALAPPDATA%\uv\cache`。

**默认值**：`None`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    cache-dir = "./.uv_cache"
    ```
=== "uv.toml"

    ```toml
    cache-dir = "./.uv_cache"
    ```

---

### [`cache-keys`](#cache-keys) {: #cache-keys }

在为项目缓存构建时考虑的键。

缓存键允许你指定哪些文件或目录在修改时应触发重建。默认情况下，uv 会在修改项目目录中的 `pyproject.toml`、`setup.py` 或 `setup.cfg` 文件时重建项目，即：

```toml
cache-keys = [{ file = "pyproject.toml" }, { file = "setup.py" }, { file = "setup.cfg" }]
```

例如：如果项目使用动态元数据从 `requirements.txt` 文件读取其依赖项，你可以指定 `cache-keys = [{ file = "requirements.txt" }, { file = "pyproject.toml" }]` 来确保每当 `requirements.txt` 文件被修改时（除了 `pyproject.toml` 文件的监视之外），项目会被重建。

支持通配符，遵循 [`glob`](https://docs.rs/glob/0.3.1/glob/struct.Pattern.html) crate 的语法。例如，要使项目目录或其任何子目录中的 `.toml` 文件被修改时使缓存失效，可以指定 `cache-keys = [{ file = "**/*.toml" }]`。请注意，使用通配符可能会比较消耗性能，因为 uv 可能需要遍历文件系统来确定是否有文件已更改。

缓存键还可以包括版本控制信息。例如，如果项目使用 `setuptools_scm` 从 Git 提交读取版本，你可以指定 `cache-keys = [{ git = { commit = true }, { file = "pyproject.toml" }]` 来将当前的 Git 提交哈希包括在缓存键中（除了 `pyproject.toml` 文件）。Git 标签也可以通过 `cache-keys = [{ git = { commit = true, tags = true } }]` 支持。

缓存键仅影响由 `pyproject.toml` 定义的项目（而不是例如影响工作空间中的所有成员），并且所有路径和通配符都相对于项目目录进行解释。

**默认值**：`[{ file = "pyproject.toml" }, { file = "setup.py" }, { file = "setup.cfg" }]`

**类型**：`list[dict]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    cache-keys = [{ file = "pyproject.toml" }, { file = "requirements.txt" }, { git = { commit = true }] }
    ```
=== "uv.toml"

    ```toml
    cache-keys = [{ file = "pyproject.toml" }, { file = "requirements.txt" }, { git = { commit = true }] }
    ```

---

### [`compile-bytecode`](#compile-bytecode) {: #compile-bytecode }

在安装后将 Python 文件编译为字节码。

默认情况下，uv 不会将 Python（`.py`）文件编译为字节码（`__pycache__/*.pyc`）；相反，编译会在首次导入模块时惰性地进行。对于启动时间至关重要的使用场景，如命令行应用程序和 Docker 容器，可以启用此选项，以通过延长安装时间来换取更快的启动时间。

启用时，uv 会处理整个 `site-packages` 目录（包括当前操作未修改的包）以确保一致性。与 pip 类似，它也会忽略错误。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    compile-bytecode = true
    ```
=== "uv.toml"

    ```toml
    compile-bytecode = true
    ```

---

### [`concurrent-builds`](#concurrent-builds) {: #concurrent-builds }

uv 在任何给定时间内并发构建的源分发包的最大数量。

默认为可用 CPU 核心的数量。

**默认值**：`None`

**类型**：`int`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    concurrent-builds = 4
    ```
=== "uv.toml"

    ```toml
    concurrent-builds = 4
    ```

---

### [`concurrent-downloads`](#concurrent-downloads) {: #concurrent-downloads }

uv 在任何给定时间内并发进行的最大下载数。

**默认值**：`50`

**类型**：`int`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    concurrent-downloads = 4
    ```
=== "uv.toml"

    ```toml
    concurrent-downloads = 4
    ```

---

### [`concurrent-installs`](#concurrent-installs) {: #concurrent-installs }

安装和解压包时使用的线程数。

默认为可用 CPU 核心的数量。

**默认值**：`None`

**类型**：`int`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    concurrent-installs = 4
    ```
=== "uv.toml"

    ```toml
    concurrent-installs = 4
    ```

---

### [`config-settings`](#config-settings) {: #config-settings }

传递给 [PEP 517](https://peps.python.org/pep-0517/) 构建后端的设置，指定为 `KEY=VALUE` 键值对。

**默认值**：`{}`

**类型**：`dict`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    config-settings = { editable_mode = "compat" }
    ```
=== "uv.toml"

    ```toml
    config-settings = { editable_mode = "compat" }
    ```

---

### [`dependency-metadata`](#dependency-metadata) {: #dependency-metadata }

项目（直接或传递）依赖项的预定义静态元数据。提供此项后，解析器将使用指定的元数据，而不是查询注册表或从源代码构建相关的包。

元数据应遵循 [Metadata 2.3](https://packaging.python.org/en/latest/specifications/core-metadata/) 标准，但只尊重以下字段：

- `name`：包的名称。
- （可选）`version`：包的版本。如果省略，元数据将应用于包的所有版本。
- （可选）`requires-dist`：包的依赖项（例如，`werkzeug>=0.14`）。
- （可选）`requires-python`：包所需的 Python 版本（例如，`>=3.10`）。
- （可选）`provides-extras`：包提供的附加功能。

**默认值**：`[]`

**类型**：`list[dict]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    dependency-metadata = [
        { name = "flask", version = "1.0.0", requires-dist = ["werkzeug"], requires-python = ">=3.6" },
    ]
    ```
=== "uv.toml"

    ```toml
    dependency-metadata = [
        { name = "flask", version = "1.0.0", requires-dist = ["werkzeug"], requires-python = ">=3.6" },
    ]
    ```

---

### [`exclude-newer`](#exclude-newer) {: #exclude-newer }

将候选包限制为在给定日期之前上传的包。

接受 [RFC 3339](https://www.rfc-editor.org/rfc/rfc3339.html) 时间戳（例如，`2006-12-02T02:07:43Z`）以及系统配置时区中的本地日期（例如，`2006-12-02`）。

**默认值**：`None`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    exclude-newer = "2006-12-02"
    ```
=== "uv.toml"

    ```toml
    exclude-newer = "2006-12-02"
    ```

---

### [`extra-index-url`](#extra-index-url) {: #extra-index-url }

除了 `--index-url` 之外，还可以使用额外的包索引 URL。

接受符合 [PEP 503](https://peps.python.org/pep-0503/)（简单仓库 API）规范的仓库，或本地目录，以相同格式布局。

通过此标志提供的所有索引优先于 [`index_url`](#index-url) 或 [`index`](#index) 中指定的 `default = true` 的索引。提供多个索引时，先定义的索引具有更高优先级。

要控制在多个索引存在时 uv 的解析策略，请参见 [`index_strategy`](#index-strategy)。

（已弃用：请改用 `index`。）

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    extra-index-url = ["https://download.pytorch.org/whl/cpu"]
    ```
=== "uv.toml"

    ```toml
    extra-index-url = ["https://download.pytorch.org/whl/cpu"]
    ```

---

### [`find-links`](#find-links) {: #find-links }

除了在注册表索引中找到的包外，还可以在以下位置查找候选发行版。

如果是路径，则目标必须是一个目录，该目录包含以轮子文件（`.whl`）或源代码发行版（如 `.tar.gz` 或 `.zip`）为顶级文件的包。

如果是 URL，则页面必须包含一个平面列表，链接指向符合上述格式的包文件。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    find-links = ["https://download.pytorch.org/whl/torch_stable.html"]
    ```
=== "uv.toml"

    ```toml
    find-links = ["https://download.pytorch.org/whl/torch_stable.html"]
    ```

---

### [`index`](#index) {: #index }

解析依赖时要使用的包索引。

接受符合 [PEP 503](https://peps.python.org/pep-0503/)（简单仓库 API）规范的仓库，或本地目录，且目录布局符合相同的格式。

索引的优先级按照定义的顺序进行考虑，先定义的索引具有最高优先级。此外，通过此设置提供的索引优先于通过 [`index_url`](#index-url) 或 [`extra_index_url`](#extra-index-url) 指定的索引。uv 将只考虑包含给定包的第一个索引，除非指定了其他的 [索引策略](#index-strategy)。

如果索引标记为 `explicit = true`，则该索引将仅用于那些通过 `[tool.uv.sources]` 显式选择它的依赖项，例如：

```toml
[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cu121"
explicit = true

[tool.uv.sources]
torch = { index = "pytorch" }
```

如果索引标记为 `default = true`，则该索引将被移到优先级列表的末尾，在解析包时优先级最低。此外，将索引标记为默认将禁用 PyPI 默认索引。

**默认值**：`"[]"`

**类型**：`dict`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [[tool.uv.index]]
    name = "pytorch"
    url = "https://download.pytorch.org/whl/cu121"
    ```
=== "uv.toml"

    ```toml
    [[tool.uv.index]]
    name = "pytorch"
    url = "https://download.pytorch.org/whl/cu121"
    ```

---

### [`index-strategy`](#index-strategy) {: #index-strategy }

解析多个索引 URL 时使用的策略。

默认情况下，uv 会在找到给定包的第一个索引时停止，并将解析限制为该索引中的包（`first-match`）。这可以防止“依赖混淆”攻击，攻击者可以在另一个索引中上传恶意包，伪装成相同的包名。

**默认值**：`"first-index"`

**可能值**：

- `"first-index"`：只使用第一个返回匹配结果的索引中的结果
- `"unsafe-first-match"`：在所有索引中搜索每个包名，先消耗第一个索引中的所有版本，再继续查找下一个索引
- `"unsafe-best-match"`：在所有索引中搜索每个包名，优先选择找到的“最佳”版本。如果某个包版本出现在多个索引中，只查看第一个索引中的条目

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    index-strategy = "unsafe-best-match"
    ```
=== "uv.toml"

    ```toml
    index-strategy = "unsafe-best-match"
    ```

---

### [`index-url`](#index-url) {: #index-url }

Python 包索引的 URL（默认值：<https://pypi.org/simple>）。

接受符合 [PEP 503](https://peps.python.org/pep-0503/)（简单仓库 API）规范的仓库，或一个本地目录，目录结构符合相同的格式。

通过此设置提供的索引优先级低于通过 [`extra_index_url`](#extra-index-url) 或 [`index`](#index) 指定的任何索引。

（已弃用：请改用 `index`。）

**默认值**：`"https://pypi.org/simple"`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    index-url = "https://test.pypi.org/simple"
    ```
=== "uv.toml"

    ```toml
    index-url = "https://test.pypi.org/simple"
    ```

---

### [`keyring-provider`](#keyring-provider) {: #keyring-provider }

尝试使用 `keyring` 进行索引 URL 的身份验证。

目前，仅支持 `--keyring-provider subprocess`，该选项配置 uv 使用 `keyring` CLI 来处理身份验证。

**默认值**：`"disabled"`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    keyring-provider = "subprocess"
    ```
=== "uv.toml"

    ```toml
    keyring-provider = "subprocess"
    ```

---

### [`link-mode`](#link-mode) {: #link-mode }

从全局缓存安装包时使用的方法。

在 macOS 上默认为 `clone`（即写时复制），在 Linux 和 Windows 上默认为 `hardlink`。

**默认值**：`"clone"（macOS）或 "hardlink"（Linux、Windows）`

**可能值**：

- `"clone"`：从 wheel 文件中克隆（即写时复制）包到 `site-packages` 目录
- `"copy"`：将包从 wheel 文件复制到 `site-packages` 目录
- `"hardlink"`：从 wheel 文件创建硬链接到 `site-packages` 目录
- `"symlink"`：从 wheel 文件创建符号链接到 `site-packages` 目录

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    link-mode = "copy"
    ```
=== "uv.toml"

    ```toml
    link-mode = "copy"
    ```

---

### [`native-tls`](#native-tls) {: #native-tls }

是否从平台的本地证书存储加载 TLS 证书。

默认情况下，uv 从捆绑的 `webpki-roots` crate 加载证书。`webpki-roots` 是一组由 Mozilla 提供的可靠信任根，将其包含在 uv 中提高了可移植性和性能（特别是在 macOS 上）。

然而，在某些情况下，您可能希望使用平台的本地证书存储，尤其是当您依赖于系统证书存储中包含的企业信任根（例如，强制代理）时。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    native-tls = true
    ```
=== "uv.toml"

    ```toml
    native-tls = true
    ```

---

### [`no-binary`](#no-binary) {: #no-binary }

不安装预构建的轮子文件。

指定的包将从源代码构建并安装。解析器仍然会使用预构建的轮子文件来提取包的元数据（如果有的话）。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    no-binary = true
    ```
=== "uv.toml"

    ```toml
    no-binary = true
    ```

---

### [`no-binary-package`](#no-binary-package) {: #no-binary-package }

不为特定包安装预构建的轮子文件。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    no-binary-package = ["ruff"]
    ```
=== "uv.toml"

    ```toml
    no-binary-package = ["ruff"]
    ```

---

### [`no-build`](#no-build) {: #no-build }

不构建源分发包。

启用后，解析时不会运行任意的 Python 代码。已经构建好的源分发包的缓存轮子文件将被重用，但需要构建分发包的操作将会报错退出。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    no-build = true
    ```
=== "uv.toml"

    ```toml
    no-build = true
    ```

---

### [`no-build-isolation`](#no-build-isolation) {: #no-build-isolation }

构建源分发包时禁用隔离。

假设已安装 [PEP 518](https://peps.python.org/pep-0518/) 中指定的构建依赖。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    no-build-isolation = true
    ```
=== "uv.toml"

    ```toml
    no-build-isolation = true
    ```

---

### [`no-build-isolation-package`](#no-build-isolation-package) {: #no-build-isolation-package }

为特定包构建源分发包时禁用隔离。

假设包的构建依赖已按 [PEP 518](https://peps.python.org/pep-0518/) 中指定安装。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    no-build-isolation-package = ["package1", "package2"]
    ```
=== "uv.toml"

    ```toml
    no-build-isolation-package = ["package1", "package2"]
    ```

---

### [`no-build-package`](#no-build-package) {: #no-build-package }

不为特定包构建源分发包。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    no-build-package = ["ruff"]
    ```
=== "uv.toml"

    ```toml
    no-build-package = ["ruff"]
    ```

---

### [`no-cache`](#no-cache) {: #no-cache }

避免从缓存中读取或写入，而是使用一个临时目录来执行操作。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    no-cache = true
    ```
=== "uv.toml"

    ```toml
    no-cache = true
    ```

---

### [`no-index`](#no-index) {: #no-index }

忽略所有注册表索引（例如 PyPI），而是依赖直接 URL 依赖项和通过 `--find-links` 提供的依赖项。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    no-index = true
    ```
=== "uv.toml"

    ```toml
    no-index = true
    ```

---

### [`no-sources`](#no-sources) {: #no-sources }

在解析依赖项时忽略 `tool.uv.sources` 表格。用于锁定符合标准的、可发布的包元数据，而不是使用任何本地或 Git 来源。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    no-sources = true
    ```
=== "uv.toml"

    ```toml
    no-sources = true
    ```

---

### [`offline`](#offline) {: #offline }

禁用网络访问，仅依赖本地缓存数据和本地可用文件。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    offline = true
    ```
=== "uv.toml"

    ```toml
    offline = true
    ```

---

### [`prerelease`](#prerelease) {: #prerelease }

在考虑预发布版本时使用的策略。

默认情况下，uv 将接受仅发布预发布版本的包，以及那些在声明的版本要求中明确包含预发布标记的第一方要求（`if-necessary-or-explicit`）。

**默认值**：`"if-necessary-or-explicit"`

**可能值**：

- `"disallow"`：不允许任何预发布版本
- `"allow"`：允许所有预发布版本
- `"if-necessary"`：如果包的所有版本都是预发布版本，则允许预发布版本
- `"explicit"`：允许第一方包的预发布版本，只要其版本要求中有明确的预发布标记
- `"if-necessary-or-explicit"`：如果包的所有版本都是预发布版本，或包的版本要求中包含明确的预发布标记，则允许预发布版本

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    prerelease = "allow"
    ```
=== "uv.toml"

    ```toml
    prerelease = "allow"
    ```

---

### [`preview`](#preview) {: #preview }

是否启用实验性预览功能。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    preview = true
    ```
=== "uv.toml"

    ```toml
    preview = true
    ```

---

### [`publish-url`](#publish-url) {: #publish-url }

用于将包发布到 Python 包索引的 URL（默认值：<https://upload.pypi.org/legacy/>）。

**默认值**：`"https://upload.pypi.org/legacy/"`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    publish-url = "https://test.pypi.org/legacy/"
    ```
=== "uv.toml"

    ```toml
    publish-url = "https://test.pypi.org/legacy/"
    ```

---

### [`pypy-install-mirror`](#pypy-install-mirror) {: #pypy-install-mirror }

用于下载管理的 PyPy 安装的镜像 URL。

默认情况下，管理的 PyPy 安装从 [downloads.python.org](https://downloads.python.org/) 下载。
此变量可以设置为镜像 URL，使用不同的源来下载 PyPy 安装包。
提供的 URL 将替代 `https://downloads.python.org/pypy`，例如 `https://downloads.python.org/pypy/pypy3.8-v7.3.7-osx64.tar.bz2`。

也可以使用 `file://` URL 方案从本地目录读取发行版。

**默认值**：`None`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    pypy-install-mirror = "https://downloads.python.org/pypy"
    ```
=== "uv.toml"

    ```toml
    pypy-install-mirror = "https://downloads.python.org/pypy"
    ```

---

### [`python-downloads`](#python-downloads) {: #python-downloads }

是否允许 Python 下载。

**默认值**：`"automatic"`

**可能值**：

- `"automatic"`：在需要时自动下载管理的 Python 安装
- `"manual"`：不自动下载管理的 Python 安装；需要显式安装
- `"never"`：绝不允许 Python 下载

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    python-downloads = "manual"
    ```
=== "uv.toml"

    ```toml
    python-downloads = "manual"
    ```

---

### [`python-install-mirror`](#python-install-mirror) {: #python-install-mirror }

用于下载管理的 Python 安装的镜像 URL。

默认情况下，管理的 Python 安装从 [`python-build-standalone`](https://github.com/indygreg/python-build-standalone) 下载。
此变量可以设置为镜像 URL，使用不同的源来下载 Python 安装包。
提供的 URL 将替代 `https://github.com/indygreg/python-build-standalone/releases/download`，例如 `https://github.com/indygreg/python-build-standalone/releases/download/20240713/cpython-3.12.4%2B20240713-aarch64-apple-darwin-install_only.tar.gz`。

也可以使用 `file://` URL 方案从本地目录读取发行版。

**默认值**：`None`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    python-install-mirror = "https://github.com/indygreg/python-build-standalone/releases/download"
    ```
=== "uv.toml"

    ```toml
    python-install-mirror = "https://github.com/indygreg/python-build-standalone/releases/download"
    ```

---

### [`python-preference`](#python-preference) {: #python-preference }

是否优先使用已安装在系统上的 Python 环境，还是优先使用由 uv 下载并安装的 Python 环境。

**默认值**：`"managed"`

**可能值**：

- `"only-managed"`：仅使用管理的 Python 安装；绝不使用系统 Python 安装
- `"managed"`：优先使用管理的 Python 安装，而非系统 Python 安装
- `"system"`：优先使用系统 Python 安装，而非管理的 Python 安装
- `"only-system"`：仅使用系统 Python 安装；绝不使用管理的 Python 安装

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    python-preference = "managed"
    ```
=== "uv.toml"

    ```toml
    python-preference = "managed"
    ```

---

### [`reinstall`](#reinstall) {: #reinstall }

重新安装所有包，不管它们是否已经安装。此设置意味着会执行 `refresh` 操作。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    reinstall = true
    ```
=== "uv.toml"

    ```toml
    reinstall = true
    ```

---

### [`reinstall-package`](#reinstall-package) {: #reinstall-package }

重新安装指定的包，不管它是否已经安装。此设置意味着会执行 `refresh-package` 操作。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    reinstall-package = ["ruff"]
    ```
=== "uv.toml"

    ```toml
    reinstall-package = ["ruff"]
    ```

---

### [`resolution`](#resolution) {: #resolution }

选择满足给定包需求的多个兼容版本时使用的策略。

默认情况下，uv 将使用每个包的最新兼容版本（`highest`）。

**默认值**：`"highest"`

**可能值**：

- `"highest"`：解析每个包的最高兼容版本
- `"lowest"`：解析每个包的最低兼容版本
- `"lowest-direct"`：解析所有直接依赖项的最低兼容版本，并解析所有传递依赖项的最高兼容版本

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    resolution = "lowest-direct"
    ```
=== "uv.toml"

    ```toml
    resolution = "lowest-direct"
    ```

---

### [`trusted-publishing`](#trusted-publishing) {: #trusted-publishing }

通过 GitHub Actions 配置受信任的发布。

默认情况下，uv 在 GitHub Actions 中运行时会检查是否为受信任的发布，但如果未配置或工作流没有足够的权限（例如来自分支的拉取请求），则会忽略此设置。

**默认值**：`automatic`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    trusted-publishing = "always"
    ```
=== "uv.toml"

    ```toml
    trusted-publishing = "always"
    ```

---

### [`upgrade`](#upgrade) {: #upgrade }

允许包的升级，忽略任何现有输出文件中固定的版本。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    upgrade = true
    ```
=== "uv.toml"

    ```toml
    upgrade = true
    ```

---

### [`upgrade-package`](#upgrade-package) {: #upgrade-package }

允许特定包的升级，忽略任何现有输出文件中固定的版本。

接受独立的包名称（例如 `ruff`）和版本说明符（例如 `ruff<0.5.0`）。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv]
    upgrade-package = ["ruff"]
    ```
=== "uv.toml"

    ```toml
    upgrade-package = ["ruff"]
    ```

---

### `pip`

特定于 `uv pip` 命令行界面的设置。

当运行 `uv pip` 命令以外的命令时（例如 `uv lock`，`uvx`），这些值将被忽略。

#### [`all-extras`](#pip_all-extras) {: #pip_all-extras }
<span id="all-extras"></span>

包含所有可选依赖项。

仅适用于 `pyproject.toml`、`setup.py` 和 `setup.cfg` 文件来源。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    all-extras = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    all-extras = true
    ```

---

#### [`allow-empty-requirements`](#pip_allow-empty-requirements) {: #pip_allow-empty-requirements }
<span id="allow-empty-requirements"></span>

允许 `uv pip sync` 使用空的依赖项，这将清空环境中的所有包。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    allow-empty-requirements = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    allow-empty-requirements = true
    ```

---

#### [`annotation-style`](#pip_annotation-style) {: #pip_annotation-style }
<span id="annotation-style"></span>

输出文件中注释的风格，用于指示每个包的来源。

**默认值**：`"split"`

**可能的值**：

- `"line"`：将注释放在单行，并用逗号分隔
- `"split"`：将每个注释放在单独的行上

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    annotation-style = "line"
    ```
=== "uv.toml"

    ```toml
    [pip]
    annotation-style = "line"
    ```

---

#### [`break-system-packages`](#pip_break-system-packages) {: #pip_break-system-packages }
<span id="break-system-packages"></span>

允许 uv 修改 `EXTERNALLY-MANAGED` 的 Python 安装。

警告：`--break-system-packages` 用于持续集成（CI）环境中，当安装到由外部包管理器（如 `apt`）管理的 Python 安装时使用。应谨慎使用，因为此类 Python 安装明确推荐不允许其他包管理器（如 uv 或 pip）进行修改。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    break-system-packages = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    break-system-packages = true
    ```

---

#### [`compile-bytecode`](#pip_compile-bytecode) {: #pip_compile-bytecode }
<span id="compile-bytecode"></span>

安装后编译 Python 文件为字节码。

默认情况下，uv 不会将 Python（`.py`）文件编译为字节码（`__pycache__/*.pyc`）；相反，编译会在第一次导入模块时懒加载执行。对于那些启动时间关键的使用场景（如 CLI 应用程序和 Docker 容器），可以启用此选项，将更长的安装时间换取更快的启动时间。

启用后，uv 会处理整个 `site-packages` 目录（包括当前操作未修改的包）以保证一致性。像 pip 一样，它也会忽略错误。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    compile-bytecode = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    compile-bytecode = true
    ```

---

#### [`config-settings`](#pip_config-settings) {: #pip_config-settings }
<span id="config-settings"></span>

传递给 [PEP 517](https://peps.python.org/pep-0517/) 构建后端的设置，指定为 `KEY=VALUE` 键值对。

**默认值**：`{}`

**类型**：`dict`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    config-settings = { editable_mode = "compat" }
    ```
=== "uv.toml"

    ```toml
    [pip]
    config-settings = { editable_mode = "compat" }
    ```

---

#### [`custom-compile-command`](#pip_custom-compile-command) {: #pip_custom-compile-command }
<span id="custom-compile-command"></span>

在 `uv pip compile` 生成的输出文件顶部包含的头部注释。

用于反映自定义构建脚本和命令，这些脚本和命令包装了 `uv pip compile`。

**默认值**：`None`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    custom-compile-command = "./custom-uv-compile.sh"
    ```
=== "uv.toml"

    ```toml
    [pip]
    custom-compile-command = "./custom-uv-compile.sh"
    ```

---

#### [`dependency-metadata`](#pip_dependency-metadata) {: #pip_dependency-metadata }
<span id="dependency-metadata"></span>

项目依赖项（直接或传递依赖项）的预定义静态元数据。当提供时，解析器将使用指定的元数据，而不是查询注册表或从源代码构建相关包。

元数据应符合 [Metadata 2.3](https://packaging.python.org/en/latest/specifications/core-metadata/) 标准，但仅以下字段会被尊重：

- `name`: 包的名称。
- （可选）`version`: 包的版本。如果省略，元数据将应用于该包的所有版本。
- （可选）`requires-dist`: 包的依赖项（例如，`werkzeug>=0.14`）。
- （可选）`requires-python`: 包所需的 Python 版本（例如，`>=3.10`）。
- （可选）`provides-extras`: 包提供的扩展。

**默认值**：`[]`

**类型**：`list[dict]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    dependency-metadata = [
        { name = "flask", version = "1.0.0", requires-dist = ["werkzeug"], requires-python = ">=3.6" },
    ]
    ```
=== "uv.toml"

    ```toml
    [pip]
    dependency-metadata = [
        { name = "flask", version = "1.0.0", requires-dist = ["werkzeug"], requires-python = ">=3.6" },
    ]
    ```

---

#### [`emit-build-options`](#pip_emit-build-options) {: #pip_emit-build-options }
<span id="emit-build-options"></span>

在 `uv pip compile` 生成的输出文件中包含 `--no-binary` 和 `--only-binary` 条目。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    emit-build-options = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    emit-build-options = true
    ```

---

#### [`emit-find-links`](#pip_emit-find-links) {: #pip_emit-find-links }
<span id="emit-find-links"></span>

在 `uv pip compile` 生成的输出文件中包含 `--find-links` 条目。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    emit-find-links = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    emit-find-links = true
    ```

---

#### [`emit-index-annotation`](#pip_emit-index-annotation) {: #pip_emit-index-annotation }
<span id="emit-index-annotation"></span>

在输出文件中包含注释，指示用于解析每个包的索引（例如，`# 来自 https://pypi.org/simple`）。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    emit-index-annotation = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    emit-index-annotation = true
    ```

---

#### [`emit-index-url`](#pip_emit-index-url) {: #pip_emit-index-url }
<span id="emit-index-url"></span>

在 `uv pip compile` 生成的输出文件中包含 `--index-url` 和 `--extra-index-url` 条目。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    emit-index-url = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    emit-index-url = true
    ```

---

#### [`emit-marker-expression`](#pip_emit-marker-expression) {: #pip_emit-marker-expression }
<span id="emit-marker-expression"></span>

是否输出一个标记字符串，指示在什么条件下，固定的依赖集合是有效的。

即使标记表达式为假，固定的依赖项也可能有效，但当表达式为真时，要求被认为是正确的。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    emit-marker-expression = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    emit-marker-expression = true
    ```

---

#### [`exclude-newer`](#pip_exclude-newer) {: #pip_exclude-newer }
<span id="exclude-newer"></span>

将候选包限制为在给定日期之前上传的包。

接受 [RFC 3339](https://www.rfc-editor.org/rfc/rfc3339.html) 时间戳（例如，`2006-12-02T02:07:43Z`）以及在系统配置时区下的本地日期（例如，`2006-12-02`）。

**默认值**：`None`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    exclude-newer = "2006-12-02"
    ```
=== "uv.toml"

    ```toml
    [pip]
    exclude-newer = "2006-12-02"
    ```

---

#### [`extra`](#pip_extra) {: #pip_extra }
<span id="extra"></span>

包括来自指定额外项的可选依赖项；可以多次提供。

仅适用于 `pyproject.toml`、`setup.py` 和 `setup.cfg` 源。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    extra = ["dev", "docs"]
    ```
=== "uv.toml"

    ```toml
    [pip]
    extra = ["dev", "docs"]
    ```

---

#### [`extra-index-url`](#pip_extra-index-url) {: #pip_extra-index-url }
<span id="extra-index-url"></span>

除了 `--index-url` 之外，还可以使用的额外包索引 URL。

接受符合 [PEP 503](https://peps.python.org/pep-0503/)（简单仓库 API）规范的仓库，或按照相同格式组织的本地目录。

所有通过此标志提供的索引优先级高于 [`index_url`](#index-url) 指定的索引。当提供多个索引时，较早的值具有更高优先级。

要控制 uv 在多个索引存在时的解析策略，请参见 [`index-strategy`](#index-strategy)。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    extra-index-url = ["https://download.pytorch.org/whl/cpu"]
    ```
=== "uv.toml"

    ```toml
    [pip]
    extra-index-url = ["https://download.pytorch.org/whl/cpu"]
    ```

---

#### [`find-links`](#pip_find-links) {: #pip_find-links }
<span id="find-links"></span>

除了注册表索引中找到的包外，还要搜索候选分发的其他位置。

如果是路径，目标必须是包含顶层的 wheel 文件（`.whl`）或源代码分发文件（例如 `.tar.gz` 或 `.zip`）的目录。

如果是 URL，页面必须包含指向符合上述格式的包文件的平面链接列表。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    find-links = ["https://download.pytorch.org/whl/torch_stable.html"]
    ```
=== "uv.toml"

    ```toml
    [pip]
    find-links = ["https://download.pytorch.org/whl/torch_stable.html"]
    ```

---

#### [`generate-hashes`](#pip_generate-hashes) {: #pip_generate-hashes }
<span id="generate-hashes"></span>

在输出文件中包含分发哈希。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    generate-hashes = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    generate-hashes = true
    ```

---

#### [`index-strategy`](#pip_index-strategy) {: #pip_index-strategy }
<span id="index-strategy"></span>

解析多个索引 URL 时使用的策略。

默认情况下，uv 会在第一个索引上找到给定包时停止，并将解析限制在该索引中的包（`first-match`）。这可以防止“依赖混淆”攻击，即攻击者可以将恶意包以相同的名称上传到另一个索引中。

**默认值**：`"first-index"`

**可能的值**：

- `"first-index"`：仅使用第一个返回匹配结果的索引中的包
- `"unsafe-first-match"`：在所有索引中查找每个包的名称，在继续查找下一个索引之前，首先用完第一个索引中的版本
- `"unsafe-best-match"`：在所有索引中查找每个包的名称，优先选择找到的“最佳”版本。如果一个包版本在多个索引中都有，仅查看第一个索引中的条目

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    index-strategy = "unsafe-best-match"
    ```
=== "uv.toml"

    ```toml
    [pip]
    index-strategy = "unsafe-best-match"
    ```

---

#### [`index-url`](#pip_index-url) {: #pip_index-url }
<span id="index-url"></span>

Python 包索引的 URL（默认值：<https://pypi.org/simple>）。

接受符合 [PEP 503](https://peps.python.org/pep-0503/)（简单仓库 API）规范的仓库，或按照相同格式组织的本地目录。

此设置提供的索引优先级低于通过 [`extra_index_url`](#extra-index-url) 指定的任何索引。

**默认值**：`"https://pypi.org/simple"`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    index-url = "https://test.pypi.org/simple"
    ```
=== "uv.toml"

    ```toml
    [pip]
    index-url = "https://test.pypi.org/simple"
    ```

---

#### [`keyring-provider`](#pip_keyring-provider) {: #pip_keyring-provider }
<span id="keyring-provider"></span>

尝试使用 `keyring` 进行索引 URL 的身份验证。

目前，仅支持 `--keyring-provider subprocess`，它将配置 uv 使用 `keyring` CLI 进行身份验证处理。

**默认值**：`disabled`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    keyring-provider = "subprocess"
    ```
=== "uv.toml"

    ```toml
    [pip]
    keyring-provider = "subprocess"
    ```

---

#### [`link-mode`](#pip_link-mode) {: #pip_link-mode }
<span id="link-mode"></span>

从全局缓存安装包时使用的方法。

在 macOS 上默认为 `clone`（也称为写时复制），在 Linux 和 Windows 上默认为 `hardlink`。

**默认值**：`"clone" (macOS) 或 "hardlink" (Linux, Windows)`

**可能的值**：

- `"clone"`：从 wheel 中克隆（即写时复制）包到 `site-packages` 目录
- `"copy"`：将包从 wheel 复制到 `site-packages` 目录
- `"hardlink"`：从 wheel 创建硬链接到 `site-packages` 目录
- `"symlink"`：从 wheel 创建符号链接到 `site-packages` 目录

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    link-mode = "copy"
    ```
=== "uv.toml"

    ```toml
    [pip]
    link-mode = "copy"
    ```

---

#### [`no-annotate`](#pip_no-annotate) {: #pip_no-annotate }
<span id="no-annotate"></span>

从 `uv pip compile` 生成的输出文件中排除注释注解，注解用于指示每个包的来源。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    no-annotate = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    no-annotate = true
    ```

---

#### [`no-binary`](#pip_no-binary) {: #pip_no-binary }
<span id="no-binary"></span>

不安装预构建的 wheel 包。

指定的包将从源代码构建并安装。如果有可用的预构建 wheel 包，解析器仍会使用它们来提取包的元数据。

可以提供多个包。通过 `:all:` 禁用所有包的二进制文件。通过 `:none:` 清除先前指定的包。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    no-binary = ["ruff"]
    ```
=== "uv.toml"

    ```toml
    [pip]
    no-binary = ["ruff"]
    ```

---

#### [`no-build`](#pip_no-build) {: #pip_no-build }
<span id="no-build"></span>

不构建源代码分发包。

启用时，解析过程中不会运行任意的 Python 代码。已经构建好的源代码分发包的缓存 wheel 将被重用，但需要构建分发包的操作将会报错退出。

这是 `--only-binary :all:` 的别名。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    no-build = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    no-build = true
    ```

---

#### [`no-build-isolation`](#pip_no-build-isolation) {: #pip_no-build-isolation }
<span id="no-build-isolation"></span>

在构建源代码分发包时禁用隔离。

假设已经安装了由 [PEP 518](https://peps.python.org/pep-0518/) 指定的构建依赖项。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    no-build-isolation = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    no-build-isolation = true
    ```

---

#### [`no-build-isolation-package`](#pip_no-build-isolation-package) {: #pip_no-build-isolation-package }
<span id="no-build-isolation-package"></span>

在为特定包构建源代码分发包时禁用隔离。

假设该包的构建依赖项已由 [PEP 518](https://peps.python.org/pep-0518/) 指定并且已经安装。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    no-build-isolation-package = ["package1", "package2"]
    ```
=== "uv.toml"

    ```toml
    [pip]
    no-build-isolation-package = ["package1", "package2"]
    ```

---

#### [`no-deps`](#pip_no-deps) {: #pip_no-deps }
<span id="no-deps"></span>

忽略包的依赖项，仅将命令行上明确列出的包添加到生成的要求文件中。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    no-deps = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    no-deps = true
    ```

---

#### [`no-emit-package`](#pip_no-emit-package) {: #pip_no-emit-package }
<span id="no-emit-package"></span>

指定一个包，从输出解析结果中省略该包。它的依赖项仍会包含在解析结果中。相当于 pip-compile 的 `--unsafe-package` 选项。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    no-emit-package = ["ruff"]
    ```
=== "uv.toml"

    ```toml
    [pip]
    no-emit-package = ["ruff"]
    ```

---

#### [`no-extra`](#pip_no-extra) {: #pip_no-extra }
<span id="no-extra"></span>

如果提供了 `all-extras`，则排除指定的可选依赖项。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    all-extras = true
    no-extra = ["dev", "docs"]
    ```
=== "uv.toml"

    ```toml
    [pip]
    all-extras = true
    no-extra = ["dev", "docs"]
    ```

---

#### [`no-header`](#pip_no-header) {: #pip_no-header }
<span id="no-header"></span>

在 `uv pip compile` 生成的输出文件中排除注释头部。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    no-header = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    no-header = true
    ```

---

#### [`no-index`](#pip_no-index) {: #pip_no-index }
<span id="no-index"></span>

忽略所有注册表索引（例如 PyPI），仅依赖直接的 URL 依赖项和通过 `--find-links` 提供的依赖项。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    no-index = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    no-index = true
    ```

---

#### [`no-sources`](#pip_no-sources) {: #pip_no-sources }
<span id="no-sources"></span>

在解析依赖项时忽略 `tool.uv.sources` 表。用于锁定符合标准、可发布的包元数据，而不是使用任何本地或 Git 源。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    no-sources = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    no-sources = true
    ```

---

#### [`no-strip-extras`](#pip_no-strip-extras) {: #pip_no-strip-extras }
<span id="no-strip-extras"></span>

在输出文件中包含额外依赖项（extras）。

默认情况下，uv 会剥离额外依赖项，因为由 extras 引入的任何包已经作为依赖项直接包含在输出文件中。此外，使用 `--no-strip-extras` 生成的输出文件不能在 `install` 和 `sync` 调用中用作约束文件。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    no-strip-extras = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    no-strip-extras = true
    ```

---

#### [`no-strip-markers`](#pip_no-strip-markers) {: #pip_no-strip-markers }
<span id="no-strip-markers"></span>

在 `uv pip compile` 生成的输出文件中包含环境标记。

默认情况下，uv 会去除环境标记，因为 `compile` 生成的解析结果仅在目标环境下被保证是正确的。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    no-strip-markers = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    no-strip-markers = true
    ```

---

#### [`only-binary`](#pip_only-binary) {: #pip_only-binary }
<span id="only-binary"></span>

仅使用预构建的 wheel 文件，不构建源代码分发包。

启用时，解析将不会运行给定包中的代码。已构建的源代码分发包的缓存 wheel 将被重用，但需要构建分发包的操作将以错误退出。

可以提供多个包。通过 `:all:` 禁用所有包的二进制文件。通过 `:none:` 清除之前指定的包。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    only-binary = ["ruff"]
    ```
=== "uv.toml"

    ```toml
    [pip]
    only-binary = ["ruff"]
    ```

---

#### [`output-file`](#pip_output-file) {: #pip_output-file }
<span id="output-file"></span>

将 `uv pip compile` 生成的要求写入指定的 `requirements.txt` 文件。

如果文件已存在，解析时将优先使用现有版本，除非同时指定了 `--upgrade`。

**默认值**：`None`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    output-file = "requirements.txt"
    ```
=== "uv.toml"

    ```toml
    [pip]
    output-file = "requirements.txt"
    ```

---

#### [`prefix`](#pip_prefix) {: #pip_prefix }
<span id="prefix"></span>

将包安装到指定目录下的 `lib`、`bin` 和其他顶级文件夹中，类似于在该位置存在虚拟环境。

通常，建议使用 `--python` 选项将包安装到其他环境中，因为通过 `--prefix` 安装的脚本和其他工件将引用安装时使用的解释器，而不是任何添加到 `--prefix` 目录中的解释器，这样会导致它们变得不可移植。

**默认值**：`None`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    prefix = "./prefix"
    ```
=== "uv.toml"

    ```toml
    [pip]
    prefix = "./prefix"
    ```

---

#### [`prerelease`](#pip_prerelease) {: #pip_prerelease }
<span id="prerelease"></span>

在考虑预发布版本时使用的策略。

默认情况下，uv 将接受仅发布预发布版本的包，以及那些在声明的规格中明确标记为预发布的第一方要求（`if-necessary-or-explicit`）。

**默认值**：`"if-necessary-or-explicit"`

**可能的值**：

- `"disallow"`：不允许任何预发布版本
- `"allow"`：允许所有预发布版本
- `"if-necessary"`：如果某包的所有版本都是预发布版本，则允许预发布版本
- `"explicit"`：允许具有明确预发布版本标记的第一方包的预发布版本
- `"if-necessary-or-explicit"`：如果某包的所有版本都是预发布版本，或者该包的版本要求中有明确的预发布标记，则允许预发布版本

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    prerelease = "allow"
    ```
=== "uv.toml"

    ```toml
    [pip]
    prerelease = "allow"
    ```

---

#### [`python`](#pip_python) {: #pip_python }
<span id="python"></span>

指定包应安装到的 Python 解释器。

默认情况下，uv 将安装到当前工作目录或任何父目录中的虚拟环境中。`--python` 选项允许您指定不同的解释器，主要用于持续集成（CI）环境或其他自动化工作流中。

支持的格式：
- `3.10`：在 Windows 的注册表中查找安装的 Python 3.10（参见 `py --list-paths`），或者在 Linux 和 macOS 上查找 `python3.10`。
- `python3.10` 或 `python.exe`：在 `PATH` 中查找具有给定名称的二进制文件。
- `/home/ferris/.local/bin/python3.10`：使用给定路径的确切 Python 解释器。

**默认值**：`None`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    python = "3.10"
    ```
=== "uv.toml"

    ```toml
    [pip]
    python = "3.10"
    ```

---

#### [`python-platform`](#pip_python-platform) {: #pip_python-platform }
<span id="python-platform"></span>

指定要解析的要求所针对的平台。

以“目标三元组”的形式表示，这是一个描述目标平台的字符串，通常包括其 CPU、厂商和操作系统名称，例如 `x86_64-unknown-linux-gnu` 或 `aarch64-apple-darwin`。

**默认值**：`None`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    python-platform = "x86_64-unknown-linux-gnu"
    ```
=== "uv.toml"

    ```toml
    [pip]
    python-platform = "x86_64-unknown-linux-gnu"
    ```

---

#### [`python-version`](#pip_python-version) {: #pip_python-version }
<span id="python-version"></span>

解析的要求应支持的最低 Python 版本（例如，`3.8` 或 `3.8.17`）。

如果省略了补丁版本，则假定最低补丁版本。例如，`3.8` 将映射为 `3.8.0`。

**默认值**：`None`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    python-version = "3.8"
    ```
=== "uv.toml"

    ```toml
    [pip]
    python-version = "3.8"
    ```

---

#### [`reinstall`](#pip_reinstall) {: #pip_reinstall }
<span id="reinstall"></span>

重新安装所有包，无论它们是否已安装。等同于 `refresh`。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    reinstall = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    reinstall = true
    ```

---

#### [`reinstall-package`](#pip_reinstall-package) {: #pip_reinstall-package }
<span id="reinstall-package"></span>

重新安装指定的包，无论它是否已安装。等同于 `refresh-package`。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    reinstall-package = ["ruff"]
    ```
=== "uv.toml"

    ```toml
    [pip]
    reinstall-package = ["ruff"]
    ```

---

#### [`require-hashes`](#pip_require-hashes) {: #pip_require-hashes }
<span id="require-hashes"></span>

要求每个依赖项提供匹配的哈希值。

哈希检查模式是全有或全无。如果启用，则_所有_依赖项必须提供相应的哈希值或哈希值集合。此外，如果启用，_所有_依赖项必须要么固定到确切的版本（例如 `==1.0.0`），要么通过直接 URL 指定。

哈希检查模式引入了一些附加约束：

- 不支持 Git 依赖项。
- 不支持可编辑安装。
- 不支持本地依赖项，除非它们指向特定的 wheel 文件（`.whl`）或源归档（`.zip`、`.tar.gz`），而不是一个目录。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    require-hashes = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    require-hashes = true
    ```

---

#### [`resolution`](#pip_resolution) {: #pip_resolution }
<span id="resolution"></span>

选择给定包依赖项的不同兼容版本时采用的策略。

默认情况下，uv 会使用每个包的最新兼容版本（`highest`）。

**默认值**：`"highest"`

**可选值**：

- `"highest"`：解析每个包的最高兼容版本
- `"lowest"`：解析每个包的最低兼容版本
- `"lowest-direct"`：解析任何直接依赖项的最低兼容版本，和任何传递依赖项的最高兼容版本

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    resolution = "lowest-direct"
    ```
=== "uv.toml"

    ```toml
    [pip]
    resolution = "lowest-direct"
    ```

---

#### [`strict`](#pip_strict) {: #pip_strict }
<span id="strict"></span>

验证 Python 环境，以检测缺少依赖项的包和其他问题。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    strict = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    strict = true
    ```

---

#### [`system`](#pip_system) {: #pip_system }
<span id="system"></span>

将包安装到系统 Python 环境中。

默认情况下，uv 会安装到当前工作目录或任何父目录中的虚拟环境。`--system` 选项指示 uv 使用系统 `PATH` 中找到的第一个 Python。

警告：`--system` 选项是为持续集成（CI）环境设计的，使用时应谨慎，因为它可能会修改系统的 Python 安装。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    system = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    system = true
    ```

---

#### [`target`](#pip_target) {: #pip_target }
<span id="target"></span>

将包安装到指定的目录，而不是安装到虚拟环境或系统 Python 环境中。包将被安装在该目录的顶层。

**默认值**：`None`

**类型**：`str`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    target = "./target"
    ```
=== "uv.toml"

    ```toml
    [pip]
    target = "./target"
    ```

---

#### [`universal`](#pip_universal) {: #pip_universal }
<span id="universal"></span>

执行通用解析，尝试生成一个兼容所有操作系统、架构和 Python 实现的单一 `requirements.txt` 输出文件。

在通用模式下，当前的 Python 版本（或用户提供的 `--python-version`）将被视为下限。例如，`--universal --python-version 3.7` 将为 Python 3.7 及更高版本生成一个通用解析。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    universal = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    universal = true
    ```

---

#### [`upgrade`](#pip_upgrade) {: #pip_upgrade }
<span id="upgrade"></span>

允许包升级，忽略任何现有输出文件中的固定版本。

**默认值**：`false`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    upgrade = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    upgrade = true
    ```

---

#### [`upgrade-package`](#pip_upgrade-package) {: #pip_upgrade-package }
<span id="upgrade-package"></span>

允许特定包的升级，忽略任何现有输出文件中的固定版本。

接受独立的包名称（`ruff`）和版本说明符（`ruff<0.5.0`）。

**默认值**：`[]`

**类型**：`list[str]`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    upgrade-package = ["ruff"]
    ```
=== "uv.toml"

    ```toml
    [pip]
    upgrade-package = ["ruff"]
    ```

---

#### [`verify-hashes`](#pip_verify-hashes) {: #pip_verify-hashes }
<span id="verify-hashes"></span>

验证在依赖文件中提供的哈希值。

与 `--require-hashes` 不同，`--verify-hashes` 不要求所有依赖项都有哈希值；相反，它将仅验证那些包含哈希值的依赖项。

**默认值**：`true`

**类型**：`bool`

**示例用法**：

=== "pyproject.toml"

    ```toml
    [tool.uv.pip]
    verify-hashes = true
    ```
=== "uv.toml"

    ```toml
    [pip]
    verify-hashes = true
    ```

---


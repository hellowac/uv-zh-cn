# CLI 参考

## uv {: #uv}

一个极快的 Python 包管理器。

<h3 class="cli-reference">用法</h3>

```
uv [选项] <命令>
```

<h3 class="cli-reference">命令</h3>

<dl class="cli-reference"><dt><a href="#uv-run"><code>uv run</code></a></dt><dd><p>运行一个命令或脚本</p>
</dd>
<dt><a href="#uv-init"><code>uv init</code></a></dt><dd><p>创建一个新项目</p>
</dd>
<dt><a href="#uv-add"><code>uv add</code></a></dt><dd><p>向项目添加依赖</p>
</dd>
<dt><a href="#uv-remove"><code>uv remove</code></a></dt><dd><p>从项目中移除依赖</p>
</dd>
<dt><a href="#uv-sync"><code>uv sync</code></a></dt><dd><p>更新项目的环境</p>
</dd>
<dt><a href="#uv-lock"><code>uv lock</code></a></dt><dd><p>更新项目的锁文件</p>
</dd>
<dt><a href="#uv-export"><code>uv export</code></a></dt><dd><p>将项目的锁文件导出为另一种格式</p>
</dd>
<dt><a href="#uv-tree"><code>uv tree</code></a></dt><dd><p>显示项目的依赖树</p>
</dd>
<dt><a href="#uv-tool"><code>uv tool</code></a></dt><dd><p>运行并安装由 Python 包提供的命令</p>
</dd>
<dt><a href="#uv-python"><code>uv python</code></a></dt><dd><p>管理 Python 版本和安装</p>
</dd>
<dt><a href="#uv-pip"><code>uv pip</code></a></dt><dd><p>通过与 pip 兼容的接口管理 Python 包</p>
</dd>
<dt><a href="#uv-venv"><code>uv venv</code></a></dt><dd><p>创建虚拟环境</p>
</dd>
<dt><a href="#uv-build"><code>uv build</code></a></dt><dd><p>将 Python 包构建为源代码分发包和 wheel 包</p>
</dd>
<dt><a href="#uv-publish"><code>uv publish</code></a></dt><dd><p>将分发包上传到索引</p>
</dd>
<dt><a href="#uv-cache"><code>uv cache</code></a></dt><dd><p>管理 uv 的缓存</p>
</dd>
<dt><a href="#uv-self"><code>uv self</code></a></dt><dd><p>管理 uv 可执行文件</p>
</dd>
<dt><a href="#uv-version"><code>uv version</code></a></dt><dd><p>显示 uv 的版本</p>
</dd>
<dt><a href="#uv-help"><code>uv help</code></a></dt><dd><p>显示某个命令的文档</p>
</dd>
</dl>

## uv run {: #uv-run}

运行一个命令或脚本。

确保命令在 Python 环境中运行。

当与以 `.py` 结尾的文件或 HTTP(S) URL 一起使用时，该文件将被视为脚本并使用 Python 解释器运行，即 `uv run file.py` 等价于 `uv run python file.py`。对于 URL，脚本会在执行前临时下载。如果脚本包含内联的依赖元数据，它将在一个隔离的、临时的环境中安装。当与 `-` 一起使用时，输入将从标准输入读取，并作为 Python 脚本处理。

在项目中使用时，项目环境会在执行命令前创建并更新。

在项目外使用时，如果在当前目录或父目录中可以找到虚拟环境，命令将在该环境中运行。否则，命令将在发现的解释器环境中运行。

命令（或脚本）后面的参数不会被视为 uv 的参数。所有 uv 的选项必须在命令之前提供，例如 `uv run --verbose foo`。可以使用 `--` 来分隔命令和 uv 选项，以便清晰标识，例如 `uv run --python 3.12 -- python`。

<h3 class="cli-reference">用法</h3>

```
uv run [选项] [命令]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--all-extras</code></dt><dd><p>包括所有可选依赖。</p>

<p>可选依赖通过 <code>project.optional-dependencies</code> 在 <code>pyproject.toml</code> 中定义。</p>

<p>此选项仅在项目中运行时可用。</p>

</dd><dt><code>--all-groups</code></dt><dd><p>包括所有依赖组中的依赖。</p>

<p><code>--no-group</code> 可用于排除特定组。</p>

</dd><dt><code>--all-packages</code></dt><dd><p>在安装所有工作区成员后运行命令。</p>

<p>工作区的环境（<code>.venv</code>）将更新为包括所有工作区成员。</p>

<p>通过 <code>--extra</code>、<code>--group</code> 或相关选项指定的任何 extras 或 groups 将应用于所有工作区成员。</p>

</dd><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机进行不安全的连接。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如 <code>localhost</code>）、主机-端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：此列表中包含的主机将不会与系统的证书存储进行验证。仅在安全网络中与经过验证的来源一起使用 <code>--allow-insecure-host</code>，因为它会绕过 SSL 验证，可能会暴露于中间人攻击（MITM）。</p>

<p>也可以通过环境变量 <code>UV_INSECURE_HOST</code> 设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认路径为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（在 macOS 和 Linux 上），以及 <code>%LOCALAPPDATA%\uv\cache</code>（在 Windows 上）。</p>

<p>要查看缓存目录的位置，可以运行 <code>uv cache dir</code>。</p>

<p>也可以通过环境变量 <code>UV_CACHE_DIR</code> 设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值: auto]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>: 仅当输出显示在终端或支持的 TTY 上时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--compile-bytecode</code></dt><dd><p>安装后将 Python 文件编译为字节码。</p>

<p>默认情况下，uv 不会将 Python (<code>.py</code>) 文件编译为字节码 (<code>__pycache__/*.pyc</code>)；相反，首次导入模块时会懒编译。对于启动时间至关重要的用例，如 CLI 应用程序和 Docker 容器，可以启用此选项，通过较长的安装时间换取更快的启动时间。</p>

<p>启用时，uv 将处理整个 site-packages 目录（包括当前操作未修改的包）以确保一致性。与 pip 一样，它还会忽略错误。</p>

<p>也可以通过环境变量 <code>UV_COMPILE_BYTECODE</code> 设置。</p>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这样做。</p>

<p>也可以通过环境变量 <code>UV_CONFIG_FILE</code> 设置。</p>
</dd><dt><code>--config-setting</code>, <code>-C</code> <i>config-setting</i></dt><dd><p>传递给 PEP 517 构建后端的设置，以 <code>KEY=VALUE</code> 对的形式指定</p>

</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认值：<https://pypi.org/simple>）。</p>

<p>接受符合 PEP 503（简单仓库 API）的仓库，或以相同格式布局的本地目录。</p>

<p>此标志指定的索引优先级低于通过 <code>--index</code> 标志指定的其他所有索引。</p>

<p>也可以通过环境变量 <code>UV_DEFAULT_INDEX</code> 设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令前切换到指定的目录。</p>

<p>相对路径将以指定目录为基准进行解析。</p>

<p>请参见 <code>--project</code>，仅更改项目根目录。</p>

</dd><dt><code>--env-file</code> <i>env-file</i></dt><dd><p>从 <code>.env</code> 文件加载环境变量。</p>

<p>可以多次提供，后续的文件将覆盖先前文件中定义的值。</p>

<p>也可以通过环境变量 <code>UV_ENV_FILE</code> 设置。</p>
</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>将候选包限制为在给定日期之前上传的包。</p>

<p>接受 RFC 3339 时间戳（例如 <code>2006-12-02T02:07:43Z</code>）以及系统配置时区中的本地日期（例如 <code>2006-12-02</code>）。</p>

<p>也可以通过环境变量 <code>UV_EXCLUDE_NEWER</code> 设置。</p>
</dd><dt><code>--extra</code> <i>extra</i></dt><dd><p>包括指定额外名称的可选依赖。</p>

<p>可以多次提供。</p>

<p>可选依赖通过 <code>project.optional-dependencies</code> 在 <code>pyproject.toml</code> 中定义。</p>

<p>此选项仅在项目中运行时可用。</p>

</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：请使用 <code>--index</code>）额外的包索引 URL，用于补充 <code>--index-url</code>。</p>

<p>接受符合 PEP 503（简单仓库 API）的仓库，或以相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--index-url</code>（默认为 PyPI）指定的索引。当提供多个 <code>--extra-index-url</code> 标志时，较早的值具有更高优先级。</p>

<p>也

可以通过环境变量 <code>UV_EXTRA_INDEX_URL</code> 设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>搜索候选分发位置，除了在注册表索引中找到的分发位置。</p>

<p>如果是路径，目标必须是包含包的目录，包应为 wheel 文件（<code>.whl</code>）或源代码分发包（例如 <code>.tar.gz</code> 或 <code>.zip</code>）放在顶层。</p>

<p>如果是 URL，页面必须包含符合上述格式的包文件链接的平面列表。</p>

<p>也可以通过环境变量 <code>UV_FIND_LINKS</code> 设置。</p>
</dd><dt><code>--frozen</code></dt><dd><p>在不更新 <code>uv.lock</code> 文件的情况下运行。</p>

<p>而不是检查锁文件是否为最新，它使用锁文件中的版本作为事实来源。如果锁文件缺失，uv 将退出并报错。如果 <code>pyproject.toml</code> 包含尚未在锁文件中包含的依赖项更改，这些更改不会出现在环境中。</p>

<p>也可以通过环境变量 <code>UV_FROZEN</code> 设置。</p>
</dd><dt><code>--group</code> <i>group</i></dt><dd><p>包括指定依赖组中的依赖。</p>

<p>可以多次提供。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简洁帮助信息</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>在解析依赖时使用的 URL，除了默认的索引。</p>

<p>接受符合 PEP 503（简单仓库 API）的仓库，或以相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--default-index</code> 指定的索引（默认为 PyPI）。当提供多个 <code>--index</code> 标志时，先前的值优先。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>在解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 将停留在第一个提供指定包的索引上，并限制解析仅限于该索引中的内容（<code>first-match</code>）。这可以防止“依赖混淆”攻击，攻击者可能会将恶意包以相同名称上传到其他索引。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>first-index</code>:  仅使用第一个返回匹配的包名的索引的结果</li>

<li><code>unsafe-first-match</code>:  在所有索引中搜索每个包名，先用完第一个索引中的版本，再转到下一个索引</li>

<li><code>unsafe-best-match</code>:  在所有索引中搜索每个包名，优先选择找到的“最佳”版本。如果一个包的版本存在于多个索引中，则只查看第一个索引中的条目</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：请使用 <code>--default-index</code> 替代）Python 包索引的 URL（默认：&lt;https://pypi.org/simple&gt;）。</p>

<p>接受符合 PEP 503（简单存储库 API）规范的存储库，或采用相同格式布局的本地目录。</p>

<p>此标志指定的索引优先级低于通过 <code>--extra-index-url</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量设置。</p>
</dd><dt><code>--isolated</code></dt><dd><p>在隔离的虚拟环境中运行命令。</p>

<p>通常，为了性能，项目环境会被重用。此选项强制使用一个新的环境来运行项目，严格隔离依赖项和需求声明。</p>

<p>项目仍会使用可编辑的安装。</p>

<p>当与 <code>--with</code> 或 <code>--with-requirements</code> 一起使用时，附加的依赖项将被分层到第二个环境中。</p>

</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行索引 URL 的认证。</p>

<p>目前，仅支持 <code>--keyring-provider subprocess</code>，它配置 uv 使用 <code>keyring</code> CLI 来处理认证。</p>

<p>默认值为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disabled</code>:  不使用 keyring 查找凭据</li>

<li><code>subprocess</code>:  使用 <code>keyring</code> 命令查找凭据</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>安装包时使用的方法。</p>

<p>在 macOS 上默认使用 <code>clone</code>（即写时复制），在 Linux 和 Windows 上默认使用 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>clone</code>:  从 wheel 中克隆（即写时复制）包到 <code>site-packages</code> 目录</li>

<li><code>copy</code>:  将包从 wheel 复制到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>:  从 wheel 创建硬链接到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>:  从 wheel 创建符号链接到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--locked</code></dt><dd><p>确保 <code>uv.lock</code> 文件保持不变。</p>

<p>要求锁定文件是最新的。如果锁定文件丢失或需要更新，uv 将退出并报错。</p>

<p>也可以通过 <code>UV_LOCKED</code> 环境变量设置。</p>
</dd><dt><code>--module</code>, <code>-m</code></dt><dd><p>运行一个 Python 模块。</p>

<p>等同于 <code>python -m &lt;module&gt;</code>。</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储中加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 中加载证书。<code>webpki-roots</code> 是一组来自 Mozilla 的可靠信任根，包含它们能够提高 uv 的可移植性和性能（特别是在 macOS 上）。</p>

<p>但是，在某些情况下，您可能希望使用平台的本地证书存储，尤其是当您依赖于系统证书存储中包含的企业信任根（例如，用于强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置。</p>
</dd><dt><code>--no-binary</code></dt><dd><p>不安装预构建的 wheel 包。</p>

<p>指定的包将从源代码构建并安装。解析器仍然会使用预构建的 wheel 包来提取包的元数据（如果可用）。</p>

</dd><dt><code>--no-binary-package</code> <i>no-binary-package</i></dt><dd><p>不为特定包安装预构建的 wheel 包。</p>

</dd><dt><code>--no-build</code></dt><dd><p>不构建源代码分发。</p>

<p>启用时，解析过程将不会运行任意的 Python 代码。已经构建好的源代码分发的缓存 wheel 包将被重用，但需要构建分发的操作将会退出并报错。</p>

</dd><dt><code>--no-build-isolation</code></dt><dd><p>禁用构建源代码分发时的隔离。</p>

<p>假设 PEP 518 指定的构建依赖项已安装。</p>

<p>也可以通过 <code>UV_NO_BUILD_ISOLATION</code> 环境变量设置。</p>
</dd><dt><code>--no-build-isolation-package</code> <i>no-build-isolation-package</i></dt><dd><p>禁用为特定包构建源代码分发时的隔离。</p>

<p>假设该包的构建依赖项已通过 PEP 518 指定并安装。</p>

</dd><dt><code>--no-build-package</code> <i>no-build-package</i></dt><dd><p>不为特定包构建源代码分发。</p>

</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录进行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量设置。</p>
</dd><dt><code>--no-dev</code></dt><dd><p>省略开发依赖组。</p>

<p>此选项是 <code>--no-group dev</code> 的别名。</p>

<p>此选项仅在运行项目时可用。</p>

</dd><dt><code>--no-editable</code></dt><dd><p>安装任何可编辑的依赖项，包括项目及任何工作区成员，作为非可编辑的依赖项。</p>

</dd><dt><code>--no-env-file</code></dt><dd><p>避免从 <code>.env</code> 文件读取环境变量。</p>

<p>也可以通过 <code>UV_NO_ENV_FILE</code> 环境变量设置。</p>
</dd><dt><code>--no-extra</code> <i>no-extra</i></dt><dd><p>如果提供了 <code>--all-extras</code>，则排除指定的可选依赖项。</p>

<p>可以多次提供此选项。</p>

</dd><dt><code>--no-group</code> <i>no-group</i></dt><dd><p>排除指定依赖组中的依赖项。</p>

<p>可以多次提供此选项。</p>

</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如，PyPI），改为依赖于直接的 URL 依赖项和通过 <code>--find-links</code> 提供的依赖项。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，进度指示器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--no-project</code></dt><dd><p>避免发现项目或工作区。</p>

<p>此时将不会在当前目录或父目录中搜索项目，而是以隔离的、临时的环境运行，由 <code>--with</code> 需求填充。</p>

<p>如果存在活动的虚拟环境或在当前或父目录中找到虚拟环境，则将其用作没有项目或工作区的情况下。</p>

</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 自动下载。</p>

</dd><dt><code>--no-sources</code></dt><dd><p>在解析依赖项时忽略 <code>tool.uv.sources</code> 表。用于锁定标准兼容的、可发布的包元数据，而不是使用任何本地或 Git 源。</p>

</dd><dt><code>--no-sync</code></dt><dd><p>避免同步虚拟环境。</p>

<p>此选项隐含 <code>--frozen</code>，因为项目依赖项将被忽略（即，锁定文件将不会更新，因为环境将不会被同步）。</p>

<p>也可以通过 <code>UV_NO_SYNC</code> 环境变量设置。</p>
</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

翻译：

<p>禁用时，uv 仅会使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--only-dev</code></dt><dd><p>仅包含开发依赖组。</p>

<p>省略其他依赖项。项目本身也将被省略。</p>

<p>此选项是 <code>--only-group dev</code> 的别名。</p>

</dd><dt><code>--only-group</code> <i>only-group</i></dt><dd><p>仅包含指定依赖组中的依赖项。</p>

<p>可以多次提供此选项。</p>

<p>项目本身也将被省略。</p>

</dd><dt><code>--package</code> <i>package</i></dt><dd><p>在工作区中的特定包内运行命令。</p>

<p>如果工作区成员不存在，uv 将退出并报错。</p>

</dd><dt><code>--prerelease</code> <i>prerelease</i></dt><dd><p>考虑预发布版本时使用的策略。</p>

<p>默认情况下，uv 将接受仅发布预发布版本的包，以及那些在声明的指定符中包含显式预发布标记的第一方依赖（<code>if-necessary-or-explicit</code>）。</p>

<p>也可以通过 <code>UV_PRERELEASE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disallow</code>:  禁止所有预发布版本</li>

<li><code>allow</code>:  允许所有预发布版本</li>

<li><code>if-necessary</code>:  如果包的所有版本都是预发布版本，则允许预发布版本</li>

<li><code>explicit</code>:  允许为具有显式预发布标记的第一方包使用预发布版本</li>

<li><code>if-necessary-or-explicit</code>:  如果包的所有版本都是预发布版本，或包在版本要求中有显式预发布标记，则允许预发布版本</li>
</ul>
</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树进行发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>查看 <code>--directory</code> 以完全更改工作目录。</p>

<p>此设置在 <code>uv pip</code> 接口中无效。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>为运行环境指定使用的 Python 解释器。</p>

<p>如果解释器请求能由发现的环境满足，则该环境将被使用。</p>

<p>请参阅 <a href="#uv-python">uv python</a> 查看支持的请求格式。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装，还是系统安装的 Python。</p>

<p>默认情况下，uv 优先使用它所管理的 Python 版本。但如果未安装 uv 管理的 Python，它将使用系统安装的 Python。此选项允许优先使用或忽略系统安装的 Python。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>:  仅使用由 uv 管理的 Python 安装；绝不使用系统 Python 安装</li>

<li><code>managed</code>:  优先使用由 uv 管理的 Python 安装，而非系统 Python 安装</li>

<li><code>system</code>:  优先使用系统 Python 安装，而非由 uv 管理的 Python 安装</li>

<li><code>only-system</code>:  仅使用系统 Python 安装；绝不使用由 uv 管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--refresh</code></dt><dd><p>刷新所有缓存数据</p>

</dd><dt><code>--refresh-package</code> <i>refresh-package</i></dt><dd><p>刷新特定包的缓存数据</p>

</dd><dt><code>--reinstall</code></dt><dd><p>重新安装所有包，不管它们是否已安装。隐式使用 <code>--refresh</code></p>

</dd><dt><code>--reinstall-package</code> <i>reinstall-package</i></dt><dd><p>重新安装特定包，不管它是否已安装。隐式使用 <code>--refresh-package</code></p>

</dd><dt><code>--resolution</code> <i>resolution</i></dt><dd><p>在为给定的包需求选择兼容版本时使用的策略。</p>

<p>默认情况下，uv 将使用每个包的最新兼容版本（<code>highest</code>）。</p>

<p>也可以通过 <code>UV_RESOLUTION</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>highest</code>:  解析每个包的最高兼容版本</li>

<li><code>lowest</code>:  解析每个包的最低兼容版本</li>

<li><code>lowest-direct</code>:  解析任何直接依赖的最低兼容版本，解析任何传递依赖的最高兼容版本</li>
</ul>
</dd><dt><code>--script</code>, <code>-s</code></dt><dd><p>将给定路径作为 Python 脚本运行。</p>

<p>使用 <code>--script</code> 时，会尝试将路径解析为 PEP 723 脚本，无论其扩展名如何。</p>

</dd><dt><code>--upgrade</code>, <code>-U</code></dt><dd><p>允许包的升级，忽略现有输出文件中固定版本的限制。隐式使用 <code>--refresh</code></p>

</dd><dt><code>--upgrade-package</code>, <code>-P</code> <i>upgrade-package</i></dt><dd><p>允许特定包的升级，忽略现有输出文件中固定版本的限制。隐式使用 <code>--refresh-package</code></p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>你可以使用 <code>RUST_LOG</code> 环境变量配置更细粒度的日志记录。 (&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd><dt><code>--with</code> <i>with</i></dt><dd><p>使用已安装的给定包运行。</p>

<p>当在项目中使用时，这些依赖项将被叠加在项目环境之上，形成一个独立的临时环境。这些依赖项可以与项目中指定的依赖项发生冲突。</p>

</dd><dt><code>--with-editable</code> <i>with-editable</i></dt><dd><p>使用已安装的可编辑包运行。</p>

<p>当在项目中使用时，这些依赖项将被叠加在项目环境之上，形成一个独立的临时环境。这些依赖项可以与项目中指定的依赖项发生冲突。</p>

</dd><dt><code>--with-requirements</code> <i>with-requirements</i></dt><dd><p>使用给定的 <code>requirements.txt</code> 文件中列出的所有包运行。</p>

<p>与 <code>--with</code> 相同的环境语义适用。</p>

<p>不允许使用 <code>pyproject.toml</code>、<code>setup.py</code> 或 <code>setup.cfg</code> 文件。</p>

</dd></dl>

## uv init

创建一个新项目。

遵循 `pyproject.toml` 规范。

如果目标位置已存在 `pyproject.toml`，uv 将以错误退出。

如果在目标路径的任何父目录中找到 `pyproject.toml`，项目将作为工作区成员添加到父目录中。

某些项目状态在需要时才会创建，例如项目的虚拟环境（`.venv`）和锁文件（`uv.lock`），这些会在第一次同步时懒加载创建。

<h3 class="cli-reference">用法</h3>

```
uv init [OPTIONS] [PATH]
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>PATH</code></dt><dd><p>用于项目/脚本的路径。</p>

<p>在初始化应用或库时默认使用当前工作目录；初始化脚本时则需要指定路径。接受相对路径和绝对路径。</p>

<p>如果在目标路径的任何父目录中找到 `pyproject.toml`，项目将作为工作区成员添加到父目录中，除非提供了 <code>--no-workspace</code>。</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机进行不安全的连接。</p>

<p>可以多次提供此选项。</p>

<p>期望接收主机名（如 <code>localhost</code>）、主机-端口对（如 <code>localhost:8080</code>）或 URL（如 <code>https://localhost</code>）。</p>

<p>警告：在此列表中的主机将不会与系统的证书存储进行验证。仅在安全的网络和验证过的来源中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会让你面临中间人攻击（MITM）的风险。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--app</code></dt><dd><p>为应用创建一个项目。</p>

<p>如果未请求 <code>--lib</code>，这是默认行为。</p>

<p>此项目类型适用于 Web 服务器、脚本和命令行界面。</p>

<p>默认情况下，应用程序不打算作为 Python 包进行构建和分发。可以使用 <code>--package</code> 选项创建一个可以分发的应用程序，例如，如果你希望通过 PyPI 分发命令行界面。</p>

</dd><dt><code>--author-from</code> <i>author-from</i></dt><dd><p>填充 <code>pyproject.toml</code> 中的 <code>authors</code> 字段。</p>

<p>默认情况下，uv 会尝试从某些来源（如 Git）推断作者信息（<code>auto</code>）。使用 <code>--author-from git</code> 仅从 Git 配置中推断作者信息。使用 <code>--author-from none</code> 以避免推断作者信息。</p>

<p>可能的值：</p>

<ul>
<li><code>auto</code>:  自动从某些来源（如 Git）获取作者信息</li>

<li><code>git</code>:  仅从 Git 配置获取作者信息</li>

<li><code>none</code>:  不推断作者信息</li>
</ul>
</dd><dt><code>--build-backend</code> <i>build-backend</i></dt><dd><p>为项目初始化所选的构建后端。</p>

<p>隐式设置 <code>--package</code>。</p>

<p>可能的值：</p>

<ul>
<li><code>hatch</code>:  使用 <a href='https://pypi.org/project/hatchling'>hatchling</a> 作为项目构建后端</li>

<li><code>flit</code>:  使用 <a href='https://pypi.org/project/flit-core'>flit-core</a> 作为项目构建后端</li>

<li><code>pdm</code>:  使用 <a href='https://pypi.org/project/pdm-backend'>pdm-backend</a> 作为项目构建后端</li>

<li><code>setuptools</code>:  使用 <a href='https://pypi.org/project/setuptools'>setuptools</a> 作为项目构建后端</li>

<li><code>maturin</code>:  使用 <a href='https://pypi.org/project/maturin'>maturin</a> 作为项目构建后端</li>

<li><code>scikit</code>:  使用 <a href='https://pypi.org/project/scikit-build-core'>scikit-build-core</a> 作为项目构建后端</li>
</ul>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code>，或在 macOS 和 Linux 上为 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，可以运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认: auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>:  仅在输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>:  无论检测到的环境如何，始终启用彩色输出</li>

<li><code>never</code>:  禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>要使用的 <code>uv.toml</code> 配置文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许使用。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以给定目录作为基准进行解析。</p>

<p>请参阅 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示该命令的简要帮助</p>

</dd><dt><code>--lib</code></dt><dd><p>为库创建一个项目。</p>

<p>库是一个旨在作为 Python 包进行构建和分发的项目。</p>

</dd><dt><code>--name</code> <i>name</i></dt><dd><p>项目的名称。</p>

<p>默认值为目录的名称。</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 会从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是来自 Mozilla 的可信根集合，将其包含在 uv 中可以提高可移植性和性能（特别是在 macOS 上）。</p>

<p>但是，在某些情况下，你可能希望使用平台的本地证书存储，特别是当你依赖公司信任根（例如，强制代理）时，这些根证书包含在系统的证书存储中。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录来执行操作</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>, <code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-package</code></dt><dd><p>不要将项目设置为作为 Python 包进行构建。</p>

<p>不会为项目包含 <code>[build-system]</code> 部分。</p>

<p>在使用 <code>--app</code> 时，这是默认行为。</p>

</dd><dt><code>--no-pin-python</code></dt><dd><p>不要为项目创建 <code>.python-version</code> 文件。</p>

<p>默认情况下，uv 会创建一个包含发现的 Python 解释器小版本号的 <code>.python-version</code> 文件，这会使得后续的 uv 命令使用该版本。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--no-readme</code></dt><dd><p>不创建 <code>README.md</code> 文件</p>

</dd><dt><code>--no-workspace</code></dt><dd><p>避免发现工作区，创建独立的项目。</p>

<p>默认情况下，uv 会在当前目录或任何父目录中搜索工作区。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>当禁用时，uv 将仅使用本地缓存数据和本地可用文件。</p>

</dd><dt><code>--package</code></dt><dd><p>将项目设置为作为 Python 包进行构建。</p>

<p>为项目定义 <code>[build-system]</code> 部分。</p>

<p>在使用 <code>--lib</code> 或 <code>--build-backend</code> 时，这是默认行为。</p>

<p>在使用 <code>--app</code> 时，这将包括一个 <code>[project.scripts]</code> 入口点，并使用 <code>src/</code> 项目结构。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

翻译：

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树进行发现，项目的虚拟环境 (<code>.venv</code>) 也是如此。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>请参见 <code>--directory</code> 以完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>用于确定最小支持的 Python 版本的 Python 解释器。</p>

<p>请参阅 <a href="#uv-python">uv python</a> 查看支持的请求格式。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装。</p>

<p>默认情况下，uv 更倾向于使用它管理的 Python 版本。如果没有安装 uv 管理的 Python 版本，则会使用系统的 Python 安装。此选项允许优先使用或忽略系统的 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>:  仅使用 uv 管理的 Python 安装；永不使用系统的 Python 安装</li>

<li><code>managed</code>:  优先使用 uv 管理的 Python 安装，而不是系统的 Python 安装</li>

<li><code>system</code>:  优先使用系统的 Python 安装，而不是 uv 管理的 Python 安装</li>

<li><code>only-system</code>:  仅使用系统的 Python 安装；永不使用 uv 管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--script</code></dt><dd><p>创建一个脚本。</p>

<p>脚本是一个独立的文件，内嵌元数据列出了它的依赖项，以及任何 Python 版本要求，按照 PEP 723 规范定义。</p>

<p>PEP 723 脚本可以通过 <code>uv run</code> 直接执行。</p>

<p>默认情况下，添加对系统 Python 版本的要求；使用 <code>--python</code> 指定替代的 Python 版本要求。</p>

</dd><dt><code>--vcs</code> <i>vcs</i></dt><dd><p>为项目初始化版本控制系统。</p>

<p>默认情况下，uv 会初始化一个 Git 仓库（<code>git</code>）。使用 <code>--vcs none</code> 显式避免初始化版本控制系统。</p>

<p>可能的值：</p>

<ul>
<li><code>git</code>:  使用 Git 作为版本控制系统</li>

<li><code>none</code>:  不使用任何版本控制系统</li>
</ul>
</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>你可以通过 <code>RUST_LOG</code> 环境变量配置细粒度日志记录。(&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本</p>

</dd></dl>

## uv add

向项目添加依赖。

依赖将添加到项目的 `pyproject.toml` 文件中。

如果给定的依赖已经存在，它将更新为新的版本规范，除非它包含与现有规范不同的标记，在这种情况下将为该依赖项添加另一个条目。

如果未为依赖提供约束或 URL，则会添加一个下限，等于该包的最新兼容版本，例如 `>=1.2.3`，除非提供了 `--frozen`，在这种情况下不执行解析。

锁定文件和项目环境将更新，以反映所添加的依赖。要跳过更新锁定文件，请使用 `--frozen`。要跳过更新环境，请使用 `--no-sync`。

如果任何请求的依赖项无法找到，uv 将退出并报错，除非提供了 `--frozen` 标志，在这种情况下 uv 会原样添加这些依赖，而不检查它们是否存在或与项目兼容。

uv 会在当前目录或任何父目录中搜索项目。如果找不到项目，uv 将退出并报错。

<h3 class="cli-reference">用法</h3>

```
uv add [OPTIONS] <PACKAGES|--requirements <REQUIREMENTS>>
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>PACKAGES</code></dt><dd><p>要添加的包，以 PEP 508 规范的要求形式提供（例如，<code>ruff==0.5.0</code>）</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机建立不安全的连接。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如 <code>localhost</code>）、主机端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：列表中的主机将不会与系统的证书存储进行验证。仅在安全的网络中使用 <code>--allow-insecure-host</code>，并确保源已验证，因为它会绕过 SSL 验证，可能会暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置。</p>
</dd><dt><code>--branch</code> <i>branch</i></dt><dd><p>从 Git 添加依赖时使用的分支</p>

</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（在 macOS 和 Linux 上），以及 <code>%LOCALAPPDATA%\uv\cache</code>（在 Windows 上）。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值：auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅在输出转到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，始终启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--compile-bytecode</code></dt><dd><p>安装后将 Python 文件编译为字节码。</p>

<p>默认情况下，uv 不会将 Python（<code>.py</code>）文件编译为字节码（<code>__pycache__/*.pyc</code>）；相反，它会在首次导入模块时惰性地执行编译。在启动时间关键的用例中（如 CLI 应用程序和 Docker 容器），可以启用此选项，通过更长的安装时间来换取更快的启动时间。</p>

<p>启用时，uv 将处理整个 site-packages 目录（包括当前操作未修改的包），以确保一致性。像 pip 一样，它也会忽略错误。</p>

<p>也可以通过 <code>UV_COMPILE_BYTECODE</code> 环境变量进行设置。</p>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>要使用的 <code>uv.toml</code> 配置文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这样做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--config-setting</code>, <code>-C</code> <i>config-setting</i></dt><dd><p>作为 <code>KEY=VALUE</code> 键值对传递给 PEP 517 构建后端的设置。</p>

</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认值：<code>https://pypi.org/simple</code>）。</p>

<p>接受符合 PEP 503（简单存储库 API）标准的仓库，或者按照相同格式布局的本地目录。</p>

<p>通过此标志指定的索引优先级低于通过 <code>--index</code> 标志指定的其他所有索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--dev</code></dt><dd><p>将依赖项添加到开发依赖组。</p>

<p>此选项是 <code>--group dev</code> 的别名。</p>

</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径会以指定目录作为基准进行解析。</p>

<p>参见 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--editable</code></dt><dd><p>将依赖项添加为可编辑的</p>

</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>将候选包限制为在给定日期之前上传的版本。</p>

<p>接受 RFC 3339 时间戳（例如，<code>2006-12-02T02:07:43Z</code>）以及系统配置时区中的本地日期（例如，<code>2006-12-02</code>）。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量进行设置。</p>
</dd><dt><code>--extra</code> <i>extra</i></dt><dd><p>为依赖项启用的额外功能。</p>

<p>可以多次提供。</p>

<p>要将此依赖项添加到可选的附加功能中，请参见 <code>--optional</code>。</p>

</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：请改用 <code>--index</code>）额外的包索引 URL，除了 <code>--index-url</code> 之外。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或按照相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先级高于通过 <code>--index-url</code> 指定的索引（默认为 PyPI）。如果提供多个 <code>--extra-index-url</code> 标志，则较早的值优先。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>除了在注册表索引中找到的包外，还要搜索候选分发的位置。</p>

<p>如果是路径，则目标必须是一个包含 wheel 文件（<code>.whl</code>）或源分发包（例如，<code>.tar.gz</code> 或 <code>.zip</code>）的目录，并且这些包必须位于目录的顶层。</p>

<p>如果是 URL，则页面必须包含指向符合上述格式的包文件的链接的平面列表。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量进行设置。</p>
</dd><dt><code>--frozen</code></dt><dd><p>添加依赖项而不重新锁定项目。</p>

<p>项目环境将不会同步。</p>

<p>也可以通过 <code>UV_FROZEN</code> 环境变量进行设置。</p>
</dd><dt><code>--group</code> <i>group</i></dt><dd><p>将依赖项添加到指定的依赖组。</p>

<p>这些依赖项将不会包含在项目的发布元数据中。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简洁帮助</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>解析依赖项时使用的 URL，除了默认索引之外。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或按照相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先级高于通过 <code>--default-index</code> 指定的索引（默认为 PyPI）。如果提供多个 <code>--index</code> 标志，则较早的值优先。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 会在给定包可用的第一个索引处停止，并将解析限制为该索引中存在的版本（<code>first-match</code>）。这样可以防止“依赖混淆”攻击，其中攻击者可以将恶意包上传到一个替代的索引，并使用相同的包名。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>first-index</code>: 仅使用第一个返回匹配项的索引中的结果</li>

<li><code>unsafe-first-match</code>: 在所有索引中搜索每个包名，先用完第一个索引中的版本，然后才移动到下一个索引</li>

<li><code>unsafe-best-match</code>: 在所有索引中搜索每个包名，优先选择找到的“最佳”版本。如果一个包版本出现在多个索引中，则只查看第一个索引中的条目</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：请改用 <code>--default-index</code>）Python 包索引的 URL（默认为：<code>https://pypi.org/simple</code>）。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或按照相同格式布局的本地目录。</p>

<p>通过此标志指定的索引优先级低于通过 <code>--extra-index-url</code> 标志指定的所有索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行索引 URL 的身份验证。</p>

<p>目前，仅支持 <code>--keyring-provider subprocess</code>，它配置 uv 使用 <code>keyring</code> CLI 来处理身份验证。</p>

<p>默认为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disabled</code>: 不使用 keyring 进行凭证查找</li>

<li><code>subprocess</code>: 使用 <code>keyring</code> 命令进行凭证查找</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>安装包时使用的方法，从全局缓存中获取。</p>

<p>在 macOS 上默认为 <code>clone</code>（也称为写时复制），在 Linux 和 Windows 上默认为 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>clone</code>: 从 wheel 文件克隆（即写时复制）包到 <code>site-packages</code> 目录</li>

<li><code>copy</code>: 从 wheel 文件复制包到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>: 从 wheel 文件创建硬链接到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>: 从 wheel 文件创建符号链接到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--locked</code></dt><dd><p>确保 <code>uv.lock</code> 文件保持不变。</p>

<p>要求锁文件是最新的。如果锁文件缺失或需要更新，uv 会以错误退出。</p>

<p>也可以通过 <code>UV_LOCKED</code> 环境变量进行设置。</p>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 会从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是一组来自 Mozilla 的可信根证书，将其包含在 uv 中可提高便携性和性能（特别是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，特别是当您依赖于一个企业信任根（例如，用于强制代理的信任根），该根包含在系统的证书存储中时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-binary</code></dt><dd><p>不安装预构建的 wheel 文件。</p>

<p>给定的包将从源代码构建并安装。解析器仍会使用预构建的 wheel 文件来提取包的元数据（如果有的话）。</p>

</dd><dt><code>--no-binary-package</code> <i>no-binary-package</i></dt><dd><p>不为特定包安装预构建的 wheel 文件</p>

</dd><dt><code>--no-build</code></dt><dd><p>不构建源分发包。</p>

<p>启用时，解析将不会运行任意 Python 代码。已经构建的源分发包的缓存 wheel 将被重用，但需要构建分发包的操作将以错误退出。</p>

</dd><dt><code>--no-build-isolation</code></dt><dd><p>构建源分发包时禁用隔离。</p>

<p>假设 PEP 518 中指定的构建依赖已经安装。</p>

<p>也可以通过 <code>UV_NO_BUILD_ISOLATION</code> 环境变量进行设置。</p>
</dd><dt><code>--no-build-isolation-package</code> <i>no-build-isolation-package</i></dt><dd><p>为特定包构建源分发包时禁用隔离。</p>

<p>假设该包的构建依赖项（由 PEP 518 指定）已经安装。</p>

</dd><dt><code>--no-build-package</code> <i>no-build-package</i></dt><dd><p>不为特定包构建源分发包</p>

</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录进行操作</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>、<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如 PyPI），改为依赖直接的 URL 依赖和通过 <code>--find-links</code> 提供的依赖</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--no-sources</code></dt><dd><p>在解析依赖时忽略 <code>tool.uv.sources</code> 表。用于锁定标准兼容的、可发布的包元数据，而不是使用任何本地或 Git 源</p>

</dd><dt><code>--no-sync</code></dt><dd><p>避免同步虚拟环境</p>

<p>也可以通过 <code>UV_NO_SYNC</code> 环境变量进行设置。</p>
</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 仅会使用本地缓存数据和本地可用文件。</p>

</dd><dt><code>--optional</code> <i>optional</i></dt><dd><p>将依赖项添加到包的指定附加项的可选依赖项中。</p>

<p>然后可以在安装项目时通过 <code>--extra</code> 标志激活该组。</p>

<p>要为此要求启用可选附加项，请参见 <code>--extra</code>。</p>

</dd><dt><code>--package</code> <i>package</i></dt><dd><p>将依赖项添加到工作区的特定包中</p>

</dd><dt><code>--prerelease</code> <i>prerelease</i></dt><dd><p>在考虑预发布版本时使用的策略。</p>

<p>默认情况下，uv 会接受仅发布预发布版本的包以及那些在声明的说明符中包含显式预发布标记的首方依赖（<code>if-necessary-or-explicit</code>）。</p>

<p>也可以通过 <code>UV_PRERELEASE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disallow</code>: 禁止所有预发布版本</li>

<li><code>allow</code>: 允许所有预发布版本</li>

<li><code>if-necessary</code>: 如果所有版本都是预发布版本，则允许预发布版本</li>

<li><code>explicit</code>: 仅允许具有显式预发布标记的首方包的预发布版本</li>

<li><code>if-necessary-or-explicit</code>: 如果所有版本都是预发布版本，或包在其版本要求中有显式的预发布标记，则允许预发布版本</li>
</ul>
</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在指定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>请参见 <code>--directory</code> 完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>用于解析和同步的 Python 解释器。</p>

<p>有关 Python 发现和支持的请求格式的详细信息，请参见 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装。</p>

<p>默认情况下，uv 优先使用其管理的 Python 版本。如果没有安装 uv 管理的 Python，则会使用系统的 Python 安装。此选项允许优先考虑或忽略系统的 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用 uv 管理的 Python 安装；永不使用系统的 Python 安装</li>

<li><code>managed</code>: 优先使用 uv 管理的 Python 安装，而不是系统的 Python 安装</li>

<li><code>system</code>: 优先使用系统的 Python 安装，而不是 uv 管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统的 Python 安装；永不使用 uv 管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--raw-sources</code></dt><dd><p>将源要求添加到 <code>project.dependencies</code> 中，而不是 <code>tool.uv.sources</code>。</p>

<p>默认情况下，uv 会使用 <code>tool.uv.sources</code> 部分来记录 Git、本地、可编辑和直接 URL 要求的源信息。</p>

</dd><dt><code>--refresh</code></dt><dd><p>刷新所有缓存数据</p>

</dd><dt><code>--refresh-package</code> <i>refresh-package</i></dt><dd><p>刷新特定包的缓存数据</p>

</dd><dt><code>--reinstall</code></dt><dd><p>重新安装所有包，无论它们是否已经安装。等同于 <code>--refresh</code></p>

</dd><dt><code>--reinstall-package</code> <i>reinstall-package</i></dt><dd><p>重新安装特定包，无论它是否已经安装。等同于 <code>--refresh-package</code></p>

</dd><dt><code>--requirements</code>, <code>-r</code> <i>requirements</i></dt><dd><p>添加给定 <code>requirements.txt</code> 文件中列出的所有包</p>

</dd><dt><code>--resolution</code> <i>resolution</i></dt><dd><p>在选择不同兼容版本时使用的策略。</p>

<p>默认情况下，uv 会使用每个包的最新兼容版本（<code>highest</code>）。</p>

<p>也可以通过 <code>UV_RESOLUTION</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>highest</code>: 解析每个包的最高兼容版本</li>

<li><code>lowest</code>: 解析每个包的最低兼容版本</li>

<li><code>lowest-direct</code>: 解析任何直接依赖项的最低兼容版本，并解析任何传递依赖项的最高兼容版本</li>
</ul>
</dd><dt><code>--rev</code> <i>rev</i></dt><dd><p>从 Git 添加依赖时使用的提交</p>

</dd><dt><code>--script</code> <i>script</i></dt><dd><p>将依赖项添加到指定的 Python 脚本，而不是添加到项目中。</p>

<p>如果提供，uv 会将依赖项添加到脚本的内联元数据表中，遵循 PEP 723。如果没有该内联元数据表，uv 将创建一个新的并将其添加到脚本中。当通过 <code>uv run</code> 执行时，uv 会为脚本创建一个临时环境，并安装所有内联依赖项。</p>

</dd><dt><code>--tag</code> <i>tag</i></dt><dd><p>从 Git 添加依赖时使用的标签</p>

</dd><dt><code>--upgrade</code>, <code>-U</code></dt><dd><p>允许包的升级，忽略任何现有输出文件中固定的版本。等同于 <code>--refresh</code></p>

</dd><dt><code>--upgrade-package</code>, <code>-P</code> <i>upgrade-package</i></dt><dd><p>允许特定包的升级，忽略任何现有输出文件中固定的版本。等同于 <code>--refresh-package</code></p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置细粒度的日志记录。(&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本</p>

</dd></dl>

## uv remove

从项目中删除依赖项。

依赖项将从项目的 `pyproject.toml` 文件中删除。

如果某个依赖项存在多个条目（即每个条目有不同的标记），则所有条目都会被删除。

锁定文件和项目环境将更新，以反映删除的依赖项。要跳过更新锁定文件，请使用 `--frozen`。要跳过更新环境，请使用 `--no-sync`。

如果请求的某个依赖项在项目中不存在，uv 将返回错误并退出。

如果包已通过手动安装（例如，使用 `uv pip install`），则 `uv remove` 不会将其删除。

uv 会在当前目录或任何父目录中搜索项目。如果找不到项目，uv 将返回错误并退出。

<h3 class="cli-reference">用法</h3>

```
uv remove [OPTIONS] <PACKAGES>...
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>PACKAGES</code></dt><dd><p>要删除的依赖项名称（例如，<code>ruff</code>）</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许连接到不安全的主机。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：此列表中的主机不会与系统的证书存储进行验证。仅在安全的网络和经过验证的源中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会使您暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认情况下，缓存目录为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（在 macOS 和 Linux 上），以及 <code>%LOCALAPPDATA%\uv\cache</code>（在 Windows 上）。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值：auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅当输出发送到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，始终启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--compile-bytecode</code></dt><dd><p>安装后编译 Python 文件为字节码。</p>

<p>默认情况下，uv 不会将 Python（<code>.py</code>）文件编译为字节码（<code>__pycache__/*.pyc</code>）；而是在第一次导入模块时延迟编译。对于启动时间至关重要的用例，例如 CLI 应用程序和 Docker 容器，可以启用此选项，以通过延长安装时间来换取更快的启动时间。</p>

<p>启用时，uv 会处理整个 site-packages 目录（包括当前操作未修改的包），以确保一致性。与 pip 一样，它也会忽略错误。</p>

<p>也可以通过 <code>UV_COMPILE_BYTECODE</code> 环境变量进行设置。</p>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许使用。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--config-setting</code>, <code>-C</code> <i>config-setting</i></dt><dd><p>传递给 PEP 517 构建后端的设置，指定为 <code>KEY=VALUE</code> 键值对</p>

</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认：&lt;https://pypi.org/simple&gt;）。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或按相同格式布局的本地目录。</p>

<p>此标志指定的索引的优先级低于通过 <code>--index</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--dev</code></dt><dd><p>从开发依赖组中删除包。</p>

<p>此选项是 <code>--group dev</code> 的别名。</p>

</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前，切换到指定的目录。</p>

<p>相对路径会以给定目录为基础进行解析。</p>

<p>参见 <code>--project</code> 以仅更改项目根目录。</p>

</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>限制候选包为在给定日期之前上传的包。</p>

<p>接受 RFC 3339 时间戳（例如，<code>2006-12-02T02:07:43Z</code>）和系统配置时区中的本地日期（例如，<code>2006-12-02</code>）格式。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量进行设置。</p>
</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：改用 <code>--index</code>）附加的包索引 URL，用于在 <code>--index-url</code> 之外使用。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或按相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--index-url</code>（默认指向 PyPI）指定的索引。当提供多个 <code>--extra-index-url</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>除注册表索引中找到的分发包外，还要搜索候选分发包的位置。</p>

<p>如果是路径，则目标必须是一个目录，该目录包含顶层的 wheel 文件（<code>.whl</code>）或源分发包（例如，<code>.tar.gz</code> 或 <code>.zip</code>）。</p>

<p>如果是 URL，则页面必须包含符合上述格式的包文件链接的平面列表。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量进行设置。</p>
</dd><dt><code>--frozen</code></dt><dd><p>删除依赖项时不重新锁定项目。</p>

<p>项目环境不会同步。</p>

<p>也可以通过 <code>UV_FROZEN</code> 环境变量进行设置。</p>
</dd><dt><code>--group</code> <i>group</i></dt><dd><p>从指定的依赖组中删除包</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简要帮助信息</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>解析依赖时使用的 URL，除了默认索引之外。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或按相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--default-index</code>（默认指向 PyPI）指定的索引。当提供多个 <code>--index</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>在解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 会停留在第一个包含给定包的索引上，并将解析限制为该索引中的内容（<code>first-match</code>）。这可以防止“依赖混淆”攻击，攻击者可能会在另一个索引上上传一个恶意包，并使用相同的名称。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>first-index</code>: 仅使用第一个返回匹配的索引中的结果</li>

<li><code>unsafe-first-match</code>: 在所有索引中搜索每个包名，先处理第一个索引中的版本，再处理下一个索引</li>

<li><code>unsafe-best-match</code>: 在所有索引中搜索每个包名，优先选择找到的“最佳”版本。如果一个包版本在多个索引中存在，仅查看第一个索引中的条目</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：改用 <code>--default-index</code>）Python 包索引的 URL（默认：&lt;https://pypi.org/simple&gt;）。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或按相同格式布局的本地目录。</p>

<p>此标志指定的索引的优先级低于通过 <code>--extra-index-url</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行索引 URL 的身份验证。</p>

<p>目前，仅支持 <code>--keyring-provider subprocess</code>，它将配置 uv 使用 <code>keyring</code> CLI 来处理身份验证。</p>

<p>默认为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disabled</code>: 不使用 keyring 查找凭据</li>

<li><code>subprocess</code>: 使用 <code>keyring</code> 命令查找凭据</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>安装包时使用的方法。</p>

<p>在 macOS 上默认为 <code>clone</code>（也称为写时复制），在 Linux 和 Windows 上默认为 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>clone</code>: 从 wheel 包中克隆（即写时复制）到 <code>site-packages</code> 目录</li>

<li><code>copy</code>: 从 wheel 包中复制到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>: 从 wheel 包中硬链接到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>: 从 wheel 包中符号链接到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--locked</code></dt><dd><p>确保 <code>uv.lock</code> 文件保持不变。</p>

<p>要求锁定文件是最新的。如果锁定文件丢失或需要更新，uv 将退出并报错。</p>

<p>也可以通过 <code>UV_LOCKED</code> 环境变量进行设置。</p>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储中加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 中加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一组可靠的信任根，将其包含在 uv 中可以提高可移植性和性能（特别是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，特别是当您依赖于系统证书存储中包含的企业信任根（例如，强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-binary</code></dt><dd><p>不要安装预构建的 wheel 包。</p>

<p>给定的包将从源代码构建并安装。解析器仍会使用预构建的 wheel 包来提取包的元数据（如果可用）。</p>

</dd><dt><code>--no-binary-package</code> <i>no-binary-package</i></dt><dd><p>不为特定包安装预构建的 wheel 包</p>

</dd><dt><code>--no-build</code></dt><dd><p>不要构建源代码分发包。</p>

<p>启用时，解析将不会运行任意的 Python 代码。已构建的源分发包的缓存 wheel 将被重用，但需要构建分发包的操作将会报错并退出。</p>

</dd><dt><code>--no-build-isolation</code></dt><dd><p>构建源代码分发包时禁用隔离。</p>

<p>假设 PEP 518 指定的构建依赖项已经安装。</p>

<p>也可以通过 <code>UV_NO_BUILD_ISOLATION</code> 环境变量进行设置。</p>
</dd><dt><code>--no-build-isolation-package</code> <i>no-build-isolation-package</i></dt><dd><p>为特定包构建源代码分发包时禁用隔离。</p>

<p>假设该包的构建依赖项（根据 PEP 518）已安装。</p>

</dd><dt><code>--no-build-package</code> <i>no-build-package</i></dt><dd><p>不为特定包构建源代码分发包</p>

</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录进行操作</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如 PyPI），而是依赖于直接的 URL 依赖和通过 <code>--find-links</code> 提供的依赖</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，进度条或加载指示器。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--no-sources</code></dt><dd><p>在解析依赖项时忽略 <code>tool.uv.sources</code> 表。用于根据符合标准的、可发布的包元数据进行锁定，而不是使用任何本地或 Git 源</p>

</dd><dt><code>--no-sync</code></dt><dd><p>在重新锁定项目后，避免同步虚拟环境。</p>

<p>也可以通过 <code>UV_NO_SYNC</code> 环境变量进行设置。</p>
</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 只会使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--optional</code> <i>optional</i></dt><dd><p>从项目的可选依赖中移除指定额外依赖的包。</p>

</dd><dt><code>--package</code> <i>package</i></dt><dd><p>从工作空间中特定包的依赖中移除。</p>

</dd><dt><code>--prerelease</code> <i>prerelease</i></dt><dd><p>考虑预发布版本时使用的策略。</p>

<p>默认情况下，uv 将接受仅发布预发布版本的包，以及包含显式预发布标记的第一方要求（<code>if-necessary-or-explicit</code>）。</p>

<p>也可以通过 <code>UV_PRERELEASE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disallow</code>: 不允许所有预发布版本</li>

<li><code>allow</code>: 允许所有预发布版本</li>

<li><code>if-necessary</code>: 如果一个包的所有版本都是预发布版本，则允许预发布版本</li>

<li><code>explicit</code>: 仅允许第一方包中版本要求中有显式预发布标记的预发布版本</li>

<li><code>if-necessary-or-explicit</code>: 如果一个包的所有版本都是预发布版本，或者包的版本要求中有显式预发布标记，则允许预发布版本</li>
</ul>
</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将在从项目根目录开始向上遍历目录树时被发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（例如相对路径）将相对于当前工作目录进行解析。</p>

<p>请参阅 <code>--directory</code> 以完全更改工作目录。</p>

<p>此设置在 <code>uv pip</code> 接口中没有效果。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>用于解析和同步的 Python 解释器。</p>

<p>有关 Python 发现和支持的请求格式的详细信息，请参阅 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装，或系统 Python 安装。</p>

<p>默认情况下，uv 更倾向于使用它管理的 Python 版本。如果未安装 uv 管理的 Python，uv 将使用系统 Python 安装。此选项允许优先选择或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用 uv 管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>: 更倾向于使用 uv 管理的 Python 安装，而非系统 Python 安装</li>

<li><code>system</code>: 更倾向于使用系统 Python 安装，而非 uv 管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永远不使用 uv 管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出。</p>

</dd><dt><code>--refresh</code></dt><dd><p>刷新所有缓存数据。</p>

</dd><dt><code>--refresh-package</code> <i>refresh-package</i></dt><dd><p>刷新特定包的缓存数据。</p>

</dd><dt><code>--reinstall</code></dt><dd><p>重新安装所有包，无论它们是否已经安装。意味着 <code>--refresh</code>。</p>

</dd><dt><code>--reinstall-package</code> <i>reinstall-package</i></dt><dd><p>重新安装特定包，无论它是否已经安装。意味着 <code>--refresh-package</code>。</p>

</dd><dt><code>--resolution</code> <i>resolution</i></dt><dd><p>在选择给定包需求的不同兼容版本时使用的策略。</p>

<p>默认情况下，uv 将使用每个包的最新兼容版本（<code>highest</code>）。</p>

<p>也可以通过 <code>UV_RESOLUTION</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>highest</code>: 解析每个包的最高兼容版本</li>

<li><code>lowest</code>: 解析每个包的最低兼容版本</li>

<li><code>lowest-direct</code>: 解析所有直接依赖的最低兼容版本，并解析所有传递依赖的最高兼容版本</li>
</ul>

</dd><dt><code>--script</code> <i>script</i></dt><dd><p>从指定的 Python 脚本中移除依赖，而不是从项目中移除。</p>

<p>如果提供此选项，uv 将从脚本的内联元数据表中移除依赖，以符合 PEP 723。</p>

</dd><dt><code>--upgrade</code>, <code>-U</code></dt><dd><p>允许包升级，忽略任何现有输出文件中锁定的版本。意味着 <code>--refresh</code>。</p>

</dd><dt><code>--upgrade-package</code>, <code>-P</code> <i>upgrade-package</i></dt><dd><p>允许特定包的升级，忽略任何现有输出文件中锁定的版本。意味着 <code>--refresh-package</code>。</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>可以使用 <code>RUST_LOG</code> 环境变量配置更细粒度的日志记录。（<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>）</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本。</p>

</dd></dl>

## uv sync

更新项目的环境。

同步确保所有项目依赖项都已安装并与锁定文件保持一致。

默认情况下，会执行精确同步：uv 会删除那些未声明为项目依赖的包。使用 `--inexact` 标志可以保留多余的包。请注意，如果一个多余的包与项目依赖冲突，它仍然会被删除。此外，如果使用了 `--no-build-isolation`，uv 将不会删除多余的包，以避免删除可能的构建依赖。

如果项目的虚拟环境（` .venv`）不存在，它将被创建。

在同步之前，项目将重新锁定，除非提供了 `--locked` 或 `--frozen` 标志。

uv 将在当前目录或任何父目录中搜索项目。如果找不到项目，uv 将退出并显示错误。

请注意，从锁定文件安装时，uv 不会为被移除的包版本提供警告。

<h3 class="cli-reference">用法</h3>

```
uv sync [选项]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--all-extras</code></dt><dd><p>包括所有可选依赖。</p>

<p>当两个或更多的额外依赖在 <code>tool.uv.conflicts</code> 中被声明为冲突时，使用此标志将始终导致错误。</p>

<p>请注意，所有可选依赖始终会被包括在解析中；此选项仅影响要安装的包的选择。</p>

</dd><dt><code>--all-groups</code></dt><dd><p>包括来自所有依赖组的依赖。</p>

<p>可以使用 <code>--no-group</code> 来排除特定的组。</p>

</dd><dt><code>--all-packages</code></dt><dd><p>同步工作空间中的所有包。</p>

<p>工作空间的环境（<code>.venv</code>）将更新为包含所有工作空间成员。</p>

<p>通过 <code>--extra</code>、<code>--group</code> 或相关选项指定的任何额外依赖或组将应用于所有工作空间成员。</p>

</dd><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许连接到不安全的主机。</p>

<p>可以多次提供此选项。</p>

<p>期望接收主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：此列表中包含的主机将不会与系统的证书存储进行验证。仅在具有经过验证来源的安全网络中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能暴露给中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（macOS 和 Linux），以及 <code>%LOCALAPPDATA%\uv\cache</code>（Windows）。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色。</p>

<p>[默认值：auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅当输出到终端或支持的 TTY 时启用彩色输出。</li>

<li><code>always</code>: 无论检测到的环境如何，始终启用彩色输出。</li>

<li><code>never</code>: 禁用彩色输出。</li>
</ul>
</dd><dt><code>--compile-bytecode</code></dt><dd><p>安装后编译 Python 文件为字节码。</p>

<p>默认情况下，uv 不会将 Python（<code>.py</code>）文件编译为字节码（<code>__pycache__/*.pyc</code>）；而是首次导入模块时懒加载编译。对于启动时间至关重要的用例（如 CLI 应用和 Docker 容器），可以启用此选项，用更长的安装时间换取更快的启动时间。</p>

<p>启用时，uv 将处理整个 site-packages 目录（包括当前操作未修改的包）以确保一致性。像 pip 一样，它也会忽略错误。</p>

<p>也可以通过 <code>UV_COMPILE_BYTECODE</code> 环境变量进行设置。</p>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这样做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--config-setting</code>, <code>-C</code> <i>config-setting</i></dt><dd><p>传递给 PEP 517 构建后端的设置，指定为 <code>KEY=VALUE</code> 对。</p>

</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认值：<https://pypi.org/simple>）。</p>

<p>接受符合 PEP 503（简单仓库 API）的仓库，或者采用相同格式的本地目录。</p>

<p>此标志指定的索引的优先级低于通过 <code>--index</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定目录。</p>

<p>相对路径会以指定的目录作为基准进行解析。</p>

<p>参见 <code>--project</code>，仅更改项目根目录。</p>

</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>将候选包限制为在指定日期之前上传的包。</p>

<p>接受 RFC 3339 时间戳（例如，<code>2006-12-02T02:07:43Z</code>）和在系统配置时区下的本地日期（例如，<code>2006-12-02</code>）。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量进行设置。</p>
</dd><dt><code>--extra</code> <i>extra</i></dt><dd><p>包括指定额外名称的可选依赖。</p>

<p>可以多次提供此选项。</p>

<p>当多个额外依赖或组在 <code>tool.uv.conflicts</code> 中被声明为冲突时，uv 将报告错误。</p>

<p>请注意，所有可选依赖始终会被包含在解析中；此选项仅影响要安装的包的选择。</p>

</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：请使用 <code>--index</code>）除了 <code>--index-url</code> 外，还要使用的额外包索引 URL。</p>

<p>接受符合 PEP 503（简单仓库 API）的仓库，或采用相同格式的本地目录。</p>

<p>所有通过此标志提供的索引优先于通过 <code>--index-url</code> 指定的索引（默认为 PyPI）。当提供多个 <code>--extra-index-url</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>除了在注册表索引中找到的内容外，还要搜索候选分发包的路径。</p>

<p>如果是路径，目标必须是一个包含 wheel 文件（<code>.whl</code>）或源分发文件（例如，<code>.tar.gz</code> 或 <code>.zip</code>）的目录，文件必须在顶层。</p>

<p>如果是 URL，页面必须包含符合上述格式的包文件链接的平面列表。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量进行设置。</p>
</dd><dt><code>--frozen</code></dt><dd><p>在不更新 <code>uv.lock</code> 文件的情况下进行同步。</p>

<p>不检查锁定文件是否为最新版本，而是使用锁定文件中的版本作为真相来源。如果锁定文件丢失，uv 将退出并显示错误。如果 <code>pyproject.toml</code> 包含依赖项的更改但尚未在锁定文件中，环境中将不会包含这些更改。</p>

<p>也可以通过 <code>UV_FROZEN</code> 环境变量进行设置。</p>
</dd><dt><code>--group</code> <i>group</i></dt><dd><p>包括指定依赖组中的依赖。</p>

<p>当多个额外依赖或组在 <code>tool.uv.conflicts</code> 中被声明为冲突时，uv 将报告错误。</p>

<p>可以多次提供此选项。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简要帮助</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>在解析依赖时使用的索引 URL，除了默认的索引之外。</p>

<p>接受符合 PEP 503（简单仓库 API）的仓库，或采用相同格式的本地目录。</p>

<p>所有通过此标志提供的索引优先于通过 <code>--default-index</code> 指定的索引（默认为 PyPI）。当提供多个 <code>--index</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>在多个索引 URL 之间解析时使用的策略。</p>

<p>默认情况下，uv 会在第一个可用包的索引上停止，并将解析限制为该索引上存在的包（<code>first-match</code>）。这样可以防止“依赖冲突”攻击，其中攻击者可以将恶意包上传到另一个索引并使用相同的包名。</p>

<p>May also be set with the <code>UV_INDEX_STRATEGY</code> environment variable.</p>
<p>Possible values:</p>

<ul>
<li><code>first-index</code>:  仅使用第一个返回匹配结果的索引中的结果。</li>

<li><code>unsafe-first-match</code>:  在所有索引中搜索每个包名，在切换到下一个索引之前先耗尽第一个索引中的版本。</li>

<li><code>unsafe-best-match</code>:  在所有索引中搜索每个包名，优先选择找到的“最佳”版本。如果某个包版本存在于多个索引中，仅查看第一个索引中的条目。</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：请使用 <code>--default-index</code>）Python 包索引的 URL（默认值：<https://pypi.org/simple>）。</p>

<p>接受符合 PEP 503（简单仓库 API）的仓库，或采用相同格式的本地目录。</p>

<p>此标志指定的索引的优先级低于通过 <code>--extra-index-url</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--inexact</code></dt><dd><p>不移除环境中多余的包。</p>

<p>启用时，uv 将仅进行最少必要的更改以满足要求。默认情况下，同步操作将移除环境中任何多余的包。</p>

</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行索引 URL 的身份验证。</p>

<p>目前，仅支持 <code>--keyring-provider subprocess</code>，该选项配置 uv 使用 <code>keyring</code> CLI 处理身份验证。</p>

<p>默认值为 <code>disabled</code>。</p>

<p>可设置的值：</p>

<ul>
<li><code>disabled</code>:  不使用 keyring 进行凭证查找</li>

<li><code>subprocess</code>:  使用 <code>keyring</code> 命令进行凭证查找</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>安装包时使用的方法，从全局缓存中获取。</p>

<p>默认情况下，在 macOS 上使用 <code>clone</code>（也称为写时复制），在 Linux 和 Windows 上使用 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量进行设置。</p>
<p>可设置的值：</p>

<ul>
<li><code>clone</code>:  从 wheel 文件克隆（即写时复制）包到 <code>site-packages</code> 目录</li>

<li><code>copy</code>:  从 wheel 文件复制包到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>:  从 wheel 文件创建硬链接到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>:  从 wheel 文件创建符号链接到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--locked</code></dt><dd><p>声明 <code>uv.lock</code> 文件保持不变。</p>

<p>要求锁定文件是最新的。如果锁定文件丢失或需要更新，uv 将退出并显示错误。</p>

<p>也可以通过 <code>UV_LOCKED</code> 环境变量进行设置。</p>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一组可靠的信任根，将其包含在 uv 中可以提高可移植性和性能（特别是在 macOS 上）。</p>

<p>但是，在某些情况下，您可能希望使用平台的本地证书存储，特别是如果您依赖于包含在系统证书存储中的企业信任根（例如用于强制代理）。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-binary</code></dt><dd><p>不安装预构建的 wheel 包。</p>

<p>指定的包将从源代码构建并安装。解析器仍会使用预构建的 wheel 包提取包元数据（如果有的话）。</p>

</dd><dt><code>--no-binary-package</code> <i>no-binary-package</i></dt><dd><p>不为特定包安装预构建的 wheel 包。</p>

</dd><dt><code>--no-build</code></dt><dd><p>不构建源分发包。</p>

<p>启用时，解析将不会运行任意的 Python 代码。已构建的源分发包的缓存 wheel 会被重用，但需要构建分发包的操作将以错误退出。</p>

</dd><dt><code>--no-build-isolation</code></dt><dd><p>构建源分发包时禁用隔离。</p>

<p>假设 PEP 518 指定的构建依赖已经安装。</p>

<p>也可以通过 <code>UV_NO_BUILD_ISOLATION</code> 环境变量进行设置。</p>
</dd><dt><code>--no-build-isolation-package</code> <i>no-build-isolation-package</i></dt><dd><p>为特定包构建源分发包时禁用隔离。</p>

<p>假设该包的构建依赖已通过 PEP 518 指定并安装。</p>

</dd><dt><code>--no-build-package</code> <i>no-build-package</i></dt><dd><p>不为特定包构建源分发包。</p>

</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录执行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-dev</code></dt><dd><p>省略开发依赖组。</p>

<p>此选项是 <code>--no-group dev</code> 的别名。</p>

</dd><dt><code>--no-editable</code></dt><dd><p>将任何可编辑的依赖项，包括项目和任何工作区成员，安装为非可编辑。</p>

</dd><dt><code>--no-extra</code> <i>no-extra</i></dt><dd><p>如果提供了 <code>--all-extras</code>，则排除指定的可选依赖项。</p>

<p>可以多次提供。</p>

</dd><dt><code>--no-group</code> <i>no-group</i></dt><dd><p>排除指定依赖组中的依赖项。</p>

<p>可以多次提供。</p>

</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如，PyPI），改为依赖直接 URL 依赖项和通过 <code>--find-links</code> 提供的依赖项。</p>

</dd><dt><code>--no-install-package</code> <i>no-install-package</i></dt><dd><p>不要安装指定的包。</p>

<p>默认情况下，项目的所有依赖项都会安装到环境中。<code>--no-install-package</code> 选项允许排除特定的包。请注意，这可能导致环境损坏，应谨慎使用。</p>

</dd><dt><code>--no-install-project</code></dt><dd><p>不要安装当前项目。</p>

<p>默认情况下，当前项目会连同其所有依赖项一起安装到环境中。<code>--no-install-project</code> 选项允许排除项目，但仍然安装其所有依赖项。这在像构建 Docker 镜像的情况下特别有用，其中将项目与其依赖项分开安装可以实现最佳的层缓存。</p>

</dd><dt><code>--no-install-workspace</code></dt><dd><p>不要安装任何工作区成员，包括根项目。</p>

<p>默认情况下，所有工作区成员及其依赖项会安装到环境中。<code>--no-install-workspace</code> 选项允许排除所有工作区成员，但仍保留其依赖项。这在像构建 Docker 镜像的情况下特别有用，其中将工作区与其依赖项分开安装可以实现最佳的层缓存。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--no-sources</code></dt><dd><p>在解析依赖项时忽略 <code>tool.uv.sources</code> 表。用于锁定符合标准的、可发布的包元数据，而不是使用任何本地或 Git 源。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 将仅使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--only-dev</code></dt><dd><p>仅包含开发依赖组。</p>

<p>省略其他依赖项。项目本身也将被省略。</p>

<p>此选项是 <code>--only-group dev</code> 的别名。</p>

</dd><dt><code>--only-group</code> <i>only-group</i></dt><dd><p>仅包含指定依赖组中的依赖项。</p>

<p>可以多次提供。</p>

<p>项目本身也将被省略。</p>

</dd><dt><code>--package</code> <i>package</i></dt><dd><p>同步工作区中的特定包。</p>

<p>工作区的环境（<code>.venv</code>）将更新，以反映指定工作区成员包声明的依赖项子集。</p>

<p>如果工作区成员不存在，uv 将退出并显示错误。</p>

</dd><dt><code>--prerelease</code> <i>prerelease</i></dt><dd><p>考虑预发布版本时使用的策略。</p>

<p>默认情况下，uv 会接受仅发布预发布版本的包，以及声明了显式预发布标记的第一方依赖（<code>if-necessary-or-explicit</code>）。</p>

<p>也可以通过 <code>UV_PRERELEASE</code> 环境变量进行设置。</p>
<p>可设置的值：</p>

<ul>
<li><code>disallow</code>:  不允许所有预发布版本</li>

<li><code>allow</code>:  允许所有预发布版本</li>

<li><code>if-necessary</code>:  如果包的所有版本都是预发布版本，则允许预发布版本</li>

<li><code>explicit</code>:  允许具有显式预发布标记的第一方包的预发布版本</li>

<li><code>if-necessary-or-explicit</code>:  如果包的所有版本都是预发布版本，或者包在其版本要求中有显式的预发布标记，则允许预发布版本。</li>
</ul>
</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将在从项目根目录向上遍历目录树时发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>请参见 <code>--directory</code> 以完全更改工作目录。</p>

<p>此设置在 <code>uv pip</code> 接口中没有效果。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>为项目环境使用的 Python 解释器。</p>

<p>默认情况下，将使用满足项目 <code>requires-python</code> 限制的第一个解释器。</p>

<p>如果提供了虚拟环境中的 Python 解释器，则包不会同步到该环境中。解释器将用于在项目中创建虚拟环境。</p>

<p>有关 Python 发现和支持的请求格式的详细信息，请参见 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 更倾向于使用它管理的 Python 版本。然而，如果没有安装 uv 管理的 Python，它将使用系统 Python 安装。此选项允许优先或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可设置的值：</p>

<ul>
<li><code>only-managed</code>:  仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>:  更倾向于使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>:  更倾向于使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>:  仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--refresh</code></dt><dd><p>刷新所有缓存数据</p>

</dd><dt><code>--refresh-package</code> <i>refresh-package</i></dt><dd><p>刷新特定包的缓存数据</p>

</dd><dt><code>--reinstall</code></dt><dd><p>重新安装所有包，不管它们是否已经安装。隐含 <code>--refresh</code></p>

</dd><dt><code>--reinstall-package</code> <i>reinstall-package</i></dt><dd><p>重新安装特定包，不管它是否已经安装。隐含 <code>--refresh-package</code></p>

</dd><dt><code>--resolution</code> <i>resolution</i></dt><dd><p>在选择给定包要求的不同兼容版本时使用的策略。</p>

<p>默认情况下，uv 将使用每个包的最新兼容版本（<code>highest</code>）。</p>

<p>也可以通过 <code>UV_RESOLUTION</code> 环境变量进行设置。</p>
<p>可设置的值：</p>

<ul>
<li><code>highest</code>:  解析每个包的最高兼容版本</li>

<li><code>lowest</code>:  解析每个包的最低兼容版本</li>

<li><code>lowest-direct</code>:  解析任何直接依赖项的最低兼容版本，解析任何传递依赖项的最高兼容版本</li>
</ul>
</dd><dt><code>--upgrade</code>, <code>-U</code></dt><dd><p>允许包的升级，忽略任何现有输出文件中锁定的版本。隐含 <code>--refresh</code></p>

</dd><dt><code>--upgrade-package</code>, <code>-P</code> <i>upgrade-package</i></dt><dd><p>允许特定包的升级，忽略任何现有输出文件中锁定的版本。隐含 <code>--refresh-package</code></p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置更精细的日志记录。（<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives</a>）</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

## uv lock

更新项目的锁文件。

如果项目锁文件（`uv.lock`）不存在，将会创建一个。如果锁文件已存在，其内容将被用作解析的首选项。

如果项目的依赖项没有变化，除非提供了 `--upgrade` 标志，否则锁定将没有任何效果。

<h3 class="cli-reference">用法</h3>

```
uv lock [选项]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许不安全的主机连接。</p>

<p>可以多次提供此选项。</p>

<p>期望接收主机名（例如 <code>localhost</code>）、主机-端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：此列表中的主机不会与系统的证书存储进行验证。仅在有验证源的安全网络中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会使你暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（macOS 和 Linux），以及 <code>%LOCALAPPDATA%\uv\cache</code>（Windows）。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色。</p>

<p>[默认值：自动]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>:  仅当输出发送到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>:  无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>:  禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>要使用的 <code>uv.toml</code> 配置文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许使用。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--config-setting</code>, <code>-C</code> <i>config-setting</i></dt><dd><p>传递给 PEP 517 构建后端的设置，格式为 <code>KEY=VALUE</code> 对。</p>

</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认值：&lt;https://pypi.org/simple&gt;）。</p>

<p>接受符合 PEP 503 的仓库（即简单仓库 API）或本地目录，该目录采用相同的格式。</p>

<p>通过此标志指定的索引的优先级低于通过 <code>--index</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到给定的目录。</p>

<p>相对路径将使用给定目录作为基础进行解析。</p>

<p>请参见 <code>--project</code>，仅更改项目根目录。</p>

</dd><dt><code>--dry-run</code></dt><dd><p>执行试运行，不写入锁文件。</p>

<p>在试运行模式下，uv 将解析项目的依赖项并报告结果变化，但不会将锁文件写入磁盘。</p>

</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>将候选包限制为在给定日期之前上传的包。</p>

<p>接受 RFC 3339 时间戳（例如 <code>2006-12-02T02:07:43Z</code>）和本地日期（例如 <code>2006-12-02</code>），使用系统配置的时区。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量进行设置。</p>
</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已废弃：请使用 <code>--index</code>）额外的包索引 URL，除了 <code>--index-url</code> 外，还将使用这些 URL。</p>

<p>接受符合 PEP 503 的仓库（即简单仓库 API）或本地目录，该目录采用相同的格式。</p>

<p>所有通过此标志提供的索引优先于通过 <code>--index-url</code> 指定的索引（默认指向 PyPI）。当提供多个 <code>--extra-index-url</code> 标志时，较早的值具有更高优先级。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>除了在注册表索引中找到的包外，还会在这些位置查找候选分发。</p>

<p>如果是路径，目标必须是包含包文件的目录，这些包文件可以是 wheel 文件（<code>.whl</code>）或源分发包（例如 <code>.tar.gz</code> 或 <code>.zip</code>），并位于顶层。</p>

<p>如果是 URL，该页面必须包含符合上述格式的包文件链接的平面列表。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量进行设置。</p>
</dd><dt><code>--frozen</code></dt><dd><p>确保 <code>uv.lock</code> 文件存在，而不更新它。</p>

<p>也可以通过 <code>UV_FROZEN</code> 环境变量进行设置。</p>
</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示该命令的简洁帮助信息</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>在解析依赖项时，除了默认索引外，使用的 URL。</p>

<p>接受符合 PEP 503 的仓库（即简单仓库 API）或本地目录，该目录采用相同的格式。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--default-index</code> 指定的索引（默认指向 PyPI）。当提供多个 <code>--index</code> 标志时，较早的值具有更高优先级。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 将在第一个找到的索引处停止，并将解析限制为该索引中存在的版本（<code>first-match</code>）。这种策略可以防止“依赖混淆”攻击，攻击者可能会在不同的索引上上传同名的恶意包。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>first-index</code>:  仅使用第一个返回匹配包名的索引的结果</li>

<li><code>unsafe-first-match</code>:  在所有索引中搜索每个包名，在转到下一个索引之前，先耗尽第一个索引中的版本</li>

<li><code>unsafe-best-match</code>:  在所有索引中搜索每个包名，优先选择找到的“最佳”版本。如果一个包的版本出现在多个索引中，则仅查看第一个索引中的条目</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已废弃：请使用 <code>--default-index</code>）Python 包索引的 URL（默认值：&lt;https://pypi.org/simple&gt;）。</p>

<p>接受符合 PEP 503 的仓库（即简单仓库 API）或本地目录，该目录采用相同的格式。</p>

<p>通过此标志指定的索引的优先级低于通过 <code>--extra-index-url</code> 提供的所有索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行索引 URL 的身份验证。</p>

<p>目前，仅支持 <code>--keyring-provider subprocess</code>，它配置 uv 使用 <code>keyring</code> CLI 来处理身份验证。</p>

<p>默认值为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>disabled</code>:  不使用 keyring 进行凭证查找</li>

<li><code>subprocess</code>:  使用 <code>keyring</code> 命令进行凭证查找</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>安装包时使用的方法，来源于全局缓存。</p>

<p>此选项仅在构建源分发包时使用。</p>

<p>默认情况下，macOS 使用 <code>clone</code>（也称为写时复制），而 Linux 和 Windows 使用 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>clone</code>:  从 wheel 文件中克隆（即写时复制）包到 <code>site-packages</code> 目录</li>

<li><code>copy</code>:  从 wheel 文件中复制包到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>:  从 wheel 文件中硬链接包到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>:  从 wheel 文件中符号链接包到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--locked</code></dt><dd><p>确保 <code>uv.lock</code> 保持不变。</p>

<p>要求锁文件是最新的。如果锁文件丢失或需要更新，uv 将以错误退出。</p>

<p>也可以通过 <code>UV_LOCKED</code> 环境变量进行设置。</p>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一组可靠的信任根，包含它可以提高 uv 的可移植性和性能（尤其在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，尤其是当依赖于公司信任根（例如用于强制代理的根证书）时，这些根证书包含在系统的证书存储中。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-binary</code></dt><dd><p>不安装预构建的 wheel 文件。</p>

<p>指定的包将从源代码构建并安装。解析器仍然会使用预构建的 wheel 文件提取包的元数据（如果有的话）。</p>

</dd><dt><code>--no-binary-package</code> <i>no-binary-package</i></dt><dd><p>不为特定包安装预构建的 wheel 文件。</p>

</dd><dt><code>--no-build</code></dt><dd><p>不构建源分发包。</p>

<p>启用时，解析将不会运行任意的 Python 代码。已构建的源分发包的缓存 wheel 文件将被重用，但需要构建分发包的操作将会以错误退出。</p>

</dd><dt><code>--no-build-isolation</code></dt><dd><p>在构建源分发包时禁用隔离。</p>

<p>假定已安装 PEP 518 中指定的构建依赖项。</p>

<p>也可以通过 <code>UV_NO_BUILD_ISOLATION</code> 环境变量进行设置。</p>
</dd><dt><code>--no-build-isolation-package</code> <i>no-build-isolation-package</i></dt><dd><p>在为特定包构建源分发包时禁用隔离。</p>

<p>假定该包的构建依赖项（根据 PEP 518）已安装。</p>

</dd><dt><code>--no-build-package</code> <i>no-build-package</i></dt><dd><p>不为特定包构建源分发包。</p>

</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存读取或写入数据，而是使用临时目录进行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（如 <code>pyproject.toml</code> 和 <code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中查找。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如 PyPI），而是依赖于直接的 URL 依赖项以及通过 <code>--find-links</code> 提供的依赖项。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，隐藏旋转器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--no-sources</code></dt><dd><p>在解析依赖项时忽略 <code>tool.uv.sources</code> 表。用于锁定符合标准的、可发布的包元数据，而不是使用任何本地或 Git 源。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 只会使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--prerelease</code> <i>prerelease</i></dt><dd><p>在考虑预发布版本时使用的策略。</p>

<p>默认情况下，uv 会接受仅发布预发布版本的包，以及包含明确预发布标记的第一方要求（<code>if-necessary-or-explicit</code>）。</p>

<p>也可以通过 <code>UV_PRERELEASE</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>disallow</code>:  禁止所有预发布版本</li>

<li><code>allow</code>:  允许所有预发布版本</li>

<li><code>if-necessary</code>:  如果一个包的所有版本都是预发布版本，则允许预发布版本</li>

<li><code>explicit</code>:  仅允许第一方包中的预发布标记明确要求的预发布版本</li>

<li><code>if-necessary-or-explicit</code>:  如果一个包的所有版本都是预发布版本，或者该包的版本要求中有明确的预发布标记，则允许预发布版本</li>
</ul>
</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在指定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将在从项目根目录向上遍历目录树时被发现，同时也会发现项目的虚拟环境（<code>.venv</code>）。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录解析。</p>

<p>查看 <code>--directory</code> 以完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置没有效果。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>解析期间使用的 Python 解释器。</p>

<p>构建源分发包时需要 Python 解释器，以便在没有 wheel 文件时确定包的元数据。</p>

<p>当 <code>requires-python</code> 未设置时，解释器也会作为最低 Python 版本的备用值。</p>

<p>查看 <a href="#uv-python">uv python</a> 获取有关 Python 查找和支持的请求格式的详细信息。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装。</p>

<p>默认情况下，uv 优先使用它所管理的 Python 版本。然而，如果没有安装 uv 管理的 Python，它将使用系统的 Python 安装。此选项允许优先使用或忽略系统的 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>only-managed</code>:  仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>:  优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>:  优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>:  仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--refresh</code></dt><dd><p>刷新所有缓存的数据</p>

</dd><dt><code>--refresh-package</code> <i>refresh-package</i></dt><dd><p>刷新特定包的缓存数据</p>

</dd><dt><code>--resolution</code> <i>resolution</i></dt><dd><p>选择给定包需求的不同兼容版本时使用的策略。</p>

<p>默认情况下，uv 会使用每个包的最新兼容版本（<code>highest</code>）。</p>

<p>也可以通过 <code>UV_RESOLUTION</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>highest</code>:  解析每个包的最高兼容版本</li>

<li><code>lowest</code>:  解析每个包的最低兼容版本</li>

<li><code>lowest-direct</code>:  解析任何直接依赖项的最低兼容版本，以及任何传递依赖项的最高兼容版本</li>
</ul>
</dd><dt><code>--upgrade</code>, <code>-U</code></dt><dd><p>允许包的升级，忽略任何现有输出文件中固定的版本。暗含 <code>--refresh</code>。</p>

</dd><dt><code>--upgrade-package</code>, <code>-P</code> <i>upgrade-package</i></dt><dd><p>允许特定包的升级，忽略任何现有输出文件中固定的版本。暗含 <code>--refresh-package</code>。</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置更精细的日志记录。(&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本</p>

</dd></dl>

## uv export

将项目的锁定文件导出为其他格式。

目前，仅支持 `requirements-txt` 格式。

在导出之前，除非提供了 `--locked` 或 `--frozen` 标志，否则项目会重新锁定。

uv 会在当前目录或任何父目录中查找项目。如果找不到项目，uv 将退出并显示错误。

如果在工作区中操作，默认会导出根项目；然而，可以使用 `--package` 选项选择特定的成员。

<h3 class="cli-reference">用法</h3>

```
uv export [选项]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--all-extras</code></dt><dd><p>包括所有可选依赖项</p>

</dd><dt><code>--all-groups</code></dt><dd><p>包括所有依赖组中的依赖项。</p>

<p>可以使用 <code>--no-group</code> 来排除特定的组。</p>

</dd><dt><code>--all-packages</code></dt><dd><p>导出整个工作区。</p>

<p>所有工作区成员的依赖项都会包含在导出的要求文件中。</p>

<p>通过 <code>--extra</code>、<code>--group</code> 或相关选项指定的任何 extras 或组将应用于所有工作区成员。</p>

</dd><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许连接到不安全的主机。</p>

<p>可以多次提供此选项。</p>

<p>期望接收主机名（例如 <code>localhost</code>）、主机-端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：此列表中的主机将不会与系统的证书存储进行验证。仅在安全的网络和经过验证的来源中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会使您暴露于中间人攻击。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（在 macOS 和 Linux 上），以及 <code>%LOCALAPPDATA%\uv\cache</code>（在 Windows 上）。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值：auto]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>:  仅在输出到支持终端或 TTY 时启用彩色输出</li>

<li><code>always</code>:  无论检测到的环境如何，始终启用彩色输出</li>

<li><code>never</code>:  禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>要使用的 <code>uv.toml</code> 配置文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许使用。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--config-setting</code>, <code>-C</code> <i>config-setting</i></dt><dd><p>传递给 PEP 517 构建后端的设置，格式为 <code>KEY=VALUE</code> 键值对</p>

</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认值：<code>https://pypi.org/simple</code>）。</p>

<p>接受符合 PEP 503（简单仓库 API）标准的仓库，或者按相同格式布局的本地目录。</p>

<p>此标志给定的索引优先级低于通过 <code>--index</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在执行命令之前切换到指定目录。</p>

<p>相对路径将使用给定目录作为基准进行解析。</p>

<p>请参见 <code>--project</code>，以仅更改项目根目录。</p>

</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>将候选包限制为上传日期在指定日期之前的包。</p>

<p>接受 RFC 3339 时间戳（例如 <code>2006-12-02T02:07:43Z</code>）和系统配置时区中的本地日期（例如 <code>2006-12-02</code>）。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量进行设置。</p>
</dd><dt><code>--extra</code> <i>extra</i></dt><dd><p>包含指定额外名称的可选依赖项。</p>

<p>可以多次提供此选项。</p>

</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：请改用 <code>--index</code>）额外的包索引 URL，除了 <code>--index-url</code> 外使用。</p>

<p>接受符合 PEP 503（简单仓库 API）标准的仓库，或按相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先于 <code>--index-url</code> 指定的索引（默认是 PyPI）。当提供多个 <code>--extra-index-url</code> 标志时，先提供的值优先。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>除了注册表索引中找到的包，还可指定查找候选分发包的位置。</p>

<p>如果是路径，目标必须是一个目录，目录中包含顶层的 wheel 文件（<code>.whl</code>）或源分发包（例如，<code>.tar.gz</code> 或 <code>.zip</code>）。</p>

<p>如果是 URL，页面必须包含指向包文件的平面链接列表，且文件格式符合上述描述。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量进行设置。</p>
</dd><dt><code>--format</code> <i>format</i></dt><dd><p>将 <code>uv.lock</code> 导出的格式。</p>

<p>目前，仅支持 <code>requirements-txt</code> 格式。</p>

<p>[默认值：requirements-txt]</p>
<p>可选值：</p>

<ul>
<li><code>requirements-txt</code>:  导出为 <code>requirements.txt</code> 格式</li>
</ul>
</dd><dt><code>--frozen</code></dt><dd><p>导出前不更新 <code>uv.lock</code> 文件。</p>

<p>如果 <code>uv.lock</code> 文件不存在，uv 将退出并显示错误。</p>

<p>也可以通过 <code>UV_FROZEN</code> 环境变量进行设置。</p>
</dd><dt><code>--group</code> <i>group</i></dt><dd><p>包括指定依赖组中的依赖项。</p>

<p>可以多次提供此选项。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示该命令的简洁帮助</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>解析依赖时使用的 URL，除了默认的索引外。</p>

<p>接受符合 PEP 503（简单仓库 API）标准的仓库，或按相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先于 <code>--default-index</code> 指定的索引（默认是 PyPI）。当提供多个 <code>--index</code> 标志时，先提供的值优先。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 会在第一个找到的索引上停止并解析包，限制解析结果为该索引上的内容（<code>first-match</code>）。这样可以防止“依赖混淆”攻击，即攻击者可能会将恶意包上传到一个备用索引中，使用与正常包相同的名称。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>first-index</code>:  仅使用第一个返回匹配的索引中的结果</li>

<li><code>unsafe-first-match</code>:  在所有索引中搜索每个包名，在完成第一个索引中的所有版本后，再移动到下一个索引</li>

<li><code>unsafe-best-match</code>:  在所有索引中搜索每个包名，优先选择“最佳”版本。如果一个包版本存在于多个索引中，则只查看第一个索引中的条目</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：请改用 <code>--default-index</code>）Python 包索引的 URL（默认值：<code>https://pypi.org/simple</code>）。</p>

<p>接受符合 PEP 503（简单仓库 API）标准的仓库，或按相同格式布局的本地目录。</p>

<p>此标志指定的索引优先级低于通过 <code>--extra-index-url</code> 提供的所有其他索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行索引 URL 的身份验证。</p>

<p>目前，仅支持 <code>--keyring-provider subprocess</code>，该选项配置 uv 使用 <code>keyring</code> CLI 来处理身份验证。</p>

<p>默认值为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>disabled</code>:  不使用 keyring 查找凭证</li>

<li><code>subprocess</code>:  使用 <code>keyring</code> 命令查找凭证</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>安装来自全局缓存的包时使用的方法。</p>

<p>此选项仅在构建源分发包时使用。</p>

<p>默认情况下，macOS 上使用 <code>clone</code>（也称为写时复制），Linux 和 Windows 上使用 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>clone</code>:  从 wheel 文件中克隆（即写时复制）包到 <code>site-packages</code> 目录</li>

<li><code>copy</code>:  将包从 wheel 文件复制到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>:  从 wheel 文件创建硬链接到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>:  从 wheel 文件创建符号链接到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--locked</code></dt><dd><p>确保 <code>uv.lock</code> 文件保持不变。</p>

<p>要求锁定文件是最新的。如果锁定文件缺失或需要更新，uv 将退出并显示错误。</p>

<p>也可以通过 <code>UV_LOCKED</code> 环境变量进行设置。</p>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一套可靠的信任根，包含它们有助于提高 uv 的可移植性和性能（特别是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，特别是当您依赖于系统证书存储中的公司信任根（例如，强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-binary</code></dt><dd><p>不安装预构建的 wheel 文件。</p>

<p>指定的包将从源代码构建并安装。解析器仍然会使用预构建的 wheel 文件来提取包的元数据（如果可用）。</p>

</dd><dt><code>--no-binary-package</code> <i>no-binary-package</i></dt><dd><p>不为特定包安装预构建的 wheel 文件。</p>

</dd><dt><code>--no-build</code></dt><dd><p>不构建源分发包。</p>

<p>启用此选项时，解析将不会执行任意 Python 代码。已经构建好的源分发包的缓存 wheel 文件将被重用，但需要构建的操作将会报错。</p>

</dd><dt><code>--no-build-isolation</code></dt><dd><p>构建源分发包时禁用隔离。</p>

<p>假设 PEP 518 中指定的构建依赖项已经安装。</p>

<p>也可以通过 <code>UV_NO_BUILD_ISOLATION</code> 环境变量进行设置。</p>
</dd><dt><code>--no-build-isolation-package</code> <i>no-build-isolation-package</i></dt><dd><p>为特定包构建源分发包时禁用隔离。</p>

<p>假设该包的构建依赖项（由 PEP 518 指定）已经安装。</p>

</dd><dt><code>--no-build-package</code> <i>no-build-package</i></dt><dd><p>不为特定包构建源分发包。</p>

</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，改为使用临时目录进行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>, <code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-dev</code></dt><dd><p>省略开发依赖组。</p>

<p>此选项是 <code>--no-group dev</code> 的别名。</p>

</dd><dt><code>--no-editable</code></dt><dd><p>将任何可编辑的依赖项（包括项目本身及任何工作区成员）作为非可编辑安装。</p>

</dd><dt><code>--no-emit-package</code> <i>no-emit-package</i></dt><dd><p>不输出指定的包。</p>

<p>默认情况下，项目的所有依赖项都会包含在导出的要求文件中。使用 <code>--no-install-package</code> 选项可以排除特定的包。</p>

</dd><dt><code>--no-emit-project</code></dt><dd><p>不输出当前项目。</p>

<p>默认情况下，当前项目及其所有依赖项会包含在导出的要求文件中。使用 <code>--no-emit-project</code> 选项可以将项目排除在外，但保留其所有依赖项。</p>

</dd><dt><code>--no-emit-workspace</code></dt><dd><p>不输出任何工作区成员，包括根项目。</p>

<p>默认情况下，所有工作区成员及其依赖项都会包含在导出的要求文件中。使用 <code>--no-emit-workspace</code> 选项可以排除所有工作区成员，但保留它们的依赖项。</p>

</dd><dt><code>--no-extra</code> <i>no-extra</i></dt><dd><p>如果提供了 <code>--all-extras</code>，则排除指定的可选依赖项。</p>

<p>可以多次提供此选项。</p>

</dd><dt><code>--no-group</code> <i>no-group</i></dt><dd><p>排除指定依赖组中的依赖项。</p>

<p>可以多次提供此选项。</p>

</dd><dt><code>--no-hashes</code></dt><dd><p>在生成的输出中省略哈希值。</p>

</dd><dt><code>--no-header</code></dt><dd><p>在生成的输出文件顶部省略注释头。</p>

</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如 PyPI），改为依赖直接的 URL 依赖项和通过 <code>--find-links</code> 提供的依赖项。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，进度条或旋转指示器。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--no-sources</code></dt><dd><p>在解析依赖项时忽略 <code>tool.uv.sources</code> 表。用于将锁定文件与符合标准的、可发布的包元数据进行比较，而不是使用任何本地或 Git 源。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 只会使用本地缓存数据和本地可用文件。</p>

</dd><dt><code>--only-dev</code></dt><dd><p>仅包括开发依赖组。</p>

<p>省略其他依赖项。项目本身也将被省略。</p>

<p>此选项是 <code>--only-group dev</code> 的别名。</p>

</dd><dt><code>--only-group</code> <i>only-group</i></dt><dd><p>仅包括指定依赖组中的依赖项。</p>

<p>可以多次提供此选项。</p>

<p>项目本身也将被省略。</p>

</dd><dt><code>--output-file</code>, <code>-o</code> <i>output-file</i></dt><dd><p>将导出的要求写入指定的文件。</p>

</dd><dt><code>--package</code> <i>package</i></dt><dd><p>导出工作区中特定包的依赖项。</p>

<p>如果工作区成员不存在，uv 将退出并显示错误。</p>

</dd><dt><code>--prerelease</code> <i>prerelease</i></dt><dd><p>考虑预发布版本时使用的策略。</p>

<p>默认情况下，uv 将接受仅发布预发布版本的包，以及声明规格中明确包含预发布标记的第一方要求（<code>if-necessary-or-explicit</code>）。</p>

<p>也可以通过 <code>UV_PRERELEASE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disallow</code>:  不允许任何预发布版本</li>

<li><code>allow</code>:  允许所有预发布版本</li>

<li><code>if-necessary</code>:  如果一个包的所有版本都是预发布版本，则允许预发布版本</li>

<li><code>explicit</code>:  仅允许包含明确预发布标记的第一方包的预发布版本</li>

<li><code>if-necessary-or-explicit</code>:  如果一个包的所有版本都是预发布版本，或该包在其版本要求中有明确的预发布标记，则允许预发布版本</li>
</ul>
</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在指定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树进行发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>有关更改工作目录的详细信息，请参见 <code>--directory</code>。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--prune</code> <i>prune</i></dt><dd><p>从依赖树中修剪指定的包。</p>

<p>修剪的包将从导出的要求文件中排除，任何在修剪包被移除后不再需要的依赖项也将被排除。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>在解析过程中使用的 Python 解释器。</p>

<p>构建源分发包时需要 Python 解释器，以便在没有 wheel 文件时确定包的元数据。</p>

<p>如果 <code>requires-python</code> 未设置，解释器还将作为最低 Python 版本的回退值。</p>

<p>有关 Python 发现和支持的请求格式的详细信息，请参见 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装。</p>

<p>默认情况下，uv 优先使用它所管理的 Python 版本。然而，如果未安装 uv 管理的 Python，它将使用系统 Python 安装。此选项允许优先或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>:  仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>:  优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>:  优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>:  仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--refresh</code></dt><dd><p>刷新所有缓存的数据</p>

</dd><dt><code>--refresh-package</code> <i>refresh-package</i></dt><dd><p>刷新特定包的缓存数据</p>

</dd><dt><code>--resolution</code> <i>resolution</i></dt><dd><p>选择不同兼容版本时使用的策略。</p>

<p>默认情况下，uv 将使用每个包的最新兼容版本（<code>highest</code>）。</p>

<p>也可以通过 <code>UV_RESOLUTION</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>highest</code>:  解析每个包的最高兼容版本</li>

<li><code>lowest</code>:  解析每个包的最低兼容版本</li>

<li><code>lowest-direct</code>:  解析任何直接依赖的最低兼容版本，解析任何传递依赖的最高兼容版本</li>
</ul>
</dd><dt><code>--upgrade</code>, <code>-U</code></dt><dd><p>允许包升级，忽略任何现有输出文件中的固定版本。意味着 <code>--refresh</code>。</p>

</dd><dt><code>--upgrade-package</code>, <code>-P</code> <i>upgrade-package</i></dt><dd><p>允许特定包的升级，忽略任何现有输出文件中的固定版本。意味着 <code>--refresh-package</code>。</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>你可以使用 <code>RUST_LOG</code> 环境变量配置细粒度的日志记录。(&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本</p>

</dd></dl>

## uv tree

显示项目的依赖树

<h3 class="cli-reference">用法</h3>

```
uv tree [OPTIONS]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--all-groups</code></dt><dd><p>包括所有依赖组中的依赖项。</p>

<p><code>--no-group</code> 可用于排除特定的依赖组。</p>

</dd><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许不安全的主机连接。</p>

<p>可以多次提供此选项。</p>

<p>期望接收主机名（例如，<code>localhost</code>）、主机端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：此列表中的主机将不会与系统的证书存储进行验证。仅在安全网络和经过验证的源中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会暴露你于中间人攻击。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（在 macOS 和 Linux 上），以及 <code>%LOCALAPPDATA%\uv\cache</code>（在 Windows 上）。</p>

<p>要查看缓存目录的位置，可以运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值：auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>:  仅在输出到终端或支持 TTY 的终端时启用彩色输出</li>

<li><code>always</code>:  无论检测到的环境如何，始终启用彩色输出</li>

<li><code>never</code>:  禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--config-setting</code>, <code>-C</code> <i>config-setting</i></dt><dd><p>传递给 PEP 517 构建后端的设置，指定为 <code>KEY=VALUE</code> 对</p>

</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认：<code>https://pypi.org/simple</code>）。</p>

<p>接受符合 PEP 503（简单存储库 API）规范的仓库，或采用相同格式的本地目录。</p>

<p>此标志指定的索引优先级低于通过 <code>--index</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--depth</code>, <code>-d</code> <i>depth</i></dt><dd><p>依赖树的最大显示深度</p>

<p>[默认值：255]</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径以指定的目录为基准进行解析。</p>

<p>参见 <code>--project</code> 以仅更改项目根目录。</p>

</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>限制候选包为在指定日期之前上传的包。</p>

<p>接受 RFC 3339 时间戳（例如，<code>2006-12-02T02:07:43Z</code>）和本地日期（例如，<code>2006-12-02</code>）格式，按系统的时区设置。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量进行设置。</p>
</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：请改用 <code>--index</code>）额外的包索引 URL，除了 <code>--index-url</code> 外。</p>

<p>接受符合 PEP 503（简单存储库 API）规范的仓库，或采用相同格式的本地目录。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--index-url</code> 指定的索引（默认是 PyPI）。当提供多个 <code>--extra-index-url</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>搜索候选发行版的路径，除了在注册表索引中找到的之外。</p>

<p>如果是路径，则目标必须是一个包含轮子文件（<code>.whl</code>）或源代码发行版（例如，<code>.tar.gz</code> 或 <code>.zip</code>）的目录。</p>

<p>如果是 URL，则该页面必须包含符合上述格式的包文件链接的平面列表。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量进行设置。</p>
</dd><dt><code>--frozen</code></dt><dd><p>显示要求而不锁定项目。</p>

<p>如果缺少锁定文件，uv 将退出并报告错误。</p>

<p>也可以通过 <code>UV_FROZEN</code> 环境变量进行设置。</p>
</dd><dt><code>--group</code> <i>group</i></dt><dd><p>包括指定依赖组中的依赖项。</p>

<p>可以多次提供此选项。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示该命令的简明帮助</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>解析依赖时使用的 URL，除了默认索引外。</p>

<p>接受符合 PEP 503（简单存储库 API）规范的仓库，或采用相同格式的本地目录。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--default-index</code> 指定的索引（默认是 PyPI）。当提供多个 <code>--index</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 会在第一个包含给定包的索引上停止，并限制解析到该索引中存在的版本（<code>first-match</code>）。这可以防止“依赖混淆”攻击，攻击者可能会将恶意包上传到一个替代索引，并使用相同的包名。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>first-index</code>:  仅使用第一个返回匹配结果的索引中的结果</li>

<li><code>unsafe-first-match</code>:  在所有索引中搜索每个包名，在从第一个索引获取版本后，才转到下一个索引</li>

<li><code>unsafe-best-match</code>:  在所有索引中搜索每个包名，优先使用找到的“最佳”版本。如果一个包版本在多个索引中存在，只查看第一个索引中的条目</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：请改用 <code>--default-index</code>）Python 包索引的 URL（默认值：<code>https://pypi.org/simple</code>）。</p>

<p>接受符合 PEP 503（简单存储库 API）规范的仓库，或采用相同格式的本地目录。</p>

<p>此标志指定的索引优先级低于通过 <code>--extra-index-url</code> 提供的所有其他索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--invert</code></dt><dd><p>显示给定包的反向依赖关系。此标志会反转树并显示依赖于给定包的包。</p>

</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行身份验证，以访问索引 URL。</p>

<p>目前，仅支持 <code>--keyring-provider subprocess</code>，它配置 uv 使用 <code>keyring</code> 命令行工具来处理身份验证。</p>

<p>默认值为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disabled</code>:  不使用 keyring 查找凭据</li>

<li><code>subprocess</code>:  使用 <code>keyring</code> 命令行查找凭据</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>从全局缓存安装包时使用的方法。</p>

<p>此选项仅在构建源代码发行版时使用。</p>

<p>默认值：在 macOS 上为 <code>clone</code>（也称为写时复制），在 Linux 和 Windows 上为 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>clone</code>:  从 wheel 文件克隆（即写时复制）包到 <code>site-packages</code> 目录</li>

<li><code>copy</code>:  从 wheel 文件复制包到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>:  将 wheel 文件中的包硬链接到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>:  将 wheel 文件中的包符号链接到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--locked</code></dt><dd><p>确保 <code>uv.lock</code> 文件保持不变。</p>

<p>要求锁定文件是最新的。如果锁定文件缺失或需要更新，uv 将退出并报告错误。</p>

<p>也可以通过 <code>UV_LOCKED</code> 环境变量进行设置。</p>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是来自 Mozilla 的可靠信任根集合，将其包含在 uv 中有助于提高可移植性和性能（特别是在 macOS 上）。</p>

<p>但是，在某些情况下，您可能希望使用平台的本地证书存储，特别是如果您依赖于公司信任根（例如，用于强制代理）且该信任根已包含在系统的证书存储中。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-binary</code></dt><dd><p>不安装预构建的轮子文件。</p>

<p>给定的包将从源代码构建并安装。解析器仍然会使用预构建的轮子文件来提取包的元数据（如果可用）。</p>

</dd><dt><code>--no-binary-package</code> <i>no-binary-package</i></dt><dd><p>不为特定包安装预构建的轮子文件</p>

</dd><dt><code>--no-build</code></dt><dd><p>不构建源代码发行版。</p>

<p>启用时，解析将不会运行任意 Python 代码。已经构建的源代码发行版的缓存轮子文件将被重用，但需要构建发行版的操作将会出错并退出。</p>

</dd><dt><code>--no-build-isolation</code></dt><dd><p>构建源代码发行版时禁用隔离。</p>

<p>假设 PEP 518 中指定的构建依赖项已经安装。</p>

<p>也可以通过 <code>UV_NO_BUILD_ISOLATION</code> 环境变量进行设置。</p>
</dd><dt><code>--no-build-isolation-package</code> <i>no-build-isolation-package</i></dt><dd><p>为特定包构建源代码发行版时禁用隔离。</p>

<p>假设该包的构建依赖项已经安装，且符合 PEP 518 标准。</p>

</dd><dt><code>--no-build-package</code> <i>no-build-package</i></dt><dd><p>不为特定包构建源代码发行版</p>

</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录来执行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>, <code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父级目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-dedupe</code></dt><dd><p>不去重重复的依赖项。通常，当一个包的依赖项已经显示时，后续出现的依赖项不会重新显示，并会加上 (*) 来表示已经显示过了。启用此标志将导致这些重复项被重复显示。</p>

</dd><dt><code>--no-dev</code></dt><dd><p>省略开发依赖组。</p>

<p>此选项是 <code>--no-group dev</code> 的别名。</p>

</dd><dt><code>--no-group</code> <i>no-group</i></dt><dd><p>排除指定依赖组中的依赖项。</p>

<p>可以多次提供此选项。</p>

</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如，PyPI），而是依赖直接的 URL 依赖项和通过 <code>--find-links</code> 提供的依赖项。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，隐藏进度条或旋转器。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--no-sources</code></dt><dd><p>在解析依赖关系时忽略 <code>tool.uv.sources</code> 表。用于锁定符合标准的、可发布的包元数据，而不是使用任何本地或 Git 源。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>当禁用时，uv 只会使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--only-dev</code></dt><dd><p>仅包括开发依赖组。</p>

<p>省略其他依赖项。项目本身也会被省略。</p>

<p>此选项是 <code>--only-group dev</code> 的别名。</p>

</dd><dt><code>--only-group</code> <i>only-group</i></dt><dd><p>仅包括指定依赖组中的依赖项。</p>

<p>可以多次提供此选项。</p>

<p>项目本身也会被省略。</p>

</dd><dt><code>--outdated</code></dt><dd><p>显示树中每个包的最新可用版本。</p>

</dd><dt><code>--package</code> <i>package</i></dt><dd><p>仅显示指定的包。</p>

</dd><dt><code>--prerelease</code> <i>prerelease</i></dt><dd><p>在考虑预发布版本时使用的策略。</p>

<p>默认情况下，uv 会接受仅发布预发布版本的包，以及包含显式预发布标记的第一方依赖项（<code>if-necessary-or-explicit</code>）。</p>

<p>也可以通过 <code>UV_PRERELEASE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disallow</code>:  不允许任何预发布版本</li>

<li><code>allow</code>:  允许所有预发布版本</li>

<li><code>if-necessary</code>:  如果某个包的所有版本都是预发布版本，则允许预发布版本</li>

<li><code>explicit</code>:  允许第一方包中明确标记为预发布版本的包</li>

<li><code>if-necessary-or-explicit</code>:  如果某个包的所有版本都是预发布版本，或者该包的版本要求中包含显式的预发布标记，则允许预发布版本</li>
</ul>
</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树进行发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录解析。</p>

<p>参见 <code>--directory</code> 以完全更改工作目录。</p>

<p>在使用 <code>uv pip</code> 接口时此设置无效。</p>

</dd><dt><code>--prune</code> <i>prune</i></dt><dd><p>从依赖树显示中删除给定的包。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>用于锁定和过滤的 Python 解释器。</p>

<p>默认情况下，树会被过滤以匹配 Python 解释器报告的当前平台。使用 <code>--universal</code> 可以显示所有平台的树，或者使用 <code>--python-version</code> 或 <code>--python-platform</code> 来覆盖一部分标记。</p>

<p>请参见 <a href="#uv-python">uv python</a> 获取有关 Python 发现和支持的请求格式的详细信息。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-platform</code> <i>python-platform</i></dt><dd><p>用于过滤依赖树时的目标平台。</p>

<p>例如，传递 <code>--platform windows</code> 来显示安装在 Windows 上时会包含的依赖项。</p>

<p>该平台用“目标三元组”表示，即一个字符串，描述目标平台的 CPU、供应商和操作系统名称，例如 <code>x86_64-unknown-linux-gnu</code> 或 <code>aarch64-apple-darwin</code>。</p>

<p>可能的值：</p>

<ul>
<li><code>windows</code>:  <code>x86_64-pc-windows-msvc</code> 的别名，这是 Windows 的默认目标</li>

<li><code>linux</code>:  <code>x86_64-unknown-linux-gnu</code> 的别名，这是 Linux 的默认目标</li>

<li><code>macos</code>:  <code>aarch64-apple-darwin</code> 的别名，这是 macOS 的默认目标</li>

<li><code>x86_64-pc-windows-msvc</code>:  64 位 x86 Windows 目标</li>

<li><code>i686-pc-windows-msvc</code>:  32 位 x86 Windows 目标</li>

<li><code>x86_64-unknown-linux-gnu</code>:  x86 Linux 目标。等效于 <code>x86_64-manylinux_2_17</code></li>

<li><code>aarch64-apple-darwin</code>:  基于 ARM 的 macOS 目标，适用于 Apple Silicon 设备</li>

<li><code>x86_64-apple-darwin</code>:  x86 macOS 目标</li>

<li><code>aarch64-unknown-linux-gnu</code>:  ARM64 Linux 目标。等效于 <code>aarch64-manylinux_2_17</code></li>

<li><code>aarch64-unknown-linux-musl</code>:  ARM64 Linux 目标</li>

<li><code>x86_64-unknown-linux-musl</code>:  <code>x86_64</code> Linux 目标</li>

<li><code>x86_64-manylinux_2_17</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_17</code> 平台</li>

<li><code>x86_64-manylinux_2_28</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_28</code> 平台</li>

<li><code>x86_64-manylinux_2_31</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_31</code> 平台</li>

<li><code>x86_64-manylinux_2_32</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_32</code> 平台</li>

<li><code>x86_64-manylinux_2_33</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_33</code> 平台</li>

<li><code>x86_64-manylinux_2_34</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_34</code> 平台</li>

<li><code>x86_64-manylinux_2_35</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_35</code> 平台</li>

<li><code>x86_64-manylinux_2_36</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_36</code> 平台</li>

<li><code>x86_64-manylinux_2_37</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_37</code> 平台</li>

<li><code>x86_64-manylinux_2_38</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_38</code> 平台</li>

<li><code>x86_64-manylinux_2_39</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_39</code> 平台</li>

<li><code>x86_64-manylinux_2_40</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_40</code> 平台</li>

<li><code>aarch64-manylinux_2_17</code>:  ARM64 目标，适用于 <code>manylinux_2_17</code> 平台</li>

<li><code>aarch64-manylinux_2_28</code>:  ARM64 目标，适用于 <code>manylinux_2_28</code> 平台</li>

<li><code>aarch64-manylinux_2_31</code>:  ARM64 目标，适用于 <code>manylinux_2_31</code> 平台</li>

<li><code>aarch64-manylinux_2_32</code>:  ARM64 目标，适用于 <code>manylinux_2_32</code> 平台</li>

<li><code>aarch64-manylinux_2_33</code>:  ARM64 目标，适用于 <code>manylinux_2_33</code> 平台</li>

<li><code>aarch64-manylinux_2_34</code>:  ARM64 目标，适用于 <code>manylinux_2_34</code> 平台</li>

<li><code>aarch64-manylinux_2_35</code>:  ARM64 目标，适用于 <code>manylinux_2_35</code> 平台</li>

<li><code>aarch64-manylinux_2_36</code>:  ARM64 目标，适用于 <code>manylinux_2_36</code> 平台</li>

<li><code>aarch64-manylinux_2_37</code>:  ARM64 目标，适用于 <code>manylinux_2_37</code> 平台</li>

<li><code>aarch64-manylinux_2_38</code>:  ARM64 目标，适用于 <code>manylinux_2_38</code> 平台</li>

<li><code>aarch64-manylinux_2_39</code>:  ARM64 目标，适用于 <code>manylinux_2_39</code> 平台</li>

<li><code>aarch64-manylinux_2_40</code>:  ARM64 目标，适用于 <code>manylinux_2_40</code> 平台</li>
</ul>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装，或者系统的 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。如果没有安装 uv 管理的 Python，则会使用系统的 Python 安装。此选项允许优先考虑或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>:  仅使用管理的 Python 安装；从不使用系统的 Python 安装</li>

<li><code>managed</code>:  优先使用管理的 Python 安装，而不是系统的 Python 安装</li>

<li><code>system</code>:  优先使用系统的 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>:  仅使用系统的 Python 安装；从不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--python-version</code> <i>python-version</i></dt><dd><p>过滤树时使用的 Python 版本。</p>

<p>例如，传递 <code>--python-version 3.10</code> 以显示在 Python 3.10 上安装时将包含的依赖项。</p>

<p>默认为发现的 Python 解释器版本。</p>

</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出。</p>

</dd><dt><code>--resolution</code> <i>resolution</i></dt><dd><p>选择给定包依赖项时，采用的策略。</p>

<p>默认情况下，uv 会使用每个包的最新兼容版本（<code>highest</code>）。</p>

<p>也可以通过 <code>UV_RESOLUTION</code> 环境变量设置此选项。</p>
<p>可能的值：</p>

<ul>
<li><code>highest</code>:  解析每个包的最新兼容版本</li>

<li><code>lowest</code>:  解析每个包的最低兼容版本</li>

<li><code>lowest-direct</code>:  解析所有直接依赖项的最低兼容版本，解析所有传递依赖项的最高兼容版本</li>
</ul>
</dd><dt><code>--universal</code></dt><dd><p>显示平台无关的依赖树。</p>

<p>显示所有 Python 版本和平台的解析包版本，而不是仅显示当前环境相关的版本。</p>

<p>每个包可能会显示多个版本。</p>

</dd><dt><code>--upgrade</code>, <code>-U</code></dt><dd><p>允许包升级，忽略任何现有输出文件中的固定版本。隐式启用 <code>--refresh</code></p>

</dd><dt><code>--upgrade-package</code>, <code>-P</code> <i>upgrade-package</i></dt><dd><p>允许升级指定的包，忽略任何现有输出文件中的固定版本。隐式启用 <code>--refresh-package</code></p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>可以通过 <code>RUST_LOG</code> 环境变量配置细粒度日志记录。(<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives</a>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本</p>

</dd></dl>

## uv tool {: #uv-tool}

运行和安装由 Python 包提供的命令

<h3 class="cli-reference">使用方法</h3>

```
uv tool [OPTIONS] <COMMAND>
```

<h3 class="cli-reference">命令</h3>

<dl class="cli-reference"><dt><a href="#uv-tool-run"><code>uv tool run</code></a></dt><dd><p>运行由 Python 包提供的命令</p>
</dd>
<dt><a href="#uv-tool-install"><code>uv tool install</code></a></dt><dd><p>安装由 Python 包提供的命令</p>
</dd>
<dt><a href="#uv-tool-upgrade"><code>uv tool upgrade</code></a></dt><dd><p>升级已安装的工具</p>
</dd>
<dt><a href="#uv-tool-list"><code>uv tool list</code></a></dt><dd><p>列出已安装的工具</p>
</dd>
<dt><a href="#uv-tool-uninstall"><code>uv tool uninstall</code></a></dt><dd><p>卸载工具</p>
</dd>
<dt><a href="#uv-tool-update-shell"><code>uv tool update-shell</code></a></dt><dd><p>确保工具可执行文件目录在 <code>PATH</code> 中</p>
</dd>
<dt><a href="#uv-tool-dir"><code>uv tool dir</code></a></dt><dd><p>显示 uv 工具目录的路径</p>
</dd>
</dl>

### uv tool run

运行由 Python 包提供的命令。

默认情况下，假定安装的包与命令名称相匹配。

命令名称可以包括确切的版本号，格式为 `<package>@<version>`，例如，`uv tool run ruff@0.3.0`。如果需要更复杂的版本指定，或者如果命令是由不同的包提供的，可以使用 `--from`。

如果工具已安装，即通过 `uv tool install` 安装，将使用已安装的版本，除非指定了版本或使用了 `--isolated` 标志。

`uvx` 是 `uv tool run` 的便捷别名，它们的行为是相同的。

如果没有提供命令，将显示已安装的工具。

包将安装到 uv 缓存目录中的临时虚拟环境中。

<h3 class="cli-reference">使用方法</h3>

```
uv tool run [OPTIONS] [COMMAND]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机进行不安全的连接。</p>

<p>可以多次提供。</p>

<p>期望接收一个主机名（例如，<code>localhost</code>），主机-端口对（例如，<code>localhost:8080</code>）或一个 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：此列表中包含的主机将不会与系统的证书存储进行验证。仅在安全的网络和经过验证的来源中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会让您暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置此选项。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认为 <code>$XDG_CACHE_HOME/uv</code> 或在 macOS 和 Linux 上为 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，可以运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置此选项。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值：自动]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅当输出发送到支持的终端或 TTY 时，才启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，始终启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--compile-bytecode</code></dt><dd><p>安装后将 Python 文件编译为字节码。</p>

<p>默认情况下，uv 不会将 Python (<code>.py</code>) 文件编译为字节码（<code>__pycache__/*.pyc</code>）；而是在第一次导入模块时懒加载编译。对于启动时间至关重要的使用场景，如命令行应用程序和 Docker 容器，可以启用此选项，以换取更长的安装时间换取更快的启动时间。</p>

<p>启用时，uv 将处理整个 site-packages 目录（包括当前操作未修改的包）以确保一致性。与 pip 一样，它也会忽略错误。</p>

<p>也可以通过 <code>UV_COMPILE_BYTECODE</code> 环境变量设置此选项。</p>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量设置此选项。</p>
</dd><dt><code>--config-setting</code>, <code>-C</code> <i>config-setting</i></dt><dd><p>传递给 PEP 517 构建后端的设置，以 <code>KEY=VALUE</code> 对的形式指定</p>

</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认为：<a href="https://pypi.org/simple">https://pypi.org/simple</a>）。</p>

<p>接受符合 PEP 503（简单仓库 API）标准的仓库，或者以相同格式布局的本地目录。</p>

<p>通过此标志指定的索引优先级低于通过 <code>--index</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量设置此选项。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以给定目录为基准进行解析。</p>

<p>请参阅 <code>--project</code> 以仅更改项目根目录。</p>

</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>限制候选包为上传日期早于给定日期的包。</p>

<p>接受 RFC 3339 时间戳（例如，<code>2006-12-02T02:07:43Z</code>）和本地日期（例如，<code>2006-12-02</code>），按系统配置的时区。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量设置此选项。</p>
</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：请改用 <code>--index</code>）要使用的额外包索引 URL，除了 <code>--index-url</code>。</p>

<p>接受符合 PEP 503（简单仓库 API）标准的仓库，或者以相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先级高于通过 <code>--index-url</code> 指定的索引（默认为 PyPI）。当提供多个 <code>--extra-index-url</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量设置此选项。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>除了在注册表索引中找到的包外，还要查找候选分发的位置。</p>

<p>如果是路径，则目标必须是一个包含 wheel 文件（<code>.whl</code>）或源分发（例如，<code>.tar.gz</code> 或 <code>.zip</code>）的目录，且这些文件位于目录的顶部。</p>

<p>如果是 URL，则页面必须包含符合上述格式的包文件链接的平面列表。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量设置此选项。</p>
</dd><dt><code>--from</code> <i>from</i></dt><dd><p>使用指定的包提供命令。</p>

<p>默认情况下，假定包名与命令名称匹配。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简明帮助</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>解析依赖项时使用的 URL，除了默认索引。</p>

<p>接受符合 PEP 503（简单仓库 API）标准的仓库，或者以相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先级高于通过 <code>--default-index</code> 指定的索引（默认为 PyPI）。当提供多个 <code>--index</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量设置此选项。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 会在找到给定包的第一个可用索引时停止，并限制解析仅限于该第一个索引上存在的包（<code>first-match</code>）。这可以防止“依赖混淆”攻击，攻击者可以在一个替代索引上上传一个恶意包，并使用相同的名称。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量设置此选项。</p>
<p>可能的值：</p>

<ul>
<li><code>first-index</code>:  仅使用第一个返回匹配的索引中的结果</li>

<li><code>unsafe-first-match</code>:  在所有索引中搜索每个包名，在转到下一个索引之前，先用尽第一个索引中的版本</li>

<li><code>unsafe-best-match</code>:  在所有索引中搜索每个包名，优先选择找到的“最佳”版本。如果包版本存在于多个索引中，则只查看第一个索引中的条目</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：请改用 <code>--default-index</code>）Python 包索引的 URL（默认为：<a href="https://pypi.org/simple">https://pypi.org/simple</a>）。</p>

<p>接受符合 PEP 503（简单仓库 API）标准的仓库，或者以相同格式布局的本地目录。</p>

<p>通过此标志指定的索引优先级低于通过 <code>--extra-index-url</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量设置此选项。</p>
</dd><dt><code>--isolated</code></dt><dd><p>在隔离的虚拟环境中运行工具，忽略任何已经安装的工具</p>

</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行索引 URL 的身份验证。</p>

<p>目前，仅支持 <code>--keyring-provider subprocess</code>，这会将 uv 配置为使用 <code>keyring</code> CLI 处理身份验证。</p>

<p>默认为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量设置此选项。</p>
<p>可能的值：</p>

<ul>
<li><code>disabled</code>:  不使用 keyring 进行凭证查找</li>

<li><code>subprocess</code>:  使用 <code>keyring</code> 命令进行凭证查找</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>从全局缓存安装包时使用的方法。</p>

<p>在 macOS 上默认为 <code>clone</code>（也称为写时复制），在 Linux 和 Windows 上默认为 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量设置此选项。</p>
<p>可能的值：</p>

<ul>
<li><code>clone</code>:  从 wheel 克隆（即写时复制）包到 <code>site-packages</code> 目录</li>

<li><code>copy</code>:  从 wheel 复制包到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>:  从 wheel 硬链接包到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>:  从 wheel 符号链接包到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是来自 Mozilla 的可靠信任根集，包含它可以提高 uv 的可移植性和性能（特别是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，尤其是当您依赖于一个包含在系统证书存储中的公司信任根（例如，用于强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置此选项。</p>
</dd><dt><code>--no-binary</code></dt><dd><p>不要安装预构建的 wheel 文件。</p>

<p>给定的包将从源代码构建并安装。解析器仍将使用预构建的 wheel 文件来提取包元数据（如果可用）。</p>

</dd><dt><code>--no-binary-package</code> <i>no-binary-package</i></dt><dd><p>对于特定包，不安装预构建的 wheel 文件</p>

</dd><dt><code>--no-build</code></dt><dd><p>不要构建源分发包。</p>

<p>启用时，解析将不会运行任何 Python 代码。已经构建的源分发的缓存 wheel 文件将被重用，但需要构建分发的操作将会报错退出。</p>

</dd><dt><code>--no-build-isolation</code></dt><dd><p>在构建源分发包时禁用隔离。</p>

<p>假定按 PEP 518 指定的构建依赖项已经安装。</p>

<p>也可以通过 <code>UV_NO_BUILD_ISOLATION</code> 环境变量设置此选项。</p>
</dd><dt><code>--no-build-isolation-package</code> <i>no-build-isolation-package</i></dt><dd><p>为特定包构建源分发包时禁用隔离。</p>

<p>假定该包的构建依赖项已按 PEP 518 安装。</p>

</dd><dt><code>--no-build-package</code> <i>no-build-package</i></dt><dd><p>对于特定包，不构建源分发包。</p>

</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存中读取或写入数据，而是使用临时目录进行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>, <code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量设置。</p>
</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如 PyPI），改为依赖直接的 URL 依赖关系和通过 <code>--find-links</code> 提供的依赖关系。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--no-sources</code></dt><dd><p>在解析依赖关系时忽略 <code>tool.uv.sources</code> 表。用于锁定符合标准的、可发布的包元数据，而不是使用任何本地或 Git 源。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 将只使用本地缓存数据和本地可用文件。</p>

</dd><dt><code>--prerelease</code> <i>prerelease</i></dt><dd><p>考虑预发布版本时使用的策略。</p>

<p>默认情况下，uv 会接受仅发布预发布版本的包，以及声明明确预发布标记的第一方依赖项（<code>if-necessary-or-explicit</code>）。</p>

<p>也可以通过 <code>UV_PRERELEASE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disallow</code>: 禁止所有预发布版本</li>

<li><code>allow</code>: 允许所有预发布版本</li>

<li><code>if-necessary</code>: 仅在所有版本为预发布版本时允许预发布版本</li>

<li><code>explicit</code>: 仅允许第一方包具有明确预发布标记的版本要求</li>

<li><code>if-necessary-or-explicit</code>: 如果所有版本为预发布版本，或包在版本要求中有明确的预发布标记，则允许预发布版本</li>
</ul>
</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将在从项目根目录开始向上遍历目录树时被发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录解析。</p>

<p>请参阅 <code>--directory</code> 以完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用此设置没有效果。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>用于构建运行环境的 Python 解释器。</p>

<p>有关 Python 发现和支持的请求格式的详细信息，请参阅 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装。</p>

<p>默认情况下，uv 优先使用其管理的 Python 版本。如果没有安装 uv 管理的 Python，则会使用系统 Python 安装。此选项允许优先考虑或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；绝不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而非系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而非管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；绝不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--refresh</code></dt><dd><p>刷新所有缓存数据</p>

</dd><dt><code>--refresh-package</code> <i>refresh-package</i></dt><dd><p>刷新特定包的缓存数据</p>

</dd><dt><code>--reinstall</code></dt><dd><p>重新安装所有包，无论它们是否已经安装。隐式设置 <code>--refresh</code></p>

</dd><dt><code>--reinstall-package</code> <i>reinstall-package</i></dt><dd><p>重新安装特定包，无论它是否已经安装。隐式设置 <code>--refresh-package</code></p>

</dd><dt><code>--resolution</code> <i>resolution</i></dt><dd><p>在选择给定包要求的不同兼容版本时使用的策略。</p>

<p>默认情况下，uv 会使用每个包的最新兼容版本（<code>highest</code>）。</p>

<p>也可以通过 <code>UV_RESOLUTION</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>highest</code>:  解析每个包的最高兼容版本</li>

<li><code>lowest</code>:  解析每个包的最低兼容版本</li>

<li><code>lowest-direct</code>:  解析任何直接依赖项的最低兼容版本，解析任何传递依赖项的最高兼容版本</li>
</ul>
</dd><dt><code>--upgrade</code>, <code>-U</code></dt><dd><p>允许升级包，忽略任何现有输出文件中的固定版本。隐式设置 <code>--refresh</code></p>

</dd><dt><code>--upgrade-package</code>, <code>-P</code> <i>upgrade-package</i></dt><dd><p>允许特定包的升级，忽略任何现有输出文件中的固定版本。隐式设置 <code>--refresh-package</code></p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置细粒度的日志记录。 (&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd><dt><code>--with</code> <i>with</i></dt><dd><p>在安装了给定包的情况下运行</p>

</dd><dt><code>--with-editable</code> <i>with-editable</i></dt><dd><p>在安装给定包为可编辑包的情况下运行</p>

<p>当在项目中使用时，这些依赖项将在单独的、临时的环境中叠加在 uv 工具的环境之上。这些依赖项允许与已指定的依赖项发生冲突。</p>

</dd><dt><code>--with-requirements</code> <i>with-requirements</i></dt><dd><p>在给定的 <code>requirements.txt</code> 文件中列出的所有包的情况下运行</p>

</dd></dl>

### uv tool install

安装 Python 包提供的命令。

包被安装到 uv 工具目录中的隔离虚拟环境中。可执行文件链接到工具可执行文件目录，该目录根据 XDG 标准确定，并可以通过 `uv tool dir --bin` 获取。

如果工具之前已安装，现有工具通常会被替换。

<h3 class="cli-reference">用法</h3>

```
uv tool install [OPTIONS] <PACKAGE>
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>PACKAGE</code></dt><dd><p>要安装命令的包</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机建立不安全的连接。</p>

<p>可以多次提供此选项。</p>

<p>期望接收主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：在此列表中包含的主机不会与系统的证书存储进行验证。仅在安全网络中使用 <code>--allow-insecure-host</code>，并且源已验证，因为它会绕过 SSL 验证，可能会暴露于中间人攻击（MITM）中。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（在 macOS 和 Linux 上），以及 <code>%LOCALAPPDATA%\uv\cache</code>（在 Windows 上）。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认：自动]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅在输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，始终启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--compile-bytecode</code></dt><dd><p>安装后将 Python 文件编译为字节码。</p>

<p>默认情况下，uv 不会将 Python (<code>.py</code>) 文件编译为字节码（<code>__pycache__/*.pyc</code>）；而是首次导入模块时进行惰性编译。对于启动时间至关重要的用例，如 CLI 应用程序和 Docker 容器，可以启用此选项，在较长的安装时间和更快的启动时间之间做出权衡。</p>

<p>启用时，uv 将处理整个 site-packages 目录（包括当前操作未修改的包），以确保一致性。像 pip 一样，它也会忽略错误。</p>

<p>也可以通过 <code>UV_COMPILE_BYTECODE</code> 环境变量设置。</p>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这样做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量设置。</p>
</dd><dt><code>--config-setting</code>, <code>-C</code> <i>config-setting</i></dt><dd><p>传递给 PEP 517 构建后端的设置，以 <code>KEY=VALUE</code> 键值对的形式指定</p>

</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认为：<code>https://pypi.org/simple</code>）。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或以相同格式布局的本地目录。</p>

<p>通过此标志指定的索引优先级低于通过 <code>--index</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定目录。</p>

<p>相对路径将以指定目录为基础解析。</p>

<p>参见 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--editable</code>, <code>-e</code></dt><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>限制候选包为在指定日期之前上传的包。</p>

<p>接受 RFC 3339 时间戳（例如，<code>2006-12-02T02:07:43Z</code>）和本地日期（例如，<code>2006-12-02</code>）格式，基于系统配置的时区。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量设置。</p>
</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：请改用 <code>--index</code>）额外的包索引 URL，除了 <code>--index-url</code> 外。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或以相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--index-url</code> 指定的索引（默认为 PyPI）。当提供多个 <code>--extra-index-url</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>除了在注册表索引中找到的包外，还可以在这些位置查找候选分发包。</p>

<p>如果是路径，目标必须是包含以 wheel 文件（<code>.whl</code>）或源分发包（例如，<code>.tar.gz</code> 或 <code>.zip</code>）为顶级文件的目录。</p>

<p>如果是 URL，页面必须包含一个链接列表，指向符合上述格式的包文件。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量设置。</p>
</dd><dt><code>--force</code></dt><dd><p>强制安装该工具。</p>

<p>将替换可执行目录中具有相同名称的任何现有入口点。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简洁帮助</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>解析依赖关系时使用的 URL，除了默认索引外。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或以相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--default-index</code> 指定的索引（默认为 PyPI）。当提供多个 <code>--index</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>在解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 会在给定的包在第一个索引上可用时停止，并将解析限制在该第一个索引中存在的版本（<code>first-match</code>）。这可以防止“依赖冲突”攻击，其中攻击者可以将恶意包上传到备用索引中，并与同名的合法包冲突。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>first-index</code>:  仅使用返回给定包名匹配的第一个索引中的结果</li>

<li><code>unsafe-first-match</code>:  在所有索引中搜索每个包名，在移动到下一个索引之前，先耗尽第一个索引中的所有版本</li>

<li><code>unsafe-best-match</code>:  在所有索引中搜索每个包名，优先选择找到的“最佳”版本。如果一个包的版本存在于多个索引中，仅查看第一个索引中的条目</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：请改用 <code>--default-index</code>）Python 包索引的 URL（默认为：<code>https://pypi.org/simple</code>）。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或以相同格式布局的本地目录。</p>

<p>通过此标志指定的索引优先级低于通过 <code>--extra-index-url</code> 指定的所有其他索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量设置。</p>
</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行索引 URL 的身份验证。</p>

<p>目前仅支持 <code>--keyring-provider subprocess</code>，该选项配置 uv 使用 <code>keyring</code> CLI 来处理身份验证。</p>

<p>默认值为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disabled</code>:  不使用 keyring 查找凭据</li>

<li><code>subprocess</code>:  使用 <code>keyring</code> 命令查找凭据</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>安装来自全局缓存的包时使用的方法。</p>

<p>默认为 macOS 上的 <code>clone</code>（也称为写时复制），以及 Linux 和 Windows 上的 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>clone</code>:  从 wheel 克隆（即写时复制）包到 <code>site-packages</code> 目录</li>

<li><code>copy</code>:  从 wheel 复制包到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>:  从 wheel 硬链接包到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>:  从 wheel 符号链接包到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一组可靠的信任根，将其包含在 uv 中可以提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>但是，在某些情况下，您可能希望使用平台的本地证书存储，特别是当您依赖于系统证书存储中包含的企业信任根（例如，用于强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置。</p>
</dd><dt><code>--no-binary</code></dt><dd><p>不安装预构建的 wheel 文件。</p>

<p>指定的包将从源代码构建并安装。解析器仍将使用预构建的 wheel 文件来提取包元数据（如果可用）。</p>

</dd><dt><code>--no-binary-package</code> <i>no-binary-package</i></dt><dd><p>不为特定包安装预构建的 wheel 文件</p>

</dd><dt><code>--no-build</code></dt><dd><p>不构建源代码分发包。</p>

<p>启用时，解析将不会运行任意 Python 代码。已经构建的源代码分发包的缓存 wheel 将被重用，但需要构建分发包的操作将导致错误。</p>

</dd><dt><code>--no-build-isolation</code></dt><dd><p>在构建源代码分发包时禁用隔离。</p>

<p>假设通过 PEP 518 指定的构建依赖项已经安装。</p>

<p>也可以通过 <code>UV_NO_BUILD_ISOLATION</code> 环境变量设置。</p>
</dd><dt><code>--no-build-isolation-package</code> <i>no-build-isolation-package</i></dt><dd><p>在为特定包构建源代码分发包时禁用隔离。</p>

<p>假设该包的构建依赖项通过 PEP 518 已经安装。</p>

</dd><dt><code>--no-build-package</code> <i>no-build-package</i></dt><dd><p>不为特定包构建源代码分发包</p>

</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存中读取或写入，而是使用临时目录来执行操作</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量设置。</p>
</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如，PyPI），而是依赖于直接的 URL 依赖和通过 <code>--find-links</code> 提供的依赖</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，进度条或加载指示器。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--no-sources</code></dt><dd><p>解析依赖时忽略 <code>tool.uv.sources</code> 表。用于锁定与标准兼容的、可发布的包元数据，而不是使用任何本地或 Git 来源</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 只会使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--prerelease</code> <i>prerelease</i></dt><dd><p>考虑预发布版本时使用的策略。</p>

<p>默认情况下，uv 将接受仅发布预发布版本的包，以及包含显式预发布标记的第一方要求（<code>if-necessary-or-explicit</code>）。</p>

<p>也可以通过 <code>UV_PRERELEASE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disallow</code>:  禁止所有预发布版本</li>

<li><code>allow</code>:  允许所有预发布版本</li>

<li><code>if-necessary</code>:  如果一个包的所有版本都是预发布版本，则允许预发布版本</li>

<li><code>explicit</code>:  允许具有显式预发布标记的第一方包的预发布版本</li>

<li><code>if-necessary-or-explicit</code>:  如果一个包的所有版本都是预发布版本，或者包的版本要求中有显式的预发布标记，则允许预发布版本</li>
</ul>
</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将在从项目根目录开始向上遍历目录树时被发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录解析。</p>

<p>参见 <code>--directory</code> 来完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>用于构建工具环境的 Python 解释器。</p>

<p>有关 Python 发现和支持的请求格式，请参见 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装，或系统 Python 安装。</p>

<p>默认情况下，uv 更倾向于使用它管理的 Python 版本。然而，如果没有安装 uv 管理的 Python，uv 将使用系统的 Python 安装。此选项允许优先考虑或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>:  仅使用管理的 Python 安装；绝不使用系统 Python 安装</li>

<li><code>managed</code>:  优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>:  优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>:  仅使用系统 Python 安装；绝不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--refresh</code></dt><dd><p>刷新所有缓存数据</p>

</dd><dt><code>--refresh-package</code> <i>refresh-package</i></dt><dd><p>刷新特定包的缓存数据</p>

</dd><dt><code>--reinstall</code></dt><dd><p>重新安装所有包，无论它们是否已经安装。隐含 <code>--refresh</code></p>

</dd><dt><code>--reinstall-package</code> <i>reinstall-package</i></dt><dd><p>重新安装特定包，无论它是否已经安装。隐含 <code>--refresh-package</code></p>

</dd><dt><code>--resolution</code> <i>resolution</i></dt><dd><p>在选择给定包需求的不同兼容版本时使用的策略。</p>

<p>默认情况下，uv 将使用每个包的最新兼容版本（<code>highest</code>）。</p>

<p>也可以通过 <code>UV_RESOLUTION</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>highest</code>:  解析每个包的最高兼容版本</li>

<li><code>lowest</code>:  解析每个包的最低兼容版本</li>

<li><code>lowest-direct</code>:  解析任何直接依赖项的最低兼容版本，和任何传递依赖项的最高兼容版本</li>
</ul>
</dd><dt><code>--upgrade</code>, <code>-U</code></dt><dd><p>允许包升级，忽略任何现有输出文件中固定的版本。隐含 <code>--refresh</code></p>

</dd><dt><code>--upgrade-package</code>, <code>-P</code> <i>upgrade-package</i></dt><dd><p>允许特定包的升级，忽略任何现有输出文件中固定的版本。隐含 <code>--refresh-package</code></p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置细粒度的日志记录。(<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives</a>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd><dt><code>--with</code> <i>with</i></dt><dd><p>包括以下额外的依赖项</p>

</dd><dt><code>--with-editable</code> <i>with-editable</i></dt><dd><p>将给定的包作为可编辑包包括</p>

</dd><dt><code>--with-requirements</code> <i>with-requirements</i></dt><dd><p>运行给定 <code>requirements.txt</code> 文件中列出的所有依赖项</p>

</dd></dl>

### uv tool upgrade

升级已安装的工具。

如果工具是通过版本约束安装的，升级时将尊重这些约束——若要将工具升级到超出原始约束的版本，请再次使用 `uv tool install`。

如果工具是通过特定设置安装的，升级时这些设置也会被尊重。例如，如果在安装过程中提供了 `--prereleases allow`，那么在升级时它将继续生效。

<h3 class="cli-reference">用法</h3>

```
uv tool upgrade [选项] <名称>...
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>NAME</code></dt><dd><p>要升级的工具名称，后面可以跟上可选的版本说明符</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--all</code></dt><dd><p>升级所有工具</p>

</dd><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与不安全主机的连接。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如 <code>localhost</code>）、主机-端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：此列表中的主机将不会通过系统的证书存储进行验证。仅在有验证源的安全网络中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会暴露于中间人攻击。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认路径为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（在 macOS 和 Linux 上），在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，可以运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值：auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>:  仅当输出目标是支持颜色的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>:  无论环境如何，都启用彩色输出</li>

<li><code>never</code>:  禁用彩色输出</li>
</ul>
</dd><dt><code>--compile-bytecode</code></dt><dd><p>安装后将 Python 文件编译为字节码。</p>

<p>默认情况下，uv 不会将 Python（<code>.py</code>）文件编译为字节码（<code>__pycache__/*.pyc</code>）；相反，编译会在首次导入模块时按需进行。对于启动时间至关重要的使用场景，例如命令行应用程序和 Docker 容器，可以启用此选项，以用较长的安装时间换取更快的启动时间。</p>

<p>启用时，uv 将处理整个 site-packages 目录（包括当前操作没有修改的包），确保一致性。与 pip 类似，它也会忽略错误。</p>

<p>也可以通过 <code>UV_COMPILE_BYTECODE</code> 环境变量设置。</p>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>要用于配置的 <code>uv.toml</code> 文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这么做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量设置。</p>
</dd><dt><code>--config-setting</code>, <code>-C</code> <i>config-setting</i></dt><dd><p>传递给 PEP 517 构建后端的设置，格式为 <code>KEY=VALUE</code> 对</p>

</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认值：<code>https://pypi.org/simple</code>）。</p>

<p>接受符合 PEP 503 的存储库（简单存储库 API）或以相同格式布局的本地目录。</p>

<p>通过此标志给定的索引优先级低于所有通过 <code>--index</code> 标志指定的其他索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到给定目录。</p>

<p>相对路径将以给定目录作为基准进行解析。</p>

<p>参见 <code>--project</code> 来仅更改项目根目录。</p>

</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>将候选包限制为在给定日期之前上传的包。</p>

<p>接受 RFC 3339 时间戳（例如 <code>2006-12-02T02:07:43Z</code>）和本地日期（例如 <code>2006-12-02</code>），并会根据系统配置的时区使用这些日期。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量设置。</p>
</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：请改用 <code>--index</code>）附加的包索引 URL，除了 <code>--index-url</code> 外。</p>

<p>接受符合 PEP 503 的存储库（简单存储库 API）或以相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--index-url</code> 指定的索引（默认为 PyPI）。当提供多个 <code>--extra-index-url</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>要搜索候选分发包的位置，除了在注册表索引中找到的包之外。</p>

<p>如果是路径，则目标必须是一个目录，该目录的顶层包含 wheel 文件（<code>.whl</code>）或源分发包（例如 <code>.tar.gz</code> 或 <code>.zip</code>）。</p>

<p>如果是 URL，则该页面必须包含指向包文件的平面链接列表，符合上述格式。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量设置。</p>
</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示该命令的简洁帮助</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>解析依赖关系时使用的 URL，除了默认的索引外。</p>

<p>接受符合 PEP 503 的存储库（简单存储库 API）或以相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--default-index</code> 指定的索引（默认为 PyPI）。当提供多个 <code>--index</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 会在给定包可用的第一个索引处停止，并将解析限制为该索引中的包（<code>first-match</code>）。这可以防止“依赖混淆”攻击，攻击者可能会将恶意包以相同名称上传到备用索引。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>first-index</code>:  仅使用返回匹配包名称的第一个索引中的结果</li>

<li><code>unsafe-first-match</code>:  在所有索引中搜索每个包名称，在移动到下一个索引之前耗尽第一个索引中的版本</li>

<li><code>unsafe-best-match</code>:  在所有索引中搜索每个包名称，优先选择找到的“最佳”版本。如果某个包的版本出现在多个索引中，则仅查看第一个索引中的条目</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：请改用 <code>--default-index</code>）Python 包索引的 URL（默认为：<code>https://pypi.org/simple</code>）。</p>

<p>接受符合 PEP 503 的存储库（简单存储库 API）或以相同格式布局的本地目录。</p>

<p>通过此标志给定的索引优先级低于所有通过 <code>--extra-index-url</code> 标志指定的其他索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量设置。</p>
</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行索引 URL 的身份验证。</p>

<p>目前仅支持 <code>--keyring-provider subprocess</code>，它配置 uv 使用 <code>keyring</code> CLI 处理身份验证。</p>

<p>默认值为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disabled</code>:  不使用 keyring 查找凭证</li>

<li><code>subprocess</code>:  使用 <code>keyring</code> 命令查找凭证</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>从全局缓存安装包时使用的方法。</p>

<p>在 macOS 上默认使用 <code>clone</code>（也称为写时复制），在 Linux 和 Windows 上默认使用 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>clone</code>:  从 wheel 文件克隆（即写时复制）包到 <code>site-packages</code> 目录</li>

<li><code>copy</code>:  将包从 wheel 文件复制到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>:  将包从 wheel 文件硬链接到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>:  将包从 wheel 文件符号链接到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一组可靠的信任根，将其包含在 uv 中可以提高可移植性和性能（特别是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，尤其是当您依赖公司信任根（例如，针对强制代理的信任根）并且该根已包含在系统的证书存储中时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置。</p>
</dd><dt><code>--no-binary</code></dt><dd><p>不安装预构建的 wheel 文件。</p>

<p>给定的包将从源代码构建并安装。解析器仍会使用预构建的 wheel 文件提取包的元数据（如果可用）。</p>

</dd><dt><code>--no-binary-package</code> <i>no-binary-package</i></dt><dd><p>不为特定包安装预构建的 wheel 文件。</p>

</dd><dt><code>--no-build</code></dt><dd><p>不构建源分发包。</p>

<p>启用时，解析过程将不会运行任意的 Python 代码。已构建源分发包的缓存 wheel 将被重用，但需要构建分发包的操作将会失败并显示错误。</p>

</dd><dt><code>--no-build-isolation</code></dt><dd><p>构建源分发包时禁用隔离。</p>

<p>假定已安装 PEP 518 中指定的构建依赖。</p>

<p>也可以通过 <code>UV_NO_BUILD_ISOLATION</code> 环境变量设置。</p>
</dd><dt><code>--no-build-isolation-package</code> <i>no-build-isolation-package</i></dt><dd><p>为特定包构建源分发包时禁用隔离。</p>

<p>假定该包的构建依赖（由 PEP 518 指定）已安装。</p>

</dd><dt><code>--no-build-package</code> <i>no-build-package</i></dt><dd><p>不为特定包构建源分发包。</p>

</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录进行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>, <code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量设置。</p>
</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如 PyPI），而依赖直接的 URL 依赖和通过 <code>--find-links</code> 提供的依赖。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转指示器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--no-sources</code></dt><dd><p>在解析依赖关系时忽略 <code>tool.uv.sources</code> 表。用于锁定符合标准的可发布包元数据，而不是使用任何本地或 Git 源。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 仅使用本地缓存数据和本地可用文件。</p>

</dd><dt><code>--prerelease</code> <i>prerelease</i></dt><dd><p>在考虑预发布版本时使用的策略。</p>

<p>默认情况下，uv 会接受仅发布预发布版本的包，以及声明中明确标记预发布版本的第一方要求（<code>if-necessary-or-explicit</code>）。</p>

<p>也可以通过 <code>UV_PRERELEASE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disallow</code>:  不允许任何预发布版本</li>

<li><code>allow</code>:  允许所有预发布版本</li>

<li><code>if-necessary</code>:  如果所有版本都是预发布，则允许预发布版本</li>

<li><code>explicit</code>:  仅允许带有明确预发布标记的第一方包的预发布版本</li>

<li><code>if-necessary-or-explicit</code>:  如果所有版本都是预发布，或包的版本要求中有明确的预发布标记，则允许预发布版本</li>
</ul>
</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录解析。</p>

<p>请参见 <code>--directory</code> 以完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时此设置无效。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>升级工具，并指定使用给定的 Python 解释器来构建其环境。与 <code>--all</code> 一起使用可应用于所有工具。</p>

<p>有关 Python 发现和支持的请求格式的详细信息，请参见 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统的 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。但如果没有安装 uv 管理的 Python，它将使用系统的 Python 安装。此选项允许优先选择或忽略系统的 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>:  仅使用管理的 Python 安装；永不使用系统 Python 安装</li>

<li><code>managed</code>:  优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>:  优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>:  仅使用系统 Python 安装；永不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--reinstall</code></dt><dd><p>重新安装所有包，无论它们是否已经安装。隐式使用 <code>--refresh</code></p>

</dd><dt><code>--reinstall-package</code> <i>reinstall-package</i></dt><dd><p>重新安装特定包，无论它是否已经安装。隐式使用 <code>--refresh-package</code></p>

</dd><dt><code>--resolution</code> <i>resolution</i></dt><dd><p>在选择给定包需求的不同兼容版本时使用的策略。</p>

<p>默认情况下，uv 会使用每个包的最新兼容版本（<code>highest</code>）。</p>

<p>也可以通过 <code>UV_RESOLUTION</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>highest</code>:  解析每个包的最高兼容版本</li>

<li><code>lowest</code>:  解析每个包的最低兼容版本</li>

<li><code>lowest-direct</code>:  解析所有直接依赖的最低兼容版本，解析所有传递依赖的最高兼容版本</li>
</ul>
</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>你可以使用 <code>RUST_LOG</code> 环境变量配置更精细的日志记录。(&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

### uv tool list

列出已安装的工具

<h3 class="cli-reference">用法</h3>

```
uv tool list [OPTIONS]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机的非安全连接。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如 <code>localhost</code>）、主机-端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：此列表中的主机将不会与系统的证书存储进行验证。只在受信任的网络和已验证的源中使用 <code>--allow-insecure-host</code>，因为它会绕过 SSL 验证，可能使你暴露于中间人攻击。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（macOS 和 Linux），以及 <code>%LOCALAPPDATA%\uv\cache</code>（Windows）。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色。</p>

<p>[默认：auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>:  仅在输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>:  无论检测到的环境如何，始终启用彩色输出</li>

<li><code>never</code>:  禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许使用。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定目录。</p>

<p>相对路径会以给定目录作为基准进行解析。</p>

<p>请参见 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简要帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是来自 Mozilla 的一组可靠的信任根，将它们包含在 uv 中可以提高便携性和性能（特别是在 macOS 上）。</p>

<p>然而，在某些情况下，你可能希望使用平台的本地证书存储，特别是如果你依赖于系统证书存储中包含的企业信任根（例如，强制代理的信任根）。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存中读取或写入，而是使用临时目录执行操作</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转图标或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>当禁用时，uv 只会使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（例如相对路径）将相对于当前工作目录进行解析。</p>

<p>请参见 <code>--directory</code> 完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置没有效果。</p>

</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--show-paths</code></dt><dd><p>是否显示每个工具环境和已安装可执行文件的路径</p>

</dd><dt><code>--show-version-specifiers</code></dt><dd><p>是否显示用于安装每个工具的版本说明符</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>你可以使用 <code>RUST_LOG</code> 环境变量配置更精细的日志记录。(&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

### uv tool uninstall

卸载工具

<h3 class="cli-reference">用法</h3>

```
uv tool uninstall [OPTIONS] <NAME>...
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>NAME</code></dt><dd><p>要卸载的工具名称</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--all</code></dt><dd><p>卸载所有工具</p>

</dd><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机的非安全连接。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：此列表中的主机将不会与系统的证书存储进行验证。仅在安全网络和经过验证的来源中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或在 macOS 和 Linux 上为 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，可以运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值: auto]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>: 仅在输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>要使用的 <code>uv.toml</code> 配置文件路径。</p>

<p>尽管 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这样做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到给定目录。</p>

<p>相对路径将以给定目录为基准进行解析。</p>

<p>参见 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简明帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是一组来自 Mozilla 的可靠的信任根，将它们包含在 uv 中可以提高便携性和性能（特别是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，特别是如果您依赖于系统证书存储中包含的企业信任根（例如，用于强制代理的信任根）。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存中读取或写入，而是使用临时目录执行操作</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转图标或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 将只使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录解析。</p>

<p>参见 <code>--directory</code> 来完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。然而，如果没有安装 uv 管理的 Python，uv 将使用系统中的 Python 安装。此选项允许优先使用或忽略系统的 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量设置。</p>
<p>可选值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永远不使用系统的 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，超过系统的 Python 安装</li>

<li><code>system</code>: 优先使用系统的 Python 安装，超过管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统的 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置细粒度的日志记录。(&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

### uv tool update-shell

确保工具的可执行目录在 `PATH` 中。

如果工具的可执行目录不在 `PATH` 中，uv 将尝试将其添加到相关的 shell 配置文件中。

如果 shell 配置文件已经包含将可执行目录添加到路径中的说明，但该目录不在 `PATH` 中，uv 将退出并报告错误。

工具的可执行目录根据 XDG 标准确定，可以通过 `uv tool dir --bin` 获取。

<h3 class="cli-reference">用法</h3>

```
uv tool update-shell [OPTIONS]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机的非安全连接。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：此列表中的主机将不会与系统的证书存储进行验证。仅在安全网络和经过验证的来源中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或在 macOS 和 Linux 上为 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，可以运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值: auto]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>: 仅在输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>要使用的 <code>uv.toml</code> 配置文件路径。</p>

<p>尽管 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这样做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到给定目录。</p>

<p>相对路径将以给定目录为基准进行解析。</p>

<p>参见 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简明帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是一组可靠的 Mozilla 信任根，包含它们可以提高 uv 的可移植性和性能（尤其是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，特别是当您依赖于企业信任根（例如，强制代理）时，该信任根已包含在系统的证书存储中。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录进行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>, <code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转图标或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 仅使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录内运行命令。</p>

<p>所有的 <code>pyproject.toml</code>, <code>uv.toml</code>, 和 <code>.python-version</code> 文件会通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录解析。</p>

<p>参见 <code>--directory</code> 来完全更改工作目录。</p>

<p>此设置在 <code>uv pip</code> 接口中使用时无效。</p>

</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。然而，如果没有安装 uv 管理的 Python，uv 将使用系统中的 Python 安装。此选项允许优先使用或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量设置。</p>
<p>可选值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永远不使用系统的 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，超过系统的 Python 安装</li>

<li><code>system</code>: 优先使用系统的 Python 安装，超过管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统的 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置细粒度的日志记录。(&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

### uv tool dir

显示 uv 工具目录的路径。

工具目录用于存储已安装工具的环境和元数据。

默认情况下，工具存储在 uv 数据目录中，在 Unix 上是 <code>$XDG_DATA_HOME/uv/tools</code> 或 <code>$HOME/.local/share/uv/tools</code>，在 Windows 上是 <code>%APPDATA%\uv\data\tools</code>。

工具安装目录可以通过 <code>$UV_TOOL_DIR</code> 覆盖。

要查看 uv 安装可执行文件的目录，请使用 <code>--bin</code> 标志。

<h3 class="cli-reference">用法</h3>

```
uv tool dir [OPTIONS]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机的非安全连接。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：此列表中的主机将不会与系统的证书存储进行验证。仅在安全网络和经过验证的来源中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置。</p>
</dd><dt><code>--bin</code></dt><dd><p>显示 <code>uv tool</code> 将安装可执行文件的目录。</p>

<p>默认情况下，<code>uv tool dir</code> 显示的是工具 Python 环境本身的安装目录，而不是包含链接可执行文件的目录。</p>

<p>工具可执行文件目录根据 XDG 标准确定，并按优先顺序从以下环境变量中推导：</p>

<ul>
<li><code>$UV_TOOL_BIN_DIR</code></li>

<li><code>$XDG_BIN_HOME</code></li>

<li><code>$XDG_DATA_HOME/../bin</code></li>

<li><code>$HOME/.local/bin</code></li>
</ul>

</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或 macOS 和 Linux 上的 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值：auto]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>: 仅在输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论环境如何，始终启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以给定目录为基准解析。</p>

<p>参见 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简要帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是一组可靠的 Mozilla 信任根，包含它们可以提高 uv 的可移植性和性能（尤其是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，特别是当您依赖于企业信任根（例如，强制代理）时，该信任根已包含在系统的证书存储中。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存中读取或写入，而是使用临时目录执行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>, <code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转图标或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 仅使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在指定的项目目录内运行命令。</p>

<p>所有的 <code>pyproject.toml</code>, <code>uv.toml</code>, 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录解析。</p>

<p>参见 <code>--directory</code> 完全更改工作目录。</p>

<p>此设置在 <code>uv pip</code> 接口中使用时无效。</p>

</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。然而，如果没有安装 uv 管理的 Python，uv 将使用系统中的 Python 安装。此选项允许优先使用或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量设置。</p>
<p>可选值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以通过 <code>RUST_LOG</code> 环境变量配置细粒度日志记录。(&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

## uv python {: #uv-python}

管理 Python 版本和安装

通常，uv 首先在虚拟环境中查找 Python，无论该环境是否激活，或者在当前工作目录或任何父目录中的 `.venv` 目录下。如果不需要虚拟环境，uv 将继续查找 Python 解释器。Python 解释器通过在 `PATH` 环境变量中查找 Python 可执行文件来发现。

在 Windows 上，注册表也会被搜索以查找 Python 可执行文件。

默认情况下，如果找不到版本，uv 将下载 Python。您可以使用 `--no-python-downloads` 标志或 `python-downloads` 设置禁用此行为。

`--python` 选项允许请求使用不同的解释器。

支持以下 Python 版本请求格式：

- `<version>` 例如 `3`、`3.12`、`3.12.3`
- `<version-specifier>` 例如 `>=3.12,<3.13`
- `<implementation>` 例如 `cpython` 或 `cp`
- `<implementation>@<version>` 例如 `cpython@3.12`
- `<implementation><version>` 例如 `cpython3.12` 或 `cp312`
- `<implementation><version-specifier>` 例如 `cpython>=3.12,<3.13`
- `<implementation>-<version>-<os>-<arch>-<libc>` 例如 `cpython-3.12.3-macos-aarch64-none`

此外，还可以通过以下方式请求特定的系统 Python 解释器：

- `<executable-path>` 例如 `/opt/homebrew/bin/python3`
- `<executable-name>` 例如 `mypython3`
- `<install-dir>` 例如 `/some/environment/`

当使用 `--python` 选项时，正常的发现规则仍然适用，但会检查已发现的解释器是否与请求兼容。例如，如果请求 `pypy`，uv 将首先检查虚拟环境中是否包含 PyPy 解释器，然后检查路径中的每个可执行文件是否为 PyPy 解释器。

uv 支持发现 CPython、PyPy 和 GraalPy 解释器。对于不受支持的解释器，发现过程中会被跳过。如果请求了不受支持的解释器实现，uv 会以错误退出。

<h3 class="cli-reference">用法</h3>

```
uv python [OPTIONS] <COMMAND>
```

<h3 class="cli-reference">命令</h3>

<dl class="cli-reference"><dt><a href="#uv-python-list"><code>uv python list</code></a></dt><dd><p>列出可用的 Python 安装</p>
</dd>
<dt><a href="#uv-python-install"><code>uv python install</code></a></dt><dd><p>下载并安装 Python 版本</p>
</dd>
<dt><a href="#uv-python-find"><code>uv python find</code></a></dt><dd><p>搜索 Python 安装</p>
</dd>
<dt><a href="#uv-python-pin"><code>uv python pin</code></a></dt><dd><p>固定到特定的 Python 版本</p>
</dd>
<dt><a href="#uv-python-dir"><code>uv python dir</code></a></dt><dd><p>显示 uv Python 安装目录</p>
</dd>
<dt><a href="#uv-python-uninstall"><code>uv python uninstall</code></a></dt><dd><p>卸载 Python 版本</p>
</dd>
</dl>

### uv python list

列出可用的 Python 安装。

默认情况下，将显示已安装的 Python 版本和每个支持的 Python 主要版本的最新修补版本的下载。

显示的版本会受到 `--python-preference` 选项的过滤，即，如果使用 `only-system`，则不会显示管理的 Python 版本。

使用 `--all-versions` 查看所有可用的修补版本。

使用 `--only-installed` 仅显示已安装版本，忽略可用的下载。

```
uv python list [OPTIONS]
```

<h3 class="cli-reference">Options</h3>

<dl class="cli-reference"><dt><code>--all-platforms</code></dt><dd><p>列出所有平台的 Python 下载。</p>

<p>默认情况下，仅显示当前平台的下载。</p>

</dd><dt><code>--all-versions</code></dt><dd><p>列出所有 Python 版本，包括旧的修补版本。</p>

<p>默认情况下，仅显示每个次版本的最新修补版本。</p>

</dd><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与不安全主机的连接。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如 <code>localhost</code>）、主机-端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：在此列表中的主机不会经过系统证书库验证。仅在安全网络和验证源的环境中使用 <code>--allow-insecure-host</code>，因为它会绕过 SSL 验证，可能会使您遭遇中间人攻击。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认情况下，在 macOS 和 Linux 上为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认：auto]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>: 仅当输出发送到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以指定目录为基准进行解析。</p>

<p>查看 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示该命令的简明帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的原生证书库加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是来自 Mozilla 的一组可靠的信任根，将它们包含在 uv 中可以提高可移植性和性能（特别是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的原生证书库，尤其是当您依赖于系统证书库中包含的企业信任根（例如强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存读取或写入，而是使用临时目录来执行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>、<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 只会使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--only-installed</code></dt><dd><p>仅显示已安装的 Python 版本，排除可用下载。</p>

<p>默认情况下，将显示当前平台的可用下载。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在指定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也将被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录解析。</p>

<p>查看 <code>--directory</code> 完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。然而，如果没有安装 uv 管理的 Python，它将使用系统 Python 安装。此选项允许优先选择或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量设置。</p>
<p>可选值：</p>

<ul>
<li><code>only-managed</code>:  仅使用管理的 Python 安装；绝不使用系统 Python 安装</li>

<li><code>managed</code>:  优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>:  优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>:  仅使用系统 Python 安装；绝不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>可以通过 <code>RUST_LOG</code> 环境变量配置更细粒度的日志记录。(<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">详细说明</a>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本</p>

</dd></dl>

### uv python install

下载并安装 Python 版本。

可以请求多个 Python 版本。

支持 CPython 和 PyPy。CPython 发行版从 `python-build-standalone` 项目下载，PyPy 发行版从 `python.org` 下载。

Python 版本将安装到 uv Python 目录中，该目录可以通过 `uv python dir` 获取。

不会全局提供 `python` 可执行文件，管理的 Python 版本仅在 uv 命令中或在活动的虚拟环境中使用。实验性地支持将 Python 可执行文件添加到 `PATH` 中 — 使用 `--preview` 标志启用此行为。

请参见 `uv help python` 查看支持的请求格式。

<h3 class="cli-reference">用法</h3>

```
uv python install [OPTIONS] [TARGETS]...
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>TARGETS</code></dt><dd><p>要安装的 Python 版本。</p>

<p>如果未提供，将从 <code>.python-versions</code> 或 <code>.python-version</code> 文件中读取所请求的 Python 版本。如果这两个文件都不存在，uv 将检查是否已安装任何 Python 版本。如果没有，它将安装最新的稳定版 Python。</p>

<p>参见 <a href="#uv-python">uv python</a> 查看支持的请求格式。</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与不安全主机的连接。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如 <code>localhost</code>）、主机-端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：在此列表中的主机不会经过系统证书库验证。仅在安全网络和验证源的环境中使用 <code>--allow-insecure-host</code>，因为它会绕过 SSL 验证，可能会使您遭遇中间人攻击。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认情况下，在 macOS 和 Linux 上为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认：auto]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>: 仅当输出发送到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以给定目录作为基准来解析。</p>

<p>参见 <code>--project</code> 以仅更改项目根目录。</p>

</dd><dt><code>--force</code>, <code>-f</code></dt><dd><p>在安装过程中替换现有的 Python 可执行文件。</p>

<p>默认情况下，uv 会拒绝替换它未管理的可执行文件。</p>

<p>此选项等同于 <code>--reinstall</code>。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简明帮助信息</p>

</dd><dt><code>--mirror</code> <i>mirror</i></dt><dd><p>设置用于下载 Python 安装的源 URL。</p>

<p>提供的 URL 将替换 <code>https://github.com/indygreg/python-build-standalone/releases/download</code>，例如：<code>https://github.com/indygreg/python-build-standalone/releases/download/20240713/cpython-3.12.4%2B20240713-aarch64-apple-darwin-install_only.tar.gz</code>。</p>

<p>可以使用 <code>file://</code> URL 方案从本地目录读取发行版。</p>

<p>也可以通过 <code>UV_PYTHON_INSTALL_MIRROR</code> 环境变量设置。</p>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的原生证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是来自 Mozilla 的一组可靠的信任根，将其包含在 uv 中可以提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的原生证书存储，特别是如果您依赖于系统证书存储中包含的企业信任根（例如强制代理）。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录来完成操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免查找配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用后，uv 仅会使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树的方式被发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（例如相对路径）将相对于当前工作目录进行解析。</p>

<p>参见 <code>--directory</code> 以完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--pypy-mirror</code> <i>pypy-mirror</i></dt><dd><p>设置用于下载 PyPy 安装的源 URL。</p>

<p>提供的 URL 将替换 <code>https://downloads.python.org/pypy</code>，例如：<code>https://downloads.python.org/pypy/pypy3.8-v7.3.7-osx64.tar.bz2</code>。</p>

<p>可以使用 <code>file://</code> URL 方案从本地目录读取发行版。</p>

<p>也可以通过 <code>UV_PYPY_INSTALL_MIRROR</code> 环境变量设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。然而，如果没有安装 uv 管理的 Python，它将使用系统 Python 安装。此选项允许优先选择或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量设置。</p>
<p>可选值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；绝不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；绝不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--reinstall</code>, <code>-r</code></dt><dd><p>重新安装已请求的 Python 版本，如果它已经安装。</p>

<p>默认情况下，如果版本已经安装，uv 将成功退出。</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>可以通过 <code>RUST_LOG</code> 环境变量配置更细粒度的日志记录。(<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">详细说明</a>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本</p>

</dd></dl>

### uv python find

搜索 Python 安装。

显示 Python 可执行文件的路径。

参见 `uv help python` 以查看支持的请求格式和发现行为的详细信息。

<h3 class="cli-reference">用法</h3>

```
uv python find [选项] [请求]
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>REQUEST</code></dt><dd><p>Python 请求。</p>

<p>参见 <a href="#uv-python">uv python</a> 以查看支持的请求格式。</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机建立不安全连接。</p>

<p>此选项可以提供多次。</p>

<p>期望接收主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：列表中的主机不会与系统的证书存储进行验证。仅在经过验证的安全网络中使用 <code>--allow-insecure-host</code>，因为它会绕过 SSL 验证，并可能暴露于中间人攻击（MITM）中。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（macOS 和 Linux），以及 <code>%LOCALAPPDATA%\uv\cache</code>（Windows）。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值: auto]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>: 仅在输出被发送到支持颜色的终端或 TTY 时启用颜色输出</li>

<li><code>always</code>: 无论检测到的环境如何，都启用颜色输出</li>

<li><code>never</code>: 禁用颜色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>尽管 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许使用。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以给定目录作为基准进行解析。</p>

<p>参见 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简明帮助信息</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的原生证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是来自 Mozilla 的一组可靠的信任根，将其包含在 uv 中可以提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的原生证书存储，特别是如果您依赖于系统证书存储中包含的企业信任根（例如强制代理）。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录进行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免查找配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--no-project</code></dt><dd><p>避免发现项目或工作空间。</p>

<p>否则，当未提供请求时，将使用当前目录或父目录中项目的 Python 要求。</p>

</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用后，uv 将仅使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在指定的项目目录内运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>参见 <code>--directory</code> 完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用此设置没有效果。</p>

</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装，还是系统 Python 安装。</p>

<p>默认情况下，uv 更倾向于使用它管理的 Python 版本。然而，如果未安装 uv 管理的 Python，则会使用系统 Python 安装。此选项允许优先使用或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；从不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；从不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--system</code></dt><dd><p>仅查找系统 Python 解释器。</p>

<p>默认情况下，uv 将报告它将使用的第一个 Python 解释器，包括在活动的虚拟环境中或当前工作目录或任何父目录中的虚拟环境中的解释器。</p>

<p><code>--system</code> 选项指示 uv 跳过虚拟环境中的 Python 解释器，并将其搜索限制在系统路径上。</p>

<p>也可以通过 <code>UV_SYSTEM_PYTHON</code> 环境变量设置。</p>
</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>可以通过 <code>RUST_LOG</code> 环境变量配置更细粒度的日志记录。（<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>）</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

### uv python pin

固定为特定的 Python 版本。

将固定的版本写入 `.python-version` 文件，其他 uv 命令在确定所需的 Python 版本时会读取该文件。

参见 `uv help python` 查看支持的请求格式。

<h3 class="cli-reference">用法</h3>

```
uv python pin [选项] [请求]
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>REQUEST</code></dt><dd><p>Python 版本请求。</p>

<p>uv 支持比其他读取 <code>.python-version</code> 文件的工具更多的格式，即 <code>pyenv</code>。如果需要与这些工具兼容，请仅使用版本号，而不要使用复杂的请求，如 <code>cpython@3.10</code>。</p>

<p>参见 <a href="#uv-python">uv python</a> 查看支持的请求格式。</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机建立不安全连接。</p>

<p>此选项可以提供多次。</p>

<p>期望接收主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：列表中的主机不会与系统的证书存储进行验证。仅在经过验证的安全网络中使用 <code>--allow-insecure-host</code>，因为它会绕过 SSL 验证，并可能暴露于中间人攻击（MITM）中。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（macOS 和 Linux），以及 <code>%LOCALAPPDATA%\uv\cache</code>（Windows）。</p>

<p>要查看缓存目录的位置，运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值: auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅在输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，始终启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这么做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以给定目录为基础进行解析。</p>

<p>参见 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示该命令的简洁帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的原生证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一组可靠的信任根，包含它们可以提高 uv 的可移植性和性能（尤其是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的原生证书存储，特别是如果您依赖于企业信任根（例如，为强制代理使用的信任根），并且它已包含在系统的证书存储中。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录来执行操作</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>、<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，进度条或旋转图标。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--no-project</code></dt><dd><p>避免验证 Python 固定版本是否与项目或工作区兼容。</p>

<p>默认情况下，会在当前目录或任何父目录中发现项目或工作区。如果找到工作区，则会根据工作区的 <code>requires-python</code> 约束验证 Python 固定版本。</p>

</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 将仅使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在指定的项目目录内运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>参见 <code>--directory</code> 完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用此设置没有效果。</p>

</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装，还是系统 Python 安装。</p>

<p>默认情况下，uv 更倾向于使用它管理的 Python 版本。然而，如果未安装 uv 管理的 Python，则会使用系统 Python 安装。此选项允许优先使用或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；从不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；从不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--resolved</code></dt><dd><p>写入解析后的 Python 解释器路径，而不是请求。</p>

<p>确保使用完全相同的解释器。</p>

<p>此选项通常不适合在提交 <code>.python-version</code> 文件到版本控制时使用。</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置细粒度的日志记录。 (<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives</a>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本</p>

</dd></dl>

### uv python dir

显示 uv Python 安装目录。

默认情况下，Python 安装存储在 uv 数据目录中，Unix 系统上为 `$XDG_DATA_HOME/uv/python` 或 `$HOME/.local/share/uv/python`，Windows 系统上为 `%APPDATA%\uv\data\python`。

Python 安装目录可以通过 `$UV_PYTHON_INSTALL_DIR` 进行覆盖。

要查看 uv 安装 Python 可执行文件的目录，请使用 `--bin` 标志。请注意，Python 可执行文件仅在启用预览模式时安装。

<h3 class="cli-reference">用法</h3>

```
uv python dir [OPTIONS]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许连接到不安全的主机。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：此列表中的主机将不会通过系统的证书存储进行验证。只有在安全网络和经过验证的来源中使用 <code>--allow-insecure-host</code>，因为它会绕过 SSL 验证，可能会暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置。</p>
</dd><dt><code>--bin</code></dt><dd><p>显示 <code>uv python</code> 将安装 Python 可执行文件的目录。</p>

<p>请注意，只有在启用预览模式时才会使用此目录来安装 Python。</p>

<p>Python 可执行文件目录根据 XDG 标准确定，按照优先顺序由以下环境变量派生：</p>

<ul>
<li><code>$UV_PYTHON_BIN_DIR</code></li>

<li><code>$XDG_BIN_HOME</code></li>

<li><code>$XDG_DATA_HOME/../bin</code></li>

<li><code>$HOME/.local/bin</code></li>
</ul>

</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认情况下，macOS 和 Linux 为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>，Windows 为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值: auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅在输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，始终启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>要使用的 <code>uv.toml</code> 配置文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这么做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以给定目录为基础进行解析。</p>

<p>参见 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示该命令的简洁帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的原生证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一组可靠的信任根，包含它们可以提高 uv 的可移植性和性能（尤其是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的原生证书存储，特别是如果您依赖于系统证书存储中包含的企业信任根（例如，用于强制代理）。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存中读取或写入数据，改为使用临时目录进行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>, <code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父级目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，进度指示器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 仅使用本地缓存数据和本地可用文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件都会通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（例如相对路径）将相对于当前工作目录解析。</p>

<p>参见 <code>--directory</code> 完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用其管理的 Python 版本。如果未安装 uv 管理的 Python，则会使用系统 Python 安装。此选项允许优先使用或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置细粒度的日志记录。(<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives</a>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本</p>

</dd></dl>

### uv python uninstall

卸载 Python 版本

<h3 class="cli-reference">用法</h3>

```
uv python uninstall [选项] <目标>...
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>TARGETS</code></dt><dd><p>要卸载的 Python 版本。</p>

<p>请参阅 <a href="#uv-python">uv python</a> 以查看支持的请求格式。</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--all</code></dt><dd><p>卸载所有管理的 Python 版本</p>

</dd><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许不安全的连接到主机。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如 <code>localhost</code>）、主机-端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：此列表中的主机将不会与系统的证书存储进行验证。仅在具有验证源的安全网络中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会使您暴露于中间人攻击。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（macOS 和 Linux），以及 <code>%LOCALAPPDATA%\uv\cache</code>（Windows）。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值: auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅在输出发送到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径会以指定的目录为基准进行解析。</p>

<p>参见 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示该命令的简要帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的原生证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> 库加载证书。<code>webpki-roots</code> 是来自 Mozilla 的一组可靠的信任根，包含它们可以提高 uv 的可移植性和性能（尤其在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的原生证书存储，特别是如果您依赖于系统证书存储中包含的企业信任根（例如，用于强制代理）。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存中读取或写入数据，改为使用临时目录进行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>、<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父级目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，进度指示器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 仅使用本地缓存数据和本地可用文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件都会通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（例如相对路径）将相对于当前工作目录解析。</p>

<p>参见 <code>--directory</code> 以完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。如果未安装 uv 管理的 Python，uv 会使用系统 Python 安装。此选项允许优先使用或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装而非系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装而非管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置细粒度的日志记录。 (<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives</a>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

## uv pip

使用与 pip 兼容的接口管理 Python 包

<h3 class="cli-reference">用法</h3>

```
uv pip [选项] <命令>
```

<h3 class="cli-reference">命令</h3>

<dl class="cli-reference"><dt><a href="#uv-pip-compile"><code>uv pip compile</code></a></dt><dd><p>将 <code>requirements.in</code> 文件编译为 <code>requirements.txt</code> 文件</p>
</dd>
<dt><a href="#uv-pip-sync"><code>uv pip sync</code></a></dt><dd><p>将环境与 <code>requirements.txt</code> 文件同步</p>
</dd>
<dt><a href="#uv-pip-install"><code>uv pip install</code></a></dt><dd><p>将包安装到环境中</p>
</dd>
<dt><a href="#uv-pip-uninstall"><code>uv pip uninstall</code></a></dt><dd><p>从环境中卸载包</p>
</dd>
<dt><a href="#uv-pip-freeze"><code>uv pip freeze</code></a></dt><dd><p>以 requirements 格式列出已安装的包</p>
</dd>
<dt><a href="#uv-pip-list"><code>uv pip list</code></a></dt><dd><p>以表格格式列出已安装的包</p>
</dd>
<dt><a href="#uv-pip-show"><code>uv pip show</code></a></dt><dd><p>显示一个或多个已安装包的信息</p>
</dd>
<dt><a href="#uv-pip-tree"><code>uv pip tree</code></a></dt><dd><p>显示环境的依赖树</p>
</dd>
<dt><a href="#uv-pip-check"><code>uv pip check</code></a></dt><dd><p>验证已安装的包是否具有兼容的依赖关系</p>
</dd>
</dl>

### uv pip compile

将 `requirements.in` 文件编译为 `requirements.txt` 文件

<h3 class="cli-reference">用法</h3>

```
uv pip compile [选项] <源文件>...
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>SRC_FILE</code></dt><dd><p>包含所有在给定的 <code>requirements.in</code> 文件中列出的包。</p>

<p>如果提供了 <code>pyproject.toml</code>、<code>setup.py</code> 或 <code>setup.cfg</code> 文件，uv 将提取相关项目的依赖。</p>

<p>如果提供了 <code>-</code>，则要求从标准输入读取。</p>

<p>要求文件及其内容的顺序用于在解析时确定优先级。</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--all-extras</code></dt><dd><p>包括所有可选依赖项。</p>

<p>仅适用于 <code>pyproject.toml</code>、<code>setup.py</code> 和 <code>setup.cfg</code> 来源。</p>

</dd><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许不安全的连接到主机。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如 <code>localhost</code>）、主机-端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：此列表中包含的主机将不会与系统的证书存储进行验证。仅在安全的网络和已验证的源中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会使您暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量设置。</p>
</dd><dt><code>--annotation-style</code> <i>annotation-style</i></dt><dd><p>输出文件中包含的注释样式，用于指示每个包的来源。</p>

<p>默认为 <code>split</code>。</p>

<p>可能的值：</p>

<ul>
<li><code>line</code>: 将注释渲染为单行，用逗号分隔</li>

<li><code>split</code>: 将每个注释放在单独的一行</li>
</ul>
</dd><dt><code>--build-constraints</code>, <code>-b</code> <i>build-constraints</i></dt><dd><p>在构建源分发时，使用给定的要求文件约束构建依赖关系。</p>

<p>约束文件类似于 <code>requirements.txt</code> 文件，只控制已安装的依赖项的 <em>版本</em>。但是，将包包括在约束文件中并不会触发该包的安装。</p>

<p>也可以通过 <code>UV_BUILD_CONSTRAINT</code> 环境变量设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认为 <code>$XDG_CACHE_HOME/uv</code> 或在 macOS 和 Linux 上为 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值：auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅当输出进入支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这样做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量设置。</p>
</dd><dt><code>--config-setting</code>, <code>-C</code> <i>config-setting</i></dt><dd><p>要传递给 PEP 517 构建后端的设置，格式为 <code>KEY=VALUE</code> 对。</p>

</dd><dt><code>--constraints</code>, <code>-c</code> <i>constraints</i></dt><dd><p>使用给定的要求文件约束版本。</p>

<p>约束文件类似于 <code>requirements.txt</code> 文件，只控制已安装的依赖项的 <em>版本</em>。但是，将包包括在约束文件中并不会触发该包的安装。</p>

<p>这等同于 pip 的 <code>--constraint</code> 选项。</p>

<p>也可以通过 <code>UV_CONSTRAINT</code> 环境变量设置。</p>
</dd><dt><code>--custom-compile-command</code> <i>custom-compile-command</i></dt><dd><p>在由 <code>uv pip compile</code> 生成的输出文件顶部包含的头注释。</p>

<p>用于反映自定义构建脚本和包装 <code>uv pip compile</code> 的命令。</p>

<p>也可以通过 <code>UV_CUSTOM_COMPILE_COMMAND</code> 环境变量设置。</p>
</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认值：&lt;https://pypi.org/simple&gt;）。</p>

<p>接受符合 PEP 503（简单仓库 API）的仓库或以相同格式布局的本地目录。</p>

<p>此标志给定的索引优先级低于通过 <code>--index</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前更改为给定的目录。</p>

<p>相对路径会以给定目录为基础解析。</p>

<p>参见 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--emit-build-options</code></dt><dd><p>在生成的输出文件中包含 <code>--no-binary</code> 和 <code>--only-binary</code> 条目</p>

</dd><dt><code>--emit-find-links</code></dt><dd><p>在生成的输出文件中包含 <code>--find-links</code> 条目</p>

</dd><dt><code>--emit-index-annotation</code></dt><dd><p>在输出文件中包含注释注释，指示解析每个包时使用的索引（例如，<code># 来自 https://pypi.org/simple</code>）</p>

</dd><dt><code>--emit-index-url</code></dt><dd><p>在生成的输出文件中包含 <code>--index-url</code> 和 <code>--extra-index-url</code> 条目</p>

</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>将候选包限制为上传时间在给定日期之前的包。</p>

<p>接受 RFC 3339 时间戳（例如，<code>2006-12-02T02:07:43Z</code>）和系统配置时区中的本地日期（例如，<code>2006-12-02</code>）。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量设置。</p>
</dd><dt><code>--extra</code> <i>extra</i></dt><dd><p>包括指定额外名称的可选依赖项；可以提供多个此选项。</p>

<p>仅适用于 <code>pyproject.toml</code>、<code>setup.py</code> 和 <code>setup.cfg</code> 源。</p>

</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：请改用 <code>--index</code>）额外的包索引 URL，将与 <code>--index-url</code> 一起使用。</p>

<p>接受符合 PEP 503（简单仓库 API）的仓库，或本地以相同格式布局的目录。</p>

<p>此标志提供的所有索引优先级高于通过 <code>--index-url</code> 标志指定的索引（默认为 PyPI）。当提供多个 <code>--extra-index-url</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>除了在注册索引中找到的分发包外，搜索候选分发包的其他位置。</p>

<p>如果是路径，则目标必须是一个目录，目录中包含 wheel 文件（<code>.whl</code>）或源分发文件（如 <code>.tar.gz</code> 或 <code>.zip</code>）放在顶层。</p>

<p>如果是 URL，则该页面必须包含一个平面链接列表，指向符合上述格式的包文件。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量设置。</p>
</dd><dt><code>--generate-hashes</code></dt><dd><p>在输出文件中包含分发包哈希值</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简洁帮助</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>解析依赖项时使用的 URL，除了默认索引。</p>

<p>接受符合 PEP 503（简单仓库 API）的仓库，或本地以相同格式布局的目录。</p>

<p>通过此标志提供的所有索引优先级高于通过 <code>--default-index</code> 标志指定的索引（默认为 PyPI）。当提供多个 <code>--index</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 会在找到给定包的第一个索引时停止，并将解析限制在该索引中的包（<code>first-match</code>）。这样可以防止“依赖混淆”攻击，攻击者可以将恶意包上传到其他索引，并使用相同的包名。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>first-index</code>: 仅使用第一个返回给定包名匹配的索引的结果</li>

<li><code>unsafe-first-match</code>: 在所有索引中搜索每个包名，在继续搜索下一个索引之前耗尽第一个索引中的所有版本</li>

<li><code>unsafe-best-match</code>: 在所有索引中搜索每个包名，优先选择找到的“最佳”版本。如果一个包版本出现在多个索引中，则只查看第一个索引中的条目</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：请改用 <code>--default-index</code>）Python 包索引的 URL（默认值：&lt;https://pypi.org/simple&gt;）。</p>

<p>接受符合 PEP 503（简单仓库 API）的仓库，或本地以相同格式布局的目录。</p>

<p>此标志提供的索引优先级低于通过 <code>--extra-index-url</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量设置。</p>
</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行索引 URL 的身份验证。</p>

<p>目前仅支持 <code>--keyring-provider subprocess</code>，它配置 uv 使用 <code>keyring</code> CLI 处理身份验证。</p>

<p>默认值为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disabled</code>: 不使用 keyring 查找凭证</li>

<li><code>subprocess</code>: 使用 <code>keyring</code> 命令查找凭证</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>安装包时使用的方法，来自全局缓存。</p>

<p>此选项仅在构建源分发包时使用。</p>

<p>默认值为 macOS 上的 <code>clone</code>（也称为写时复制），在 Linux 和 Windows 上为 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>clone</code>: 将包从 wheel 克隆（即写时复制）到 <code>site-packages</code> 目录</li>

<li><code>copy</code>: 将包从 wheel 复制到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>: 将包从 wheel 硬链接到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>: 将包从 wheel 符号链接到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的可靠信任根集合，包含它们有助于提高 uv 的可移植性和性能（特别是在 macOS 上）。</p>

<p>但是，在某些情况下，您可能希望使用平台的本地证书存储，尤其是当您依赖于系统证书存储中包含的公司信任根（例如，强制代理的信任根）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量设置。</p>
</dd><dt><code>--no-annotate</code></dt><dd><p>排除注释标记，指示每个包的来源。</p>

</dd><dt><code>--no-binary</code> <i>no-binary</i></dt><dd><p>不安装预构建的 wheel 文件。</p>

<p>给定的包将从源代码构建并安装。解析器仍然会使用预构建的 wheel 来提取包元数据（如果可用）。</p>

<p>可以提供多个包。使用 <code>:all:</code> 禁用所有包的二进制文件。使用 <code>:none:</code> 清除先前指定的包。</p>

</dd><dt><code>--no-build</code></dt><dd><p>不构建源分发包。</p>

<p>启用时，解析将不会运行任意的 Python 代码。已经构建的源分发包的缓存 wheel 文件将被重用，但任何需要构建分发包的操作将会报错。</p>

<p>是 <code>--only-binary :all:</code> 的别名。</p>

</dd><dt><code>--no-build-isolation</code></dt><dd><p>禁用构建源分发包时的隔离。</p>

<p>假定由 PEP 518 指定的构建依赖项已经安装。</p>

<p>也可以通过 <code>UV_NO_BUILD_ISOLATION</code> 环境变量设置。</p>
</dd><dt><code>--no-build-isolation-package</code> <i>no-build-isolation-package</i></dt><dd><p>禁用为特定包构建源分发包时的隔离。</p>

<p>假定该包的构建依赖项由 PEP 518 指定并已安装。</p>

</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存中读取或写入，而是使用临时目录进行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量设置。</p>
</dd><dt><code>--no-deps</code></dt><dd><p>忽略包依赖项，仅将命令行中显式列出的包添加到结果需求文件中。</p>

</dd><dt><code>--no-emit-package</code> <i>no-emit-package</i></dt><dd><p>指定一个包从输出解析中排除。其依赖项仍将包含在解析中。等同于 pip-compile 的 <code>--unsafe-package</code> 选项。</p>

</dd><dt><code>--no-header</code></dt><dd><p>排除生成的输出文件顶部的注释头。</p>

</dd><dt><code>--no-index</code></dt><dd><p>忽略注册索引（例如，PyPI），而是依赖直接的 URL 依赖项和通过 <code>--find-links</code> 提供的依赖项。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--no-sources</code></dt><dd><p>解析依赖项时忽略 <code>tool.uv.sources</code> 表。用于锁定与标准兼容的可发布包元数据，而不是使用任何本地或 Git 源。</p>

</dd><dt><code>--no-strip-extras</code></dt><dd><p>在输出文件中包含 extras。</p>

<p>默认情况下，uv 会去除 extras，因为由 extras 引入的任何包已经作为依赖项直接包含在输出文件中。此外，使用 <code>--no-strip-extras</code> 生成的输出文件不能用作 <code>install</code> 和 <code>sync</code> 调用中的约束文件。</p>

</dd><dt><code>--no-strip-markers</code></dt><dd><p>在输出文件中包含环境标记。</p>

<p>默认情况下，uv 会去除环境标记，因为由 <code>compile</code> 生成的解析仅保证对目标环境正确。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 将仅使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--only-binary</code> <i>only-binary</i></dt><dd><p>仅使用预构建的 wheel 文件；不构建源分发包。</p>

<p>启用时，解析将不会运行来自给定包的代码。已经构建的源分发包的缓存 wheel 文件将被重用，但任何需要构建分发包的操作将会报错。</p>

<p>可以提供多个包。使用 <code>:all:</code> 禁用所有包的二进制文件。使用 <code>:none:</code> 清除先前指定的包。</p>

</dd><dt><code>--output-file</code>, <code>-o</code> <i>output-file</i></dt><dd><p>将编译后的需求写入指定的 <code>requirements.txt</code> 文件。</p>

<p>如果文件已存在，解析时将优先使用现有版本，除非同时指定了 <code>--upgrade</code>。</p>

</dd><dt><code>--overrides</code> <i>overrides</i></dt><dd><p>使用给定的需求文件覆盖版本。</p>

<p>覆盖文件是类似于 <code>requirements.txt</code> 的文件，它强制安装特定版本的依赖项，无论任何组成包声明的依赖项如何，也无论这是否会被视为无效解析。</p>

<p>约束是 <em>累加式</em> 的，即它们与组成包的需求一起结合，而覆盖是 <em>绝对式</em> 的，即它们完全替代组成包的需求。</p>

<p>也可以通过 <code>UV_OVERRIDE</code> 环境变量设置。</p>
</dd><dt><code>--prerelease</code> <i>prerelease</i></dt><dd><p>考虑预发布版本时使用的策略。</p>

<p>默认情况下，uv 将接受仅发布预发布版本的包，以及包含明确预发布标记的第一方需求（在声明的规范中使用 <code>if-necessary-or-explicit</code>）。</p>

<p>也可以通过 <code>UV_PRERELEASE</code> 环境变量设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disallow</code>:  禁止所有预发布版本</li>

<li><code>allow</code>:  允许所有预发布版本</li>

<li><code>if-necessary</code>:  如果所有版本都是预发布版本，则允许预发布版本</li>

<li><code>explicit</code>:  允许包含明确预发布标记的第一方包的预发布版本</li>

<li><code>if-necessary-or-explicit</code>:  如果所有版本都是预发布版本，或者包的版本要求中有明确的预发布标记，则允许预发布版本</li>
</ul>
</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将在从项目根目录向上遍历目录树时被发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录解析。</p>

<p>请参见 <code>--directory</code> 以完全更改工作目录。</p>

<p>在使用 <code>uv pip</code> 接口时，此设置无效。</p>

</dd><dt><code>--python</code> <i>python</i></dt><dd><p>解析时使用的 Python 解释器。</p>

<p>构建源分发包时需要 Python 解释器来确定包元数据（当没有 wheel 文件时）。</p>

<p>该解释器还用于确定默认的最低 Python 版本，除非提供了 <code>--python-version</code>。</p>

<p>有关 Python 发现和支持的请求格式的详细信息，请参见 <a href="#uv-python">uv python</a>。</p>

</dd><dt><code>--python-platform</code> <i>python-platform</i></dt><dd><p>解析依赖项时应该针对的平台。</p>

<p>表示为 "目标三元组"，这是描述目标平台的字符串，包括其 CPU、供应商和操作系统名称，例如 <code>x86_64-unknown-linux-gnu</code> 或 <code>aarch64-apple-darwin</code>。</p>

<p>可能的值：</p>

<ul>
<li><code>windows</code>:  <code>x86_64-pc-windows-msvc</code> 的别名，Windows 的默认目标</li>

<li><code>linux</code>:  <code>x86_64-unknown-linux-gnu</code> 的别名，Linux 的默认目标</li>

<li><code>macos</code>:  <code>aarch64-apple-darwin</code> 的别名，macOS 的默认目标</li>

<li><code>x86_64-pc-windows-msvc</code>:  64 位 x86 Windows 目标</li>

<li><code>i686-pc-windows-msvc</code>:  32 位 x86 Windows 目标</li>

<li><code>x86_64-unknown-linux-gnu</code>:  x86 Linux 目标。等同于 <code>x86_64-manylinux_2_17</code></li>

<li><code>aarch64-apple-darwin</code>:  基于 ARM 的 macOS 目标，适用于 Apple Silicon 设备</li>

<li><code>x86_64-apple-darwin</code>:  x86 macOS 目标</li>

<li><code>aarch64-unknown-linux-gnu</code>:  ARM64 Linux 目标。等同于 <code>aarch64-manylinux_2_17</code></li>

<li><code>aarch64-unknown-linux-musl</code>:  ARM64 Linux 目标</li>

<li><code>x86_64-unknown-linux-musl</code>:  <code>x86_64</code> Linux 目标</li>

<li><code>x86_64-manylinux_2_17</code>:  针对 <code>manylinux_2_17</code> 平台的 <code>x86_64</code> 目标</li>

<li><code>x86_64-manylinux_2_28</code>:  针对 <code>manylinux_2_28</code> 平台的 <code>x86_64</code> 目标</li>

<li><code>x86_64-manylinux_2_31</code>:  针对 <code>manylinux_2_31</code> 平台的 <code>x86_64</code> 目标</li>

<li><code>x86_64-manylinux_2_32</code>:  针对 <code>manylinux_2_32</code> 平台的 <code>x86_64</code> 目标</li>

<li><code>x86_64-manylinux_2_33</code>:  针对 <code>manylinux_2_33</code> 平台的 <code>x86_64</code> 目标</li>

<li><code>x86_64-manylinux_2_34</code>:  针对 <code>manylinux_2_34</code> 平台的 <code>x86_64</code> 目标</li>

<li><code>x86_64-manylinux_2_35</code>:  针对 <code>manylinux_2_35</code> 平台的 <code>x86_64</code> 目标</li>

<li><code>x86_64-manylinux_2_36</code>:  针对 <code>manylinux_2_36</code> 平台的 <code>x86_64</code> 目标</li>

<li><code>x86_64-manylinux_2_37</code>:  针对 <code>manylinux_2_37</code> 平台的 <code>x86_64</code> 目标</li>

<li><code>x86_64-manylinux_2_38</code>:  针对 <code>manylinux_2_38</code> 平台的 <code>x86_64</code> 目标</li>

<li><code>x86_64-manylinux_2_39</code>:  针对 <code>manylinux_2_39</code> 平台的 <code>x86_64</code> 目标</li>

<li><code>x86_64-manylinux_2_40</code>:  针对 <code>manylinux_2_40</code> 平台的 <code>x86_64</code> 目标</li>

<li><code>aarch64-manylinux_2_17</code>:  针对 <code>manylinux_2_17</code> 平台的 ARM64 目标</li>

<li><code>aarch64-manylinux_2_28</code>:  针对 <code>manylinux_2_28</code> 平台的 ARM64 目标</li>

<li><code>aarch64-manylinux_2_31</code>:  针对 <code>manylinux_2_31</code> 平台的 ARM64 目标</li>

<li><code>aarch64-manylinux_2_32</code>:  针对 <code>manylinux_2_32</code> 平台的 ARM64 目标</li>

<li><code>aarch64-manylinux_2_33</code>:  针对 <code>manylinux_2_33</code> 平台的 ARM64 目标</li>

<li><code>aarch64-manylinux_2_34</code>:  针对 <code>manylinux_2_34</code> 平台的 ARM64 目标</li>

<li><code>aarch64-manylinux_2_35</code>:  针对 <code>manylinux_2_35</code> 平台的 ARM64 目标</li>

<li><code>aarch64-manylinux_2_36</code>:  针对 <code>manylinux_2_36</code> 平台的 ARM64 目标</li>

<li><code>aarch64-manylinux_2_37</code>:  针对 <code>manylinux_2_37</code> 平台的 ARM64 目标</li>

<li><code>aarch64-manylinux_2_38</code>:  针对 <code>manylinux_2_38</code> 平台的 ARM64 目标</li>

<li><code>aarch64-manylinux_2_39</code>:  针对 <code>manylinux_2_39</code> 平台的 ARM64 目标</li>

<li><code>aarch64-manylinux_2_40</code>:  针对 <code>manylinux_2_40</code> 平台的 ARM64 目标</li>
</ul>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。然而，如果没有安装 uv 管理的 Python，它将使用系统 Python 安装。此选项允许优先选择或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量设置。</p>

<p>可能的值：</p>

<ul>
<li><code>only-managed</code>:  只使用 uv 管理的 Python 安装；从不使用系统 Python 安装</li>

<li><code>managed</code>:  优先使用 uv 管理的 Python 安装，而非系统 Python 安装</li>

<li><code>system</code>:  优先使用系统 Python 安装，而非 uv 管理的 Python 安装</li>

<li><code>only-system</code>:  只使用系统 Python 安装；从不使用 uv 管理的 Python 安装</li>
</ul>
</dd><dt><code>--python-version</code>, <code>-p</code> <i>python-version</i></dt><dd><p>用于解析的 Python 版本。</p>

<p>例如，<code>3.8</code> 或 <code>3.8.17</code>。</p>

<p>默认为用于解析的 Python 解释器的版本。</p>

<p>定义了解析后的要求必须支持的最低 Python 版本。</p>

<p>如果省略了补丁版本，则假定最低的补丁版本。例如，<code>3.8</code> 被映射为 <code>3.8.0</code>。</p>

</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--refresh</code></dt><dd><p>刷新所有缓存数据</p>

</dd><dt><code>--refresh-package</code> <i>refresh-package</i></dt><dd><p>刷新特定包的缓存数据</p>

</dd><dt><code>--resolution</code> <i>resolution</i></dt><dd><p>选择给定包要求的不同兼容版本时使用的策略。</p>

<p>默认情况下，uv 将使用每个包的最新兼容版本（<code>highest</code>）。</p>

<p>也可以通过 <code>UV_RESOLUTION</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>highest</code>:  解析每个包的最高兼容版本</li>

<li><code>lowest</code>:  解析每个包的最低兼容版本</li>

<li><code>lowest-direct</code>:  解析任何直接依赖项的最低兼容版本，以及任何传递依赖项的最高兼容版本</li>
</ul>
</dd><dt><code>--system</code></dt><dd><p>将包安装到系统 Python 环境中。</p>

<p>默认情况下，uv 使用当前工作目录或任何父目录中的虚拟环境，若找不到则会回退到在 <code>PATH</code> 中查找 Python 可执行文件。<code>--system</code> 选项指示 uv 避免使用虚拟环境中的 Python，而将搜索限制为系统路径。</p>

<p>也可以通过 <code>UV_SYSTEM_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--universal</code></dt><dd><p>执行通用解析，尝试生成一个与所有操作系统、架构和 Python 实现兼容的单一 <code>requirements.txt</code> 输出文件。</p>

<p>在通用模式下，当前的 Python 版本（或用户提供的 <code>--python-version</code>）将作为下限。例如，<code>--universal --python-version 3.7</code> 将生成一个适用于 Python 3.7 及更高版本的通用解析。</p>

<p>隐含 <code>--no-strip-markers</code>。</p>

</dd><dt><code>--upgrade</code>, <code>-U</code></dt><dd><p>允许包升级，忽略任何现有输出文件中的固定版本。隐含 <code>--refresh</code></p>

</dd><dt><code>--upgrade-package</code>, <code>-P</code> <i>upgrade-package</i></dt><dd><p>允许特定包的升级，忽略任何现有输出文件中的固定版本。隐含 <code>--refresh-package</code></p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置细粒度的日志记录。 (<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives</a>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

### uv pip sync

将环境与 `requirements.txt` 文件同步

<h3 class="cli-reference">用法</h3>

```
uv pip sync [选项] <SRC_FILE>...
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>SRC_FILE</code></dt><dd><p>包含给定 <code>requirements.txt</code> 文件中列出的所有包。</p>

<p>如果提供了 <code>pyproject.toml</code>、<code>setup.py</code> 或 <code>setup.cfg</code> 文件，uv 将提取相关项目的要求。</p>

<p>如果提供 <code>-</code>，则要求将从标准输入中读取。</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-empty-requirements</code></dt><dd><p>允许同步空的要求，这将清空环境中的所有包</p>

</dd><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与不安全主机的连接。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如 <code>localhost</code>）、主机-端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：此列表中的主机不会与系统的证书存储进行验证。仅在安全的网络和经过验证的源中使用 <code>--allow-insecure-host</code>，因为它绕过 SSL 验证，可能会暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--break-system-packages</code></dt><dd><p>允许 uv 修改 <code>EXTERNALLY-MANAGED</code> 的 Python 安装。</p>

<p>警告：<code>--break-system-packages</code> 仅用于持续集成（CI）环境，在向由外部包管理器（如 <code>apt</code>）管理的 Python 安装中安装时使用。应谨慎使用，因为此类 Python 安装明确不推荐其他包管理器（如 uv 或 <code>pip</code>）进行修改。</p>

<p>也可以通过 <code>UV_BREAK_SYSTEM_PACKAGES</code> 环境变量进行设置。</p>
</dd><dt><code>--build-constraints</code>, <code>-b</code> <i>build-constraints</i></dt><dd><p>在构建源分发时，使用给定的要求文件来约束构建依赖项。</p>

<p>约束文件类似于 <code>requirements.txt</code> 文件，只控制安装的要求的 <em>版本</em>。然而，包含某个包在约束文件中并不会触发安装该包。</p>

<p>也可以通过 <code>UV_BUILD_CONSTRAINT</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认情况下，设置为 <code>$XDG_CACHE_HOME/uv</code> 或在 macOS 和 Linux 上为 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值: auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>:  仅当输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>:  无论环境如何，始终启用彩色输出</li>

<li><code>never</code>:  禁用彩色输出</li>
</ul>
</dd><dt><code>--compile-bytecode</code></dt><dd><p>安装后将 Python 文件编译为字节码。</p>

<p>默认情况下，uv 不会将 Python (<code>.py</code>) 文件编译为字节码 (<code>__pycache__/*.pyc</code>)；相反，当首次导入模块时会进行懒编译。对于启动时间至关重要的用例，如 CLI 应用程序和 Docker 容器，可以启用此选项，以牺牲更长的安装时间来换取更快的启动时间。</p>

<p>启用时，uv 将处理整个 site-packages 目录（包括未被当前操作修改的包），以保持一致性。像 pip 一样，它也会忽略错误。</p>

<p>也可以通过 <code>UV_COMPILE_BYTECODE</code> 环境变量进行设置。</p>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>要使用的 <code>uv.toml</code> 配置文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许使用。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--config-setting</code>, <code>-C</code> <i>config-setting</i></dt><dd><p>要传递给 PEP 517 构建后端的设置，格式为 <code>KEY=VALUE</code> 对</p>

</dd><dt><code>--constraints</code>, <code>-c</code> <i>constraints</i></dt><dd><p>使用给定的要求文件约束版本。</p>

<p>约束文件类似于 <code>requirements.txt</code> 文件，只控制安装的要求的 <em>版本</em>。然而，包含某个包在约束文件中并不会触发安装该包。</p>

<p>这相当于 pip 的 <code>--constraint</code> 选项。</p>

<p>也可以通过 <code>UV_CONSTRAINT</code> 环境变量进行设置。</p>
</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认值：&lt;https://pypi.org/simple&gt;）。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或者以相同格式布置的本地目录。</p>

<p>此标志指定的索引优先级低于通过 <code>--index</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以指定的目录为基准进行解析。</p>

<p>参见 <code>--project</code> 以仅更改项目根目录。</p>

</dd><dt><code>--dry-run</code></dt><dd><p>执行试运行，即不实际安装任何内容，只解析依赖并打印生成的计划</p>

</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>将候选包限制为在给定日期之前上传的包。</p>

<p>接受 RFC 3339 时间戳（例如 <code>2006-12-02T02:07:43Z</code>）和本地日期（例如 <code>2006-12-02</code>）在系统配置的时区中的格式。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量进行设置。</p>
</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：请改用 <code>--index</code>）额外的包索引 URL，除了 <code>--index-url</code> 外。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或者以相同格式布置的本地目录。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--index-url</code>（默认为 PyPI）指定的索引。当提供多个 <code>--extra-index-url</code> 标志时，先前的值具有更高优先级。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>除了在注册表索引中找到的包外，还要搜索指定位置的候选分发包。</p>

<p>如果是路径，则目标必须是一个目录，该目录包含以 wheel 文件（<code>.whl</code>）或源分发文件（如 <code>.tar.gz</code> 或 <code>.zip</code>）为顶级的包。</p>

<p>如果是 URL，则页面必须包含遵循上述格式的包文件链接的平面列表。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量进行设置。</p>
</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简洁帮助</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>在解析依赖时使用的 URL，除了默认索引外。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或者以相同格式布置的本地目录。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--default-index</code>（默认为 PyPI）指定的索引。当提供多个 <code>--index</code> 标志时，先前的值具有更高优先级。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>在解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 会在给定包可用的第一个索引处停止，并将解析限制为该索引中存在的版本（<code>first-match</code>）。这样可以防止“依赖混淆”攻击，攻击者可以通过将恶意包上传到另一个索引并使用相同的包名进行攻击。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>first-index</code>:  仅使用第一个返回匹配的索引中的结果</li>

<li><code>unsafe-first-match</code>:  在所有索引中搜索每个包名，在转到下一个索引之前先消耗第一个索引中的版本</li>

<li><code>unsafe-best-match</code>:  在所有索引中搜索每个包名，优先选择找到的“最佳”版本。如果一个包版本出现在多个索引中，则仅查看第一个索引中的条目</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：请改用 <code>--default-index</code>）Python 包索引的 URL（默认值：&lt;https://pypi.org/simple&gt;）。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或者以相同格式布置的本地目录。</p>

<p>此标志指定的索引优先级低于通过 <code>--extra-index-url</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行索引 URL 的身份验证。</p>

<p>目前，仅支持 <code>--keyring-provider subprocess</code>，它配置 uv 使用 <code>keyring</code> CLI 来处理身份验证。</p>

<p>默认值为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disabled</code>:  不使用 keyring 进行凭据查找</li>

<li><code>subprocess</code>:  使用 <code>keyring</code> 命令进行凭据查找</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>安装来自全局缓存的包时使用的方法。</p>

<p>默认情况下，在 macOS 上使用 <code>clone</code>（也称为写时复制），在 Linux 和 Windows 上使用 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>clone</code>:  从 wheel 中克隆（即写时复制）包到 <code>site-packages</code> 目录</li>

<li><code>copy</code>:  从 wheel 中复制包到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>:  从 wheel 中硬链接包到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>:  从 wheel 中符号链接包到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 中加载证书。<code>webpki-roots</code> 是 Mozilla 提供的可靠信任根，将其包含在 uv 中可以提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，特别是当您依赖于系统证书存储中包含的企业信任根（例如用于强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-allow-empty-requirements</code></dt><dt><code>--no-binary</code> <i>no-binary</i></dt><dd><p>不安装预构建的 wheel 文件。</p>

<p>指定的包将从源代码构建并安装。解析器仍然会使用预构建的 wheel 文件提取包的元数据（如果可用）。</p>

<p>可以提供多个包。通过 <code>:all:</code> 禁用所有包的二进制文件。通过 <code>:none:</code> 清除之前指定的包。</p>

</dd><dt><code>--no-break-system-packages</code></dt><dt><code>--no-build</code></dt><dd><p>不构建源分发包。</p>

<p>启用时，解析将不会运行任意 Python 代码。已构建的源分发包的缓存 wheel 文件将被重用，但需要构建分发包的操作将会报错。</p>

<p>相当于 <code>--only-binary :all:</code>。</p>

</dd><dt><code>--no-build-isolation</code></dt><dd><p>禁用构建源分发包时的隔离。</p>

<p>假设按照 PEP 518 指定的构建依赖项已经安装。</p>

<p>也可以通过 <code>UV_NO_BUILD_ISOLATION</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存中读取或写入数据，而是使用临时目录执行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如，PyPI），改为依赖直接的 URL 依赖和通过 <code>--find-links</code> 提供的依赖。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转条或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--no-sources</code></dt><dd><p>在解析依赖时忽略 <code>tool.uv.sources</code> 表。用于锁定符合标准的、可发布的包元数据，而不是使用任何本地或 Git 源。</p>

</dd><dt><code>--no-verify-hashes</code></dt><dd><p>禁用对要求文件中哈希值的验证。</p>

<p>默认情况下，uv 会验证要求文件中所有可用的哈希值，但不会要求所有要求都有关联的哈希值。要强制哈希验证，请使用 <code>--require-hashes</code>。</p>

<p>也可以通过 <code>UV_NO_VERIFY_HASHES</code> 环境变量进行设置。</p>
</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 只会使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--only-binary</code> <i>only-binary</i></dt><dd><p>仅使用预构建的 wheel 文件；不构建源分发包。</p>

<p>启用时，解析不会运行指定包中的代码。已构建的源分发包的缓存 wheel 文件将被重用，但需要构建分发包的操作将会报错。</p>

<p>可以提供多个包。通过 <code>:all:</code> 禁用所有包的二进制文件。通过 <code>:none:</code> 清除之前指定的包。</p>

</dd><dt><code>--prefix</code> <i>prefix</i></dt><dd><p>将包安装到指定目录下的 <code>lib</code>、<code>bin</code> 和其他顶级文件夹中，就像该位置存在虚拟环境一样。</p>

<p>通常，建议使用 <code>--python</code> 安装到另一个环境，因为通过 <code>--prefix</code> 安装的脚本和其他工件将引用安装的解释器，而不是添加到 <code>--prefix</code> 目录中的解释器，这使得它们不可移植。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在指定的项目目录内运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将在从项目根目录向上遍历目录树时被发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>参见 <code>--directory</code> 以完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>安装包的 Python 解释器。</p>

<p>默认情况下，同步操作需要虚拟环境。可以提供替代 Python 的路径，但仅建议在持续集成（CI）环境中使用，并应谨慎使用，因为这可能会修改系统的 Python 安装。</p>

<p>有关 Python 自动发现和支持的请求格式，请参见 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-platform</code> <i>python-platform</i></dt><dd><p>应安装要求的目标平台。</p>

<p>表示为 "目标三元组"，一个描述目标平台的字符串，包含其 CPU、供应商和操作系统名称，如 <code>x86_64-unknown-linux-gnu</code> 或 <code>aarch64-apple-darwin</code>。</p>

<p>警告：当指定时，uv 将选择与 <em>目标</em> 平台兼容的 wheel 文件；因此，安装的分发包可能与 <em>当前</em> 平台不兼容。反之，任何从源代码构建的分发包可能与 <em>目标</em> 平台不兼容，因为它们将为 <em>当前</em> 平台构建。<code>--python-platform</code> 选项用于高级用例。</p>

<p>可能的值：</p>

<ul>
<li><code>windows</code>:  <code>x86_64-pc-windows-msvc</code> 的别名，Windows 的默认目标</li>

<li><code>linux</code>:  <code>x86_64-unknown-linux-gnu</code> 的别名，Linux 的默认目标</li>

<li><code>macos</code>:  <code>aarch64-apple-darwin</code> 的别名，macOS 的默认目标</li>

<li><code>x86_64-pc-windows-msvc</code>:  64 位 x86 Windows 目标</li>

<li><code>i686-pc-windows-msvc</code>:  32 位 x86 Windows 目标</li>

<li><code>x86_64-unknown-linux-gnu</code>:  x86 Linux 目标。等同于 <code>x86_64-manylinux_2_17</code></li>

<li><code>aarch64-apple-darwin</code>:  基于 ARM 的 macOS 目标，适用于 Apple Silicon 设备</li>

<li><code>x86_64-apple-darwin</code>:  x86 macOS 目标</li>

<li><code>aarch64-unknown-linux-gnu</code>:  ARM64 Linux 目标。等同于 <code>aarch64-manylinux_2_17</code></li>

<li><code>aarch64-unknown-linux-musl</code>:  ARM64 Linux 目标</li>

<li><code>x86_64-unknown-linux-musl</code>:  <code>x86_64</code> Linux 目标</li>

<li><code>x86_64-manylinux_2_17</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_17</code> 平台</li>

<li><code>x86_64-manylinux_2_28</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_28</code> 平台</li>

<li><code>x86_64-manylinux_2_31</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_31</code> 平台</li>

<li><code>x86_64-manylinux_2_32</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_32</code> 平台</li>

<li><code>x86_64-manylinux_2_33</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_33</code> 平台</li>

<li><code>x86_64-manylinux_2_34</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_34</code> 平台</li>

<li><code>x86_64-manylinux_2_35</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_35</code> 平台</li>

<li><code>x86_64-manylinux_2_36</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_36</code> 平台</li>

<li><code>x86_64-manylinux_2_37</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_37</code> 平台</li>

<li><code>x86_64-manylinux_2_38</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_38</code> 平台</li>

<li><code>x86_64-manylinux_2_39</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_39</code> 平台</li>

<li><code>x86_64-manylinux_2_40</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_40</code> 平台</li>

<li><code>aarch64-manylinux_2_17</code>:  ARM64 目标，适用于 <code>manylinux_2_17</code> 平台</li>

<li><code>aarch64-manylinux_2_28</code>:  ARM64 目标，适用于 <code>manylinux_2_28</code> 平台</li>

<li><code>aarch64-manylinux_2_31</code>:  ARM64 目标，适用于 <code>manylinux_2_31</code> 平台</li>

<li><code>aarch64-manylinux_2_32</code>:  ARM64 目标，适用于 <code>manylinux_2_32</code> 平台</li>

<li><code>aarch64-manylinux_2_33</code>:  ARM64 目标，适用于 <code>manylinux_2_33</code> 平台</li>

<li><code>aarch64-manylinux_2_34</code>:  ARM64 目标，适用于 <code>manylinux_2_34</code> 平台</li>

<li><code>aarch64-manylinux_2_35</code>:  ARM64 目标，适用于 <code>manylinux_2_35</code> 平台</li>

<li><code>aarch64-manylinux_2_36</code>:  ARM64 目标，适用于 <code>manylinux_2_36</code> 平台</li>

<li><code>aarch64-manylinux_2_37</code>:  ARM64 目标，适用于 <code>manylinux_2_37</code> 平台</li>

<li><code>aarch64-manylinux_2_38</code>:  ARM64 目标，适用于 <code>manylinux_2_38</code> 平台</li>

<li><code>aarch64-manylinux_2_39</code>:  ARM64 目标，适用于 <code>manylinux_2_39</code> 平台</li>

<li><code>aarch64-manylinux_2_40</code>:  ARM64 目标，适用于 <code>manylinux_2_40</code> 平台</li>
</ul>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。如果没有安装 uv 管理的 Python，uv 会使用系统的 Python 安装。此选项允许优先使用或忽略系统的 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>:  仅使用管理的 Python 安装；绝不使用系统 Python 安装</li>

<li><code>managed</code>:  优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>:  优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>:  仅使用系统 Python 安装；绝不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--python-version</code> <i>python-version</i></dt><dd><p>要求支持的最低 Python 版本（例如，<code>3.7</code> 或 <code>3.7.9</code>）。</p>

<p>如果省略补丁版本，则默认为最低补丁版本。例如，<code>3.7</code> 会映射到 <code>3.7.0</code>。</p>

</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--refresh</code></dt><dd><p>刷新所有缓存数据</p>

</dd><dt><code>--refresh-package</code> <i>refresh-package</i></dt><dd><p>刷新特定包的缓存数据</p>

</dd><dt><code>--reinstall</code></dt><dd><p>重新安装所有包，无论它们是否已经安装。此选项等同于 <code>--refresh</code></p>

</dd><dt><code>--reinstall-package</code> <i>reinstall-package</i></dt><dd><p>重新安装特定包，无论它是否已经安装。此选项等同于 <code>--refresh-package</code></p>

</dd><dt><code>--require-hashes</code></dt><dd><p>要求每个要求都有匹配的哈希值。</p>

<p>默认情况下，uv 会验证要求文件中提供的哈希值，但不会要求所有要求都必须有关联的哈希值。</p>

<p>启用 <code>--require-hashes</code> 时，<em>所有</em> 要求必须包括哈希或哈希集，且 <em>所有</em> 要求必须固定到精确版本（例如，<code>==1.0.0</code>），或者通过直接 URL 指定。</p>

<p>哈希校验模式引入了一些额外的约束：</p>

<ul>
<li>不支持 Git 依赖项。- 不支持可编辑安装。- 不支持本地依赖项，除非它们指向特定的 wheel 文件（<code>.whl</code>）或源代码归档文件（<code>.zip</code>、<code>.tar.gz</code>），而不是目录。</li>
</ul>

<p>也可以通过 <code>UV_REQUIRE_HASHES</code> 环境变量进行设置。</p>
</dd><dt><code>--strict</code></dt><dd><p>安装完成后验证 Python 环境，以检测缺失依赖项或其他问题</p>

</dd><dt><code>--system</code></dt><dd><p>将包安装到系统的 Python 环境中。</p>

<p>默认情况下，uv 会安装到当前工作目录或任何父目录中的虚拟环境中。<code>--system</code> 选项指示 uv 使用系统 <code>PATH</code> 中找到的第一个 Python。</p>

<p>警告：<code>--system</code> 选项适用于持续集成（CI）环境，并应谨慎使用，因为它可能会修改系统的 Python 安装。</p>

<p>也可以通过 <code>UV_SYSTEM_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--target</code> <i>target</i></dt><dd><p>将包安装到指定的目录，而不是虚拟或系统的 Python 环境中。包将安装在目录的顶层</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置细粒度的日志记录。(&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

### uv pip install

将包安装到环境中

<h3 class="cli-reference">用法</h3>

```
uv pip install [选项] <PACKAGE|--requirements <REQUIREMENTS>|--editable <EDITABLE>>
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>PACKAGE</code></dt><dd><p>安装所有列出的包。</p>

<p>包的顺序用于在解析时确定优先级。</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--all-extras</code></dt><dd><p>包括所有可选的依赖项。</p>

<p>仅适用于 <code>pyproject.toml</code>、<code>setup.py</code> 和 <code>setup.cfg</code> 源。</p>

</dd><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机进行不安全连接。</p>

<p>可以多次提供。</p>

<p>预期接受主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：此列表中的主机将不会与系统的证书存储进行验证。仅在安全的网络中且来源已验证的情况下使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能暴露于中间人攻击（MITM）中。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--break-system-packages</code></dt><dd><p>允许 uv 修改 <code>EXTERNALLY-MANAGED</code> 的 Python 安装。</p>

<p>警告：<code>--break-system-packages</code> 选项适用于持续集成（CI）环境，当安装到由外部包管理器（如 <code>apt</code>）管理的 Python 安装时。应谨慎使用，因为此类 Python 安装明确建议其他包管理器（如 uv 或 <code>pip</code>）不要修改。</p>

<p>也可以通过 <code>UV_BREAK_SYSTEM_PACKAGES</code> 环境变量进行设置。</p>
</dd><dt><code>--build-constraints</code>, <code>-b</code> <i>build-constraints</i></dt><dd><p>在构建源分发包时，使用给定的要求文件来约束构建依赖项。</p>

<p>约束文件类似于 <code>requirements.txt</code> 文件，仅控制安装要求的 <em>版本</em>。但是，将包包含在约束文件中并不会触发该包的安装。</p>

<p>也可以通过 <code>UV_BUILD_CONSTRAINT</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认情况下，缓存目录为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（在 macOS 和 Linux 上），以及 <code>%LOCALAPPDATA%\uv\cache</code>（在 Windows 上）。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认: auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>:  仅当输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>:  无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>:  禁用彩色输出</li>
</ul>
</dd><dt><code>--compile-bytecode</code></dt><dd><p>安装后将 Python 文件编译为字节码。</p>

<p>默认情况下，uv 不会将 Python (<code>.py</code>) 文件编译为字节码（<code>__pycache__/*.pyc</code>）；而是在第一次导入模块时延迟进行编译。在启动时间至关重要的用例中，例如命令行应用程序和 Docker 容器，可以启用此选项，以便将更长的安装时间换取更快的启动时间。</p>

<p>启用时，uv 会处理整个 site-packages 目录（包括当前操作未修改的包），以确保一致性。像 pip 一样，它也会忽略错误。</p>

<p>也可以通过 <code>UV_COMPILE_BYTECODE</code> 环境变量进行设置。</p>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许使用。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--config-setting</code>, <code>-C</code> <i>config-setting</i></dt><dd><p>传递给 PEP 517 构建后端的设置，指定为 <code>KEY=VALUE</code> 对</p>

</dd><dt><code>--constraints</code>, <code>-c</code> <i>constraints</i></dt><dd><p>使用给定的要求文件约束版本。</p>

<p>约束文件类似于 <code>requirements.txt</code> 文件，仅控制安装要求的 <em>版本</em>。但是，将包包含在约束文件中并不会触发该包的安装。</p>

<p>这等同于 pip 的 <code>--constraint</code> 选项。</p>

<p>也可以通过 <code>UV_CONSTRAINT</code> 环境变量进行设置。</p>
</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认为：&lt;https://pypi.org/simple&gt;）。</p>

<p>接受符合 PEP 503 的仓库（简单仓库 API），或以相同格式排列的本地目录。</p>

<p>此标志指定的索引的优先级低于通过 <code>--index</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前，切换到指定的目录。</p>

<p>相对路径以给定目录为基础解析。</p>

<p>参见 <code>--project</code>，仅更改项目根目录。</p>

</dd><dt><code>--dry-run</code></dt><dd><p>执行模拟运行，即不实际安装任何内容，只解析依赖项并打印结果计划。</p>

</dd><dt><code>--editable</code>, <code>-e</code> <i>editable</i></dt><dd><p>根据提供的本地文件路径安装可编辑包。</p>

</dd><dt><code>--exact</code></dt><dd><p>执行精确同步，移除多余的包。</p>

<p>默认情况下，安装会做出最低限度的更改以满足需求。启用后，uv 将更新环境以精确匹配需求，移除不包含在需求中的包。</p>

</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>将候选包限制为上传日期早于给定日期的包。</p>

<p>接受 RFC 3339 时间戳（例如，<code>2006-12-02T02:07:43Z</code>）和在系统配置时区中的本地日期（例如，<code>2006-12-02</code>）。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量进行设置。</p>
</dd><dt><code>--extra</code> <i>extra</i></dt><dd><p>包括指定额外名称的可选依赖项；可以多次提供。</p>

<p>仅适用于 <code>pyproject.toml</code>、<code>setup.py</code> 和 <code>setup.cfg</code> 源。</p>

</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：改用 <code>--index</code>）除了 <code>--index-url</code> 之外，使用额外的包索引 URL。</p>

<p>接受符合 PEP 503 的仓库（简单仓库 API），或以相同格式排列的本地目录。</p>

<p>通过此标志提供的所有索引优先级高于通过 <code>--index-url</code>（默认为 PyPI）指定的索引。当提供多个 <code>--extra-index-url</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>除了在注册表索引中找到的包之外，搜索候选分发包的路径。</p>

<p>如果是路径，目标必须是一个目录，其中包含顶级的 wheel 文件（<code>.whl</code>）或源分发包（例如，<code>.tar.gz</code> 或 <code>.zip</code>）。</p>

<p>如果是 URL，该页面必须包含一个平面列表，链接到符合上述格式的包文件。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量进行设置。</p>
</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简明帮助信息</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>在解析依赖关系时使用的 URL，除了默认索引外。</p>

<p>接受符合 PEP 503 的仓库（简单仓库 API），或以相同格式排列的本地目录。</p>

<p>通过此标志提供的所有索引优先级高于通过 <code>--default-index</code> 指定的索引（默认为 PyPI）。当提供多个 <code>--index</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>在解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 会在找到给定包的第一个索引时停止，并限制解析仅在该第一个索引中进行（<code>first-match</code>）。这可以防止“依赖混淆”攻击，其中攻击者可以将恶意包上传到一个备用索引，并使用相同的包名。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>first-index</code>:  仅使用第一个返回匹配项的索引的结果</li>

<li><code>unsafe-first-match</code>:  在所有索引中搜索每个包名，先从第一个索引耗尽版本再转到下一个索引</li>

<li><code>unsafe-best-match</code>:  在所有索引中搜索每个包名，优先使用找到的“最佳”版本。如果包版本存在于多个索引中，只查看第一个索引的条目</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：改用 <code>--default-index</code>）Python 包索引的 URL（默认为：&lt;https://pypi.org/simple&gt;）。</p>

<p>接受符合 PEP 503 的仓库（简单仓库 API），或以相同格式排列的本地目录。</p>

<p>此标志指定的索引的优先级低于通过 <code>--extra-index-url</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行索引 URL 的身份验证。</p>

<p>目前，仅支持 <code>--keyring-provider subprocess</code>，它配置 uv 使用 <code>keyring</code> CLI 处理身份验证。</p>

<p>默认值为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disabled</code>:  不使用 keyring 查找凭据</li>

<li><code>subprocess</code>:  使用 <code>keyring</code> 命令查找凭据</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>安装来自全局缓存的包时使用的方法。</p>

<p>在 macOS 上默认使用 <code>clone</code>（也称为写时复制），在 Linux 和 Windows 上默认使用 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>clone</code>:  从 wheel 文件克隆（即写时复制）包到 <code>site-packages</code> 目录</li>

<li><code>copy</code>:  将包从 wheel 文件复制到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>:  将包从 wheel 文件硬链接到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>:  将包从 wheel 文件符号链接到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储中加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一套可靠的信任根，将其包含在 uv 中有助于提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>但是，在某些情况下，您可能希望使用平台的本地证书存储，特别是当您依赖于包含在系统证书存储中的公司信任根（例如，强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-binary</code> <i>no-binary</i></dt><dd><p>不要安装预构建的 wheel 文件。</p>

<p>给定的包将从源代码构建并安装。解析器仍然会使用预构建的 wheel 文件提取包的元数据（如果可用）。</p>

<p>可以提供多个包。使用 <code>:all:</code> 禁用所有包的二进制文件。使用 <code>:none:</code> 清除之前指定的包。</p>

</dd><dt><code>--no-break-system-packages</code></dt><dt><code>--no-build</code></dt><dd><p>不要构建源分发包。</p>

<p>启用时，解析将不会运行任意 Python 代码。已构建的源分发包的缓存 wheel 文件将被重用，但需要构建分发包的操作将因错误而退出。</p>

<p>这是 <code>--only-binary :all:</code> 的别名。</p>

</dd><dt><code>--no-build-isolation</code></dt><dd><p>构建源分发包时禁用隔离。</p>

<p>假设 PEP 518 指定的构建依赖项已经安装。</p>

<p>也可以通过 <code>UV_NO_BUILD_ISOLATION</code> 环境变量进行设置。</p>
</dd><dt><code>--no-build-isolation-package</code> <i>no-build-isolation-package</i></dt><dd><p>为特定包构建源分发包时禁用隔离。</p>

<p>假设该包的构建依赖项由 PEP 518 指定，并且已安装。</p>

</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存读取或写入，而是使用临时目录进行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-deps</code></dt><dd><p>忽略包的依赖关系，仅安装命令行或需求文件中明确列出的包。</p>

</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如，PyPI），而仅依赖直接的 URL 依赖项和通过 <code>--find-links</code> 提供的依赖项。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转图标或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--no-sources</code></dt><dd><p>在解析依赖关系时忽略 <code>tool.uv.sources</code> 表。用于锁定符合标准的、可发布的包元数据，而不是使用任何本地或 Git 源。</p>

</dd><dt><code>--no-verify-hashes</code></dt><dd><p>禁用对需求文件中哈希的验证。</p>

<p>默认情况下，uv 会验证需求文件中的任何可用哈希，但不会要求所有需求都必须有关联的哈希。要强制哈希验证，请使用 <code>--require-hashes</code>。</p>

<p>也可以通过 <code>UV_NO_VERIFY_HASHES</code> 环境变量进行设置。</p>
</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 仅使用本地缓存数据和本地可用的文件。</p>

</dd><dt><code>--only-binary</code> <i>only-binary</i></dt><dd><p>仅使用预构建的 wheel 文件；不要构建源分发包。</p>

<p>启用时，解析将不会运行来自给定包的代码。已构建的源分发包的缓存 wheel 文件将被重用，但需要构建分发包的操作将因错误而退出。</p>

<p>可以提供多个包。使用 <code>:all:</code> 禁用所有包的二进制文件。使用 <code>:none:</code> 清除之前指定的包。</p>

</dd><dt><code>--overrides</code> <i>overrides</i></dt><dd><p>使用给定的需求文件覆盖版本。</p>

<p>覆盖文件是类似于 <code>requirements.txt</code> 的文件，强制安装特定版本的需求，无论任何组成包声明的需求如何，也无论这是否被视为无效的解析。</p>

<p>约束是 <em>附加的</em>，即它们与组成包的需求相结合，而覆盖是 <em>绝对的</em>，即它们完全替代了组成包的需求。</p>

<p>也可以通过 <code>UV_OVERRIDE</code> 环境变量进行设置。</p>
</dd><dt><code>--prefix</code> <i>prefix</i></dt><dd><p>将包安装到指定目录下的 <code>lib</code>、<code>bin</code> 和其他顶级文件夹，就像该位置存在虚拟环境一样。</p>

<p>通常，建议使用 <code>--python</code> 安装到替代环境，因为通过 <code>--prefix</code> 安装的脚本和其他工件将引用正在安装的解释器，而不是添加到 <code>--prefix</code> 目录中的解释器，这使得它们变得不可移植。</p>

</dd><dt><code>--prerelease</code> <i>prerelease</i></dt><dd><p>考虑预发布版本时使用的策略。</p>

<p>默认情况下，uv 会接受仅发布预发布版本的包，以及声明的规范中包含显式预发布标记的第一方需求（<code>if-necessary-or-explicit</code>）。</p>

<p>也可以通过 <code>UV_PRERELEASE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disallow</code>:  不允许任何预发布版本</li>

<li><code>allow</code>:  允许所有预发布版本</li>

<li><code>if-necessary</code>:  如果所有版本都是预发布，则允许预发布版本</li>

<li><code>explicit</code>:  允许带有显式预发布标记的第一方包的预发布版本</li>

<li><code>if-necessary-or-explicit</code>:  如果所有版本都是预发布，或包在其版本要求中有显式预发布标记，则允许预发布版本</li>
</ul>
</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也将被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>有关完全更改工作目录的设置，请参见 <code>--directory</code>。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置没有效果。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>安装包的 Python 解释器。</p>

<p>默认情况下，安装需要一个虚拟环境。可以提供替代 Python 的路径，但仅建议在持续集成（CI）环境中使用，并应谨慎使用，因为它可能会修改系统 Python 安装。</p>

<p>有关 Python 发现和支持的请求格式的详细信息，请参见 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-platform</code> <i>python-platform</i></dt><dd><p>应安装需求的目标平台。</p>

<p>表示为 "目标三元组"，即描述目标平台的字符串，包括 CPU、供应商和操作系统名称，例如 <code>x86_64-unknown-linux-gnu</code> 或 <code>aarch64-apple-darwin</code>。</p>

<p>警告：指定时，uv 将选择与 <em>目标</em> 平台兼容的 wheel 文件；因此，已安装的分发可能与 <em>当前</em> 平台不兼容。相反，任何从源代码构建的分发可能与 <em>目标</em> 平台不兼容，因为它们将为 <em>当前</em> 平台构建。<code>--python-platform</code> 选项仅用于高级用例。</p>

<p>可能的值：</p>

<ul>
<li><code>windows</code>:  <code>x86_64-pc-windows-msvc</code> 的别名，Windows 的默认目标平台</li>

<li><code>linux</code>:  <code>x86_64-unknown-linux-gnu</code> 的别名，Linux 的默认目标平台</li>

<li><code>macos</code>:  <code>aarch64-apple-darwin</code> 的别名，macOS 的默认目标平台</li>

<li><code>x86_64-pc-windows-msvc</code>:  64 位 x86 Windows 目标</li>

<li><code>i686-pc-windows-msvc</code>:  32 位 x86 Windows 目标</li>

<li><code>x86_64-unknown-linux-gnu</code>:  x86 Linux 目标。等同于 <code>x86_64-manylinux_2_17</code></li>

<li><code>aarch64-apple-darwin</code>:  基于 ARM 的 macOS 目标，适用于 Apple Silicon 设备</li>

<li><code>x86_64-apple-darwin</code>:  x86 macOS 目标</li>

<li><code>aarch64-unknown-linux-gnu</code>:  ARM64 Linux 目标。等同于 <code>aarch64-manylinux_2_17</code></li>

<li><code>aarch64-unknown-linux-musl</code>:  ARM64 Linux 目标</li>

<li><code>x86_64-unknown-linux-musl</code>:  <code>x86_64</code> Linux 目标</li>

<li><code>x86_64-manylinux_2_17</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_17</code> 平台</li>

<li><code>x86_64-manylinux_2_28</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_28</code> 平台</li>

<li><code>x86_64-manylinux_2_31</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_31</code> 平台</li>

<li><code>x86_64-manylinux_2_32</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_32</code> 平台</li>

<li><code>x86_64-manylinux_2_33</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_33</code> 平台</li>

<li><code>x86_64-manylinux_2_34</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_34</code> 平台</li>

<li><code>x86_64-manylinux_2_35</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_35</code> 平台</li>

<li><code>x86_64-manylinux_2_36</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_36</code> 平台</li>

<li><code>x86_64-manylinux_2_37</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_37</code> 平台</li>

<li><code>x86_64-manylinux_2_38</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_38</code> 平台</li>

<li><code>x86_64-manylinux_2_39</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_39</code> 平台</li>

<li><code>x86_64-manylinux_2_40</code>:  <code>x86_64</code> 目标，适用于 <code>manylinux_2_40</code> 平台</li>

<li><code>aarch64-manylinux_2_17</code>:  ARM64 目标，适用于 <code>manylinux_2_17</code> 平台</li>

<li><code>aarch64-manylinux_2_28</code>:  ARM64 目标，适用于 <code>manylinux_2_28</code> 平台</li>

<li><code>aarch64-manylinux_2_31</code>:  ARM64 目标，适用于 <code>manylinux_2_31</code> 平台</li>

<li><code>aarch64-manylinux_2_32</code>:  ARM64 目标，适用于 <code>manylinux_2_32</code> 平台</li>

<li><code>aarch64-manylinux_2_33</code>:  ARM64 目标，适用于 <code>manylinux_2_33</code> 平台</li>

<li><code>aarch64-manylinux_2_34</code>:  ARM64 目标，适用于 <code>manylinux_2_34</code> 平台</li>

<li><code>aarch64-manylinux_2_35</code>:  ARM64 目标，适用于 <code>manylinux_2_35</code> 平台</li>

<li><code>aarch64-manylinux_2_36</code>:  ARM64 目标，适用于 <code>manylinux_2_36</code> 平台</li>

<li><code>aarch64-manylinux_2_37</code>:  ARM64 目标，适用于 <code>manylinux_2_37</code> 平台</li>

<li><code>aarch64-manylinux_2_38</code>:  ARM64 目标，适用于 <code>manylinux_2_38</code> 平台</li>

<li><code>aarch64-manylinux_2_39</code>:  ARM64 目标，适用于 <code>manylinux_2_39</code> 平台</li>

<li><code>aarch64-manylinux_2_40</code>:  ARM64 目标，适用于 <code>manylinux_2_40</code> 平台</li>
</ul>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装，或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用其管理的 Python 版本。然而，如果没有安装 uv 管理的 Python，它会使用系统 Python 安装。此选项允许优先选择或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>

<p>可能的值：</p>

<ul>
<li><code>only-managed</code>:  只使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>:  优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>:  优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>:  只使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--python-version</code> <i>python-version</i></dt><dd><p>应支持的最小 Python 版本（例如，<code>3.7</code> 或 <code>3.7.9</code>）。</p>

<p>如果省略补丁版本，则假定使用最小补丁版本。例如，<code>3.7</code> 被映射为 <code>3.7.0</code>。</p>

</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--refresh</code></dt><dd><p>刷新所有缓存数据</p>

</dd><dt><code>--refresh-package</code> <i>refresh-package</i></dt><dd><p>刷新指定包的缓存数据</p>

</dd><dt><code>--reinstall</code></dt><dd><p>重新安装所有包，无论它们是否已经安装。隐式使用 <code>--refresh</code></p>

</dd><dt><code>--reinstall-package</code> <i>reinstall-package</i></dt><dd><p>重新安装指定的包，无论它是否已经安装。隐式使用 <code>--refresh-package</code></p>

</dd><dt><code>--require-hashes</code></dt><dd><p>要求每个依赖项都具有匹配的哈希值。</p>

<p>默认情况下，uv 会验证要求文件中提供的所有可用哈希值，但不会要求所有要求都具有相关联的哈希值。</p>

<p>启用 <code>--require-hashes</code> 时，<em>所有</em>要求必须包括一个哈希值或一组哈希值，并且 <em>所有</em>要求必须锁定为精确版本（例如，<code>==1.0.0</code>），或通过直接 URL 指定。</p>

<p>哈希检查模式引入了许多附加限制：</p>

<ul>
<li>不支持 Git 依赖项。 - 不支持可编辑安装。 - 不支持本地依赖项，除非它们指向特定的 wheel 文件（<code>.whl</code>）或源归档文件（<code>.zip</code>，<code>.tar.gz</code>），而不是目录。</li>
</ul>

<p>也可以通过 <code>UV_REQUIRE_HASHES</code> 环境变量进行设置。</p>
</dd><dt><code>--requirements</code>, <code>-r</code> <i>requirements</i></dt><dd><p>安装给定的 <code>requirements.txt</code> 文件中列出的所有包。</p>

<p>如果提供了 <code>pyproject.toml</code>、<code>setup.py</code> 或 <code>setup.cfg</code> 文件，uv 将提取相关项目的要求。</p>

<p>如果提供了 <code>-</code>，则从标准输入读取要求。</p>

</dd><dt><code>--resolution</code> <i>resolution</i></dt><dd><p>在选择给定包要求的不同兼容版本时使用的策略。</p>

<p>默认情况下，uv 会使用每个包的最新兼容版本（<code>highest</code>）。</p>

<p>也可以通过 <code>UV_RESOLUTION</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>highest</code>:  解决每个包的最高兼容版本</li>

<li><code>lowest</code>:  解决每个包的最低兼容版本</li>

<li><code>lowest-direct</code>:  解决所有直接依赖项的最低兼容版本，并解决所有传递依赖项的最高兼容版本</li>
</ul>
</dd><dt><code>--strict</code></dt><dd><p>在安装完成后验证 Python 环境，以检测缺少依赖项或其他问题的包</p>

</dd><dt><code>--system</code></dt><dd><p>将包安装到系统 Python 环境中。</p>

<p>默认情况下，uv 将包安装到当前工作目录或任何父目录中的虚拟环境中。使用 <code>--system</code> 选项时，uv 将改为使用系统 <code>PATH</code> 中找到的第一个 Python。</p>

<p>警告：<code>--system</code> 选项适用于持续集成（CI）环境，应该小心使用，因为它可能会修改系统 Python 安装。</p>

<p>也可以通过 <code>UV_SYSTEM_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--target</code> <i>target</i></dt><dd><p>将包安装到指定的目录，而不是虚拟或系统 Python 环境。包将安装在目录的顶级。</p>

</dd><dt><code>--upgrade</code>, <code>-U</code></dt><dd><p>允许包升级，忽略现有输出文件中锁定的版本。隐式使用 <code>--refresh</code></p>

</dd><dt><code>--upgrade-package</code>, <code>-P</code> <i>upgrade-package</i></dt><dd><p>允许特定包升级，忽略现有输出文件中锁定的版本。隐式使用 <code>--refresh-package</code></p>

</dd><dt><code>--user</code></dt><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>你可以通过 <code>RUST_LOG</code> 环境变量配置精细化日志记录。(<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives</a>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

### uv pip uninstall

从环境中卸载包

<h3 class="cli-reference">用法</h3>

```
uv pip uninstall [选项] <包名|--requirements <依赖文件>>
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>PACKAGE</code></dt><dd><p>卸载所有列出的包</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许连接到不安全的主机。</p>

<p>可以多次提供。</p>

<p>预期接收主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：此列表中的主机不会通过系统的证书存储进行验证。仅在受信任的网络和已验证的源中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--break-system-packages</code></dt><dd><p>允许 uv 修改 <code>EXTERNALLY-MANAGED</code> 的 Python 安装。</p>

<p>警告：<code>--break-system-packages</code> 主要用于持续集成（CI）环境，在将包安装到由外部包管理器（如 <code>apt</code>）管理的 Python 安装中时使用。应谨慎使用，因为这类 Python 安装明确不建议由其他包管理器（如 uv 或 <code>pip</code>）进行修改。</p>

<p>也可以通过 <code>UV_BREAK_SYSTEM_PACKAGES</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或 macOS 和 Linux 上的 <code>$HOME/.cache/uv</code>，以及 Windows 上的 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，可以运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值：auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅当输出到支持颜色的终端或 TTY 时，才启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>要使用的 <code>uv.toml</code> 配置文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许使用。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定目录。</p>

<p>相对路径将根据指定目录解析。</p>

<p>查看 <code>--project</code> 以仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简洁帮助</p>

</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行远程需求文件的身份验证。</p>

<p>目前，仅支持 <code>--keyring-provider subprocess</code>，该配置使 uv 使用 <code>keyring</code> CLI 来处理身份验证。</p>

<p>默认值为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disabled</code>: 不使用 keyring 进行凭证查找</li>

<li><code>subprocess</code>: 使用 <code>keyring</code> 命令进行凭证查找</li>
</ul>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一个可靠的信任根集合，将其包含在 uv 中有助于提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>但是，在某些情况下，您可能希望使用平台的本地证书存储，特别是如果您依赖于系统证书存储中包含的企业信任根（例如，用于强制代理）。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-break-system-packages</code></dt><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录执行操作</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>、<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转指示器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 只会使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--prefix</code> <i>prefix</i></dt><dd><p>从指定的 <code>--prefix</code> 目录卸载包</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（例如相对路径）将相对于当前工作目录进行解析。</p>

<p>请参见 <code>--directory</code> 以完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>指定卸载包的 Python 解释器。</p>

<p>默认情况下，卸载需要虚拟环境。可以提供一个替代的 Python 路径，但仅推荐在持续集成（CI）环境中使用，并应谨慎使用，因为这可能会修改系统的 Python 安装。</p>

<p>有关 Python 发现和支持的请求格式的详细信息，请参阅 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 更倾向于使用它管理的 Python 版本。但如果没有安装 uv 管理的 Python，它会使用系统 Python 安装。此选项允许优先选择或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而非系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而非管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--requirements</code>, <code>-r</code> <i>requirements</i></dt><dd><p>卸载在给定依赖文件中列出的所有包</p>

</dd><dt><code>--system</code></dt><dd><p>使用系统 Python 卸载包。</p>

<p>默认情况下，uv 从当前工作目录或任何父目录中的虚拟环境卸载包。<code>--system</code> 选项指示 uv 改为使用系统 <code>PATH</code> 中找到的第一个 Python。</p>

<p>警告：<code>--system</code> 主要用于持续集成（CI）环境，应谨慎使用，因为它可能会修改系统 Python 安装。</p>

<p>也可以通过 <code>UV_SYSTEM_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--target</code> <i>target</i></dt><dd><p>从指定的 <code>--target</code> 目录卸载包</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置细粒度日志记录。(<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives</a>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

### uv pip freeze

以 requirements 格式列出环境中安装的包

<h3 class="cli-reference">用法</h3>

```
uv pip freeze [OPTIONS]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机建立不安全连接。</p>

<p>可以多次提供此选项。</p>

<p>期望接收主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：此列表中的主机将不会与系统的证书存储进行验证。仅在具有已验证源的安全网络中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会让您暴露在中间人攻击（MITM）中。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或 macOS 和 Linux 上的 <code>$HOME/.cache/uv</code>，Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值：auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅在输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论环境如何，始终启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以给定目录为基准进行解析。</p>

<p>请参阅 <code>--project</code> 以仅更改项目根目录。</p>

</dd><dt><code>--exclude-editable</code></dt><dd><p>从输出中排除任何可编辑的包</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示该命令的简要帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是来自 Mozilla 的可靠信任根集，将它们包含在 uv 中可以提高移植性和性能（尤其是在 macOS 上）。</p>

<p>但是，在某些情况下，您可能希望使用平台的本地证书存储，尤其是在依赖系统证书存储中的公司信任根（例如，强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存中读取或写入数据，而是使用临时目录执行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转指示器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 将仅使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（例如相对路径）将相对于当前工作目录进行解析。</p>

<p>请参见 <code>--directory</code> 以完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>列出包的 Python 解释器。</p>

<p>默认情况下，uv 列出虚拟环境中的包，但如果未找到虚拟环境，则会显示系统 Python 环境中的包。</p>

<p>请参见 <a href="#uv-python">uv python</a> 获取有关 Python 发现和支持的请求格式的详细信息。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。如果未安装 uv 管理的 Python，它将使用系统 Python 安装。此选项允许您优先选择或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--strict</code></dt><dd><p>验证 Python 环境，检测缺少依赖项的包和其他问题</p>

</dd><dt><code>--system</code></dt><dd><p>列出系统 Python 环境中的包。</p>

<p>禁用虚拟环境的发现。</p>

<p>请参见 <a href="#uv-python">uv python</a> 获取有关 Python 发现的详细信息。</p>

<p>也可以通过 <code>UV_SYSTEM_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置精细粒度的日志记录。（&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;）</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

### uv pip list

以表格格式列出环境中安装的包

<h3 class="cli-reference">用法</h3>

```
uv pip list [OPTIONS]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机建立不安全连接。</p>

<p>可以多次提供此选项。</p>

<p>期望接收主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：此列表中的主机将不会与系统的证书存储进行验证。仅在具有已验证源的安全网络中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会让您暴露在中间人攻击（MITM）中。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或 macOS 和 Linux 上的 <code>$HOME/.cache/uv</code>，Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值：auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅在输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论环境如何，始终启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认值：&lt;https://pypi.org/simple&gt;）。</p>

<p>接受符合 PEP 503（简单仓库 API）标准的仓库，或布局相同格式的本地目录。</p>

<p>此标志指定的索引优先级低于通过 <code>--index</code> 标志指定的其他所有索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以给定目录为基准进行解析。</p>

<p>请参见 <code>--project</code> 以仅更改项目根目录。</p>

</dd><dt><code>--editable</code>, <code>-e</code></dt><dd><p>仅包括可编辑项目</p>

</dd><dt><code>--exclude</code> <i>exclude</i></dt><dd><p>从输出中排除指定的包</p>

</dd><dt><code>--exclude-editable</code></dt><dd><p>从输出中排除任何可编辑的包</p>

</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>将候选包限制为在给定日期之前上传的包。</p>

<p>接受 RFC 3339 时间戳（例如，<code>2006-12-02T02:07:43Z</code>）和本地日期（例如，<code>2006-12-02</code>），格式与系统配置的时区一致。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量进行设置。</p>
</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：请改用 <code>--index</code>）除了 <code>--index-url</code> 外，还可以使用额外的包索引 URL。</p>

<p>接受符合 PEP 503（简单仓库 API）标准的仓库，或布局相同格式的本地目录。</p>

<p>通过此标志提供的所有索引优先级高于通过 <code>--index-url</code> 标志指定的索引（默认指向 PyPI）。当提供多个 <code>--extra-index-url</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>除了注册表索引中的包外，还要在这些位置搜索候选分发包。</p>

<p>如果是路径，目标必须是一个包含顶层的 wheel 文件（<code>.whl</code>）或源分发包（例如，<code>.tar.gz</code> 或 <code>.zip</code>）的目录。</p>

<p>如果是 URL，页面必须包含一个包含符合上述格式的包文件链接的平面列表。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量进行设置。</p>
</dd><dt><code>--format</code> <i>format</i></dt><dd><p>选择输出格式：<code>columns</code>（默认）、<code>freeze</code> 或 <code>json</code></p>

<p>[默认值：columns]</p>
<p>可能的值：</p>

<ul>
<li><code>columns</code>: 以人类可读的表格形式显示包列表</li>

<li><code>freeze</code>: 以类似 <code>pip freeze</code> 的格式显示包列表，每行一个包及其版本</li>

<li><code>json</code>: 以机器可读的 JSON 格式显示包列表</li>
</ul>
</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简要帮助</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>解析依赖关系时使用的 URL，除了默认索引。</p>

<p>接受符合 PEP 503（简单仓库 API）标准的仓库，或布局相同格式的本地目录。</p>

<p>通过此标志提供的所有索引优先级高于通过 <code>--default-index</code> 标志指定的索引（默认指向 PyPI）。当提供多个 <code>--index</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 会在找到给定包的第一个索引时停止，并限制解析到该索引中存在的包（<code>first-match</code>）。这可以防止“依赖混淆”攻击，攻击者可能会在其他索引中上传恶意包并使用相同的名称。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>first-index</code>: 仅使用返回匹配给定包名的第一个索引中的结果</li>

<li><code>unsafe-first-match</code>: 在所有索引中搜索每个包名，在切换到下一个索引之前耗尽第一个索引中的版本</li>

<li><code>unsafe-best-match</code>: 在所有索引中搜索每个包名，优先选择找到的“最佳”版本。如果包版本存在于多个索引中，则仅查看第一个索引中的条目</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：请改用 <code>--default-index</code>）Python 包索引的 URL（默认值：&lt;https://pypi.org/simple&gt;）。</p>

<p>接受符合 PEP 503（简单仓库 API）标准的仓库，或布局相同格式的本地目录。</p>

<p>此标志指定的索引优先级低于通过 <code>--extra-index-url</code> 标志指定的其他所有索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行索引 URL 的身份验证。</p>

<p>目前仅支持 <code>--keyring-provider subprocess</code>，该配置使 uv 使用 <code>keyring</code> CLI 来处理身份验证。</p>

<p>默认值为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>disabled</code>: 不使用 keyring 查找凭据</li>

<li><code>subprocess</code>: 使用 <code>keyring</code> 命令查找凭据</li>
</ul>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一组可靠的信任根，将其包含在 uv 中可以提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>但是，在某些情况下，您可能希望使用平台的本地证书存储，尤其是在您依赖企业信任根（例如，强制代理）时，这些根已包含在系统的证书存储中。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录来执行操作</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中自动发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如，PyPI），改为依赖于直接的 URL 依赖项和通过 <code>--find-links</code> 提供的依赖项。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，进度条或旋转指示器。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 将只使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--outdated</code></dt><dd><p>列出过时的包。</p>

<p>每个包的最新版本将与已安装的版本一起显示。最新的包将被从输出中省略。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将在从项目根目录向上遍历目录树时发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（例如相对路径）将相对于当前工作目录解析。</p>

<p>请参阅 <code>--directory</code> 以完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>用于列出包的 Python 解释器。</p>

<p>默认情况下，uv 会列出虚拟环境中的包，但如果未找到虚拟环境，它将显示系统 Python 环境中的包。</p>

<p>有关 Python 发现和支持的请求格式的详细信息，请参见 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 更倾向于使用它管理的 Python 版本。但是，如果未安装 uv 管理的 Python，uv 将使用系统 Python 安装。此选项允许优先选择或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>:  优先使用管理的 Python 安装，而非系统 Python 安装</li>

<li><code>system</code>:  优先使用系统 Python 安装，而非管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--strict</code></dt><dd><p>验证 Python 环境，以检测缺少依赖项和其他问题的包</p>

</dd><dt><code>--system</code></dt><dd><p>列出系统 Python 环境中的包。</p>

<p>禁用虚拟环境的发现。</p>

<p>有关 Python 发现的详细信息，请参见 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_SYSTEM_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置更细粒度的日志记录。(<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">链接</a>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本</p>

</dd></dl>

### uv pip show

显示一个或多个已安装包的信息

<h3 class="cli-reference">用法</h3>

```
uv pip show [OPTIONS] [PACKAGE]...
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>PACKAGE</code></dt><dd><p>要显示的包</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机的非安全连接。</p>

<p>可以多次提供。</p>

<p>期望接收主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：包含在此列表中的主机不会通过系统的证书存储进行验证。仅在安全网络中使用 <code>--allow-insecure-host</code>，并且来源已验证，因为它绕过了 SSL 验证，可能会使您面临中间人攻击。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认为 <code>$XDG_CACHE_HOME/uv</code> 或在 macOS 和 Linux 上的 <code>$HOME/.cache/uv</code>，以及在 Windows 上的 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，可以运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色。</p>

<p>[默认值：auto]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>: 仅当输出发送到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，始终启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许使用。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到给定的目录。</p>

<p>相对路径将以给定目录为基础进行解析。</p>

<p>请参见 <code>--project</code> 以仅更改项目根目录。</p>

</dd><dt><code>--files</code>, <code>-f</code></dt><dd><p>显示每个包的完整安装文件列表。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简洁帮助信息。</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一组可靠的信任根，将其包含在 uv 中提高了可移植性和性能（特别是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，特别是当您依赖于系统证书存储中包含的公司信任根（例如，强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存中读取或写入数据，而是使用临时目录进行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中自动发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转指示器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 将只使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将在从项目根目录向上遍历目录树时发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（例如相对路径）将相对于当前工作目录解析。</p>

<p>请参阅 <code>--directory</code> 以完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>用于查找包的 Python 解释器。</p>

<p>默认情况下，uv 会在虚拟环境中查找包，但如果未找到虚拟环境，则会在系统 Python 环境中查找包。</p>

<p>有关 Python 发现和支持的请求格式的详细信息，请参见 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 更倾向于使用它管理的 Python 版本。然而，如果未安装 uv 管理的 Python，uv 将使用系统 Python 安装。此选项允许您优先选择或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而非系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而非管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--strict</code></dt><dd><p>验证 Python 环境，检测缺少依赖项和其他问题的包</p>

</dd><dt><code>--system</code></dt><dd><p>显示系统 Python 环境中的包。</p>

<p>禁用虚拟环境的发现。</p>

<p>有关 Python 发现的详细信息，请参见 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_SYSTEM_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置细粒度的日志记录。 (&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

### uv pip tree

显示环境的依赖树

<h3 class="cli-reference">用法</h3>

```
uv pip tree [选项]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许不安全连接到主机。</p>

<p>可以多次提供。</p>

<p>期望接收一个主机名（例如 <code>localhost</code>）、一个主机-端口对（例如 <code>localhost:8080</code>），或一个 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：在此列表中的主机将不会通过系统的证书存储进行验证。仅在使用经过验证的安全网络中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会暴露您于中间人攻击。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认为 <code>$XDG_CACHE_HOME/uv</code> 或在 macOS 和 Linux 上的 <code>$HOME/.cache/uv</code>，以及在 Windows 上的 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，可以运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色。</p>

<p>[默认值：auto]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>: 仅当输出发送到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，始终启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许使用。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--depth</code>, <code>-d</code> <i>depth</i></dt><dd><p>依赖树的最大显示深度。</p>

<p>[默认值：255]</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到给定的目录。</p>

<p>相对路径将以给定目录为基础进行解析。</p>

<p>请参见 <code>--project</code> 以仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简洁帮助信息。</p>

</dd><dt><code>--invert</code></dt><dd><p>显示给定包的反向依赖。此标志将反转树，显示依赖于给定包的包。</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一组可靠的信任根，将其包含在 uv 中提高了可移植性和性能（特别是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，特别是当您依赖于系统证书存储中包含的公司信任根（例如，强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存中读取或写入数据，而是使用临时目录进行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中自动发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-dedupe</code></dt><dd><p>不去重重复的依赖项。通常，当包的依赖项已经显示时，后续出现的相同依赖项不会重新显示，并会加上 (*) 来表示它已被显示。此标志将导致这些重复的依赖项被重复显示。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转指示器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>当禁用时，uv 只会使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--outdated</code></dt><dd><p>显示每个包在树中的最新可用版本。</p>

</dd><dt><code>--package</code> <i>package</i></dt><dd><p>仅显示指定的包。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将在从项目根目录向上遍历目录树时被发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>请参见 <code>--directory</code> 以完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--prune</code> <i>prune</i></dt><dd><p>从依赖树的显示中修剪指定的包。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>列出包的 Python 解释器。</p>

<p>默认情况下，uv 列出虚拟环境中的包，但如果未找到虚拟环境，它将显示系统 Python 环境中的包。</p>

<p>请参见 <a href="#uv-python">uv python</a> 以了解有关 Python 发现和支持的请求格式的详细信息。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。然而，如果没有安装 uv 管理的 Python，它将使用系统 Python 安装。此选项允许优先或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--show-version-specifiers</code></dt><dd><p>显示对每个包施加的版本约束。</p>

</dd><dt><code>--strict</code></dt><dd><p>验证 Python 环境，以检测缺少依赖项和其他问题。</p>

</dd><dt><code>--system</code></dt><dd><p>列出系统 Python 环境中的包。</p>

<p>禁用虚拟环境的发现。</p>

<p>有关 Python 发现的详细信息，请参见 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_SYSTEM_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置细粒度的日志记录。 (&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

### uv pip check

验证已安装的包是否具有兼容的依赖项

<h3 class="cli-reference">用法</h3>

```
uv pip check [选项]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许不安全连接到主机。</p>

<p>可以多次提供。</p>

<p>期望接收一个主机名（例如 <code>localhost</code>）、一个主机-端口对（例如 <code>localhost:8080</code>），或一个 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：在此列表中的主机将不会通过系统的证书存储进行验证。仅在使用经过验证的安全网络中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会暴露您于中间人攻击。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认为 <code>$XDG_CACHE_HOME/uv</code> 或在 macOS 和 Linux 上的 <code>$HOME/.cache/uv</code>，以及在 Windows 上的 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，可以运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色。</p>

<p>[默认值：auto]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>: 仅在输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>要用于配置的 <code>uv.toml</code> 文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这么做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到给定的目录。</p>

<p>相对路径将以给定目录作为基准来解析。</p>

<p>请参见 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示该命令的简明帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是来自 Mozilla 的可靠信任根集合，将其包含在 uv 中可以提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，特别是当您依赖于系统证书存储中包含的企业信任根（例如用于强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存读取或写入，而是使用临时目录来进行操作</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>当禁用时，uv 只会使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将在从项目根目录向上遍历目录树时被发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>请参见 <code>--directory</code> 以完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>检查包所使用的 Python 解释器。</p>

<p>默认情况下，uv 会检查虚拟环境中的包，但如果没有找到虚拟环境，它将检查系统 Python 环境中的包。</p>

<p>请参见 <a href="#uv-python">uv python</a> 以了解 Python 发现和支持的请求格式的详细信息。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。然而，如果没有安装 uv 管理的 Python，它将使用系统 Python 安装。此选项允许优先或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--system</code></dt><dd><p>检查系统 Python 环境中的包。</p>

<p>禁用虚拟环境的发现。</p>

<p>有关 Python 发现的详细信息，请参见 <a href="#uv-python">uv python</a>。</p>

<p>也可以通过 <code>UV_SYSTEM_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置细粒度的日志记录。 (&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

## uv venv

创建虚拟环境。

默认情况下，在工作目录中创建一个名为 `.venv` 的虚拟环境。可以提供一个备用路径。

如果在项目中，可以通过 `UV_PROJECT_ENVIRONMENT` 环境变量更改默认的环境名称；此设置仅在从项目根目录运行时有效。

如果目标路径已有虚拟环境，它将被删除，并创建一个新的空虚拟环境。

使用 uv 时，不需要激活虚拟环境。uv 会自动在工作目录或任何父目录中查找名为 `.venv` 的虚拟环境。

<h3 class="cli-reference">用法</h3>

```
uv venv [OPTIONS] [PATH]
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>PATH</code></dt><dd><p>要创建的虚拟环境路径。</p>

<p>默认为工作目录中的 <code>.venv</code>。</p>

<p>相对路径会相对于工作目录进行解析。</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-existing</code></dt><dd><p>保留目标路径中现有的文件或目录。</p>

<p>默认情况下，<code>uv venv</code> 会删除给定路径中现有的虚拟环境，如果该路径非空但不是虚拟环境，则会退出并显示错误。使用 <code>--allow-existing</code> 选项时，会直接写入指定路径，无论该路径的内容如何，且不会先清空。</p>

<p>警告：如果现有的虚拟环境与新创建的虚拟环境链接到不同的 Python 解释器，可能会导致意外行为。</p>

</dd><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许连接到不安全的主机。</p>

<p>可以多次提供此选项。</p>

<p>接受主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：此列表中的主机将不会与系统的证书存储进行验证。仅在安全网络和验证过的来源中使用 <code>--allow-insecure-host</code>，因为它会绕过 SSL 验证，可能暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认为 <code>$XDG_CACHE_HOME/uv</code> 或在 macOS 和 Linux 上为 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值：auto]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>: 仅当输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这么做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认值：<code>https://pypi.org/simple</code>）。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或以相同格式排列的本地目录。</p>

<p>此标志指定的索引优先级低于所有通过 <code>--index</code> 标志指定的索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到给定的目录。</p>

<p>相对路径将以给定目录作为基准进行解析。</p>

<p>请参见 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>将候选包限制为在给定日期之前上传的包。</p>

<p>接受 RFC 3339 时间戳（例如，<code>2006-12-02T02:07:43Z</code>）或本地日期（例如，<code>2006-12-02</code>），并使用系统配置的时区。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量进行设置。</p>
</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：请使用 <code>--index</code>）额外的包索引 URL，用于与 <code>--index-url</code> 一起使用。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或以相同格式排列的本地目录。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--index-url</code>（默认是 PyPI）指定的索引。当提供多个 <code>--extra-index-url</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>除了在注册表索引中找到的候选分发之外，还可以搜索的位置。</p>

<p>如果是路径，则目标必须是一个包含 wheel 文件（<code>.whl</code>）或源分发（例如，<code>.tar.gz</code> 或 <code>.zip</code>）的目录，这些文件位于顶层。</p>

<p>如果是 URL，则页面必须包含符合上述格式的包文件链接的平面列表。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量进行设置。</p>
</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示该命令的简要帮助。</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>解析依赖项时使用的 URL，除了默认索引之外。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或以相同格式排列的本地目录。</p>

<p>通过此标志提供的所有索引优先于通过 <code>--default-index</code>（默认是 PyPI）指定的索引。当提供多个 <code>--index</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>在解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 会在第一个找到的索引中停止解析给定的包，并将解析限制在该第一个索引上（<code>first-match</code>）。这可以防止“依赖混淆”攻击，其中攻击者可能会将一个恶意包以相同的名称上传到另一个索引。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>first-index</code>: 仅使用第一个返回匹配的索引中的结果。</li>

<li><code>unsafe-first-match</code>: 在所有索引中搜索每个包名，在转到下一个索引之前先耗尽第一个索引中的版本。</li>

<li><code>unsafe-best-match</code>: 在所有索引中搜索每个包名，优先使用找到的“最佳”版本。如果一个包版本出现在多个索引中，仅查看第一个索引中的条目。</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：请改用 <code>--default-index</code>）用于解析依赖项的 Python 包索引 URL（默认值：<code>https://pypi.org/simple</code>）。</p>

<p>接受符合 PEP 503（简单仓库 API）规范的仓库，或以相同格式排列的本地目录。</p>

<p>此标志指定的索引优先级低于所有通过 <code>--extra-index-url</code> 标志指定的索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行索引 URL 的身份验证。</p>

<p>目前，只有 <code>--keyring-provider subprocess</code> 被支持，它配置 uv 使用 <code>keyring</code> CLI 来处理身份验证。</p>

<p>默认值为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>disabled</code>: 不使用 keyring 进行凭证查找</li>

<li><code>subprocess</code>: 使用 <code>keyring</code> 命令进行凭证查找</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>安装包时使用的方法，来自全局缓存。</p>

<p>此选项仅用于安装种子包。</p>

<p>默认情况下，macOS 上为 <code>clone</code>（也称为写时复制），Linux 和 Windows 上为 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>clone</code>: 从 wheel 克隆（即写时复制）包到 <code>site-packages</code> 目录</li>

<li><code>copy</code>: 从 wheel 复制包到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>: 从 wheel 硬链接包到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>: 从 wheel 符号链接包到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储中加载 TLS 证书。</p>

<p>默认情况下，uv 会从捆绑的 <code>webpki-roots</code> crate 中加载证书。<code>webpki-roots</code> 是 Mozilla 提供的可靠信任根集，包含它们可以提高 uv 的可移植性和性能（特别是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，特别是如果您依赖系统证书存储中包含的企业信任根（例如，强制代理）。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存中读取或写入，而是使用临时目录执行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如，PyPI），改为依赖于直接的 URL 依赖项和通过 <code>--find-links</code> 提供的依赖项。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转指示器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-project</code></dt><dd><p>避免发现项目或工作区。</p>

<p>默认情况下，uv 会在当前目录或任何父目录中搜索项目，以确定虚拟环境的默认路径并检查是否有 Python 版本约束。</p>

</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 只会使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录内运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件都将通过从项目根目录开始向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（例如，相对路径）将相对于当前工作目录解析。</p>

<p>有关更改工作目录的信息，请参见 <code>--directory</code>。</p>

<p>在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--prompt</code> <i>prompt</i></dt><dd><p>为虚拟环境提供替代的提示前缀。</p>

<p>默认情况下，提示符依赖于是否为 <code>uv venv</code> 提供了路径。如果提供了路径（例如，<code>uv venv project</code>），提示符将设置为目录名。如果未提供路径（<code>uv venv</code>），则提示符将设置为当前目录的名称。</p>

<p>如果提供了“.”，则无论是否提供了路径，都会使用当前目录的名称。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>用于虚拟环境的 Python 解释器。</p>

<p>在虚拟环境创建过程中，uv 不会在虚拟环境中查找 Python 解释器。</p>

<p>有关 Python 发现和支持的请求格式的详细信息，请参见 <code>uv python help</code>。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统的 Python 安装。</p>

<p>默认情况下，uv 更倾向于使用它管理的 Python 版本。然而，如果没有安装 uv 管理的 Python，uv 会使用系统的 Python 安装。此选项允许优先使用或忽略系统的 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；从不使用系统 Python 安装。</li>

<li><code>managed</code>: 优先使用管理的 Python 安装。</li>

<li><code>system</code>: 优先使用系统 Python 安装。</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；从不使用管理的 Python 安装。</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出。</p>

</dd><dt><code>--relocatable</code></dt><dd><p>使虚拟环境可重定位。</p>

<p>可重定位的虚拟环境可以在不同位置移动和重新分发，而不会使其相关的入口点和激活脚本失效。</p>

<p>请注意，这只能保证对于标准的 <code>console_scripts</code> 和 <code>gui_scripts</code>。如果其他脚本使用通用的 <code>#!python[w]</code> shebang，并且二进制文件保持不变，则可能会进行调整。</p>

<p>由于通过写入相对路径而非绝对路径使环境可重定位，入口点和脚本本身将 <em>不可重定位</em>。换句话说，复制这些入口点和脚本到环境之外的位置将不起作用，因为它们引用的是相对于环境本身的路径。</p>

</dd><dt><code>--seed</code></dt><dd><p>将种子包（包括 <code>pip</code>、<code>setuptools</code> 和 <code>wheel</code>）安装到虚拟环境中。</p>

<p>请注意，<code>setuptools</code> 和 <code>wheel</code> 不包含在 Python 3.12+ 环境中。</p>

</dd><dt><code>--system-site-packages</code></dt><dd><p>允许虚拟环境访问系统的 site-packages 目录。</p>

<p>与 <code>pip</code> 不同，当使用 <code>--system-site-packages</code> 创建虚拟环境时，uv 在运行 <code>uv pip list</code> 或 <code>uv pip install</code> 等命令时将 <em>不</em> 考虑系统的 site-packages。<code>--system-site-packages</code> 标志将在运行时为虚拟环境提供访问系统 site-packages 目录的权限，但不会影响 uv 命令的行为。</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置更精细的日志记录。(&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本。</p>

</dd></dl>

## uv build

将 Python 包构建为源代码分发包和 Wheel 格式。

`uv build` 接受一个目录或源代码分发包的路径，默认路径为当前工作目录。

默认情况下，如果传入一个目录，`uv build` 将从源代码目录构建一个源代码分发包（"sdist"），并从源代码分发包构建一个二进制分发包（"wheel"）。

可以使用 `uv build --sdist` 只构建源代码分发包，使用 `uv build --wheel` 只构建二进制分发包，使用 `uv build --sdist --wheel` 从源代码构建两个分发包。

如果传入的是源代码分发包，`uv build --wheel` 将从该源代码分发包构建一个 Wheel 文件。

<h3 class="cli-reference">用法</h3>

```
uv build [OPTIONS] [SRC]
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>SRC</code></dt><dd><p>要构建分发包的目录，或要构建为 Wheel 的源代码分发包档案。</p>

<p>默认为当前工作目录。</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--all-packages</code></dt><dd><p>构建工作区中的所有包。</p>

<p>工作区将从提供的源代码目录中发现，如果未提供源代码目录，则从当前目录中发现。</p>

<p>如果工作区成员不存在，uv 将退出并显示错误。</p>

</dd><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机进行不安全的连接。</p>

<p>可以多次提供此选项。</p>

<p>期望接收主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：此列表中的主机将不会与系统的证书存储进行验证。仅在安全网络且源已验证的情况下使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能使您暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--build-constraints</code>, <code>-b</code> <i>build-constraints</i></dt><dd><p>使用给定的需求文件约束构建依赖项，在构建分发包时使用。</p>

<p>约束文件类似于 <code>requirements.txt</code> 文件，它们只控制已安装构建依赖项的 <em>版本</em>。但是，将一个包包含在约束文件中并不会触发该包的自动包含。</p>

<p>也可以通过 <code>UV_BUILD_CONSTRAINT</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>（在 macOS 和 Linux 上），以及 <code>%LOCALAPPDATA%\uv\cache</code>（在 Windows 上）。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色。</p>

<p>[默认值：auto]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>: 仅当输出到支持颜色的终端或 TTY 时启用彩色输出。</li>

<li><code>always</code>: 无论环境如何，始终启用彩色输出。</li>

<li><code>never</code>: 禁用彩色输出。</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>要使用的 <code>uv.toml</code> 配置文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--config-setting</code>, <code>-C</code> <i>config-setting</i></dt><dd><p>将设置传递给 PEP 517 构建后端，指定为 <code>KEY=VALUE</code> 键值对。</p>

</dd><dt><code>--default-index</code> <i>default-index</i></dt><dd><p>默认包索引的 URL（默认为：<code>https://pypi.org/simple</code>）。</p>

<p>接受符合 PEP 503 的存储库（简单存储库 API）或本地目录，其格式与之相同。</p>

<p>此标志指定的索引优先级低于通过 <code>--index</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_DEFAULT_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定目录。</p>

<p>相对路径将以给定目录作为基础进行解析。</p>

<p>有关仅更改项目根目录的信息，请参见 <code>--project</code>。</p>

</dd><dt><code>--exclude-newer</code> <i>exclude-newer</i></dt><dd><p>限制候选包仅限于上传日期早于指定日期的包。</p>

<p>接受 RFC 3339 时间戳（例如，<code>2006-12-02T02:07:43Z</code>）和本地日期（例如，<code>2006-12-02</code>）格式，且时间格式与您系统的时区设置一致。</p>

<p>也可以通过 <code>UV_EXCLUDE_NEWER</code> 环境变量进行设置。</p>
</dd><dt><code>--extra-index-url</code> <i>extra-index-url</i></dt><dd><p>（已弃用：请改用 <code>--index</code>）额外的包索引 URL，将与 <code>--index-url</code> 一起使用。</p>

<p>接受符合 PEP 503（简单存储库 API）规范的存储库，或按相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先级高于由 <code>--index-url</code> 指定的索引（默认是 PyPI）。当提供多个 <code>--extra-index-url</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_EXTRA_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--find-links</code>, <code>-f</code> <i>find-links</i></dt><dd><p>除了注册表索引中找到的包之外，还可以在这些位置搜索候选分发包。</p>

<p>如果是路径，目标必须是一个目录，该目录的顶层包含以 Wheel 格式（<code>.whl</code>）或源代码分发包格式（例如，<code>.tar.gz</code> 或 <code>.zip</code>）存在的包。</p>

<p>如果是 URL，页面必须包含指向符合上述格式的包文件的平面链接列表。</p>

<p>也可以通过 <code>UV_FIND_LINKS</code> 环境变量进行设置。</p>
</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简洁帮助。</p>

</dd><dt><code>--index</code> <i>index</i></dt><dd><p>解析依赖项时使用的 URL，除了默认索引外。</p>

<p>接受符合 PEP 503（简单存储库 API）规范的存储库，或按相同格式布局的本地目录。</p>

<p>通过此标志提供的所有索引优先级高于由 <code>--default-index</code> 指定的索引（默认是 PyPI）。当提供多个 <code>--index</code> 标志时，较早的值优先。</p>

<p>也可以通过 <code>UV_INDEX</code> 环境变量进行设置。</p>
</dd><dt><code>--index-strategy</code> <i>index-strategy</i></dt><dd><p>在解析多个索引 URL 时使用的策略。</p>

<p>默认情况下，uv 会在找到给定包的第一个索引时停止，并将解析限制为该第一个索引中的包（<code>first-match</code>）。这样可以防止“依赖混淆”攻击，攻击者可能会将恶意包上传到另一个索引并使用相同的包名。</p>

<p>也可以通过 <code>UV_INDEX_STRATEGY</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>first-index</code>: 仅使用返回匹配的第一个索引中的结果</li>

<li><code>unsafe-first-match</code>: 在所有索引中搜索每个包名，先用完第一个索引中的版本，再去下一个索引</li>

<li><code>unsafe-best-match</code>: 在所有索引中搜索每个包名，优先选择找到的“最佳”版本。如果一个包的版本出现在多个索引中，只查看第一个索引中的条目</li>
</ul>
</dd><dt><code>--index-url</code>, <code>-i</code> <i>index-url</i></dt><dd><p>（已弃用：请改用 <code>--default-index</code>）Python 包索引的 URL（默认为：<code>https://pypi.org/simple</code>）。</p>

<p>接受符合 PEP 503（简单存储库 API）规范的存储库，或按相同格式布局的本地目录。</p>

<p>此标志指定的索引优先级低于通过 <code>--extra-index-url</code> 标志指定的所有其他索引。</p>

<p>也可以通过 <code>UV_INDEX_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行身份验证，用于索引 URL。</p>

<p>目前，仅支持 <code>--keyring-provider subprocess</code>，它将配置 uv 使用 <code>keyring</code> 命令行工具处理身份验证。</p>

<p>默认为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>disabled</code>: 不使用 keyring 查找凭证</li>

<li><code>subprocess</code>: 使用 <code>keyring</code> 命令查找凭证</li>
</ul>
</dd><dt><code>--link-mode</code> <i>link-mode</i></dt><dd><p>从全局缓存安装包时使用的方法。</p>

<p>此选项仅在构建源代码分发包时使用。</p>

<p>默认值为 macOS 上的 <code>clone</code>（即写时复制），以及 Linux 和 Windows 上的 <code>hardlink</code>。</p>

<p>也可以通过 <code>UV_LINK_MODE</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>clone</code>: 从 Wheel 包克隆（即写时复制）包到 <code>site-packages</code> 目录</li>

<li><code>copy</code>: 将包从 Wheel 复制到 <code>site-packages</code> 目录</li>

<li><code>hardlink</code>: 从 Wheel 使用硬链接包到 <code>site-packages</code> 目录</li>

<li><code>symlink</code>: 从 Wheel 使用符号链接包到 <code>site-packages</code> 目录</li>
</ul>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 会从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的可靠信任根集，将其包含在 uv 中有助于提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，特别是如果您依赖于系统证书存储中包含的公司信任根（例如，强制代理所需的信任根）。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-binary</code></dt><dd><p>不安装预构建的 wheel 文件。</p>

<p>给定的包将从源代码构建并安装。解析器仍会使用预构建的 wheel 文件来提取包的元数据（如果可用）。</p>

</dd><dt><code>--no-binary-package</code> <i>no-binary-package</i></dt><dd><p>不为特定包安装预构建的 wheel 文件。</p>

</dd><dt><code>--no-build</code></dt><dd><p>不构建源代码分发包。</p>

<p>启用时，解析不会运行任意的 Python 代码。已构建源代码分发包的缓存 wheel 文件将被重用，但需要构建分发包的操作将会出现错误。</p>

</dd><dt><code>--no-build-isolation</code></dt><dd><p>在构建源代码分发包时禁用隔离。</p>

<p>假定 PEP 518 中指定的构建依赖项已经安装。</p>

<p>也可以通过 <code>UV_NO_BUILD_ISOLATION</code> 环境变量进行设置。</p>
</dd><dt><code>--no-build-isolation-package</code> <i>no-build-isolation-package</i></dt><dd><p>为特定包构建源代码分发包时禁用隔离。</p>

<p>假定该包的构建依赖项已根据 PEP 518 安装。</p>

</dd><dt><code>--no-build-package</code> <i>no-build-package</i></dt><dd><p>不为特定包构建源代码分发包。</p>

</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存读取或写入数据，而是使用临时目录执行操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>, <code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中被发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-index</code></dt><dd><p>忽略注册表索引（例如 PyPI），改为依赖于直接 URL 依赖项和通过 <code>--find-links</code> 提供的依赖项。</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，进度条或加载动画。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--no-sources</code></dt><dd><p>在解析依赖项时忽略 <code>tool.uv.sources</code> 表。用于锁定符合标准的、可发布的包元数据，而不是使用任何本地或 Git 源。</p>

</dd><dt><code>--no-verify-hashes</code></dt><dd><p>禁用验证需求文件中的哈希值。</p>

<p>默认情况下，uv 会验证需求文件中任何可用的哈希值，但不会要求所有需求都必须有相关联的哈希值。要强制哈希验证，请使用 <code>--require-hashes</code>。</p>

<p>也可以通过 <code>UV_NO_VERIFY_HASHES</code> 环境变量进行设置。</p>
</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 将仅使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--out-dir</code>, <code>-o</code> <i>out-dir</i></dt><dd><p>将分发包写入的输出目录。</p>

<p>默认为源目录中的 <code>dist</code> 子目录，或者包含源代码分发包归档的目录。</p>

</dd><dt><code>--package</code> <i>package</i></dt><dd><p>在工作区中构建特定包。</p>

<p>工作区将从提供的源目录中发现，若未提供源目录，则从当前目录中发现。</p>

<p>如果工作区成员不存在，uv 将返回错误并退出。</p>

</dd><dt><code>--prerelease</code> <i>prerelease</i></dt><dd><p>考虑预发行版本时使用的策略。</p>

<p>默认情况下，uv 会接受仅发布预发行版本的包的预发行版本，以及包含明确预发行标记的第一方需求（<code>if-necessary-or-explicit</code>）。</p>

<p>也可以通过 <code>UV_PRERELEASE</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>disallow</code>: 不允许任何预发行版本</li>

<li><code>allow</code>: 允许所有预发行版本</li>

<li><code>if-necessary</code>: 如果一个包的所有版本都是预发行版本，则允许预发行版本</li>

<li><code>explicit</code>: 仅允许具有明确预发行标记的第一方包的预发行版本</li>

<li><code>if-necessary-or-explicit</code>: 如果一个包的所有版本都是预发行版本，或者包的版本要求中有明确的预发行标记，则允许预发行版本</li>
</ul>
</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有的 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树来发现，同时也会发现项目的虚拟环境（<code>.venv</code>）。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录解析。</p>

<p>参见 <code>--directory</code>，以完全更改工作目录。</p>

<p>在 <code>uv pip</code> 接口中使用此设置无效。</p>

</dd><dt><code>--python</code>, <code>-p</code> <i>python</i></dt><dd><p>构建环境中使用的 Python 解释器。</p>

<p>默认情况下，构建将在隔离的虚拟环境中执行。将使用发现的解释器创建这些环境，并根据平台进行符号链接或复制。</p>

<p>参见 <a href="#uv-python">uv python</a> 以查看支持的请求格式。</p>

<p>也可以通过 <code>UV_PYTHON</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装，还是系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它所管理的 Python 版本。然而，如果没有安装 uv 管理的 Python，系统 Python 将被使用。此选项允许优先使用或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>only-managed</code>: 仅使用 uv 管理的 Python 安装；永不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用 uv 管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而不是 uv 管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永不使用 uv 管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出。</p>

</dd><dt><code>--refresh</code></dt><dd><p>刷新所有缓存的数据。</p>

</dd><dt><code>--refresh-package</code> <i>refresh-package</i></dt><dd><p>刷新特定包的缓存数据。</p>

</dd><dt><code>--require-hashes</code></dt><dd><p>要求每个需求必须匹配哈希值。</p>

<p>默认情况下，uv 会验证需求文件中任何可用的哈希值，但不会要求所有需求都必须有相关联的哈希值。</p>

<p>启用 <code>--require-hashes</code> 后，<em>所有</em>需求必须包括一个哈希或一组哈希值，并且 <em>所有</em>需求必须固定为确切版本（例如，<code>==1.0.0</code>），或者通过直接 URL 指定。</p>

<p>哈希检查模式引入了以下附加约束：</p>

<ul>
<li>不支持 Git 依赖项。</li>
<li>不支持可编辑安装。</li>
<li>不支持本地依赖项，除非它们指向特定的 wheel 文件（<code>.whl</code>）或源代码归档（<code>.zip</code>, <code>.tar.gz</code>），而不是目录。</li>
</ul>

<p>也可以通过 <code>UV_REQUIRE_HASHES</code> 环境变量进行设置。</p>
</dd><dt><code>--resolution</code> <i>resolution</i></dt><dd><p>在选择给定包需求的不同兼容版本时使用的策略。</p>

<p>默认情况下，uv 会使用每个包的最新兼容版本（<code>highest</code>）。</p>

<p>也可以通过 <code>UV_RESOLUTION</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>highest</code>: 解析每个包的最高兼容版本</li>

<li><code>lowest</code>: 解析每个包的最低兼容版本</li>

<li><code>lowest-direct</code>: 解析所有直接依赖项的最低兼容版本，并解析所有传递依赖项的最高兼容版本</li>
</ul>
</dd><dt><code>--sdist</code></dt><dd><p>从给定目录构建源代码分发包（“sdist”）。</p>

</dd><dt><code>--upgrade</code>, <code>-U</code></dt><dd><p>允许包升级，忽略任何现有输出文件中固定的版本。隐式启用 <code>--refresh</code>。</p>

</dd><dt><code>--upgrade-package</code>, <code>-P</code> <i>upgrade-package</i></dt><dd><p>允许特定包的升级，忽略任何现有输出文件中固定的版本。隐式启用 <code>--refresh-package</code>。</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>可以使用 <code>RUST_LOG</code> 环境变量配置更细粒度的日志记录。（<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives</a>）</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本。</p>

</dd><dt><code>--wheel</code></dt><dd><p>从给定目录构建二进制分发包（“wheel”）。</p>

</dd></dl>

## uv publish

上传分发包到索引

<h3 class="cli-reference">用法</h3>

```
uv publish [OPTIONS] [FILES]...
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>FILES</code></dt><dd><p>要上传的文件路径。接受 glob 表达式。</p>

<p>默认为 <code>dist</code> 目录。仅选择 wheel 和源代码分发包，忽略其他文件。</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许连接到不安全的主机。</p>

<p>可以多次提供。</p>

<p>期望接收一个主机名（例如，<code>localhost</code>），主机-端口对（例如，<code>localhost:8080</code>），或一个 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：列入此列表的主机将不会通过系统的证书存储进行验证。仅在安全网络中并且源可信时使用 <code>--allow-insecure-host</code>，因为它会绕过 SSL 验证，可能会暴露于中间人攻击。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认为 <code>$XDG_CACHE_HOME/uv</code> 或 macOS 和 Linux 上的 <code>$HOME/.cache/uv</code>，以及 Windows 上的 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--check-url</code> <i>check-url</i></dt><dd><p>检查索引 URL 以跳过重复上传的现有文件。</p>

<p>此选项允许在上传失败后重新尝试发布，仅上传了部分文件，而不是全部文件，并处理因并行上传同一文件而导致的错误。</p>

<p>上传之前会检查索引。如果相同的文件已经存在于索引中，则该文件不会被上传。如果上传过程中发生错误，会重新检查索引，以处理并行上传相同文件的情况。</p>

<p>具体行为会根据索引不同而有所不同。上传到 PyPI 时，即使没有 <code>--check-url</code>，上传相同的文件也会成功，而大多数其他索引会报错。</p>

<p>该索引必须提供支持的哈希之一（SHA-256、SHA-384 或 SHA-512）。</p>

<p>也可以通过 <code>UV_PUBLISH_CHECK_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值：auto]</p>
<p>可选值：</p>

<ul>
<li><code>auto</code>: 仅当输出目标是支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论环境如何，始终启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>要使用的 <code>uv.toml</code> 配置文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这样做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到给定目录。</p>

<p>相对路径将以给定目录为基准进行解析。</p>

<p>查看 <code>--project</code> 以仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示该命令的简明帮助</p>

</dd><dt><code>--keyring-provider</code> <i>keyring-provider</i></dt><dd><p>尝试使用 <code>keyring</code> 进行远程需求文件的身份验证。</p>

<p>目前仅支持 <code>--keyring-provider subprocess</code>，它配置 uv 使用 <code>keyring</code> CLI 来处理身份验证。</p>

<p>默认值为 <code>disabled</code>。</p>

<p>也可以通过 <code>UV_KEYRING_PROVIDER</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>disabled</code>: 不使用 keyring 查找凭据</li>

<li><code>subprocess</code>: 使用 <code>keyring</code> 命令查找凭据</li>
</ul>
</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是来自 Mozilla 的可靠信任根集合，将其包含在 uv 中可以提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的本地证书存储，特别是当您依赖于系统证书存储中包含的企业信任根（例如，用于强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存中读取或写入，而是使用临时目录来完成操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>, <code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中进行查找。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转指示器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用后，uv 只会使用本地缓存数据和本地可用的文件。</p>

</dd><dt><code>--password</code>, <code>-p</code> <i>password</i></dt><dd><p>上传所需的密码</p>

<p>也可以通过 <code>UV_PUBLISH_PASSWORD</code> 环境变量进行设置。</p>
</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树进行发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（例如相对路径）将相对于当前工作目录进行解析。</p>

<p>查看 <code>--directory</code> 以完全更改工作目录。</p>

<p>此设置在 <code>uv pip</code> 接口中使用时没有任何效果。</p>

</dd><dt><code>--publish-url</code> <i>publish-url</i></dt><dd><p>上传端点的 URL（不是索引 URL）。</p>

<p>请注意，通常索引访问（例如 <code>https://.../simple</code>）和索引上传的 URL 是不同的。</p>

<p>默认值为 PyPI 的发布 URL（<https://upload.pypi.org/legacy/>）。</p>

<p>也可以通过 <code>UV_PUBLISH_URL</code> 环境变量进行设置。</p>
</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装，还是使用系统的 Python 安装。</p>

<p>默认情况下，uv 偏好使用它管理的 Python 版本。然而，如果未安装 uv 管理的 Python，uv 会使用系统的 Python 安装。此选项允许优先使用或忽略系统的 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>: 偏好使用管理的 Python 安装，而不是系统的 Python 安装</li>

<li><code>system</code>: 偏好使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--token</code>, <code>-t</code> <i>token</i></dt><dd><p>上传所需的令牌。</p>

<p>使用令牌相当于将 <code>__token__</code> 作为 <code>--username</code>，并将令牌作为 <code>--password</code>。</p>

<p>也可以通过 <code>UV_PUBLISH_TOKEN</code> 环境变量进行设置。</p>
</dd><dt><code>--trusted-publishing</code> <i>trusted-publishing</i></dt><dd><p>配置使用 GitHub Actions 进行可信发布。</p>

<p>默认情况下，uv 在 GitHub Actions 中运行时会检查是否进行可信发布，但如果未配置或工作流没有足够权限（例如，来自分叉的拉取请求），则会忽略此检查。</p>

<p>可选值：</p>

<ul>
<li><code>automatic</code>: 在已在 GitHub Actions 中时尝试进行可信发布，如果失败则继续</li>

<li><code>always</code>: 始终尝试进行可信发布</li>

<li><code>never</code>: 永不进行可信发布</li>
</ul>
</dd><dt><code>--username</code>, <code>-u</code> <i>username</i></dt><dd><p>上传所需的用户名</p>

<p>也可以通过 <code>UV_PUBLISH_USERNAME</code> 环境变量进行设置。</p>
</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>可以使用 <code>RUST_LOG</code> 环境变量配置精细化的日志记录。 (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

## uv cache

管理 uv 的缓存

<h3 class="cli-reference">使用方法</h3>

```
uv cache [OPTIONS] <COMMAND>
```

<h3 class="cli-reference">命令</h3>

<dl class="cli-reference"><dt><a href="#uv-cache-clean"><code>uv cache clean</code></a></dt><dd><p>清除缓存，删除所有条目或与特定包相关联的条目</p>
</dd>
<dt><a href="#uv-cache-prune"><code>uv cache prune</code></a></dt><dd><p>修剪缓存中所有无法访问的对象</p>
</dd>
<dt><a href="#uv-cache-dir"><code>uv cache dir</code></a></dt><dd><p>显示缓存目录</p>
</dd>
</dl>

### uv cache clean

清除缓存，删除所有条目或与特定包相关联的条目

<h3 class="cli-reference">使用方法</h3>

```
uv cache clean [OPTIONS] [PACKAGE]...
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>PACKAGE</code></dt><dd><p>要从缓存中删除的包</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机的非安全连接。</p>

<p>可以多次提供。</p>

<p>预期接收主机名（例如 <code>localhost</code>）、主机-端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：此列表中包含的主机将不会与系统的证书存储进行验证。仅在安全的网络中与经过验证的源使用 <code>--allow-insecure-host</code>，因为它会绕过 SSL 验证，可能会暴露于中间人攻击（MITM）风险。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或在 macOS 和 Linux 上为 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值: auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅在输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这样做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定目录。</p>

<p>相对路径会以给定目录为基准进行解析。</p>

<p>查看 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简洁帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的原生证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一组可靠的信任根，将它们包含在 uv 中可提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>但是，在某些情况下，您可能希望使用平台的原生证书存储，特别是如果您依赖于系统证书存储中包含的公司信任根（例如，强制代理的证书根）。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录来完成操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中进行发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，隐藏旋转指示器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 只会使用本地缓存数据和本地可用文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件都将通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>查看 <code>--directory</code> 以完全更改工作目录。</p>

<p>当在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装。</p>

<p>默认情况下，uv 偏好使用它管理的 Python 版本。如果未安装 uv 管理的 Python，则会使用系统的 Python 安装。此选项允许优先使用或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可选值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>: 偏好使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>: 偏好使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置精细的日志记录。 (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

### uv cache prune

修剪缓存中所有不可达的对象

<h3 class="cli-reference">用法</h3>

```
uv cache prune [OPTIONS]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机建立不安全的连接。</p>

<p>可以多次提供该选项。</p>

<p>期望接收主机名（例如 <code>localhost</code>）、主机端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：此列表中包含的主机将不会与系统的证书存储进行验证。仅在具有已验证来源的安全网络中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会让您暴露于中间人攻击（MITM）风险中。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或在 macOS 和 Linux 上为 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--ci</code></dt><dd><p>优化缓存以便在持续集成环境（如 GitHub Actions）中持久化。</p>

<p>默认情况下，uv 会缓存它从源代码构建的 wheels 和直接下载的预构建 wheels，以启用高性能的包安装。然而，在某些情况下，持久化预构建的 wheels 可能并不理想。例如，在 GitHub Actions 中，省略预构建的 wheels 并在每次运行时重新下载它们通常更快。不过，缓存从源代码构建的 wheels 通常是更快的，因为构建 wheel 的过程可能非常耗时，尤其是对于扩展模块。</p>

<p>在 <code>--ci</code> 模式下，uv 将从缓存中修剪掉任何预构建的 wheels，但会保留从源代码构建的 wheels。</p>

</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值: auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅在输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这样做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定目录。</p>

<p>相对路径会以给定目录为基准进行解析。</p>

<p>查看 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简洁帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的原生证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一组可靠的信任根，将它们包含在 uv 中可提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的原生证书存储，特别是如果您依赖于系统证书存储中包含的公司信任根（例如，强制代理的证书根）。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录来完成操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中进行发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，隐藏旋转指示器或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 只会使用本地缓存数据和本地可用文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在给定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件都将通过从项目根目录向上遍历目录树来发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>查看 <code>--directory</code> 以完全更改工作目录。</p>

<p>当在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。如果没有安装 uv 管理的 Python 版本，它将使用系统的 Python 安装。此选项允许优先使用或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；从不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而非系统 Python 安装</li>

<li><code>system</code>: 优先使用系统的 Python 安装，而非管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；从不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以通过 <code>RUST_LOG</code> 环境变量配置精细的日志记录。(&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本</p>

</dd></dl>

### uv cache dir

显示缓存目录。

默认情况下，缓存存储在 `$XDG_CACHE_HOME/uv` 或在 Unix 上为 `$HOME/.cache/uv`，在 Windows 上为 `%LOCALAPPDATA%\uv\cache`。

当使用 `--no-cache` 时，缓存存储在临时目录中，并在进程退出时丢弃。

可以通过 `cache-dir` 设置、`--cache-dir` 选项或 `$UV_CACHE_DIR` 环境变量指定替代的缓存目录。

请注意，为了提高性能，缓存目录应该位于 uv 所操作的 Python 环境所在的同一文件系统中。

<h3 class="cli-reference">用法</h3>

```
uv cache dir [OPTIONS]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机建立不安全的连接。</p>

<p>可以多次提供该选项。</p>

<p>期望接收主机名（例如 <code>localhost</code>）、主机端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：此列表中包含的主机将不会与系统的证书存储进行验证。仅在具有已验证来源的安全网络中使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会让您暴露于中间人攻击（MITM）风险中。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认值为 <code>$XDG_CACHE_HOME/uv</code> 或在 macOS 和 Linux 上为 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值: auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅在输出到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论检测到的环境如何，都启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这样做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定目录。</p>

<p>相对路径会以给定目录为基准进行解析。</p>

<p>查看 <code>--project</code> 仅更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简洁帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的原生证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是 Mozilla 提供的一组可靠的信任根，将它们包含在 uv 中可提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的原生证书存储，特别是如果您依赖于系统证书存储中包含的公司信任根（例如，强制代理的证书根）。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免读取或写入缓存，而是使用临时目录来完成操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中进行发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转图标或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 将仅使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在指定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树的方式被发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（例如相对路径）将相对于当前工作目录进行解析。</p>

<p>请参见 <code>--directory</code>，它可以完全更改工作目录。</p>

<p>当在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。如果没有安装 uv 管理的 Python，uv 将使用系统的 Python 安装。此选项允许优先使用或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而非系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而非管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以通过 <code>RUST_LOG</code> 环境变量配置精细的日志记录。(&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本</p>

</dd></dl>

## uv self

管理 uv 可执行文件

<h3 class="cli-reference">用法</h3>

```
uv self [OPTIONS] <COMMAND>
```

<h3 class="cli-reference">命令</h3>

<dl class="cli-reference"><dt><a href="#uv-self-update"><code>uv self update</code></a></dt><dd><p>更新 uv</p>
</dd>
</dl>

### uv self update

更新 uv

<h3 class="cli-reference">用法</h3>

```
uv self update [OPTIONS] [TARGET_VERSION]
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>TARGET_VERSION</code></dt><dd><p>更新到指定的版本。如果未提供，则 uv 将更新到最新版本</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机建立不安全的连接。</p>

<p>可以多次提供该选项。</p>

<p>期望接收主机名（例如 <code>localhost</code>）、主机端口对（例如 <code>localhost:8080</code>）或 URL（例如 <code>https://localhost</code>）。</p>

<p>警告：列表中的主机将不会与系统的证书存储进行验证。仅在安全网络中与经过验证的来源一起使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会使您暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认情况下，在 macOS 和 Linux 上为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认: auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅当输出发送到支持颜色的终端或 TTY 时，才启用颜色输出</li>

<li><code>always</code>: 无论检测到的环境如何，始终启用颜色输出</li>

<li><code>never</code>: 禁用颜色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这样做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以给定目录为基准进行解析。</p>

<p>请参见 <code>--project</code>，它只会更改项目的根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简洁帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的原生证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是来自 Mozilla 的一组可靠的信任根，将它们包含在 uv 中可以提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的原生证书存储，特别是如果您依赖于系统证书存储中包含的公司信任根（例如，强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存读取或写入数据，而是使用临时目录来完成操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>、<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转图标或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 将仅使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在指定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树的方式被发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>请参见 <code>--directory</code> 来完全更改工作目录。</p>

<p>当在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。如果 uv 管理的 Python 没有安装，uv 将使用系统的 Python 安装。此选项允许优先使用或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--token</code> <i>token</i></dt><dd><p>用于身份验证的 GitHub 令牌。虽然令牌不是必需的，但可以用来减少遇到速率限制的概率。</p>

<p>也可以通过 <code>UV_GITHUB_TOKEN</code> 环境变量进行设置。</p>
</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以通过 <code>RUST_LOG</code> 环境变量配置精细的日志记录。(&lt;https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives&gt;)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 版本</p>

</dd></dl>

## uv version

显示 uv 的版本

<h3 class="cli-reference">用法</h3>

```
uv version [选项]
```

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机建立不安全的连接。</p>

<p>可以多次提供。</p>

<p>接受主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：列表中的主机将不会与系统的证书存储进行验证。仅在安全网络中与经过验证的来源一起使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会使您暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认情况下，在 macOS 和 Linux 上为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认: auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅当输出发送到支持颜色的终端或 TTY 时，才启用颜色输出</li>

<li><code>always</code>: 无论检测到的环境如何，始终启用颜色输出</li>

<li><code>never</code>: 禁用颜色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件的路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许这样做。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以给定目录为基准进行解析。</p>

<p>请参见 <code>--project</code>，它只会更改项目的根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简洁帮助</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的原生证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是来自 Mozilla 的一组可靠的信任根，将它们包含在 uv 中可以提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>然而，在某些情况下，您可能希望使用平台的原生证书存储，特别是如果您依赖于系统证书存储中包含的公司信任根（例如，强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存读取或写入数据，而是使用临时目录来完成操作。</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>、<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，旋转图标或进度条。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用自动下载 Python。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用时，uv 将仅使用本地缓存的数据和本地可用的文件。</p>

</dd><dt><code>--output-format</code> <i>output-format</i></dt><dt><code>--project</code> <i>project</i></dt><dd><p>在指定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树的方式被发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>请参见 <code>--directory</code> 来完全更改工作目录。</p>

<p>当在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装或系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它管理的 Python 版本。如果 uv 管理的 Python 没有安装，uv 将使用系统的 Python 安装。此选项允许优先使用或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dt><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置精细的日志记录。 (<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives</a>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本</p>

</dd></dl>

## uv generate-shell-completion

生成 shell 自动补全

<h3 class="cli-reference">用法</h3>

```
uv generate-shell-completion [选项] <SHELL>
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>SHELL</code></dt><dd><p>要为其生成补全脚本的 shell</p>

</dd></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机建立不安全的连接。</p>

<p>可以多次提供。</p>

<p>接受主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：列表中的主机将不会与系统的证书存储进行验证。仅在安全网络中与经过验证的来源一起使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会使您暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以给定目录为基准进行解析。</p>

<p>请参见 <code>--project</code>，它只会更改项目的根目录。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在指定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树的方式被发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>请参见 <code>--directory</code> 来完全更改工作目录。</p>

<p>当在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd></dl>

## uv help

显示命令文档

<h3 class="cli-reference">用法</h3>

```
uv help [选项] [命令]...
```

<h3 class="cli-reference">参数</h3>

<dl class="cli-reference"><dt><code>COMMAND</code></dt></dl>

<h3 class="cli-reference">选项</h3>

<dl class="cli-reference"><dt><code>--allow-insecure-host</code> <i>allow-insecure-host</i></dt><dd><p>允许与主机建立不安全的连接。</p>

<p>可以多次提供。</p>

<p>接受主机名（例如，<code>localhost</code>）、主机-端口对（例如，<code>localhost:8080</code>）或 URL（例如，<code>https://localhost</code>）。</p>

<p>警告：列表中的主机将不会与系统的证书存储进行验证。仅在安全网络中与经过验证的来源一起使用 <code>--allow-insecure-host</code>，因为它绕过了 SSL 验证，可能会使您暴露于中间人攻击（MITM）。</p>

<p>也可以通过 <code>UV_INSECURE_HOST</code> 环境变量进行设置。</p>
</dd><dt><code>--cache-dir</code> <i>cache-dir</i></dt><dd><p>缓存目录的路径。</p>

<p>默认情况下，在 macOS 和 Linux 上为 <code>$XDG_CACHE_HOME/uv</code> 或 <code>$HOME/.cache/uv</code>，在 Windows 上为 <code>%LOCALAPPDATA%\uv\cache</code>。</p>

<p>要查看缓存目录的位置，请运行 <code>uv cache dir</code>。</p>

<p>也可以通过 <code>UV_CACHE_DIR</code> 环境变量进行设置。</p>
</dd><dt><code>--color</code> <i>color-choice</i></dt><dd><p>控制输出中的颜色</p>

<p>[默认值：auto]</p>
<p>可能的值：</p>

<ul>
<li><code>auto</code>: 仅在输出发送到支持的终端或 TTY 时启用彩色输出</li>

<li><code>always</code>: 无论环境如何，都启用彩色输出</li>

<li><code>never</code>: 禁用彩色输出</li>
</ul>
</dd><dt><code>--config-file</code> <i>config-file</i></dt><dd><p>用于配置的 <code>uv.toml</code> 文件路径。</p>

<p>虽然 uv 配置可以包含在 <code>pyproject.toml</code> 文件中，但在此上下文中不允许。</p>

<p>也可以通过 <code>UV_CONFIG_FILE</code> 环境变量进行设置。</p>
</dd><dt><code>--directory</code> <i>directory</i></dt><dd><p>在运行命令之前切换到指定的目录。</p>

<p>相对路径将以给定目录为基准进行解析。</p>

<p>请参见 <code>--project</code>，它只会更改项目根目录。</p>

</dd><dt><code>--help</code>, <code>-h</code></dt><dd><p>显示此命令的简明帮助信息</p>

</dd><dt><code>--native-tls</code></dt><dd><p>是否从平台的本地证书存储加载 TLS 证书。</p>

<p>默认情况下，uv 会从捆绑的 <code>webpki-roots</code> crate 加载证书。<code>webpki-roots</code> 是一组来自 Mozilla 的可靠信任根，将它们包含在 uv 中可以提高可移植性和性能（尤其是在 macOS 上）。</p>

<p>但是，在某些情况下，您可能希望使用平台的本地证书存储，尤其是当您依赖于系统证书存储中包含的公司信任根（例如，强制代理）时。</p>

<p>也可以通过 <code>UV_NATIVE_TLS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-cache</code>, <code>-n</code></dt><dd><p>避免从缓存读取或写入数据，而是使用临时目录进行操作</p>

<p>也可以通过 <code>UV_NO_CACHE</code> 环境变量进行设置。</p>
</dd><dt><code>--no-config</code></dt><dd><p>避免发现配置文件（<code>pyproject.toml</code>，<code>uv.toml</code>）。</p>

<p>通常，配置文件会在当前目录、父目录或用户配置目录中发现。</p>

<p>也可以通过 <code>UV_NO_CONFIG</code> 环境变量进行设置。</p>
</dd><dt><code>--no-pager</code></dt><dd><p>打印帮助信息时禁用分页器</p>

</dd><dt><code>--no-progress</code></dt><dd><p>隐藏所有进度输出。</p>

<p>例如，隐藏进度条或旋转指示器。</p>

<p>也可以通过 <code>UV_NO_PROGRESS</code> 环境变量进行设置。</p>
</dd><dt><code>--no-python-downloads</code></dt><dd><p>禁用 Python 的自动下载。</p>

</dd><dt><code>--offline</code></dt><dd><p>禁用网络访问。</p>

<p>禁用后，uv 将只使用本地缓存数据和本地可用文件。</p>

</dd><dt><code>--project</code> <i>project</i></dt><dd><p>在指定的项目目录中运行命令。</p>

<p>所有 <code>pyproject.toml</code>、<code>uv.toml</code> 和 <code>.python-version</code> 文件将通过从项目根目录向上遍历目录树的方式被发现，项目的虚拟环境（<code>.venv</code>）也会被发现。</p>

<p>其他命令行参数（如相对路径）将相对于当前工作目录进行解析。</p>

<p>请参见 <code>--directory</code> 来完全更改工作目录。</p>

<p>当在 <code>uv pip</code> 接口中使用时，此设置无效。</p>

</dd><dt><code>--python-preference</code> <i>python-preference</i></dt><dd><p>是否优先使用 uv 管理的 Python 安装，还是系统 Python 安装。</p>

<p>默认情况下，uv 优先使用它所管理的 Python 版本。如果没有安装 uv 管理的 Python，uv 将使用系统 Python 安装。此选项允许您优先或忽略系统 Python 安装。</p>

<p>也可以通过 <code>UV_PYTHON_PREFERENCE</code> 环境变量进行设置。</p>
<p>可能的值：</p>

<ul>
<li><code>only-managed</code>: 仅使用管理的 Python 安装；永远不使用系统 Python 安装</li>

<li><code>managed</code>: 优先使用管理的 Python 安装，而不是系统 Python 安装</li>

<li><code>system</code>: 优先使用系统 Python 安装，而不是管理的 Python 安装</li>

<li><code>only-system</code>: 仅使用系统 Python 安装；永远不使用管理的 Python 安装</li>
</ul>
</dd><dt><code>--quiet</code>, <code>-q</code></dt><dd><p>不打印任何输出</p>

</dd><dt><code>--verbose</code>, <code>-v</code></dd><dd><p>使用详细输出。</p>

<p>您可以使用 <code>RUST_LOG</code> 环境变量配置精细的日志记录。 (<a href="https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives">https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives</a>)</p>

</dd><dt><code>--version</code>, <code>-V</code></dt><dd><p>显示 uv 的版本</p>

</dd></dl>


# 环境变量

uv 定义并尊重以下环境变量：

### `UV_BREAK_SYSTEM_PACKAGES`

等同于 `--break-system-packages` 命令行参数。如果设置为 `true`，uv 将允许安装与系统安装的软件包冲突的软件包。
警告：`UV_BREAK_SYSTEM_PACKAGES=true` 仅应在持续集成（CI）或容器化环境中使用，并应谨慎使用，因为修改系统 Python 可能导致意外行为。

### `UV_BUILD_CONSTRAINT`

等同于 `--build-constraint` 命令行参数。如果设置，uv 将使用该文件作为源代码分发构建的约束。使用以空格分隔的文件列表。

### `UV_CACHE_DIR`

等同于 `--cache-dir` 命令行参数。如果设置，uv 将使用此目录进行缓存，而不是默认的缓存目录。

### `UV_COMPILE_BYTECODE`

等同于 `--compile-bytecode` 命令行参数。如果设置，uv 将在安装后编译 Python 源代码文件为字节码。

### `UV_CONCURRENT_BUILDS`

设置 uv 在任何给定时间并发构建的源代码分发包的最大数量。

### `UV_CONCURRENT_DOWNLOADS`

设置 uv 在任何给定时间并发下载的最大数量。

### `UV_CONCURRENT_INSTALLS`

控制安装和解压包时使用的线程数。

### `UV_CONFIG_FILE`

等同于 `--config-file` 命令行参数。期望一个指向本地 `uv.toml` 配置文件的路径。

### `UV_CONSTRAINT`

等同于 `--constraint` 命令行参数。如果设置，uv 将使用该文件作为约束文件。使用以空格分隔的文件列表。

### `UV_CUSTOM_COMPILE_COMMAND`

等同于 `--custom-compile-command` 命令行参数。用于在 `uv pip compile` 生成的 `requirements.txt` 文件的输出头部覆盖 uv。适用于从包装脚本中调用 `uv pip compile` 的情况，以便在输出文件中包含包装脚本的名称。

### `UV_DEFAULT_INDEX`

等同于 `--default-index` 命令行参数。如果设置，uv 将在搜索软件包时使用此 URL 作为默认索引。

### `UV_ENV_FILE`

`.env` 文件，用于在执行 `uv run` 命令时加载环境变量。

### `UV_EXCLUDE_NEWER`

等同于 `--exclude-newer` 命令行参数。如果设置，uv 将排除在指定日期之后发布的分发包。

### `UV_EXTRA_INDEX_URL`

等同于 `--extra-index-url` 命令行参数。如果设置，uv 将使用此空格分隔的 URL 列表作为额外的索引来搜索软件包。
（已弃用：改用 `UV_INDEX`。）

### `UV_FIND_LINKS`

等同于 `--find-links` 命令行参数。如果设置，uv 将使用此以逗号分隔的额外位置列表来搜索软件包。

### `UV_FROZEN`

等同于 `--frozen` 命令行参数。如果设置，uv 将运行而不更新 `uv.lock` 文件。

### `UV_GITHUB_TOKEN`

等同于自我更新的 `--token` 参数。用于身份验证的 GitHub 令牌。

### `UV_HTTP_TIMEOUT`

HTTP 请求的超时时间（以秒为单位）。默认值：30 秒。

### `UV_INDEX`

等同于 `--index` 命令行参数。如果设置，uv 将使用此空格分隔的 URL 列表作为额外的索引来搜索软件包。

### `UV_INDEX_STRATEGY`

等同于 `--index-strategy` 命令行参数。例如，如果设置为 `unsafe-any-match`，uv 将考虑通过所有索引 URL 提供的给定软件包版本，而不是将其搜索限制在包含该软件包的第一个索引 URL。

### `UV_INDEX_URL`

等同于 `--index-url` 命令行参数。如果设置，uv 将使用此 URL 作为默认索引来搜索软件包。
（已弃用：改用 `UV_DEFAULT_INDEX`。）

### `UV_INDEX_{name}_PASSWORD`

为 HTTP 基本身份验证密码生成环境变量键。

### `UV_INDEX_{name}_USERNAME`

为 HTTP 基本身份验证用户名生成环境变量键。

### `UV_INSECURE_HOST`

等同于 `--allow-insecure-host` 参数。

### `UV_INSTALLER_GHE_BASE_URL`

用于通过独立安装程序和 `self update` 功能下载 uv 的 URL，替代默认的 GitHub 企业版 URL。

### `UV_INSTALLER_GITHUB_BASE_URL`

用于通过独立安装程序和 `self update` 功能下载 uv 的 URL，替代默认的 GitHub URL。

### `UV_INSTALL_DIR`

使用独立安装程序和 `self update` 功能安装 uv 的目录。默认值为 `~/.local/bin`。

### `UV_KEYRING_PROVIDER`

等同于 `--keyring-provider` 命令行参数。如果设置，uv 将使用此值作为密钥环提供者。

### `UV_LINK_MODE`

等同于 `--link-mode` 命令行参数。如果设置，uv 将使用此值作为链接模式。

### `UV_LOCKED`

等同于 `--locked` 命令行参数。如果设置，uv 将确保 `uv.lock` 文件保持不变。

### `UV_NATIVE_TLS`

等同于 `--native-tls` 命令行参数。如果设置为 `true`，uv 将使用系统的信任存储，而不是捆绑的 `webpki-roots` crate。

### `UV_NO_BUILD_ISOLATION`

等同于 `--no-build-isolation` 命令行参数。如果设置，uv 将在构建源分发包时跳过隔离。

### `UV_NO_CACHE`

等同于 `--no-cache` 命令行参数。如果设置，uv 将不会在任何操作中使用缓存。

### `UV_NO_CONFIG`

等同于 `--no-config` 命令行参数。如果设置，uv 将不会读取当前目录、父目录或用户配置目录中的任何配置文件。

### `UV_NO_ENV_FILE`

执行 `uv run` 命令时忽略 `.env` 文件。

### `UV_NO_PROGRESS`

等同于 `--no-progress` 命令行参数。禁用所有进度输出。例如，旋转器和进度条。

### `UV_NO_SYNC`

等同于 `--no-sync` 命令行参数。如果设置，uv 将跳过更新环境。

### `UV_NO_VERIFY_HASHES`

等同于 `--no-verify-hashes` 命令行参数。禁用 `requirements.txt` 文件的哈希验证。

### `UV_NO_WRAP`

用于禁用诊断信息的换行。

### `UV_OVERRIDE`

等同于 `--override` 命令行参数。如果设置，uv 将使用此文件作为覆盖文件。使用以空格分隔的文件列表。

### `UV_PRERELEASE`

等同于 `--prerelease` 命令行参数。例如，如果设置为 `allow`，uv 将允许所有依赖项使用预发布版本。

### `UV_PREVIEW`

等同于 `--preview` 命令行参数。启用预览模式。

### `UV_PROJECT_ENVIRONMENT`

指定用于项目虚拟环境的目录路径。有关详细信息，请参见 [项目文档](../concepts/projects/config.md#project-environment-path)。

### `UV_PUBLISH_CHECK_URL`

如果文件已存在于索引中，则不上传该文件。值为索引的 URL。

### `UV_PUBLISH_PASSWORD`

等同于 `uv publish` 中的 `--password` 命令行参数。如果设置，uv 将使用此密码进行发布。

### `UV_PUBLISH_TOKEN`

等同于 `uv publish` 中的 `--token` 命令行参数。如果设置，uv 将使用此令牌（用户名为 `__token__`）进行发布。

### `UV_PUBLISH_URL`

等同于 `uv publish` 中的 `--publish-url` 命令行参数。用于 `uv publish` 的索引上传端点的 URL。

### `UV_PUBLISH_USERNAME`

等同于 `uv publish` 中的 `--username` 命令行参数。如果设置，uv 将使用此用户名进行发布。

### `UV_PYPY_INSTALL_MIRROR`

托管的 PyPy 安装包是从 [python.org](https://downloads.python.org/) 下载的。可以将此变量设置为镜像 URL，以使用不同的源下载 PyPy 安装包。提供的 URL 将替代 `https://downloads.python.org/pypy`，例如，`https://downloads.python.org/pypy/pypy3.8-v7.3.7-osx64.tar.bz2`。可以通过使用 `file://` URL 方案从本地目录读取分发包。

### `UV_PYTHON`

等同于 `--python` 命令行参数。如果设置为路径，uv 将使用此 Python 解释器进行所有操作。

### `UV_PYTHON_BIN_DIR`

指定存放已安装的、托管的 Python 可执行文件的目录。

### `UV_PYTHON_DOWNLOADS`

等同于 [`python-downloads`](../reference/settings.md#python-downloads) 设置，并且在禁用时等同于 `--no-python-downloads` 选项。控制 uv 是否允许下载 Python。

### `UV_PYTHON_INSTALL_DIR`

指定存储托管 Python 安装包的目录。

### `UV_PYTHON_INSTALL_MIRROR`

托管的 Python 安装包是从 [`python-build-standalone`](https://github.com/indygreg/python-build-standalone) 下载的。可以将此变量设置为镜像 URL，以使用不同的源下载 Python 安装包。提供的 URL 将替代 `https://github.com/indygreg/python-build-standalone/releases/download`，例如，`https://github.com/indygreg/python-build-standalone/releases/download/20240713/cpython-3.12.4%2B20240713-aarch64-apple-darwin-install_only.tar.gz`。可以通过使用 `file://` URL 方案从本地目录读取分发包。

### `UV_PYTHON_PREFERENCE`

等同于 `--python-preference` 命令行参数。控制 uv 是否更偏好使用系统 Python 版本或托管的 Python 版本。

### `UV_REQUEST_TIMEOUT`

HTTP 请求的超时时间（以秒为单位）。等同于 `UV_HTTP_TIMEOUT`。

### `UV_REQUIRE_HASHES`

等同于 `--require-hashes` 命令行参数。如果设置为 `true`，uv 将要求所有依赖项在要求文件中指定哈希值。

### `UV_RESOLUTION`

等同于 `--resolution` 命令行参数。例如，如果设置为 `lowest-direct`，uv 将安装所有直接依赖项的最低兼容版本。

### `UV_STACK_SIZE`

用于增加在 Windows 上调试版本中 uv 使用的堆栈大小。

### `UV_SYSTEM_PYTHON`

等同于 `--system` 命令行参数。如果设置为 `true`，uv 将使用在系统 `PATH` 中找到的第一个 Python 解释器。  
警告：`UV_SYSTEM_PYTHON=true` 仅适用于持续集成（CI）或容器化环境，应谨慎使用，因为修改系统 Python 可能会导致不可预期的行为。

### `UV_TOOL_BIN_DIR`

指定用于安装工具可执行文件的 "bin" 目录。

### `UV_TOOL_DIR`

指定 uv 存储托管工具的目录。

### `UV_UNMANAGED_INSTALL`

用于 CI 等短暂环境，将 uv 安装到特定路径，同时防止安装程序修改 shell 配置文件或环境变量。



## 外部定义的变量

uv 还读取以下外部定义的环境变量：

### `ACTIONS_ID_TOKEN_REQUEST_TOKEN`

用于通过 `uv publish` 进行受信发布。包含 oidc 请求令牌。

### `ACTIONS_ID_TOKEN_REQUEST_URL`

用于通过 `uv publish` 进行受信发布。包含 oidc 令牌 URL。

### `ALL_PROXY`

用于所有网络请求的通用代理。

### `BASH_VERSION`

用于检测是否使用了 Bash shell。

### `CLICOLOR_FORCE`

用于通过 `anstyle` 控制颜色。

### `CONDA_DEFAULT_ENV`

用于确定当前激活的 Conda 环境是否为基础环境。

### `CONDA_PREFIX`

用于检测是否激活了 Conda 环境。

### `FISH_VERSION`

用于检测是否使用了 Fish shell。

### `FORCE_COLOR`

强制输出彩色内容，无论终端是否支持。

请参阅 [force-color.org](https://force-color.org)。

### `GITHUB_ACTIONS`

用于通过 `uv publish` 进行受信发布。

### `HOME`

标准的 `HOME` 环境变量。

### `HTTPS_PROXY`

HTTPS 请求的代理。

### `HTTP_PROXY`

HTTP 请求的代理。

### `HTTP_TIMEOUT`

HTTP 请求的超时时间（以秒为单位）。等同于 `UV_HTTP_TIMEOUT`。

### `INSTALLER_NO_MODIFY_PATH`

在使用独立安装程序和 `self update` 功能安装 uv 时，避免修改 `PATH` 环境变量。

### `JPY_SESSION_NAME`

用于检测是否在 Jupyter notebook 中运行。

### `KSH_VERSION`

用于检测是否使用了 Ksh shell。

### `LOCALAPPDATA`

用于查找 Microsoft Store 中的 Python 安装。

### `MACOSX_DEPLOYMENT_TARGET`

与 `--python-platform macos` 及相关变体一起使用，用于设置部署目标（即，最低支持的 macOS 版本）。

默认为 `12.0`，即在写作时最旧的非 EOL macOS 版本。

### `NETRC`

用于设置 `.netrc` 文件的位置。

### `NO_COLOR`

禁用彩色输出（优先于 `FORCE_COLOR`）。

请参阅 [no-color.org](https://no-color.org)。

### `NU_VERSION`

用于检测 `NuShell` 的使用。

### `PAGER`

标准的 `PAGER` posix 环境变量。由 `uv` 用于配置适当的分页器。

### `PATH`

标准的 `PATH` 环境变量。

### `PROMPT`

用于检测是否使用了 Windows 命令提示符（与 PowerShell 相对）。

### `PWD`

标准的 `PWD` posix 环境变量。

### `PYC_INVALIDATION_MODE`

在使用 `--compile` 时运行时的验证模式。

请参阅 [`PycInvalidationMode`](https://docs.python.org/3/library/py_compile.html#py_compile.PycInvalidationMode)。

### `PYTHONPATH`

添加目录到 Python 模块搜索路径（例如，`PYTHONPATH=/path/to/modules`）。

### `RUST_LOG`

如果设置，uv 将使用此值作为其 `--verbose` 输出的日志级别。接受与 `tracing_subscriber` crate 兼容的任何过滤器。
例如：
* `RUST_LOG=uv=debug` 相当于在命令行中添加 `--verbose`
* `RUST_LOG=trace` 将启用追踪级别的日志记录。

请参阅 [tracing 文档](https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#example-syntax) 了解更多信息。

### `SHELL`

标准的 `SHELL` posix 环境变量。

### `SSL_CERT_FILE`

用于 SSL 连接的自定义证书捆绑文件路径。

### `SSL_CLIENT_CERT`

如果设置，uv 将使用此文件进行 mTLS 认证。
该文件应包含证书和私钥，格式为 PEM。

### `SYSTEMDRIVE`

Windows 系统中系统级配置目录的路径。

### `TRACING_DURATIONS_FILE`

用于通过 `tracing-durations-export` 功能创建追踪持续时间文件。

### `VIRTUAL_ENV`

用于检测已激活的虚拟环境。

### `VIRTUAL_ENV_DISABLE_PROMPT`

如果在虚拟环境激活之前将其设置为 `1`，则虚拟环境的名称将不会加到终端提示符前。

### `XDG_BIN_HOME`

可执行文件安装目录的路径。

### `XDG_CACHE_HOME`

Unix 系统上的缓存目录路径。

### `XDG_CONFIG_DIRS`

Unix 系统上系统级配置目录的路径。

### `XDG_CONFIG_HOME`

Unix 系统上用户级配置目录的路径。

### `XDG_DATA_HOME`

用于存储管理的 Python 安装和工具的目录路径。

### `ZDOTDIR`

用于确定在使用 Zsh 时应使用哪个 `.zshenv` 文件。

### `ZSH_VERSION`

用于检测 Zsh shell 的使用情况。


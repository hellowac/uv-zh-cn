# 身份验证

## Git 身份验证 {: #git-authentication}

uv 允许从 Git 安装包，并支持以下方案来认证与私有仓库的连接。

使用 SSH：

- `git+ssh://git@<hostname>/...`（例如 `git+ssh://git@github.com/astral-sh/uv`）
- `git+ssh://git@<host>/...`（例如 `git+ssh://git@github.com-key-2/astral-sh/uv`）

有关如何配置 SSH，请参阅 [GitHub SSH 文档](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/about-ssh)。

使用密码或令牌：

- `git+https://<user>:<token>@<hostname>/...`（例如 `git+https://git:github_pat_asdf@github.com/astral-sh/uv`）
- `git+https://<token>@<hostname>/...`（例如 `git+https://github_pat_asdf@github.com/astral-sh/uv`）
- `git+https://<user>@<hostname>/...`（例如 `git+https://git@github.com/astral-sh/uv`）

当使用 GitHub 个人访问令牌时，用户名是任意的。GitHub 不支持直接使用密码登录，尽管其他主机可能支持。如果提供了用户名但没有凭证，您将被提示输入凭证。

如果 URL 中没有凭证且需要身份验证，将会查询 [Git 凭证助手](https://git-scm.com/doc/credential-helpers)。

## HTTP 身份验证

uv 支持在查询包注册表时通过 HTTP 提供凭证。

身份验证可以通过以下来源进行，按优先级顺序排列：

- URL，例如 `https://<user>:<password>@<hostname>/...`
- 一个 [`.netrc`](https://everything.curl.dev/usingcurl/netrc) 配置文件
- 一个 [keyring](https://github.com/jaraco/keyring) 提供者（需要选择加入）

如果为单一网络位置（方案、主机和端口）找到身份验证，它将在命令期间缓存并用于该网络位置的其他查询。身份验证不会在 uv 的调用之间缓存。

`.netrc` 身份验证默认启用，如果定义了 `NETRC` 环境变量，它将被尊重，未定义时会回退到 `~/.netrc`。

要启用基于 keyring 的身份验证，请将 `--keyring-provider subprocess` 命令行参数传递给 uv，或设置 `UV_KEYRING_PROVIDER=subprocess`。

身份验证可用于以下上下文中指定的主机：

- `index-url`
- `extra-index-url`
- `find-links`
- `package @ https://...`

有关与 `pip` 的差异，请参阅 [`pip` 兼容性指南](../pip/compatibility.md#registry-authentication)。

## 自定义 CA 证书

默认情况下，uv 从捆绑的 `webpki-roots` crate 加载证书。`webpki-roots` 是来自 Mozilla 的一组可靠的信任根，将它们包括在 uv 中可以提高可移植性和性能（特别是在 macOS 上，读取系统信任存储会导致显著的延迟）。

然而，在某些情况下，您可能希望使用平台的本地证书存储，特别是如果您依赖于一个企业信任根（例如，用于强制代理的根证书），该证书包含在系统的证书存储中。要指示 uv 使用系统的信任存储，请使用 `--native-tls` 命令行标志，或将 `UV_NATIVE_TLS` 环境变量设置为 `true`。

如果需要证书的直接路径（例如，在 CI 中），请设置 `SSL_CERT_FILE` 环境变量为证书捆绑文件的路径，以指示 uv 使用该文件而非系统的信任存储。

如果需要客户端证书认证（mTLS），请将 `SSL_CLIENT_CERT` 环境变量设置为包含证书及其私钥的 PEM 格式文件的路径。

最后，如果您正在使用一个环境，在该环境中希望信任自签名证书或以其他方式禁用证书验证，可以通过 `allow-insecure-host` 配置选项指示 uv 允许连接到特定的主机，即使这些连接不安全。例如，向 `pyproject.toml` 添加以下内容将允许连接到 `example.com` 的不安全连接：

```toml
[tool.uv]
allow-insecure-host = ["example.com"]
```

`allow-insecure-host` 期望接收一个主机名（例如 `localhost`）或主机名-端口对（例如 `localhost:8080`），并且仅适用于 HTTPS 连接，因为 HTTP 连接本身是不安全的。

使用 `allow-insecure-host` 时请谨慎，并仅在受信环境中使用，因为它可能会因缺乏证书验证而暴露您面临的安全风险。

## 使用替代包索引的身份验证

有关使用流行的替代 Python 包索引进行身份验证的详细信息，请参阅 [替代索引集成指南](../guides/integration/alternative-indexes.md)。
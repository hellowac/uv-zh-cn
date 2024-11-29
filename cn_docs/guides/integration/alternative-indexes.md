# 使用替代包索引

虽然 uv 默认使用官方的 Python 包索引（PyPI），但它也支持使用替代包索引。大多数替代索引需要各种形式的身份验证，这需要一些初步的设置。

!!! important

    请阅读 uv 上有关 [使用多个索引](../../pip/compatibility.md#packages-that-exist-on-multiple-indexes) 的文档 — 默认行为与 pip 不同，以防止依赖冲突攻击，但这意味着 uv 可能无法按预期找到包的版本。

## Azure Artifacts

uv 可以从 [Azure DevOps Artifacts](https://learn.microsoft.com/en-us/azure/devops/artifacts/start-using-azure-artifacts?view=azure-devops&tabs=nuget%2Cnugetserver) 安装包。可以使用 [个人访问令牌](https://learn.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=Windows)（PAT）或通过 [`keyring`](https://github.com/jaraco/keyring) 包进行交互式身份验证。

### 使用 PAT

如果有可用的 PAT（例如在 Azure 管道中使用 [`$(System.AccessToken)`](https://learn.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml#systemaccesstoken)），则可以通过 "Basic" HTTP 身份验证方案提供凭据。将 PAT 包含在 URL 的密码字段中。用户名也必须包含，但可以是任何字符串。

例如，如果 PAT 存储在 `$ADO_PAT` 环境变量中，则使用以下命令设置索引 URL：

```console
$ export UV_EXTRA_INDEX_URL=https://dummy:$ADO_PAT@pkgs.dev.azure.com/{organisation}/{project}/_packaging/{feedName}/pypi/simple/
```

### 使用 `keyring`

如果没有 PAT 可用，可以使用 [`keyring`](https://github.com/jaraco/keyring) 包和 [`artifacts-keyring`](https://github.com/Microsoft/artifacts-keyring) 插件对 Artifacts 进行身份验证。因为这两个包是进行 Azure Artifacts 身份验证所必需的，所以必须从 Artifacts 以外的源预先安装它们。

`artifacts-keyring` 插件封装了 [Azure Artifacts Credential Provider 工具](https://github.com/microsoft/artifacts-credprovider)。该凭证提供程序支持多种身份验证模式，包括交互式登录 —— 详细的配置信息请参考 [该工具的文档](https://github.com/microsoft/artifacts-credprovider)。

uv 仅支持在 [子进程模式](https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#registry-authentication) 中使用 `keyring` 包。`keyring` 可执行文件必须在 `PATH` 中，即全局安装或安装在当前环境中。`keyring` CLI 需要在 URL 中包含用户名，且必须使用默认用户名 `VssSessionToken`。

```console
$ # 从公共 PyPI 安装 keyring 和 Artifacts 插件
$ uv tool install keyring --with artifacts-keyring

$ # 启用 keyring 身份验证
$ export UV_KEYRING_PROVIDER=subprocess

$ # 配置带有用户名的索引 URL
$ export UV_EXTRA_INDEX_URL=https://VssSessionToken@pkgs.dev.azure.com/{organisation}/{project}/_packaging/{feedName}/pypi/simple/
```

## Google Artifact Registry

uv 可以从 [Google Artifact Registry](https://cloud.google.com/artifact-registry/docs) 安装包。可以使用密码认证或通过 [`keyring`](https://github.com/jaraco/keyring) 包进行身份验证。

!!! note

    本指南假定已安装并设置好 `gcloud` CLI。

### 密码认证

可以通过 "Basic" HTTP 身份验证方案提供凭据。将访问令牌包含在 URL 的密码字段中。用户名必须是 `oauth2accesstoken`，否则身份验证将失败。

例如，如果令牌存储在 `$ARTIFACT_REGISTRY_TOKEN` 环境变量中，则使用以下命令设置索引 URL：

```bash
export ARTIFACT_REGISTRY_TOKEN=$(gcloud auth application-default print-access-token)
export UV_EXTRA_INDEX_URL=https://oauth2accesstoken:$ARTIFACT_REGISTRY_TOKEN@{region}-python.pkg.dev/{projectId}/{repositoryName}/simple
```

### 使用 `keyring`

你也可以通过 [`keyring`](https://github.com/jaraco/keyring) 包和 [`keyrings.google-artifactregistry-auth`](https://github.com/GoogleCloudPlatform/artifact-registry-python-tools) 插件对 Artifact Registry 进行身份验证。因为这两个包是进行 Artifact Registry 身份验证所必需的，所以必须从 Artifact Registry 以外的源预先安装它们。

`artifacts-keyring` 插件封装了 [gcloud CLI](https://cloud.google.com/sdk/gcloud)，用于生成短期访问令牌，并将其安全地存储在系统密钥链中，当令牌过期时会自动刷新。

uv 仅支持在 [子进程模式](https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#registry-authentication) 中使用 `keyring` 包。`keyring` 可执行文件必须在 `PATH` 中，即全局安装或安装在当前环境中。`keyring` CLI 需要在 URL 中包含用户名，且必须是 `oauth2accesstoken`。

```bash
# 从公共 PyPI 安装 keyring 和 Artifact Registry 插件
uv tool install keyring --with keyrings.google-artifactregistry-auth

# 启用 keyring 身份验证
export UV_KEYRING_PROVIDER=subprocess

# 配置带有用户名的索引 URL
export UV_EXTRA_INDEX_URL=https://oauth2accesstoken@{region}-python.pkg.dev/{projectId}/{repositoryName}/simple
```

## AWS CodeArtifact

uv 可以从 [AWS CodeArtifact](https://docs.aws.amazon.com/codeartifact/latest/ug/using-python.html) 安装包。

可以使用 `awscli` 工具获取授权令牌。

!!! note

    本指南假定 AWS CLI 已经完成身份验证。

首先，声明一些用于 CodeArtifact 仓库的常量：

```bash
export AWS_DOMAIN="<your-domain>"
export AWS_ACCOUNT_ID="<your-account-id>"
export AWS_REGION="<your-region>"
export AWS_CODEARTIFACT_REPOSITORY="<your-repository>"
```

然后，使用 `awscli` 获取令牌：

```bash
export AWS_CODEARTIFACT_TOKEN="$(
    aws codeartifact get-authorization-token \
    --domain $AWS_DOMAIN \
    --domain-owner $AWS_ACCOUNT_ID \
    --query authorizationToken \
    --output text
)"
```

配置索引 URL：

```bash
export UV_EXTRA_INDEX_URL="https://aws:${AWS_CODEARTIFACT_TOKEN}@${AWS_DOMAIN}-${AWS_ACCOUNT_ID}.d.codeartifact.${AWS_REGION}.amazonaws.com/pypi/${AWS_CODEARTIFACT_REPOSITORY}/simple/"
```

### 发布包

如果你还想将自己的包发布到 AWS CodeArtifact，可以使用 `uv publish`，如 [发布指南](../publish.md) 所述。你需要单独设置 `UV_PUBLISH_URL` 和凭据：

```bash
# 配置 uv 使用 AWS CodeArtifact
export UV_PUBLISH_URL="https://${AWS_DOMAIN}-${AWS_ACCOUNT_ID}.d.codeartifact.${AWS_REGION}.amazonaws.com/pypi/${AWS_CODEARTIFACT_REPOSITORY}/"
export UV_PUBLISH_USERNAME=aws
export UV_PUBLISH_PASSWORD="$AWS_CODEARTIFACT_TOKEN"

# 发布包
uv publish
```

## 其他索引

uv 也可以与 JFrog 的 Artifactory 一起使用。
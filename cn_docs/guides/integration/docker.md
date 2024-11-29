# 在 Docker 中使用 uv

## 开始使用

!!! tip

    查看 [`uv-docker-example`](https://github.com/astral-sh/uv-docker-example) 项目，了解使用 uv 在 Docker 中构建应用的最佳实践示例。

### 在容器中运行 uv

Docker 镜像已发布，包含 uv 的构建版本。要在容器中运行 uv 命令：

```console
$ docker run ghcr.io/astral-sh/uv --help
```

### 可用的镜像

uv 提供了一个无发行版的 Docker 镜像，包含 uv 二进制文件。发布了以下标签：

- `ghcr.io/astral-sh/uv:latest`
- `ghcr.io/astral-sh/uv:{major}.{minor}.{patch}`，例如 `ghcr.io/astral-sh/uv:0.5.5`
- `ghcr.io/astral-sh/uv:{major}.{minor}`，例如 `ghcr.io/astral-sh/uv:0.5`（最新的修补版本）

此外，uv 还发布了以下镜像：

<!-- prettier-ignore -->
- 基于 `alpine:3.20`：
    - `ghcr.io/astral-sh/uv:alpine`
    - `ghcr.io/astral-sh/uv:alpine3.20`
- 基于 `debian:bookworm-slim`：
    - `ghcr.io/astral-sh/uv:debian-slim`
    - `ghcr.io/astral-sh/uv:bookworm-slim`
- 基于 `buildpack-deps:bookworm`：
    - `ghcr.io/astral-sh/uv:debian`
    - `ghcr.io/astral-sh/uv:bookworm`
- 基于 `python3.x-alpine`：
    - `ghcr.io/astral-sh/uv:python3.13-alpine`
    - `ghcr.io/astral-sh/uv:python3.12-alpine`
    - `ghcr.io/astral-sh/uv:python3.11-alpine`
    - `ghcr.io/astral-sh/uv:python3.10-alpine`
    - `ghcr.io/astral-sh/uv:python3.9-alpine`
    - `ghcr.io/astral-sh/uv:python3.8-alpine`
- 基于 `python3.x-bookworm`：
    - `ghcr.io/astral-sh/uv:python3.13-bookworm`
    - `ghcr.io/astral-sh/uv:python3.12-bookworm`
    - `ghcr.io/astral-sh/uv:python3.11-bookworm`
    - `ghcr.io/astral-sh/uv:python3.10-bookworm`
    - `ghcr.io/astral-sh/uv:python3.9-bookworm`
    - `ghcr.io/astral-sh/uv:python3.8-bookworm`
- 基于 `python3.x-slim-bookworm`：
    - `ghcr.io/astral-sh/uv:python3.13-bookworm-slim`
    - `ghcr.io/astral-sh/uv:python3.12-bookworm-slim`
    - `ghcr.io/astral-sh/uv:python3.11-bookworm-slim`
    - `ghcr.io/astral-sh/uv:python3.10-bookworm-slim`
    - `ghcr.io/astral-sh/uv:python3.9-bookworm-slim`
    - `ghcr.io/astral-sh/uv:python3.8-bookworm-slim`
<!-- prettier-ignore-end -->

与无发行版镜像一样，每个镜像都发布了 uv 版本标签，如 `ghcr.io/astral-sh/uv:{major}.{minor}.{patch}-{base}` 和 `ghcr.io/astral-sh/uv:{major}.{minor}-{base}`，例如 `ghcr.io/astral-sh/uv:0.5.5-alpine`。

有关更多详细信息，请参见 [GitHub 容器](https://github.com/astral-sh/uv/pkgs/container/uv) 页面。

### 安装 uv

使用预先安装了 uv 的上述镜像，或者通过复制官方无发行版 Docker 镜像中的二进制文件来安装 uv：

```dockerfile title="Dockerfile"
FROM python:3.12-slim-bookworm
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/
```

或者，使用安装程序：

```dockerfile title="Dockerfile"
FROM python:3.12-slim-bookworm

# 安装程序需要 curl（和证书）来下载发行包
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates

# 下载最新安装程序
ADD https://astral.sh/uv/install.sh /uv-installer.sh

# 运行安装程序并删除它
RUN sh /uv-installer.sh && rm /uv-installer.sh

# 确保安装的二进制文件在 `PATH` 中
ENV PATH="/root/.local/bin/:$PATH"
```

请注意，这要求 `curl` 可用。

在这两种情况下，最好的做法是固定 uv 的特定版本，例如：

```dockerfile
COPY --from=ghcr.io/astral-sh/uv:0.5.5 /uv /uvx /bin/
```

或者，使用安装程序：

```dockerfile
ADD https://astral.sh/uv/0.5.5/install.sh /uv-installer.sh
```

### 安装项目

如果您使用 uv 来管理您的项目，您可以将项目复制到镜像中并进行安装：

```dockerfile title="Dockerfile"
# 将项目复制到镜像中
ADD . /app

# 将项目同步到新环境中，使用冻结的 lockfile
WORKDIR /app
RUN uv sync --frozen
```

!!! important

    最佳实践是将 `.venv` 添加到 [`.dockerignore` 文件](https://docs.docker.com/build/concepts/context/#dockerignore-files) 中，避免其被包含在镜像构建中。项目的虚拟环境依赖于本地平台，应在镜像中从头创建。

然后，默认启动应用程序：

```dockerfile title="Dockerfile"
# 假设项目提供了一个 `my_app` 命令
CMD ["uv", "run", "my_app"]
```

!!! tip

    最佳实践是使用 [中间层](#intermediate-layers)，将依赖项的安装与项目本身的安装分开，以提高 Docker 镜像构建时间。

您可以在 [`uv-docker-example` 项目](https://github.com/astral-sh/uv-docker-example/blob/main/Dockerfile) 中查看完整示例。

### 使用环境

安装完项目后，您可以通过将项目虚拟环境的二进制目录放到路径的最前面来 _激活_ 项目虚拟环境：

```dockerfile title="Dockerfile"
ENV PATH="/app/.venv/bin:$PATH"
```

或者，您也可以使用 `uv run` 来执行需要该环境的任何命令：

```dockerfile title="Dockerfile"
RUN uv run some_script.py
```

!!! tip

    或者，可以在同步之前设置 [`UV_PROJECT_ENVIRONMENT`](../../concepts/projects/config.md#project-environment-path) 设置，将项目安装到系统 Python 环境中，从而完全跳过虚拟环境的激活。

### 使用已安装的工具

要使用已安装的工具，确保 [工具的 bin 目录](../../concepts/tools.md#the-bin-directory) 在路径中：

```dockerfile title="Dockerfile"
ENV PATH=/root/.local/bin:$PATH
RUN uv tool install cowsay
```

```console
$ docker run -it $(docker build -q .) /bin/bash -c "cowsay -t hello"
  _____
| hello |
  =====
     \
      \
        ^__^
        (oo)\_______
        (__)\       )\/\
            ||----w |
            ||     ||
```

!!! note

    工具 bin 目录的位置可以通过在容器中运行 `uv tool dir --bin` 命令来确定。

    或者，它可以设置为固定位置：

    ```dockerfile title="Dockerfile"
    ENV UV_TOOL_BIN_DIR=/opt/uv-bin/
    ```

### 在 musl 基础镜像中安装 Python

虽然 uv [会安装兼容的 Python 版本](../install-python.md)（如果镜像中没有可用的版本），但 uv 目前不支持为 musl 基础的发行版安装 Python。例如，如果您使用的是没有安装 Python 的 Alpine Linux 基础镜像，您需要通过系统包管理器添加 Python：

```shell
apk add --no-cache python3~=3.12
```

## 在容器中开发

在开发过程中，将项目目录挂载到容器中非常有用。通过这种设置，项目的更改可以立即反映到容器化的服务中，而无需重新构建镜像。然而，重要的是 _不要_ 将项目的虚拟环境（`.venv`）包括在挂载中，因为虚拟环境是平台特定的，应该保留为镜像中构建的虚拟环境。

### 使用 `docker run` 挂载项目

将项目（在工作目录中）绑定挂载到 `/app`，同时保留 `.venv` 目录，并使用 [匿名卷](https://docs.docker.com/engine/storage/#volumes)：

```console
$ docker run --rm --volume .:/app --volume /app/.venv [...]
```

!!! tip

    包含 `--rm` 标志，确保容器退出时清理容器和匿名卷。

您可以在 [`uv-docker-example` 项目](https://github.com/astral-sh/uv-docker-example/blob/main/run.sh) 中查看完整示例。

### 使用 `docker compose` 配置 `watch`

在使用 Docker Compose 时，可以使用更复杂的工具进行容器开发。[`watch`](https://docs.docker.com/compose/file-watch/#compose-watch-versus-bind-mounts) 选项比绑定挂载提供了更细粒度的控制，并支持在文件发生更改时触发容器化服务的更新。

!!! 注意

    此功能需要 Docker Compose 2.22.0 版本，该版本随 Docker Desktop 4.24 一起提供。

在您的 [Docker Compose 文件](https://docs.docker.com/compose/compose-application-model/#the-compose-file) 中配置 `watch`，以挂载项目目录但不同步项目虚拟环境，并在配置更改时重建镜像：

```yaml title="compose.yaml"
services:
  example:
    build: .

    # ...

    develop:
      # 创建一个 `watch` 配置来更新应用程序
      #
      watch:
        # 将工作目录与容器中的 `/app` 目录同步
        - action: sync
          path: .
          target: /app
          # 排除项目虚拟环境
          ignore:
            - .venv/

        # 在 `pyproject.toml` 更改时重建镜像
        - action: rebuild
          path: ./pyproject.toml
```

然后，运行 `docker compose watch` 以开发设置启动容器。

您可以在 [`uv-docker-example` 项目](https://github.com/astral-sh/uv-docker-example/blob/main/compose.yml) 中查看完整示例。

## 优化

### 编译字节码

将 Python 源文件编译为字节码通常是生产镜像的一个理想选择，因为它可以改善启动时间（尽管会增加安装时间）。

要启用字节码编译，请使用 `--compile-bytecode` 标志：

```dockerfile title="Dockerfile"
RUN uv sync --compile-bytecode
```

或者，您可以设置 `UV_COMPILE_BYTECODE` 环境变量，以确保 Dockerfile 中的所有命令都编译字节码：

```dockerfile title="Dockerfile"
ENV UV_COMPILE_BYTECODE=1
```

### 缓存

可以使用 [缓存挂载](https://docs.docker.com/build/guide/mounts/#add-a-cache-mount) 来提高构建性能：

```dockerfile title="Dockerfile"
ENV UV_LINK_MODE=copy

RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync
```

改变默认的 [`UV_LINK_MODE`](../../reference/settings.md#link-mode) 设置可以消除有关无法使用硬链接的警告，因为缓存和同步目标位于不同的文件系统上。

如果不挂载缓存，可以通过使用 `--no-cache` 标志或设置 `UV_NO_CACHE` 来减少镜像的大小。

!!! note

    缓存目录的位置可以通过在容器中运行 `uv cache dir` 命令来确定。

    或者，可以将缓存设置为固定位置：

    ```dockerfile title="Dockerfile"
    ENV UV_CACHE_DIR=/opt/uv-cache/
    ```

### 中间层

如果使用 uv 管理项目，可以通过将传递依赖项安装移到独立的层来提高构建速度，方法是使用 `--no-install` 选项。

`uv sync --no-install-project` 将安装项目的依赖项，但不会安装项目本身。由于项目内容变化频繁，而依赖项通常是静态的，这可以节省大量时间。

```dockerfile title="Dockerfile"
# 安装 uv
FROM python:3.12-slim
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# 将工作目录更改为 `app` 目录
WORKDIR /app

# 安装依赖项
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project

# 将项目复制到镜像中
ADD . /app

# 同步项目
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen
```

注意，`pyproject.toml` 是识别项目根目录和名称所必需的，但项目的 _内容_ 直到最后一个 `uv sync` 命令执行时才会被复制到镜像中。

!!! tip

    如果使用 [工作空间](../../concepts/projects/workspaces.md)，可以使用 `--no-install-workspace` 标志，排除项目 _和_ 任何工作空间成员。

    如果要从同步中移除特定的包，可以使用 `--no-install-package <name>`。

### 非可编辑安装

默认情况下，uv 会以可编辑模式安装项目和工作空间成员，这样对源代码的更改会立即反映到环境中。

`uv sync` 和 `uv run` 都接受 `--no-editable` 标志，该标志指示 uv 以非可编辑模式安装项目，从而去除对源代码的依赖。

在多阶段 Docker 镜像的上下文中，可以使用 `--no-editable` 来包括来自某一阶段的项目到同步的虚拟环境中，然后将虚拟环境（而不是源代码）单独复制到最终镜像中。

例如：

```dockerfile title="Dockerfile"
# 安装 uv
FROM python:3.12-slim AS builder
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# 将工作目录更改为 `app` 目录
WORKDIR /app

# 安装依赖项
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project --no-editable

# 将项目复制到中间镜像
ADD . /app

# 同步项目
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen --no-editable

FROM python:3.12-slim

# 复制虚拟环境，但不复制源代码
COPY --from=builder --chown=app:app /app/.venv /app/.venv

# 运行应用程序
CMD ["/app/.venv/bin/hello"]
```

### 临时使用 uv

如果 uv 不需要在最终镜像中使用，可以在每次调用时挂载二进制文件：

```dockerfile title="Dockerfile"
RUN --mount=from=ghcr.io/astral-sh/uv,source=/uv,target=/bin/uv \
    uv sync
```

## 使用 pip 接口

### 安装包

在这个上下文中，系统 Python 环境是安全的，因为容器本身已经被隔离。可以使用 `--system` 标志来在系统环境中安装包：

```dockerfile title="Dockerfile"
RUN uv pip install --system ruff
```

要默认使用系统 Python 环境，可以设置 `UV_SYSTEM_PYTHON` 环境变量：

```dockerfile title="Dockerfile"
ENV UV_SYSTEM_PYTHON=1
```

另外，也可以创建并激活一个虚拟环境：

```dockerfile title="Dockerfile"
RUN uv venv /opt/venv
# 自动使用虚拟环境
ENV VIRTUAL_ENV=/opt/venv
# 将环境中的入口点添加到路径的前面
ENV PATH="/opt/venv/bin:$PATH"
```

使用虚拟环境时，uv 调用中应省略 `--system` 标志：

```dockerfile title="Dockerfile"
RUN uv pip install ruff
```

### 安装依赖

要安装 requirements 文件，将其复制到容器中：

```dockerfile title="Dockerfile"
COPY requirements.txt .
RUN uv pip install -r requirements.txt
```

### 安装项目

在安装项目和依赖项时，最佳实践是将复制依赖项的操作与复制其他源代码分开。这可以让项目的依赖项（通常变化不大）与项目本身（变化频繁）分别缓存。

```dockerfile title="Dockerfile"
COPY pyproject.toml .
RUN uv pip install -r pyproject.toml
COPY . .
RUN uv pip install -e .
```

# 在 GitLab CI/CD 中使用 uv

## 使用 uv 镜像

Astral 提供了预装 uv 的 [Docker 镜像](docker.md#available-images)，选择适合你工作流的版本。

```yaml title="gitlab-ci.yml"
variables:
  UV_VERSION: 0.5
  PYTHON_VERSION: 3.12
  BASE_LAYER: bookworm-slim

stages:
  - analysis

uv:
  stage: analysis
  image: ghcr.io/astral-sh/uv:$UV_VERSION-python$PYTHON_VERSION-$BASE_LAYER
  script:
    # 你的 `uv` 命令
```

## 缓存

在工作流运行之间持久化 uv 缓存可以提升性能。

```yaml
uv-install:
  variables:
    UV_CACHE_DIR: .uv-cache
  cache:
    - key:
        files:
          - uv.lock
      paths:
        - $UV_CACHE_DIR
  script:
    # 你的 `uv` 命令
    - uv cache prune --ci
```

有关缓存配置的更多详细信息，请参考 [GitLab 缓存文档](https://docs.gitlab.com/ee/ci/caching/)。

建议在作业结束时使用 `uv cache prune --ci` 来减小缓存的大小。有关更多详细信息，请参见 [uv 缓存文档](../../concepts/cache.md#caching-in-continuous-integration)。

## 使用 `uv pip`

如果使用 `uv pip` 接口而不是 uv 项目接口，默认情况下，uv 需要一个虚拟环境。若要允许将包安装到系统环境中，请在所有 `uv` 调用中使用 `--system` 标志，或者设置 `UV_SYSTEM_PYTHON` 变量。

`UV_SYSTEM_PYTHON` 变量可以在不同的作用域中定义。你可以阅读更多关于 [GitLab 中变量及其优先级的说明](https://docs.gitlab.com/ee/ci/variables/)。

要为整个工作流启用，可以在顶层定义：

```yaml title="gitlab-ci.yml"
variables:
  UV_SYSTEM_PYTHON: 1

# [...]
```

要取消启用，可以在任何 `uv` 调用中使用 `--no-system` 标志。

在持久化缓存时，你可能希望使用 `requirements.txt` 或 `pyproject.toml` 作为缓存键文件，而不是 `uv.lock`。
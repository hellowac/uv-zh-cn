# 版本控制

uv 使用自定义的版本控制方案，其中次版本号（minor version）在发生破坏性变化时递增，补丁版本号（patch version）则在修复 bug、增强功能以及其他非破坏性更改时递增。

uv 目前尚未拥有稳定的 API；一旦 uv 的 API 稳定（v1.0.0 版本），版本控制方案将遵循 [语义化版本控制](https://semver.org/)。

uv 的变更日志可以在 [GitHub 上查看](https://github.com/astral-sh/uv/blob/main/CHANGELOG.md)。

## 缓存版本控制

缓存版本被视为 uv 的内部内容，因此可能在次版本或补丁版本中发生变化。有关更多信息，请参阅 [缓存版本控制](../concepts/cache.md#cache-versioning)。

## lock文件版本控制

`uv.lock` 文件的架构版本被视为公共 API 的一部分，因此仅会在次版本发布中作为破坏性变化进行递增。有关更多信息，请参阅 [锁文件版本控制](../concepts/resolution.md#lockfile-versioning)。
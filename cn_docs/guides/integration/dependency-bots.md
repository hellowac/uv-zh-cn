# 依赖机器人

定期更新依赖项被认为是最佳实践，旨在避免暴露于漏洞，减少依赖项之间的不兼容性，并避免在从过旧版本升级时出现复杂的升级问题。各种工具可以通过创建自动化的拉取请求来帮助保持最新状态。许多工具支持 uv，或者正在进行支持的工作。

## Renovate

uv 已被 [Renovate](https://github.com/renovatebot/renovate) 支持。

!!! note

    更新 `uv pip compile` 输出（如 `requirements.txt`）尚不受支持。相关进展可通过以下链接跟踪：[renovatebot/renovate#30909](https://github.com/renovatebot/renovate/issues/30909)。

### `uv.lock` 输出

Renovate 通过检查 `uv.lock` 文件的存在来确定 uv 是否用于管理依赖项，并会建议升级 [项目依赖项](../../concepts/projects/dependencies.md#project-dependencies)、[可选依赖项](../../concepts/projects/dependencies.md#optional-dependencies) 和 [开发依赖项](../../concepts/projects/dependencies.md#development-dependencies)。Renovate 将同时更新 `pyproject.toml` 和 `uv.lock` 文件。

通过启用 [`lockFileMaintenance`](https://docs.renovatebot.com/configuration-options/#lockfilemaintenance) 选项，还可以定期刷新锁文件（例如更新传递依赖项）：

```jsx title="renovate.json5"
{
  $schema: "https://docs.renovatebot.com/renovate-schema.json",
  lockFileMaintenance: {
    enabled: true,
  },
}
```

### 内联脚本元数据

Renovate 支持更新使用 [脚本内联元数据](../scripts.md/#declaring-script-dependencies) 定义的依赖项。

由于 Renovate 无法自动检测哪些 Python 文件使用了脚本内联元数据，因此需要使用 [`fileMatch`](https://docs.renovatebot.com/configuration-options/#filematch) 显式定义它们的位置，例如：

```jsx title="renovate.json5"
{
  $schema: "https://docs.renovatebot.com/renovate-schema.json",
  pep723: {
    fileMatch: [
      "scripts/generate_docs\\.py",
      "scripts/run_server\\.py",
    ],
  },
}
```

## Dependabot

目前 uv 的支持尚不可用。进展可以在以下链接跟踪：

- [dependabot/dependabot-core#10478](https://github.com/dependabot/dependabot-core/issues/10478) 讨论 `uv.lock` 输出的支持
- [dependabot/dependabot-core#10039](https://github.com/dependabot/dependabot-core/issues/10039) 讨论 `uv pip compile` 输出的支持
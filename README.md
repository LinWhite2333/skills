# Anthony Fu 的 Skills 集合

这是一个精心整理的 [Agent Skills](https://agentskills.io/home) 集合，体现了 [Anthony Fu](https://github.com/antfu) 的个人偏好、实践经验与最佳实践，同时也包含这些工具的使用文档。

> [!IMPORTANT]
> 这是一个概念验证项目，目标是根据源文档生成 agent skills，并持续与上游内容保持同步。
> 我还没有完整验证这些 skills 在实际使用中的效果，因此非常欢迎反馈与贡献。

## 安装

```bash
pnpx skills add antfu/skills --skill='*'
```

或者全局安装全部 skills：

```bash
pnpx skills add antfu/skills --skill='*' -g
```

更多 CLI 用法请参阅 [skills](https://github.com/vercel-labs/skills)。

## Skills 列表

如果你的主要工作栈是 Vite / Nuxt，这个集合的目标是成为一站式技能合集。它整合了来自不同来源、覆盖范围各异的 skills。

### 手工维护的 Skills

> 带有明确主观取向

由 Anthony Fu 手工维护，采用他偏好的工具、项目约定和最佳实践。

| Skill | 说明 |
|-------|------|
| [antfu](skills/antfu) | Anthony Fu 在应用 / 库项目中的偏好与最佳实践（eslint、pnpm、vitest、vue 等） |

### 基于官方文档生成的 Skills

> 不带强主观设定，但会偏向 TypeScript、ESM、Composition API 等现代技术栈

基于官方文档生成，并由 Anthony 进一步调优。

| Skill | 说明 | 来源 |
|-------|------|------|
| [vue](skills/vue) | Vue.js 核心能力：响应式、组件、Composition API | [vuejs/docs](https://github.com/vuejs/docs) |
| [nuxt](skills/nuxt) | Nuxt 框架：文件路由、服务端路由、模块系统 | [nuxt/nuxt](https://github.com/nuxt/nuxt) |
| [pinia](skills/pinia) | Pinia：直观且类型安全的 Vue 状态管理 | [vuejs/pinia](https://github.com/vuejs/pinia) |
| [vite](skills/vite) | Vite 构建工具：配置、插件、SSR、库模式 | [vitejs/vite](https://github.com/vitejs/vite) |
| [vitepress](skills/vitepress) | VitePress：由 Vite 驱动的静态站点生成器 | [vuejs/vitepress](https://github.com/vuejs/vitepress) |
| [vitest](skills/vitest) | Vitest：基于 Vite 的单元测试框架 | [vitest-dev/vitest](https://github.com/vitest-dev/vitest) |
| [unocss](skills/unocss) | UnoCSS：原子化 CSS 引擎，支持 presets 与 transformers | [unocss/unocss](https://github.com/unocss/unocss) |
| [pnpm](skills/pnpm) | pnpm：速度快且节省磁盘空间的包管理器 | [pnpm/pnpm.io](https://github.com/pnpm/pnpm.io) |

### 供应商同步的 Skills

从外部仓库同步而来，这些仓库自行维护各自的 skills。

| Skill | 说明 | 来源 |
|-------|------|------|
| [slidev](skills/slidev) (官方) | Slidev：面向开发者的演示文稿工具 | [slidevjs/slidev](https://github.com/slidevjs/slidev) |
| [tsdown](skills/tsdown) (官方) | tsdown：基于 Rolldown 的 TypeScript 库打包工具 | [rolldown/tsdown](https://github.com/rolldown/tsdown) |
| [turborepo](skills/turborepo) (官方) | Turborepo：面向 monorepo 的高性能构建系统 | [vercel/turborepo](https://github.com/vercel/turborepo) |
| [vueuse-functions](skills/vueuse-functions) (官方) | VueUse：200+ 个 Vue Composition 工具函数 | [vueuse/skills](https://github.com/vueuse/skills) |
| [vue-best-practices](skills/vue-best-practices) | Vue 3 + TypeScript 最佳实践 | [vuejs-ai/skills](https://github.com/vuejs-ai/skills) |
| [vue-router-best-practices](skills/vue-router-best-practices) | Vue Router 最佳实践 | [vuejs-ai/skills](https://github.com/vuejs-ai/skills) |
| [vue-testing-best-practices](skills/vue-testing-best-practices) | Vue 测试最佳实践 | [vuejs-ai/skills](https://github.com/vuejs-ai/skills) |
| [web-design-guidelines](skills/web-design-guidelines) | 用于构建美观界面的 Web 设计指南 | [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) |

## 常见问题

### 这个集合有什么不同？

这个集合带有一定主观取向，但它最关键的区别在于：它通过 git submodule 直接引用源文档。这样可以提供更可靠的上下文，也能让这些 skills 随着上游变化持续保持更新。如果你的主要工作集中在 Vue / Vite / Nuxt，这个集合希望能成为一个完整的一站式选择。

这个项目也被设计得足够灵活，你完全可以把它当作模板，用来生成自己的 skills 集合。

### Skills、llms.txt 和 AGENTS.md 有什么区别？

在我看来，skills 的价值主要体现在两个方面：**可共享** 和 **按需加载**。

可共享意味着提示词更容易在不同项目之间管理和复用。按需加载则意味着你只需要在需要时引入对应 skill，这种扩展方式远远超出任何单个 agent 上下文窗口一次性能容纳的内容。

你可能会听到有人说“AGENTS.md 比 skills 更有效”。我认为这话有一定道理，因为 AGENTS.md 会在一开始就整体加载，所以 agent 通常都会遵循它；而 skills 可能会出现“该加载时没有加载”的漏召情况。不过我更倾向于把这看作工具链和集成方式还不够成熟的阶段性问题，未来应该会持续改善。归根到底，skills 只是 agent 可消费的一种标准化格式，本质上仍然是普通的 Markdown 文件。你可以把它理解成面向 agent 的知识库。如果你希望某些 skills 始终生效，也可以在自己的 AGENTS.md 中直接引用它们。

## 生成你自己的 Skills

你可以 fork 这个项目，构建自己的定制化 skill 集合。

1. Fork 或 clone 此仓库
2. 安装依赖：`pnpm install`
3. 在 `meta.ts` 中更新为你自己的项目与 skill 来源
4. 运行 `pnpm start cleanup`，移除现有 submodule 和 skills
5. 运行 `pnpm start init`，克隆这些 submodule
6. 运行 `pnpm start sync`，同步 vendored skills
7. 让你的 agent 执行 `Generate skills for <project>`（建议一次只生成一个，以便控制 token 消耗）

更详细的生成说明请参阅 [AGENTS.md](AGENTS.md)。

## Sponsors

<p align="center">
  <a href="https://cdn.jsdelivr.net/gh/antfu/static/sponsors.svg">
    <img src='https://cdn.jsdelivr.net/gh/antfu/static/sponsors.svg' alt='Sponsors' />
  </a>
</p>

## 许可证

本仓库中的 skills 与脚本均采用 [MIT](LICENSE.md) 许可证。

从外部仓库同步而来的 vendored skills 保留其原始许可证，详情请查看各自 skill 目录。

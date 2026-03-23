# Skills 生成指南

根据项目文档生成 [Agent Skills](https://agentskills.io/home)。

请严格遵循 Skill 的最佳实践：https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices

- 聚焦于 agent 的能力边界和实际使用模式。
- 忽略面向用户的指南、介绍、快速开始、安装说明等内容。
- 忽略那些 LLM agent 在训练数据中通常已经足够熟悉的内容。
- Skill 应尽可能精简，避免创建过多参考文件。

## Skill 来源类型

Skill 来源分为两类，项目列表定义在 `meta.ts` 中：

### 类型 1：生成型 Skills（`sources/`）

适用于 **尚未提供现成 skills** 的开源项目。我们会将仓库以 submodule 的形式克隆下来，再根据其文档生成 skills。

- **项目示例：** Vue、Nuxt、Vite、UnoCSS
- **工作流：** 阅读文档 → 理解内容 → 生成 skills
- **来源目录：** `sources/{project}/docs/`

### 类型 2：同步型 Skills（`vendor/`）

适用于 **已经自行维护 skills** 的项目。我们会将它们的仓库作为 submodule 克隆下来，并把指定的 skills 同步到本仓库中。

- **项目示例：** Slidev、VueUse
- **工作流：** 拉取更新 → 复制指定 skills（可选重命名）
- **来源目录：** `vendor/{project}/skills/{skill-name}/`
- **配置位置：** 每个 vendor 在 `meta.ts` 中定义需要同步哪些 skills，以及输出名称

### 类型 3：手写 Skills

这类 skill 由 Anthony Fu 根据自己的偏好、经验、审美和最佳实践亲自编写。

除非明确要求，否则你不需要处理这类内容。

## 仓库结构

```
.
├── meta.ts                     # 项目元数据（仓库与 URL）
├── instructions/               # 生成 skills 的说明
│   └── {project}.md            # 为 {project} 生成 skills 的具体说明
│
├── sources/                    # 类型 1：开源仓库（从文档生成）
│   └── {project}/
│       └── docs/               # 从这里读取文档
│
├── vendor/                     # 类型 2：已有 skills 的项目（仅同步）
│   └── {project}/
│       └── skills/
│           └── {skill-name}/   # 需要同步的单个 skill
│
└── skills/                     # 输出目录（生成或同步后的结果）
    └── {output-name}/
        ├── SKILL.md            # 所有 skills 的索引
        ├── GENERATION.md       # 生成型 skills 的跟踪元数据
        ├── SYNC.md             # 同步型 skills 的跟踪元数据
        └── references/
            └── *.md            # 单个 skill 文件
```

**重要：** 对于类型 1（生成型），`skills/{project}/` 的名称必须与 `sources/{project}/` 保持一致。对于类型 2（同步型），输出名称由 `meta.ts` 配置，可能与源 skill 名称不同。

## 工作流程

### 面向生成型 Skills（类型 1）

#### 添加新项目

1. 在 `meta.ts` 的 `submodules` 对象中 **添加条目**：
   ```ts
   export const submodules = {
     // ... existing entries
     'new-project': 'https://github.com/org/repo',
   }
   ```

2. **运行同步脚本**，克隆 submodule：
   ```bash
   nr start init -y
   ```
   这会把仓库克隆到 `sources/{project}/`

3. 按照下方的生成指南 **创建 skills**

#### 通用生成说明

- 聚焦于 agent 的能力边界和实际使用模式。对于面向用户的指南、介绍、快速开始，或 LLM 已普遍掌握的常识性内容，可以跳过。
- 将 references 按 `core`、`features`、`best-practices`、`advanced` 等类别整理，并在参考文件名中加入对应类别前缀。对于各项功能内容，也可以按需要扩展更多分类，以便更清晰地组织信息。

#### 创建新的 Skills

- **读取** `sources/{project}/docs/` 中的源文档
- 如果存在，**读取** `instructions/{project}.md` 中的专项生成说明
- **充分理解** 文档内容
- 在 `skills/{project}/references/` 中 **创建** skill 文件
- **创建** `SKILL.md` 索引，列出所有 skills
- **创建** `GENERATION.md`，记录源仓库的 git SHA

#### 更新已生成的 Skills

1. 根据 `GENERATION.md` 中记录的 SHA，**检查** git diff：
   ```bash
   cd sources/{project}
   git diff {old-sha}..HEAD -- docs/
   ```
2. 根据变更内容 **更新** 受影响的 skill 文件
3. **更新** `SKILL.md`，写入工具/项目的新版本以及 skills 表格
4. **更新** `GENERATION.md`，记录新的 SHA

### 面向同步型 Skills（类型 2）

#### 初次同步

1. 将 `vendor/{project}/skills/{skill-name}/` 中指定的 skills **复制** 到 `skills/{output-name}/`
2. **创建** `SYNC.md`，记录 vendor 仓库的 git SHA

#### 更新已同步的 Skills

1. 根据 `SYNC.md` 中记录的 SHA，**检查** git diff：
   ```bash
   cd vendor/{project}
   git diff {old-sha}..HEAD -- skills/{skill-name}/
   ```
2. 将 `vendor/{project}/skills/{skill-name}/` 中变更的文件 **复制** 到 `skills/{output-name}/`
3. **更新** `SYNC.md`，记录新的 SHA

**注意：** 不要手动修改同步型 skills。相关变更应提交到上游 vendor 项目。

## 文件格式

### `SKILL.md`

索引文件，用于列出所有 skills 及其简要说明。名称应使用 `kebab-case`。

`version` 应填写最近一次同步的日期。

同时还要记录生成这些 skills 时所对应的工具/项目版本。

```markdown
---
name: {name}
description: {description}
metadata:
  author: Anthony Fu
  version: "2026.1.1"
  source: Generated from {source-url}, scripts located at https://github.com/antfu/skills
---

> 该 skill 基于 {project} v{version}，生成日期为 {date}。

// 对项目做简洁的概述、上下文说明或介绍

## 核心参考

| Topic | Description | Reference |
|-------|-------------|-----------|
| Markdown Syntax | 分隔页、frontmatter、备注、代码块 | [core-syntax](references/core-syntax.md) |
| Animations | v-click、v-clicks、motion、transitions | [core-animations](references/core-animations.md) |
| Headmatter | 整个 deck 范围内的配置选项 | [core-headmatter](references/core-headmatter.md) |

## 功能特性

### 功能 a

| Topic | Description | Reference |
|-------|-------------|-----------|
| Feature A Editor | 功能 a 的编辑能力说明 | [feature-a](references/feature-a-foo.md) |
| Feature A Preview | 功能 b 的说明 | [feature-b](references/feature-a-bar.md) |

### 功能 b

| Topic | Description | Reference |
|-------|-------------|-----------|
| Feature B | 功能 b 的说明 | [feature-b](references/feature-b-bar.md) |

// ...
```

### `GENERATION.md`

用于记录生成型 skills（类型 1）的跟踪元数据：

```markdown
# Generation Info

- **Source:** `sources/{project}`
- **Git SHA:** `abc123def456...`
- **Generated:** 2024-01-15
```

### `SYNC.md`

用于记录同步型 skills（类型 2）的跟踪元数据：

```markdown
# Sync Info

- **Source:** `vendor/{project}/skills/{skill-name}`
- **Git SHA:** `abc123def456...`
- **Synced:** 2024-01-15
```

### `references/*.md`

单个 skill 文件。每个文件只讲一个概念。

在文件末尾附上源文档的参考链接。

```markdown
---
name: {name}
description: {description}
---

# {Concept Name}

简要说明这个 skill 覆盖的内容。

## 用法

代码示例与实用模式。

## 关键点

- 重要细节 1
- 重要细节 2

<!--
Source references:
- {source-url}
- {source-url}
- {source-url}
-->
```

## 写作准则

生成 skills 时（仅限类型 1）：

1. **改写为适合 agent 使用的内容**：不要直接照搬文档原文，而要整理、归纳后提供给 LLM 使用
2. **强调实用性**：聚焦于使用模式和代码示例
3. **保持精炼**：去掉冗余内容，只保留关键信息
4. **每个文件只讲一个概念**：大型主题要拆分成多个 skill 文件
5. **包含代码**：始终提供可运行的代码示例
6. **说明原因**：不仅说明怎么用，也说明什么时候用、为什么这样用

## 支持的项目

规范的项目列表及其仓库 URL 请以 `meta.ts` 为准。

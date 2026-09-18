# rules

通用规则（`*.instructions.md`）的存放位置。**当前为占位**。

## 为什么先占位

规则没有跨客户端的通用位置，各家读各自的目录：

| 客户端 | 项目级规则位置 | 用户级规则位置 |
| --- | --- | --- |
| VS Code | `.github/instructions/`、`.claude/rules/` | `%APPDATA%\Code\User\prompts\`、`~/.copilot/instructions/` |
| Claude Code | `.claude/rules/` | `~/.claude/rules/` |

skill 有相对通用的约定（`.agents/skills/`），规则没有。因此规则只能按「复制到项目」分发，无法像 skill 那样有统一落点。

## 放什么

只放**跨项目通用**的规则，判别标准和 `skills/` 一致：换个新项目还用得上。

- ✅ 通用：命名约定、注释风格、提交信息格式
- ❌ 不通用：某个项目特有的目录结构、内部流程、团队约定

## 命名

`<主题>.instructions.md`，例如 `coding-conventions.instructions.md`、`git-workflow.instructions.md`。

## 复制到项目

| 目标 | 说明 |
| --- | --- |
| `.github/instructions/` | VS Code 项目级位置（默认） |
| `.claude/rules/` | 需兼容 Claude Code 时额外复制 |

VS Code 的规则文件支持 `applyTo` frontmatter 限定生效范围，例如只对 Lua 文件生效：

```markdown
---
applyTo: "**/*.lua"
---
```

## 待补充

（暂无内容）

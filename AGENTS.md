# AGENTS.md — 通用能力库

本仓库存放**与具体项目无关**的通用能力（skill 与规则），是这些能力的真相源。

**收录标准**：换一个新项目后，这份能力仍可直接复用 → 收进本仓库；绑定某个具体项目的 → 留在那个项目里。

**本文件同时是本仓库的「AI readme」**：在新项目里初始化通用能力时，按下面「初始化流程」执行。

## 目录结构

| 路径 | 说明 |
| --- | --- |
| `skills/<分类>/<name>/` | 通用 skill，遵循 [Agent Skills](https://agentskills.io) 标准，必须含 `SKILL.md` |
| `rules/` | 通用规则（`*.instructions.md`），当前为占位，见 `rules/README.md` |
| `templates/sync-manifest.json` | 项目侧同步清单模板 |

`<name>` 必须与 `SKILL.md` 的 `name` 字段一致，只能用小写字母、数字、连字符。

## 分类

| 分类 | 收录内容 |
| --- | --- |
| `tooling/` | 工具链与环境用法（shell、构建、调试器、编辑器…） |
| `conventions/` | 代码风格与工程约定 |
| `dependencies/` | 第三方依赖的使用方式 |

## 能力索引

复制前先读这张表，**按项目性质挑选，不要全量复制**。新增能力后必须同步更新本表。

| 能力 | 分类 | 何时使用 |
| --- | --- | --- |
| `powershell-safe-invocation` | tooling | 在 Windows 上编写或运行 PowerShell，涉及原生程序调用、带空格路径、引号转义、`pwsh`、`Start-Process`、文件操作、shell 排错 |

## 初始化流程（在新项目里）

1. **获取内容**：`git clone https://github.com/sumneko/skill.git <临时目录>`（仓库公开，也可直接按需读取远程文件），用完删除临时目录。
2. **挑选**：根据项目性质（语言、平台、协作范围）从「能力索引」里选。带强烈个人色彩的条目（如文档语言、个人命名偏好）在不匹配的项目里不要复制。
3. **复制**：按下方映射表复制到项目。
4. **登记**：把 `templates/sync-manifest.json` 复制到项目，按实际内容填写；并在项目的 `AGENTS.md`（没有就创建）里登记来源。

### 复制映射

| 本仓库 | 目标项目 | 说明 |
| --- | --- | --- |
| `skills/<分类>/<name>/` | `.agents/skills/<name>/` | **目录名必须等于 `name`，不能带分类层级** |
| `rules/*.instructions.md` | `.github/instructions/` | 见下方客户端位置差异 |

客户端生效位置不同，默认按 `.agents/skills/` 分发：

| 客户端 | skills 位置 | 规则位置 |
| --- | --- | --- |
| VS Code | `.agents/skills/`、`.github/skills/`、`.claude/skills/` | `.github/instructions/`、`.claude/rules/` |
| Claude Code | `.claude/skills/` | `.claude/rules/` |

若项目需要兼容 Claude Code，额外复制一份到 `.claude/skills/`。

## 同步

**正向**（本仓库 → 项目）：按需挑能力复制过去，更新项目 manifest 里的 `commit` 与 `syncedAt`。

**反向**（项目 → 本仓库）：不定期执行。

1. 先读项目侧 `sync-manifest.json`。
2. `locallyModified: false` 的条目：内容应当与记录的 `commit` 一致。若本仓库已有更新，说明项目侧落后，做前向更新即可。
3. `locallyModified: true` 的条目：**先 diff 两边**。只回传与项目无关的通用改进；夹带项目专属内容（项目路径、内部规范、团队偏好）的部分**不要**回传。
4. 回传完成后，把项目侧的 `locallyModified` 复位为 `false`，并更新 `commit` 与 `syncedAt`。

## 合并冲突处理

执行同步或合并时，遇到语义层面的歧义 —— 该不该回传、两边都改过且意图不明、某条内容是否算「通用」—— **先停下来询问用户**，不要自行判断或静默取舍。

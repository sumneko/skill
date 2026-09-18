# skill

跨项目复用的通用能力库 —— 存放不绑定任何具体项目的 skill 与规则。

## 这是什么

换个新项目还能直接用的能力，收进这里；绑定某个项目的，留在那个项目里。

这份仓库不参与任何运行时的自动加载：它的内容通过**复制到项目**来生效。这样项目自包含，协作者 clone 项目后无需任何本机配置即可使用。

## 目录结构

```
skill/
├── AGENTS.md                     AI readme：能力索引 + 初始化流程 + 同步约定
├── README.md                     本文件
├── skills/                       通用能力（Agent Skills 标准）
│   ├── tooling/                  工具链与环境用法
│   ├── conventions/              代码风格与工程约定
│   └── dependencies/             第三方依赖的使用方式
├── rules/                        通用规则（*.instructions.md），占位中
└── templates/
    └── sync-manifest.json        项目侧同步清单模板
```

## 怎么用

**在新项目里启用能力** —— 对 AI 说「用 sumneko/skill 初始化这个项目的通用能力」，它会读 `AGENTS.md` 并按流程挑选、复制、登记。

手动做法：

```powershell
git clone https://github.com/sumneko/skill.git $env:TEMP\skill
Copy-Item $env:TEMP\skill\skills\tooling\powershell-safe-invocation `
          -Destination .agents\skills\powershell-safe-invocation -Recurse
```

注意：复制到 `.agents/skills/` 时**要去掉分类层级**（`tooling/` 等），目录名必须等于 `SKILL.md` 里的 `name`。

**更新能力**：改本仓库 → 正向复制到目标项目 → 更新项目的 `.agents/sync-manifest.json`。

**收回项目里的改进**：读项目清单 `.agents/sync-manifest.json`，对 `locallyModified: true` 的条目先 diff，只回传通用改进，项目专属内容留在项目里。

## 收录标准

一条能力要进本仓库，需同时满足：

- 换项目仍可复用
- 不依赖特定项目的路径、规范或团队约定
- 内容自包含（skill 的 `SKILL.md` 能独立描述何时用、怎么用）

## 许可

各能力目录内的 `LICENSE` 为准；未单独声明时适用仓库根目录约定。

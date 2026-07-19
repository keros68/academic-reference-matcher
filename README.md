# Academic Reference Matcher

用于学术文献匹配的 Agent Skill：识别需要引用的论断，查找并核验候选文献，输出可复查的引用结果。

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-SKILL.md-green.svg)](SKILL.md)

## 能做什么

- 为论文、综述、基金和 rebuttal 补充参考文献。
- 检查现有引用是否真的支撑原文，替换弱引用、错引用或撤稿文献。
- 输出 APA、GB/T 7714、Vancouver、IEEE、BibTeX、RIS 等格式。
- 区分“找到候选文献”和“文献确实支撑论断”，并说明证据来源和不确定性。

它不自带数据库账号或 PDF 下载器，也不会绕过付费墙、验证码和机构权限。检索质量取决于宿主 Agent 能访问的搜索工具、数据库和用户提供的材料。

## 安装

### 让 Agent 安装

把下面这段话发给 Codex、Claude Code 或其他支持 Skills 的 Agent：

```text
请完整安装这个 Skill：
https://github.com/keros68/academic-reference-matcher

请克隆或下载整个仓库到个人 skills 目录下的 academic-reference-matcher 文件夹，不要只保存 README.md。安装后确认该文件夹中同时存在 SKILL.md 和 references/，然后告诉我实际安装路径。
```

如果安装目录里只有 `README.md`，说明 Agent 只读取了 GitHub 首页，并未完成安装。让它替换这个不完整目录并重新克隆整个仓库即可。

### 手动安装

Windows PowerShell：

```powershell
# Codex
git clone https://github.com/keros68/academic-reference-matcher.git "$env:USERPROFILE\.codex\skills\academic-reference-matcher"

# Claude Code
git clone https://github.com/keros68/academic-reference-matcher.git "$env:USERPROFILE\.claude\skills\academic-reference-matcher"
```

macOS / Linux：

```bash
# Codex
git clone https://github.com/keros68/academic-reference-matcher.git \
  ~/.codex/skills/academic-reference-matcher

# Claude Code
git clone https://github.com/keros68/academic-reference-matcher.git \
  ~/.claude/skills/academic-reference-matcher
```

安装完成后，重启 Agent 或新开一个会话。完整安装至少应包含：

```text
academic-reference-matcher/
├── SKILL.md
├── references/
└── agents/openai.yaml
```

## 使用

在请求中直接点名 `$academic-reference-matcher`，并提供需要处理的文字或参考文献：

```text
使用 $academic-reference-matcher 为下面这段话补充参考文献，输出 claim-reference 对照表和 GB/T 7714 格式的参考文献。
```

也可以要求它核验或替换现有引用：

```text
使用 $academic-reference-matcher 检查这些引用是否支撑对应论断。保留可靠引用，替换不匹配或已撤稿的文献，并说明判断依据。
```

常见输出包括带引用的修改稿、claim-reference 对照表、引用核验说明，以及 Markdown、BibTeX 或 RIS 文件。高风险投稿、系统综述和最终格式仍建议人工复核。

## 更新

Windows PowerShell：

```powershell
git -C "$env:USERPROFILE\.codex\skills\academic-reference-matcher" pull --ff-only
```

macOS / Linux：

```bash
git -C ~/.codex/skills/academic-reference-matcher pull --ff-only
```

Claude Code 用户把命令中的 `.codex` 改为 `.claude`。

如果是由 Agent 安装，也可以直接发送：

```text
请更新已安装的 $academic-reference-matcher。拉取 GitHub 最新版本，并确认 SKILL.md 和 references/ 均已更新；不要只下载 README.md。
```

更新后建议重启 Agent 或新开会话。

## 项目文件

- [`SKILL.md`](SKILL.md)：核心工作流程和触发规则。
- [`references/`](references/)：检索规划、来源选择、证据分级、输出格式和审计规则。
- [`examples/`](examples/)：常见请求示例。
- [`agents/openai.yaml`](agents/openai.yaml)：兼容运行时的界面元数据。

## English

Academic Reference Matcher is an agent skill for finding, verifying, replacing, and formatting scholarly references.

Clone or download the **entire repository** into your agent's skills directory. An installation containing only `README.md` is incomplete; verify that both `SKILL.md` and `references/` are present.

```text
Use $academic-reference-matcher to find and verify scholarly references for this paragraph, then create a claim-reference table.
```

## License

MIT. See [LICENSE](LICENSE) and [NOTICE.md](NOTICE.md). Redistributed or modified copies must preserve the copyright and license notices and must not imply endorsement by the original author.

---

同系列 Agent Skills：[sci-select](https://github.com/keros68/sci-select)（选刊与投稿前审查） · [abstract-fig](https://github.com/keros68/abstract-fig)（图形摘要） · [cugb-doctoral-thesis-format](https://github.com/keros68/cugb-doctoral-thesis-format)（学位论文格式） · [ai-cross](https://github.com/keros68/ai-cross)（多模型交叉验证）

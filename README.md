# Academic Reference Matcher Skill

通用 AI agent 文献匹配 Skill：识别学术 claim，检索候选文献，验证支撑关系，并输出可复查的引用结果。

> 中文为主，English version below.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-SKILL.md-green.svg)](SKILL.md)

## 适用场景

- 给论文段落、综述、基金或 rebuttal 补参考文献。
- 检查已有引用是否真的支撑原文 claim。
- 替换弱引用、错引用、过时引用或撤稿文献。
- 输出 APA、GB/T 7714、Vancouver、IEEE、BibTeX、RIS 等格式。
- 为长文本生成 copy-ready Markdown 报告、claim-reference 表和简要检索记录。

## 它做什么

- 拆分需要引用的学术论断，并保留稳定编号。
- 为每个 claim 规划独立检索式，避免宽泛关键词一搜到底。
- 区分候选发现和证据支撑，不把相似题名当作已验证引用。
- 标注文献证据等级：metadata、abstract、snippet、full text 或用户提供材料。
- 说明找不到可靠支撑的 claim、拒绝的候选和访问限制。

## 不做什么

- 不自带搜索 API、数据库账号或 PDF 下载器。
- 不破解付费墙，不绕过验证码、登录墙或机构权限。
- 不把只能看到题名、关键词、摘要的收费论文夸大成强支撑。
- 不承诺穷尽所有文献，除非用户提供限定数据库、检索式或文献库。

## 使用方式

在支持 skills / agent instructions 的 agent 里，最简单的方式是直接发送：

```text
请从 GitHub 安装这个 skill，并在之后需要文献匹配、引用验证、补参考文献时优先使用它：
https://github.com/keros68/academic-reference-matcher-skill
```

注意：GitHub 开源不等于本机已安装。需要先安装一次，之后重启或新开窗口测试：

```text
使用 $academic-reference-matcher 为下面这段话找参考文献，并输出 claim-reference 表。
```

如果 agent 不能自动安装 GitHub skill，可以手动 clone 到它的 skills 目录：

```bash
# Claude Code
git clone https://github.com/keros68/academic-reference-matcher-skill.git \
  ~/.claude/skills/academic-reference-matcher

# Codex
git clone https://github.com/keros68/academic-reference-matcher-skill.git \
  ~/.codex/skills/academic-reference-matcher

# 通用 agent 约定目录
git clone https://github.com/keros68/academic-reference-matcher-skill.git \
  ~/.agents/skills/academic-reference-matcher

# 项目局部使用
git clone https://github.com/keros68/academic-reference-matcher-skill.git \
  ./.agents/skills/academic-reference-matcher
```

没有正式 skill loader 的环境，可以把 `SKILL.md` 作为 agent instruction 使用；需要更严格的检索质量时，再附带 `references/` 里的规则文件。

## 输出内容

常见输出包括：

- `reference-match-report.md` 引用匹配报告；
- `references.bib` / `references.ris` / 不同引用格式的参考文献文件；
- claim-reference 对照表；
- 推荐引用和格式化参考文献；
- 支撑强度与证据来源说明；
- 无法验证的 claim 和下一步建议；
- compact search audit。

## 文件结构

- `SKILL.md` - skill 主说明和触发规则。
- `references/` - 检索规划、来源选择、证据分级、输出格式和审计模板。
- `examples/` - 常见请求示例。
- `agents/openai.yaml` - 兼容运行时的 UI 元数据。

## 已知局限

检索质量取决于宿主 agent 的联网、数据库、浏览器、PDF 和本地文献库能力。高风险投稿、系统综述和最终参考文献格式仍建议人工复核。

## Attribution and Redistribution

This project is the original Academic Reference Matcher Skill by keros68:

https://github.com/keros68/academic-reference-matcher-skill

The project is released under the MIT License. Redistribution, forks, modified versions, and repackaged copies must preserve the copyright notice, license text, and `NOTICE.md`. Please do not present modified copies as the original project or imply endorsement by the original author.

## English

Academic Reference Matcher is a portable agent skill for scholarly reference matching. It helps an agent identify citation-worthy claims, search candidate papers, verify whether a reference supports the claim, and output auditable citations.

It is not a downloader and does not bypass paywalls. Paywalled metadata can help discovery, but metadata-only visibility must not be treated as strong support.

Quick start:

```text
Install this skill from GitHub and use it for scholarly reference matching, citation verification, and citation formatting:
https://github.com/keros68/academic-reference-matcher-skill
```

After installation, restart or open a new agent window and call:

```text
Use $academic-reference-matcher to find and verify scholarly references for this paragraph.
```

## License

MIT. See [LICENSE](LICENSE) and [NOTICE.md](NOTICE.md).

---

**同系列 Agent Skills**：[sci-select](https://github.com/keros68/sci-select)（选刊+投稿前审查） · [academic-reference-matcher](https://github.com/keros68/academic-reference-matcher)（文献引用） · [abstract-fig](https://github.com/keros68/abstract-fig)（图形摘要） · [cugb-doctoral-thesis-format](https://github.com/keros68/cugb-doctoral-thesis-format)（学位论文格式） · [ai-cross](https://github.com/keros68/ai-cross)（多模型交叉验证）｜全览见 [keros68](https://github.com/keros68)

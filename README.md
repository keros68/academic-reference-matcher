# academic-reference-matcher

通用 AI agent 文献匹配 skill：识别学术 claim，检索候选文献，验证支撑关系，输出可复查的引用结果。

> 中文为主，English version below.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-SKILL.md-green.svg)](SKILL.md)

## 使用场景

- 给论文段落、综述、基金申请或 rebuttal 补参考文献。
- 检查已有引用是否真的支撑原文 claim，替换弱引用、错引用、过时引用和撤稿文献。
- 输出 APA、GB/T 7714、Vancouver、IEEE、BibTeX、RIS 等格式，附 claim-reference 对照表和检索记录。

## 功能

拆分需要引用的学术论断并保留稳定编号，为每个 claim 规划独立检索式，不用宽泛关键词一搜到底。候选发现和证据支撑分开：相似题名不算已验证引用，每条引用标注证据等级（metadata、abstract、snippet、full text 或用户提供材料）。找不到可靠支撑的 claim、被拒绝的候选和访问限制都如实说明，不硬凑。

常见输出：引用匹配报告 `reference-match-report.md`、`references.bib` / `references.ris`、claim-reference 对照表、支撑强度说明和简要检索记录。

## 边界

- 不自带搜索 API、数据库账号或 PDF 下载器；不破解付费墙，不绕过验证码、登录墙或机构权限。
- 只能看到题名、关键词、摘要的收费论文，不会被夸大成强支撑。
- 不承诺穷尽所有文献，除非用户限定数据库、检索式或文献库。
- 检索质量取决于宿主 agent 的联网、数据库和本地文献库能力。高风险投稿、系统综述和最终参考文献格式仍建议人工复核。

## 快速开始

在支持 skills / agent instructions 的 agent 里发送：

```text
请从 GitHub 安装这个 skill，并在之后需要文献匹配、引用验证、补参考文献时优先使用它：
https://github.com/keros68/academic-reference-matcher
```

GitHub 开源不等于本机已安装，需要先装一次。装完重启或新开窗口：

```text
使用 $academic-reference-matcher 为下面这段话找参考文献，并输出 claim-reference 表。
```

不能自动安装时，手动 clone 到 skills 目录：

```bash
# Claude Code
git clone https://github.com/keros68/academic-reference-matcher.git \
  ~/.claude/skills/academic-reference-matcher

# Codex
git clone https://github.com/keros68/academic-reference-matcher.git \
  ~/.codex/skills/academic-reference-matcher
```

没有正式 skill loader 的环境，把 `SKILL.md` 作为 agent instruction 使用；需要更严格的检索质量时，再附带 `references/` 里的规则文件。

## 文件结构

- `SKILL.md` - skill 主说明和触发规则。
- `references/` - 检索规划、来源选择、证据分级、输出格式和审计模板。
- `examples/` - 常见请求示例。
- `agents/openai.yaml` - 兼容运行时的 UI 元数据。

## Attribution and Redistribution

This project is the original academic-reference-matcher skill by keros68:

https://github.com/keros68/academic-reference-matcher

The project is released under the MIT License. Redistribution, forks, modified versions, and repackaged copies must preserve the copyright notice, license text, and `NOTICE.md`. Please do not present modified copies as the original project or imply endorsement by the original author.

## English

academic-reference-matcher is a portable agent skill for scholarly reference matching. It identifies citation-worthy claims, searches candidate papers, verifies whether each reference actually supports the claim, and outputs auditable citations with evidence levels. It is not a downloader and does not bypass paywalls; metadata-only visibility is never treated as strong support.

Quick start:

```text
Install this skill from GitHub and use it for scholarly reference matching, citation verification, and citation formatting:
https://github.com/keros68/academic-reference-matcher
```

After installation, restart or open a new agent window and call:

```text
Use $academic-reference-matcher to find and verify scholarly references for this paragraph.
```

## License

MIT. See [LICENSE](LICENSE) and [NOTICE.md](NOTICE.md).

---

**同系列 Agent Skills**：[sci-select](https://github.com/keros68/sci-select)（选刊+投稿前审查） · [abstract-fig](https://github.com/keros68/abstract-fig)（图形摘要） · [cugb-doctoral-thesis-format](https://github.com/keros68/cugb-doctoral-thesis-format)（学位论文格式） · [ai-cross](https://github.com/keros68/ai-cross)（多模型交叉验证）｜全览见 [keros68](https://github.com/keros68)

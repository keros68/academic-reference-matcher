# academic-reference-matcher

academic-reference-matcher 是一个 AI agent skill，为学术文本查找并核实参考文献：拆出需要引用的论断，检索候选文献，判断文献是否真的支撑该论断，输出带证据等级的引用结果。适用于论文段落、综述、基金申请、rebuttal 和已有的参考文献列表。

> 中文为主，English version below.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-SKILL.md-green.svg)](SKILL.md)

## 安装与调用

skill 只有 Markdown 说明，不含可执行代码，检索用的是宿主 agent 已有的联网、搜索、数据库、浏览器或文献库工具。clone 到 agent 的 skills 目录即可：

```bash
# Claude Code
git clone https://github.com/keros68/academic-reference-matcher.git \
  ~/.claude/skills/academic-reference-matcher

# Codex
git clone https://github.com/keros68/academic-reference-matcher.git \
  ~/.codex/skills/academic-reference-matcher
```

也可以直接让 agent 从 GitHub 安装：

```text
请从 GitHub 安装这个 skill，并在之后需要文献匹配、引用验证、补参考文献时优先使用它：
https://github.com/keros68/academic-reference-matcher
```

装完新开一个会话，然后调用：

```text
使用 $academic-reference-matcher 为下面这段话找参考文献，并输出 claim-reference 表。
```

没有 skill loader 的环境，把 `SKILL.md` 作为 agent instruction 使用；需要更严格的检索质量时，再附带 `references/` 里的规则文件。更多写法见 `examples/example-requests.md`。

## 功能

**五种任务模式**：Add 为未引用的论断找文献；Verify 检查已有引用是否支撑它所附的论断；Replace 替换弱引用、错引用、过时引用、撤稿文献和无法获取的文献；Format 只做格式转换，不重做相关性匹配；Extract 只挑出需要引用的论断，暂不检索。

**四档检索深度**：Quick 处理 1-3 条论断，给 3-5 篇强相关文献；Standard 处理一个段落或短小节，出 claim 表，尽量用两个以上学术来源；Deep 用于长小节、综述背景或有争议的主题，启用 segment ID、来源路由和检索审计；Audit 用于系统综述准备和高风险稿件，要求可复现的检索日志并先声明范围与限制。用户提到"综述""全面""PRISMA""meta-analysis"时走 Deep 或 Audit。

**检索规划**：多条论断按 `S001`、`S002` 编号，编号在全流程保持稳定。先标注论断类型（实验结果、因果机制、比较趋势、方法与数据集、定义、标准法规、史实），由类型决定该查哪类来源；每条重要论断构造 2-4 组检索式（精确短语、概念块、研究类型、已知标识符）。中文文本同时检索中文原词和对应的英文术语、缩写、拉丁名。检索结果要和原始论断对齐，人群、材料、模型、地域、时段对不上就重写检索式。

**证据分级**：候选发现和证据支撑分开记账，题名相似不算已验证。证据等级分 Discovery-only、Abstract-supported、Snippet-supported、Fulltext-supported、Bibliographic-only 五档，对应 High / Medium / Low 置信度；全文级证据才够得上 High，纯书目性论断（作者、年份、出处、DOI）除外。每条采纳的文献要有题名、作者、年份、出处、DOI 或稳定链接，外加一句话说明它支撑哪句话；查不到就进 Could not verify，不补一条看起来合理的文献。

**输出**：小请求直接在对话里给带引用的正文和参考文献列表，未指定格式时默认作者-年份夹注加一份精简文献列表。较大的任务写成文件：`reference-match-report.md` 含引用后的正文、claim-reference 对照表、参考文献、注意事项和检索审计；按需另出 `references-apa.md`、`references-gbt7714.md`、`references-vancouver.md`、`references-ieee.md`、`references.bib`、`references.ris`。Could not verify 小节写明查了什么、候选为什么被拒、下一步建议。

## 限制

- 不含搜索引擎、付费数据库权限和引用解析器，检索质量取决于宿主 agent 可用的工具和用户提供的文献库。
- 不绕过付费墙、验证码、登录墙，也不使用未授权的 cookie 或来源。付费墙文献可以留作候选，但只看得到元数据时不能当强支撑。
- 除非用户给出限定的语料范围或可复现的数据库检索式，否则不宣称覆盖完整。
- 宿主 agent 完全没有检索或浏览工具时，skill 会要求用户提供文献列表、PDF、Zotero/Mendeley 导出或数据库检索结果，只在这些材料里核实。
- 期刊特定的最终格式和高风险稿件仍需人工复核。

## 文件结构

- `SKILL.md` - skill 主说明和触发规则。
- `references/` - 检索规划、来源选择、来源路由、付费墙处理、证据评分、输出格式和审计模板。
- `examples/` - 常见请求示例。
- `agents/openai.yaml` - 显示名、一句话简介和默认提示词。

## Attribution and Redistribution

This project is the original academic-reference-matcher skill by keros68:

https://github.com/keros68/academic-reference-matcher

The project is released under the MIT License. Redistribution, forks, modified versions, and repackaged copies must preserve the copyright notice, license text, and `NOTICE.md`. Please do not present modified copies as the original project or imply endorsement by the original author.

## English

academic-reference-matcher is an AI agent skill that finds and verifies scholarly references for academic text. It extracts citation-worthy claims, searches candidate papers, judges whether each candidate actually supports the claim, and reports every citation with an evidence tier.

The skill ships Markdown instructions only. Searching runs on whatever web, database, browser, or library tools the host agent already has. Install it by cloning into the agent's skills directory (`~/.claude/skills/` for Claude Code, `~/.codex/skills/` for Codex), start a new session, then call:

```text
Use $academic-reference-matcher to find and verify scholarly references for this paragraph.
```

Five task modes (Add, Verify, Replace, Format, Extract) and four search depths (Quick, Standard, Deep, Audit) are described in `SKILL.md`. Discovery is kept separate from support: title and metadata matches count as `Discovery-only`, and only full-text evidence is eligible for High confidence. Claims without a reliable match go into a "Could not verify" section rather than getting a plausible-looking citation. The skill does not bypass paywalls, CAPTCHAs, or login walls.

## License

MIT. See [LICENSE](LICENSE) and [NOTICE.md](NOTICE.md).

---

**同系列 Agent Skills**：[sci-select](https://github.com/keros68/sci-select)（选刊+投稿前审查） · [abstract-fig](https://github.com/keros68/abstract-fig)（图形摘要） · [cugb-doctoral-thesis-format](https://github.com/keros68/cugb-doctoral-thesis-format)（学位论文格式） · [ai-cross](https://github.com/keros68/ai-cross)（多模型交叉验证）｜全览见 [keros68](https://github.com/keros68)

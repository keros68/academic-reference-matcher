# academic-reference-matcher

academic-reference-matcher 是一个 AI agent skill，为学术文本查找并核实参考文献：拆出需要引用的论断，检索候选文献，判断文献是否真的支撑该论断，输出带证据等级的引用结果。适用于论文段落、综述、基金申请、rebuttal 和已有的参考文献列表。

> 中文为主，English below.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-SKILL.md-green.svg)](SKILL.md)

## 功能

**五种任务模式**：Add 补引用，Verify 查已有引用是否支撑对应论断，Replace 换掉弱引用、错引用、过时引用和撤稿文献，Format 只转格式，Extract 只挑出需要引用的论断。

**四档检索深度**：Quick 处理几条论断，Standard 处理一个段落并出 claim 表，Deep 用于长小节和有争议的主题，Audit 用于系统综述准备和高风险稿件，要求可复现的检索日志。

**证据分级**：候选发现和证据支撑分开记账，题名相似不算已验证。每条引用标注证据基础（元数据、摘要、摘录还是全文），只看得到元数据的文献不能当强支撑。查不到就进 Could not verify 小节，不补一条看起来合理的文献。

**输出**：小请求直接在对话里给带引用的正文和文献列表。较大的任务可写成 `reference-match-report.md`，含引用后的正文、claim-reference 对照表、参考文献和检索审计；APA、GB/T 7714、Vancouver、IEEE 格式的文献列表和 BibTeX/RIS 文件按需另出。

## 安装与调用

skill 只有 Markdown 说明，clone 到 agent 的 skills 目录即可，也可以把仓库地址发给 agent 让它自己装：

```bash
# Claude Code 用 ~/.claude/skills/，Codex 用 ~/.codex/skills/
git clone https://github.com/keros68/academic-reference-matcher.git \
  ~/.claude/skills/academic-reference-matcher
```

装完新开会话后调用：

```text
使用 $academic-reference-matcher 为下面这段话找参考文献，并输出 claim-reference 表。
```

没有 skill loader 的环境，把 `SKILL.md` 当作 agent instruction 使用，需要更严格的检索质量时再附带 `references/` 里的规则文件。更多写法见 `examples/example-requests.md`。

## 限制

- 不含搜索引擎、付费数据库权限和引用解析器，检索质量取决于宿主 agent 可用的工具和用户提供的文献库。
- 不绕过付费墙、验证码、登录墙，也不使用未授权的 cookie 或来源；付费墙文献可以留作候选。
- 除非用户给出限定的语料范围或可复现的数据库检索式，否则不宣称覆盖完整。
- 宿主 agent 没有检索或浏览工具时，skill 会要求用户提供文献列表、PDF、Zotero 导出或数据库检索结果，只在这些材料里核实。
- 期刊特定的最终格式和高风险稿件仍需人工复核。

## 文件结构

- `SKILL.md` - skill 主说明和触发规则。
- `references/` - 检索规划、来源路由、付费墙处理、证据评分、输出格式和审计模板。
- `examples/` - 常见请求示例。
- `agents/openai.yaml` - 显示名、一句话简介和默认提示词。

## Attribution and Redistribution

This repository is the original academic-reference-matcher skill by keros68, released under the MIT License. Forks, modified versions, and repackaged copies must preserve the copyright notice, the license text, and `NOTICE.md`, and must not present themselves as the original project or imply endorsement by the original author.

## English

academic-reference-matcher is an AI agent skill that finds and verifies scholarly references for academic text: it extracts citation-worthy claims, searches candidate papers, judges whether each candidate actually supports the claim, and reports each citation with its evidence basis. A metadata-only match is never treated as strong support, and a claim with no reliable match goes into a "Could not verify" section rather than getting a plausible-looking citation.

Clone it into the agent's skills directory (`~/.claude/skills/` or `~/.codex/skills/`) and start a new session:

```text
Use $academic-reference-matcher to find and verify scholarly references for this paragraph.
```

Task modes, search depths, output formats, and limits are documented in `SKILL.md`. The skill does not bypass paywalls, CAPTCHAs, or login walls.

## License

MIT. See [LICENSE](LICENSE) and [NOTICE.md](NOTICE.md).

---

**同系列 Agent Skills**：[sci-select](https://github.com/keros68/sci-select)（选刊+投稿前审查） · [abstract-fig](https://github.com/keros68/abstract-fig)（图形摘要） · [cugb-doctoral-thesis-format](https://github.com/keros68/cugb-doctoral-thesis-format)（学位论文格式） · [ai-cross](https://github.com/keros68/ai-cross)（多模型交叉验证）｜全览见 [keros68](https://github.com/keros68)

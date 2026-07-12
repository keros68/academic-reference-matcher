---
name: academic-reference-matcher
description: Use when finding, adding, verifying, replacing, or formatting scholarly references for academic claims, paragraphs, manuscripts, literature reviews, rebuttals, grant text, or citation lists; use when checking whether cited papers support a claim, identifying claims that need citations, creating copy-ready citation reports/files, or producing APA, GB/T, Vancouver, IEEE, BibTeX, RIS, or DOI-linked references. Also use for Chinese requests such as 补参考文献、找文献、引用验证、文献匹配、加引用、GB/T 7714 格式引用. Not for general fact-checking without a citation need, or legal/journalistic citation formatting.
---

# Academic Reference Matcher

## Overview

Find and verify scholarly references for user-provided academic text. Prefer the agent's built-in web, search, database, browser, citation, or library tools; this skill supplies the workflow and quality bar rather than a required external API.

## Core Rules

- Never invent citations, DOIs, author names, journal names, or publication years.
- Separate "candidate reference found" from "reference supports the claim".
- Use primary literature, systematic reviews, standards, datasets, or official documentation before blogs and secondary summaries.
- Preserve the user's requested citation style and language. If none is requested, default to author-year inline citations plus a compact reference list.
- Prefer copy-ready files for Standard+ tasks when the runtime can write files, especially if the user asks for copying, exporting, or multiple citation styles.
- If web/database access is unavailable, ask for a bibliography, search export, DOI list, PDF set, Zotero library, or pasted search results.
- Disclose access limits, weak matches, and unverified claims instead of filling gaps with plausible-looking references.

## Task Modes

Choose the mode from the user's request:

- Add: find citations for uncited claims.
- Verify: check whether existing citations support the claims they are attached to.
- Replace: find stronger or more accurate references for weak, wrong, outdated, retracted, or inaccessible citations.
- Format: convert known references to the requested style without doing new relevance matching.
- Extract: identify citation-worthy claims without searching yet.

## Search Depth

Choose the lightest depth that satisfies the user's intent:

- Quick: 1-3 claims, 3-5 strong references, fast support check.
- Standard: paragraph or short section, claim table, two or more scholarly sources when possible.
- Deep: long section, review background, or disputed topic; use segment IDs, source routing, and search audit.
- Audit: systematic-review preparation, PRISMA-like transparency, or high-stakes manuscript work; require a reproducible search log and explicit limits.

If the user asks for "thorough", "systematic", "综述", "全面", "PRISMA", "meta-analysis", or "Cochrane", use Deep or Audit. For Audit, state the scope and limits before doing the work.

## Workflow

1. Scope the task.
   - Identify the field, date sensitivity, citation style, language, task mode, and search depth.
   - For long documents, process by section and keep citation numbering stable.

2. Extract citation-worthy claims.
   - Mark empirical, causal, comparative, quantitative, methodological, historical, or definitional claims.
   - Assign stable segment IDs (`S001`, `S002`, ...) for multi-claim or long-text tasks.
   - Do not cite generic transitions, obvious background, or the user's own stated contribution unless requested.
   - Load `references/query-planning.md` when turning claims into search queries.

3. Detect host search capability and pick the route.
   - No search or browsing tool at all -> ask the user for a bibliography, PDFs, or database export (see Core Rules), then verify against those materials only.
   - General web search only (no scholarly API) -> use domain-limited queries (site:pubmed.ncbi.nlm.nih.gov, site:arxiv.org, site:doi.org) from references/search-sources.md.
   - Scholarly database or plugin available (OpenAlex, PubMed, Crossref, arXiv tools) -> follow the Default Source Order in references/search-sources.md and the domain table in references/source-routing.md.
   - Browser tool only (no query API) -> navigate directly to publisher, DOI, or PubMed pages.

4. Search broadly, then narrowly.
   - Start with exact phrases and key technical terms from the claim.
   - Search title/abstract/DOI sources before general web search when available.
   - Load `references/search-sources.md` when choosing sources or building queries.
   - Load `references/source-routing.md` for domain-specific routing or Deep/Audit work.
   - Load `references/paywall-aware-access.md` when relevant papers are paywalled or only metadata is visible.

5. Verify relevance.
   - Read enough of the title, abstract, metadata, snippets, and available full text to judge support.
   - Score each candidate using `references/verification-rubric.md` when the task has multiple candidates or high accuracy requirements.
   - Prefer papers that directly support the claim over papers that merely share keywords.
   - Require title, year, stable URL or DOI, and a support rationale before treating a match as usable.
   - Treat title, keywords, and metadata as discovery signals, not strong support, unless the claim is purely bibliographic.

6. Format and insert citations.
   - Use the user's requested style. Load `references/output-formats.md` for output contracts and style notes.
   - Keep citations adjacent to the claims they support.
   - Include uncertainty notes for weak matches instead of silently overstating confidence.
   - Create a copy-ready Markdown report when the user asks for a file, export, citation package, or easy copy/paste output.
   - For Standard+ tasks, include a compact search audit. Load `references/search-audit.md`.
   - Before finishing, confirm every claim ID from step 2 appears either in the claim-reference table or in the Could Not Verify section - no claim may be silently dropped.

## Search Strategy

Use at least two independent scholarly sources for important claims when possible. Good default order:

1. User-provided bibliography, PDFs, Zotero/Mendeley export, or project library.
2. OpenAlex, Crossref, Semantic Scholar, PubMed/Europe PMC, arXiv, IEEE/ACM/ScienceDirect/Springer/Nature pages when accessible.
3. General web search only to locate publisher pages, DOI records, preprints, or official reports.

For each selected reference, capture enough provenance to let the user audit it: title, authors, year, venue, DOI or stable URL, and a one-sentence support rationale.

## Evidence Minimums

For High or Medium confidence matches, include:

- bibliographic identity: title, authors, year, venue;
- stable locator: DOI, PMID, arXiv ID, accession, standard number, or stable URL;
- evidence basis: abstract, full text, metadata, snippet, user-provided PDF, or official record;
- support rationale: one sentence tying the reference to the exact claim.

If any of these are missing, lower confidence or add a note.

Evidence tiers:

- Discovery-only: title, keywords, index metadata, citation counts, or related-work lists suggest relevance but do not establish support.
- Abstract-supported: the abstract directly supports the claim; usually Medium unless the claim is broad and low-risk.
- Snippet-supported: an official publisher or database snippet directly addresses the claim; usually Low-Medium confidence.
- Fulltext-supported: full text, user-provided PDF, official guideline, dataset record, or publisher page directly supports the claim; eligible for High.
- Bibliographic-only: metadata is enough only for claims about existence, authorship, year, venue, DOI, or publication status.

## Output

For small requests, answer in prose with inserted citations and a reference list.

For Standard+ tasks, or whenever the user asks for "文件", "复制", "导出", "copy", "paste", "BibTeX", "RIS", "GB/T", or multiple citation styles, create file outputs when possible:

- `reference-match-report.md`: copy-ready main report with cited revision, claim-reference table, references, caveats, and search audit.
- `references-apa.md`, `references-gbt7714.md`, `references-vancouver.md`, or `references-ieee.md`: style-specific reference lists when requested.
- `references.bib` or `references.ris`: machine-readable entries when requested and enough metadata is verified.

In chat, return a short summary plus file paths. Do not duplicate the full report in chat unless files cannot be created.

For multi-claim matching, use this compact table:

| Claim | Mode | Best reference | Support | Evidence basis | Confidence | Notes |
|---|---|---|---|---|---|---|

Use confidence labels:

- High: directly supports the claim with matching population, method, mechanism, or result.
- Medium: supports the broader point but differs in scope, method, or context.
- Low: related background only; do not present as strong support.

End with a "Could not verify" section for claims with no reliable match.

For Deep or Audit tasks, include segment IDs in the table and a search audit summary.

## Limits

- This skill has no built-in search engine, paid database access, or citation parser.
- Search quality depends on the host agent's available tools and the user's provided corpus.
- Paywalls, missing abstracts, incomplete metadata, rate limits, and inaccessible PDFs can weaken verification.
- Paywalled records can still be useful for discovery and candidate ranking, but metadata-only visibility limits support confidence.
- Literature coverage is not exhaustive unless the user provides a bounded corpus or reproducible database search strategy.
- Final journal-specific formatting and high-stakes manuscript checks may still need human review.

## Additional Guardrails

- Treat citation count only as a popularity signal; judge relevance by content match.
- Cite a review for a specific experimental result only when the review clearly reports it and the user accepts secondary sources.
- Use a paper only when its content, not just its abstract's terms, matches the claim.
- Use stable scholarly records such as DOI, PubMed, arXiv, or publisher pages instead of unofficial Google Scholar scraping or CAPTCHA workarounds.

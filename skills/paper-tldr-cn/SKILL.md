---
name: paper-tldr-cn
description: Generate detailed Chinese TLDR-style guides for academic papers from PDFs, arXiv/OpenReview/conference URLs, paper lists, or selected/filter results. Use when the user asks Codex to read, summarize, translate, explain, compare, screen, or report what one paper or many papers are about, especially when the expected output includes a Chinese abstract translation, TLDR/TLDR translation, method-focused interpretation, or a six-dimension guide covering research motivation, problem, phenomenon analysis, method, data/experiments, and contributions.
---

# 中文论文 TLDR 导读

## Core Workflow

Use this skill to produce grounded, method-aware Chinese paper summaries. Prefer authoritative sources and do not infer unsupported details.

1. Collect paper metadata from the source the user provided: title, authors, venue/page link, PDF link, abstract, official TLDR if present, primary area/track if present, code/data links if present.
2. If the user provides only a URL, open/browse the URL and follow paper/PDF links as needed. For OpenReview, arXiv, ACL Anthology, conference pages, or PDFs, use the page/PDF as the primary source.
3. If summarizing many papers, first build a paper list, then process each paper independently. Do not let one paper's claims bleed into another.
4. Translate the abstract into Chinese before the guide. Preserve technical terms, method names, datasets, metrics, and abbreviations. On first mention, use `中文（English）` when a term benefits from bilingual clarity.
5. Write a TLDR. If the source provides an official TLDR, quote or paraphrase it as `TLDR`; if not, write `TLDR（自拟）` and keep it faithful to the title/abstract/full text.
6. Write `TLDR 译文` in Chinese.
7. Write a six-dimension Chinese guide. Make `主要方法` the most concrete part: name the method, describe the technical mechanism, training/inference pipeline, objective/loss, model components, assumptions, and why it addresses the problem when those details are available.

If only an abstract is available, say so in the relevant dimensions with phrases like `摘要未给出具体数据集/指标` rather than inventing details. If the user asks for deep method reading and a PDF is available, inspect the introduction, method, experiment, and conclusion sections before writing.

## Output Template

For each paper, use this structure unless the user asks for a different format:

```markdown
## <Original Paper Title>

- OpenReview: <url, if available>
- arXiv: <url, if available>
- PDF: <url, if available>
- Code: <url, if available>
- 作者: <authors, if available>
- Venue/Track: <venue or track, if available>
- Primary Area: <area, if available>
- 题名译意: <natural Chinese title translation>
- TLDR: <official TLDR, or "TLDR（自拟）: ...">
- TLDR 译文: <Chinese translation>

### 摘要译文

<faithful Chinese translation of the abstract, within applicable copyright/source limits>

### 六维导读

- **研究动机：** <why this question matters; prior context; what gap motivates the work>
- **解决问题：** <the precise research problem/task; inputs/outputs/constraints; what failure or limitation is being solved>
- **现象分析：** <what phenomenon, failure mode, empirical pattern, or theoretical mechanism the paper identifies; distinguish claims from proven facts>
- **主要方法：** <method name and detailed mechanism; components; optimization/training/inference flow; how it fixes the stated problem>
- **数据与实验：** <datasets, benchmarks, baselines, metrics, settings, ablations, and headline results if available; state when absent>
- **主要贡献：** <what is new and why it matters: problem framing, theory, algorithm, system, dataset/benchmark, empirical finding>
```

For long paper lists, repeat the per-paper block. If the list is very large and the user did not request exhaustive detail, start with a compact table of all papers and then provide full blocks for the most relevant papers, explaining the selection criterion.

## Quality Requirements

Make every summary specific enough that the user can understand what the paper does without reading the abstract.

- Do not fill the six dimensions with the same sentence repeated in different wording.
- Do not write generic phrases such as `研究相关问题`, `提出一种新方法`, `取得显著提升`, or `针对现有方法局限` unless immediately followed by concrete details.
- In `解决问题`, state the actual task or failure mode, not only the field.
- In `主要方法`, explain how the method works, not only its name.
- In `数据与实验`, include concrete benchmarks, datasets, metrics, model sizes, baselines, or result numbers when the source provides them.
- In `主要贡献`, separate what the authors claim from your synthesis. Use `主要贡献可以概括为...` when inferring from the abstract.
- Preserve uncertainty. Use `作者声称`, `摘要显示`, `从公开信息看`, or `正文中需要进一步核对` when evidence is limited.
- Keep Chinese fluent and technical. Translate meaning, not word order. Keep established names such as DPO, RLHF, Transformer, RAG, diffusion, benchmark names, and method acronyms unchanged.

## Source Handling

Use source-specific conventions:

- **OpenReview**: include OpenReview URL, PDF URL, authors, primary area/track, official TLDR if present, and forum metadata.
- **arXiv**: include arXiv abstract URL, PDF URL, authors, category, version/date if useful, and code links only if clearly present.
- **Conference accepted/oral lists**: include the list page and paper page when available; preserve oral/spotlight/workshop status if the page states it.
- **PDF-only papers**: extract title/authors/abstract from the PDF. If metadata is ambiguous, mark it as unavailable rather than guessing.
- **User-selected papers after screening**: explain the screening criterion briefly, then produce the same TLDR guide for selected papers.

When browsing web sources, cite the sources used in the final answer if citations are expected by the system or user. Avoid reproducing long non-abstract passages verbatim; summarize method and experiment sections in your own words.

## Self-Check Before Answering

Before finalizing each paper block, verify:

1. The abstract translation is present.
2. The TLDR is labeled as official or self-written when appropriate.
3. The six dimensions answer six different questions.
4. `主要方法` explains mechanism, not just motivation.
5. Missing evidence is explicitly marked instead of hallucinated.
6. Links and metadata match the paper being summarized.

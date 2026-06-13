---
confidence: high
created: 2026-06-13
sha256: 76c2bc9dbb3e3a80089e89ade6644ff92908dd10a4f2617d460cd71b0150af98
source_url: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
sources:
- https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
status: active
tags:
- hermes
- wiki
- raw
- workflow
- stable
updated: 2026-06-13
wiki_type: raw
---
# Karpathy LLM Wiki Pattern — source notes

## Verified core idea
Karpathy describes an LLM Wiki as a persistent, compounding artifact: instead of re-discovering knowledge from raw documents every time like a normal RAG setup, the LLM incrementally builds and maintains interlinked markdown pages.

## Three layers
- Raw sources: source documents; the LLM reads them but does not casually modify them.
- The wiki: LLM-generated markdown pages such as summaries, entity pages, concept pages, comparisons, overviews, and synthesis pages.
- The schema: the instruction layer that tells the LLM how to structure and maintain the wiki.

## Operations
- Ingest: add a source, then update summaries, entities, concepts, index, and log.
- Query: answer from existing wiki pages, then file valuable answers back into the wiki.
- Lint: periodically check contradictions, stale claims, orphan pages, missing links, and gaps.

## Index and log
- `index.md` is content-oriented: a catalog of pages and one-line summaries.
- `log.md` is chronological: an append-only record of ingest/query/lint work.

## Local adaptation for John
This Hermes wiki keeps the Karpathy pattern but uses a hybrid boundary: memory, skills, sessions, canonical notes, and LLM Wiki each keep their own job.

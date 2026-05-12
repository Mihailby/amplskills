# AMPL Knowledge Base: `chunks.jsonl`

`chunks.jsonl` is a ready-to-use retrieval corpus for AI assistants and RAG workflows. It packages AMPL documentation, community Q&A, solver guides, modeling examples, notebooks, API references, and optimization-model references.

Each chunk includes source metadata such as title, URL, topic area, language, headings, summary, token estimate, and cleaned text. Use the `text` field for embedding and retrieval, and `text_clean` when passing retrieved context to an LLM.

**What It Covers**

| Category | Chunks | Examples |
|---|---:|---|
| Colab model notebooks | 1,032 | End-to-end AMPL and `amplpy` models |
| AMPL community forum | 684 | Distilled Q&A from AMPL users |
| Power systems optimization | 451 | Economic dispatch, unit commitment, OPF, LMP |
| LP / MIP / NLP topics | 392 | Formulations, duality, network flow, integer models |
| MO-Book chapters | 253 | Modeling and Optimization examples |
| Solver guides and options | 150 | Gurobi, CPLEX, HiGHS, XPRESS, Knitro |
| Optimization models reference | 134 | Blending, TSP, knapsack, transportation |
| `amplpy` / APIs | 188 | Python, R, Java, C, C++, C# |

**How To Use It**

Upload or index `chunks.jsonl` in any LLM or RAG system:

- **ChatGPT / OpenAI**: upload as Knowledge in a custom GPT or index it with File Search / vector stores.
- **Claude / Claude Code**: upload to a Project or place it in your workspace and tell Claude to search it for AMPL context.
- **Desktop tools**: import into Open WebUI, LM Studio, AnythingLLM, Jan, PrivateGPT, or other local RAG tools.
- **Developer stacks**: load with LangChain, LlamaIndex, Chroma, FAISS, Qdrant, Pinecone, Weaviate, or a custom retriever.
- **Codex / GitHub Copilot / Cursor / coding agents**: keep `chunks.jsonl` in the project root and reference it in the project instructions file (`.cursorrules`, `CLAUDE.md`, etc.).

```
## Knowledge base
The file `chunks.jsonl` contains AMPL documentation chunks.
When answering AMPL questions, search this file for relevant context.
At the end of every answer, include a "Sources" section listing the `source_url`
values from the chunks used. Do not invent sources; if no relevant chunk is used,
say that no dataset source was used.

## Source Citation Instruction

Every RAG answer should include source references from the retrieved chunks. Use the chunk metadata field `source_url` and place the references at the end of the answer, for example:

Sources:
- https://dev.ampl.com/...
- https://amplpy.ampl.com/...

If multiple chunks come from the same URL, list the URL once. If a chunk has an empty or missing `source_url`, omit it and prefer chunks with source attribution when possible.
```

**Performance Note**

Answer quality and speed depend on the user LLM platform, model, embedding model, context window, retrieval settings, and subscription tier. `chunks.jsonl` provides the raw knowledge, but how well the model synthesises, explains, and writes correct AMPL code from those chunks depends on the model you are running: Higher-tier plans provide better retrieval answers.

**Contact (optional)**

For questions, corrections, or references, contact at `mikhail@ampl.com`. 

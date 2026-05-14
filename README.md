# AMPL Skill & Knowledge Base

A plug-in AMPL assistant for any LLM platform. Drop three files into your project and any supported LLM gains AMPL expertise: grounded documentation retrieval, structured response modes, solver selection guidance, and code style enforcement.

## The Three Files

| File | Layer | What it does |
|---|---|---|
| `skills/LLM.md` | Entry point | Loaded as the project/system instructions. Tells the LLM to read `SKILL.md` before every AMPL response. Rename to match each platform's convention (see setup below). |
| `skills/SKILL.md` | Behaviour | Controls how the LLM responds: environment detection, proficiency and format preferences, response modes, code style, solver selection, and session state. |
| `chunks/chunks.jsonl` | Knowledge | 3,284 retrieval chunks of AMPL documentation. The LLM searches this file to ground answers with accurate, citable content. |

The two layers are independent: `SKILL.md` defines *how* to respond; `chunks/chunks.jsonl` provides *what* to say. You can extend or replace one without touching the other.

## How It Works

```
User question
     │
     ▼
LLM.md  ──►  instructs the LLM to read SKILL.md first
     │
     ▼
SKILL.md
  • Detects environment (Jupyter, Python script, pure AMPL)
  • Confirms proficiency and format preference if not already known
  • Selects response mode (Full / Quick / Tutorial / Code-focused)
  • Applies code style, solver selection, and formulation rules
     │
     ▼
chunks/chunks.jsonl  ──►  LLM searches for relevant documentation chunks
  • Retrieves chunks by keyword / concept match
  • Uses `text` field for retrieval context
  • Uses `source_url` and `headings` fields for citations
     │
     ▼
Response
  • Grounded in retrieved chunk content
  • Formatted per SKILL.md rules
  • Sources section with source_url citations
  • Session parameters footer (proficiency · env · format)
```

## Setup by Platform

### Claude Code

In accordance with the Claude Code convention for project instructions, rename
`LLM.md` to `CLAUDE.md`, or append the contents of `LLM.md` to an existing
`CLAUDE.md`.

1. Open this folder in Claude Code.
2. Start asking AMPL questions — no further setup needed.

---

### MCPMarket

The MCPMarket skill package should include the instruction files only. The AMPL
documentation corpus is hosted externally because MCPMarket limits individual
uploaded files to 500 KB.

- Corpus URL: https://raw.githubusercontent.com/Mihailby/amplskills/main/chunks/chunks.jsonl
- Size: 9,482,250 bytes
- SHA-256: 4C378E141F0ADB6DE8197FAEABADF7E90409416C88D5A5BE66F2C2B5B7C498CF

1. Install the `SKILL.md` skill from MCPMarket.
2. Download `chunks.jsonl` from the corpus URL above.
3. Verify the SHA-256 checksum if your tool supports checksum validation.
4. Put the file at `chunks/chunks.jsonl`, or configure your RAG/vector store to
   index the downloaded JSONL file.
5. Ask AMPL, amplpy, or optimization questions. The skill controls response
   behavior, and `chunks.jsonl` provides documentation-grounded retrieval.
   
---

### Cursor / Windsurf / Copilot / coding agents

1. Copy all three files into your project root.
2. Rename `LLM.md` to the platform's instructions filename:
   - Cursor → `.cursorrules`
   - GitHub Copilot → `.github/copilot-instructions.md`
   - Windsurf → `.windsurfrules`
3. Make sure `chunks/chunks.jsonl` is indexed or included in the context window (add it to the relevant include list if needed).

---

### ChatGPT / Custom GPT

1. In the GPT editor, paste the contents of `LLM.md` into the **Instructions** field.
2. Upload `SKILL.md` and `chunks/chunks.jsonl` as **Knowledge** files.
3. Enable **File Search** so the GPT retrieves from `chunks/chunks.jsonl` automatically.

---

### Open WebUI / LM Studio / AnythingLLM / local RAG tools

1. Create a new workspace or assistant.
2. Paste the contents of `LLM.md` as the **system prompt**.
3. Import `chunks/chunks.jsonl` into the tool's document/knowledge store and enable RAG retrieval.
4. Keep `SKILL.md` in the same knowledge store so the model can read it on demand.

---

### LangChain / LlamaIndex / custom retriever

1. Index `chunks/chunks.jsonl` into your vector store. Use the `text` field as the embedding input; store all other fields as metadata.
2. At query time, retrieve the top-k chunks and pass `text_clean` as the context to the LLM.
3. Set the system prompt to the contents of `LLM.md`, and include `SKILL.md` as an additional context document or system message.

**Minimal example (LlamaIndex):**

```python
import json
from llama_index.core import Document, VectorStoreIndex

docs = []
with open("chunks/chunks.jsonl") as f:
    for line in f:
        chunk = json.loads(line)
        docs.append(Document(
            text=chunk["text"],
            metadata={"source_url": chunk["source_url"], "title": chunk["title"]}
        ))

index = VectorStoreIndex.from_documents(docs)
query_engine = index.as_query_engine()

system_prompt = open("LLM.md").read() + "\n\n" + open("SKILL.md").read()
```

---

## What chunks/chunks.jsonl Contains

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

Each chunk includes: `chunk_id`, `title`, `source_url`, `headings`, `doc_area`, `token_estimate`, `text` (for embedding), and `text_clean` (for LLM context). See `chunk_schema.json` for the full field reference.

## Performance Note

Answer quality depends on the LLM, embedding model, context window, and retrieval settings. `chunks/chunks.jsonl` provides the raw knowledge; how well the model synthesises and writes correct AMPL code depends on the model tier. Higher-capability models produce better results.

## Contact

For questions, corrections, or contributions: `mikhail@ampl.com`

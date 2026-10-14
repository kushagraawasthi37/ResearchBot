
# ResearchBot — Agentic RAG Assistant

ResearchBot is an agentic Retrieval-Augmented Generation (RAG) assistant that selects between document retrieval, SQL queries, and web search to answer questions using relevant evidence. Built with LangGraph and local Ollama models, it supports retrieval grading, query rewriting, answer-grounding checks, and RAGAS-based evaluation.

## About the Project

I built this project to explore how AI assistants can go beyond basic retrieval and generation. ResearchBot uses a graph-based workflow to select an information source, evaluate retrieved evidence, and retry when the available context is insufficient.

The project is a standalone prototype and is not currently integrated with NeuralShop or another production application.

## Key Features

- Multi-source routing across documents, SQL, and web search
- Document retrieval using ChromaDB
- Retrieval grading to assess document relevance
- Query rewriting with bounded retries
- Answer-grounding checks against retrieved evidence
- Text-to-SQL using SQLite with read-only query validation
- Local LLM inference using Ollama
- RAG evaluation using RAGAS
- Interactive Streamlit interface and command-line support

## Technology Stack

- **Language:** Python
- **Agent Orchestration:** LangGraph
- **LLM Runtime:** Ollama
- **Vector Database:** ChromaDB
- **Embeddings:** Sentence Transformers
- **Structured Data:** SQLite
- **Web Search:** DuckDuckGo
- **Evaluation:** RAGAS
- **Interface:** Streamlit

## How It Works

1. A user submits a question.
2. LangGraph routes the question to document retrieval, SQL, or web search.
3. Retrieved documents are evaluated for relevance.
4. If evidence is insufficient, the query can be rewritten and retrieval retried within a defined limit.
5. The LLM generates a response using available evidence.
6. The workflow checks answer grounding and can attempt a bounded correction.
7. The response is returned with supporting source information where available.

## Project Structure

```text
ResearchBot/
├── app.py
├── src/
│   └── agentic_rag/
│       ├── config.py
│       ├── ingestion.py
│       ├── tabular.py
│       ├── cli.py
│       ├── graph/
│       │   ├── state.py
│       │   ├── chains.py
│       │   ├── nodes.py
│       │   └── build.py
│       └── tools/
│           ├── web_search.py
│           └── sql.py
├── evaluation/
│   ├── dataset.json
│   └── run_evaluation.py
├── data/
│   └── documents/
├── scripts/
│   └── create_sample_db.py
└── tests/
```

*Verify the structure against the actual repository before using it.*

## Getting Started

### Prerequisites

- Python 3.10 or newer
- [Ollama](https://ollama.com/download)

### 1. Clone the Repository

```bash
git clone https://github.com/Slahnia/agentic-rag.git
cd agentic-rag
```

### 2. Download a Local Model

```bash
ollama pull qwen2.5:3b
```

### 3. Create a Virtual Environment

**Windows PowerShell**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**macOS/Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 4. Install Dependencies

```bash
pip install -e ".[ui,eval,dev]"
```

### 5. Prepare Sample Data

```bash
python scripts/create_sample_db.py
agentic-rag-ingest-csv
agentic-rag-ingest
```

### 6. Run the Application

Launch the Streamlit interface:

```bash
streamlit run app.py
```

Or use the command-line interface:

```bash
agentic-rag -v "What is retrieval grading?"
```

## Evaluation

Run the evaluation pipeline:

```bash
python evaluation/run_evaluation.py
```

ResearchBot uses RAGAS to evaluate retrieval and response quality.

| Metric | Purpose |
|---|---|
| Faithfulness | Measures whether answers are supported by retrieved evidence |
| Response Relevancy | Measures how well an answer addresses the question |
| Context Precision | Measures the relevance of retrieved context |
| Context Recall | Measures whether the context contains the required information |

Evaluation results depend on the dataset, model, and configuration. Run the evaluation before reporting performance metrics.

## Current Limitations

- The project is a prototype for learning and experimentation.
- The current workflow routes a question to a source; full multi-source plan-and-execute orchestration is not implemented.
- Hybrid BM25 and vector retrieval, reranking, persistent conversation memory, and advanced observability are potential enhancements.
- Model speed and answer quality depend on hardware and model selection.
- Aggregate evaluation scores do not guarantee that every answer is correct.

## Future Improvements

- Hybrid keyword and vector retrieval with reranking
- Multi-step planning across documents, SQL, and web search
- Persistent conversation memory using LangGraph checkpoints
- Better evaluation datasets, including unanswerable questions
- Latency and token-usage monitoring
- Improved source attribution and error handling

## Author

**Kushagra Awasthi**

This project is part of my hands-on exploration of agentic AI, retrieval-augmented generation, information retrieval, and LLM evaluation.

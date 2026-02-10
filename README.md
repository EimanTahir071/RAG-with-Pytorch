# RAG-with-PyTorch

A practical Retrieval-Augmented Generation (RAG) project built with PyTorch. This repository is designed as a clean, reproducible baseline for building RAG systems that combine a retriever (vector search) with a generator (LLM or seq2seq model). It emphasizes modular components, clear configuration, and production-friendly patterns so you can evolve from a prototype to a robust pipeline.

## Highlights

- End-to-end RAG pipeline: ingest -> embed -> index -> retrieve -> generate
- PyTorch-first training and inference utilities
- Pluggable vector store and model backends
- Simple configuration via `.env` and YAML
- Minimal, readable structure for fast onboarding

## Repository Layout

```
.
├── data/
│   ├── raw/                 # Source documents
│   ├── processed/           # Cleaned, chunked documents
│   └── indexes/             # Vector indexes
├── notebooks/               # Experiments and exploration
├── src/
│   ├── config/              # Settings and defaults
│   ├── ingestion/           # Document loaders
│   ├── preprocessing/       # Cleaning and chunking
│   ├── embeddings/          # Embedding models
│   ├── retriever/           # Vector search interface
│   ├── generator/           # Generation models
│   ├── pipeline/            # Orchestration
│   └── utils/               # Shared utilities
├── tests/                   # Unit and integration tests
├── .env.example             # Environment variables template
├── requirements.txt         # Python dependencies
└── README.md
```

## Quick Start

1) Create a virtual environment

```bash
python -m venv .venv
```

2) Activate it

```bash
# Windows
.\.venv\Scripts\activate

# macOS/Linux
source .venv/bin/activate
```

3) Install dependencies

```bash
pip install -r requirements.txt
```

4) Copy environment template and update it

```bash
copy .env.example .env
```

5) Run the pipeline

```bash
python -m src.pipeline.run --config src/config/pipeline.yaml
```

## Configuration

The project supports a layered configuration model:

- `.env` for credentials and secrets
- `src/config/*.yaml` for runtime settings

Example `.env.example` values:

```
VECTOR_DB_PATH=data/indexes
EMBEDDING_MODEL=sentence-transformers/all-MiniLM-L6-v2
GENERATION_MODEL=facebook/bart-large-cnn
DEVICE=cuda
```

Key settings in `pipeline.yaml`:

- `chunk_size`: token or character chunk size
- `chunk_overlap`: overlap between chunks
- `top_k`: number of retrieved chunks
- `max_tokens`: generation limit

## Example Usage

Ingest data, build index, and run a query:

```bash
python -m src.ingestion.load --input data/raw
python -m src.preprocessing.chunk --input data/raw --output data/processed
python -m src.embeddings.build --input data/processed --output data/indexes
python -m src.pipeline.query --question "What is RAG and why is it useful?"
```

## Evaluation

Use the evaluation scripts to measure retrieval and generation quality:

```bash
python -m src.pipeline.eval --dataset data/processed/qa.json
```

Suggested metrics:

- Retrieval: recall@k, MRR
- Generation: ROUGE, BLEU, faithfulness

## Roadmap

- Add hybrid search (dense + BM25)
- Add reranker support
- Add streaming generation
- Add structured citations in outputs
- Add tracing and metrics exports

## Contributing

1) Create a feature branch
2) Add or update tests
3) Run tests locally
4) Open a PR with a clear description

## License

MIT License. See `LICENSE` for details.

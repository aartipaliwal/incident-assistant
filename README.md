# 🚨 Pipeline Incident Assistant: RAG on Databricks

Pipeline Incident Assistant is an advanced AI-powered operational intelligence system designed to bridge the gap between static incident runbooks and real-time pipeline recovery. By combining PySpark data pipelines, Databricks Vector Search, and Retrieval-Augmented Generation (RAG), the system allows data platform teams to query pipeline errors and instantly receive grounded troubleshooting steps based on enterprise runbooks.

## Key Features

- **Automated Runbook Ingestion**: Processes raw operational runbooks from Databricks Volumes into managed Delta Lake tables (`bronze_docs`) using PySpark.
- **Dual-Chunking Strategy Evaluation**: Implements and benchmarks two distinct text-chunking approaches (63 small chunks vs. 30 large chunks) to optimize the balance between semantic precision and context retention.
- **Managed Vector Indexing**: Integrates natively with Databricks Vector Search endpoints (`incident_vs_endpoint`), using the `databricks-gte-large-en` embedding model via the Databricks SDK.
- **Grounded RAG Pipeline**: Leverages `databricks-meta-llama-3-3-70b-instruct` to synthesize retrieved runbook contexts into step-by-step resolution steps.
- **Deterministic Guardrails**: Features explicit system guardrails that force the model to return *"I don't know"* for out-of-scope or ungrounded queries, eliminating LLM hallucinations.
- **Incremental Data Sync**: Features automated delta synchronization (`05_incremental_update.ipynb`) that refreshes the bronze layer with new runbooks and triggers real-time vector index updates (`sync_and_wait`).

## Tech Stack

| Layer | Technology |
| :--- | :--- |
| **Platform** | Databricks Free Edition / Workspace |
| **Data Processing & Storage** | PySpark, Delta Lake, Databricks Volumes |
| **Vector Search & Embeddings** | Databricks Vector Search, `databricks-gte-large-en` |
| **LLM Engine** | `databricks-meta-llama-3-3-70b-instruct` |
| **SDK & Orchestration** | Databricks SDK for Python (`databricks-sdk`) |
| **Language** | Python 3.x |

## Installation & Setup

### Prerequisites
- Active Databricks Workspace (Free or Enterprise Edition)
- Compute cluster configured with Serverless or standard runtime
- Databricks SDK (`databricks-sdk`) installed in your cluster environment

### Setup Instructions

1. **Clone the repository**:
   ```bash
   git clone [https://github.com/aartipaliwal/incident-assistant.git](https://github.com/aartipaliwal/incident-assistant.git)
   cd incident-assistant


## Evaluation & Results

Small Chunk Performance: ~63 chunks delivered optimal precision for specific error code lookups (e.g., OutOfMemoryError or schema mismatch errors).

Large Chunk Performance: ~30 chunks preserved broader multi-step context needed for complex data skew and cascade pipeline failures.

Guardrail Accuracy: 100% compliance during out-of-scope evaluation testing, effectively suppressing hallucinations.  

## Screenshots & Visual Evidence
Execution screenshots and architecture diagrams can be found in the docs/ directory:

Search Retrieval: docs/01-search-results.png

Grounded Answer Output: docs/02-rag-answer.png

Out-of-Scope Guardrail Response: docs/03-i-dont-know.png

Incremental Sync Execution: docs/05-before-update.png



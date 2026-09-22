# AETHER-AI

A hierarchical multi-agent AI system that coordinates GPT-4, Claude, and Gemini to intelligently route and answer financial queries using a Plan-and-Execute workflow with a RAG pipeline.

## Architecture

```
User Query
    │
    ▼
Council (Orchestrator)
    │  Decomposes query, selects model based on complexity/domain
    ▼
Veritas (Retrieval & Grounding Layer)
    │  Fetches domain-specific context via RAG pipeline
    ▼
Arbiter (Executor)
    │  Routes to specialized sub-agent (GPT-4 / Claude / Gemini)
    ▼
Grounded Response
```

### Layers

**Council** — the orchestration layer. Receives a user query, breaks it into a structured plan, and decides which model and sub-agent combination is best suited for each step based on query complexity and domain specificity.

**Veritas** — the retrieval and grounding layer. Runs a RAG pipeline against domain-specific knowledge sources at inference time, ensuring responses are grounded in retrieved context rather than model memory alone. This is the primary defense against hallucination.

**Arbiter** — the execution layer. Takes the plan and retrieved context from the layers above and dispatches to the appropriate specialized sub-agent (GPT-4 for reasoning-heavy tasks, Claude for long-context synthesis, Gemini for multimodal or search-augmented queries), then aggregates and returns the final response.

## Tech Stack

- **Runtime**: TypeScript (Node.js)
- **Models**: OpenAI GPT-4, Anthropic Claude, Google Gemini
- **Workflow**: Plan-and-Execute pattern
- **Retrieval**: RAG pipeline with vector search
- **Database**: PostgreSQL (via `sql/`)
- **Scripts**: Shell automation (`scripts/`)

## Project Structure

```
apps/       — core application: agents, orchestration, RAG pipeline
scripts/    — setup and utility shell scripts
sql/        — database schema and queries
```

## Getting Started

```bash
# Install dependencies
npm install

# Set up environment variables
cp .env.example .env
# Add your API keys: OPENAI_API_KEY, ANTHROPIC_API_KEY, GOOGLE_AI_API_KEY

# Run database migrations
psql -f sql/schema.sql

# Start the system
npm run dev
```

## How It Works

1. A financial query comes in (e.g. "What are the tax implications of selling equity in a startup?")
2. **Council** decomposes it into sub-tasks and scores them by complexity and domain
3. **Veritas** retrieves relevant documents and context from the vector store
4. **Arbiter** selects the best model for each sub-task, calls it with the retrieved context, and merges the results
5. The final response is grounded, cited, and hallucination-minimized

## Status

Active development — Jan 2026 to present.

## Author

[Naorem Nganthoiba Singh](https://github.com/naormeit)

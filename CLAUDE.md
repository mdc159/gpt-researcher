# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

GPT Researcher is an open deep research agent designed for both web and local research on any given task. It produces detailed, factual, and unbiased research reports with citations. The system utilizes AI agents to plan research questions, gather information, and synthesize findings into comprehensive reports.

## Key Directories

- `/gpt_researcher/`: Core library code for the research agent
- `/backend/`: Server implementation for the web interface
- `/frontend/`: UI implementations (static HTML and NextJS)
- `/multi_agents/`: LangGraph-based multi-agent system for deeper research
- `/tests/`: Test cases
- `/docs/`: Documentation

## Development Environment Setup

### Installation

1. Install Python 3.11 or later
2. Clone the repository and set up environment:

```bash
git clone https://github.com/assafelovic/gpt-researcher.git
cd gpt-researcher
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Set up environment variables:

```bash
export OPENAI_API_KEY=<your-openai-api-key>
export TAVILY_API_KEY=<your-tavily-api-key>
```

## Common Commands

### Running the Server

Start the FastAPI server:

```bash
python -m uvicorn main:app --reload
```

Access the web interface at: http://localhost:8000

### Running with Docker

```bash
docker-compose up --build
```

If that doesn't work, try:

```bash
docker compose up --build
```

This will start:
- Python server on localhost:8000
- React app on localhost:3000 (if using NextJS frontend)

### Running Tests

```bash
pytest tests/
```

For specific test files:

```bash
pytest tests/test_logging.py
```

### Running via CLI

To generate a research report via command line:

```bash
python cli.py "your research query" --report_type research_report --tone objective
```

Available report types:
- `research_report`: Summary - Short and fast (~2 min)
- `detailed_report`: Detailed - In depth and longer (~5 min)
- `deep_research`: Deep Research - More comprehensive exploration

Available tones:
- objective, formal, analytical, persuasive, informative, explanatory, descriptive, critical, comparative, speculative, reflective, narrative, humorous, optimistic, pessimistic

### Running Multi-Agent System

```bash
cd multi_agents
python main.py
```

Make sure to configure task settings in `multi_agents/task.json` first.

## Testing Components

### Test LLM Integration

```bash
python tests/test-your-llm.py
```

### Test Retrievers

```bash
python tests/test-your-retriever.py
```

### Test Embeddings

```bash
python tests/test-your-embeddings.py
```

## Code Architecture

The system is built around several key components:

1. **Researcher Agent**: Core component that manages the research process
   - Uses planner and executor agents to divide work
   - Creates specific agents based on research queries

2. **Retrieval System**: Fetches information from various sources
   - Supports multiple search engines (Tavily, Google, Bing, etc.)
   - Configurable through `RETRIEVER` environment variable

3. **Scraper**: Extracts content from web pages
   - Multiple implementations (BeautifulSoup, Browser-based, etc.)
   - Controlled via `SCRAPER` config setting

4. **LLM Integration**: Uses language models for various tasks
   - Supports different providers (OpenAI, Anthropic, etc.)
   - Configurable through environment variables

5. **Memory System**: Maintains context during research
   - Stores and retrieves research findings
   - Uses embeddings for similarity matching

6. **Report Generation**: Creates formatted research reports
   - Supports multiple formats (Markdown, PDF, Word)
   - Customizable templates and styles

7. **Web Server**: FastAPI backend for the web interface
   - Handles WebSocket communication for real-time updates
   - Manages file uploads and downloads

8. **Deep Research**: Advanced recursive research workflow
   - Explores topics with configurable depth and breadth
   - Uses tree-like exploration pattern

## Configuration

The system uses environment variables and configuration files to control behavior. The primary configuration mechanism is through environment variables, with sensible defaults provided.

Key configuration variables:
- `OPENAI_API_KEY`: OpenAI API key for LLM access
- `TAVILY_API_KEY`: Tavily API key for web search
- `RETRIEVER`: Search engine to use (default: "tavily")
- `FAST_LLM`: Model for simpler tasks (default: "openai:gpt-4o-mini")
- `SMART_LLM`: Model for complex tasks (default: "openai:gpt-4.1")
- `STRATEGIC_LLM`: Model for strategic planning (default: "openai:o4-mini")
- `EMBEDDING`: Embedding model (default: "openai:text-embedding-3-small")
- `DOC_PATH`: Path for local document research (default: "./my-docs")

## Working with the Codebase

When implementing new features or fixing bugs:

1. Follow the existing code structure and patterns
2. Add appropriate tests for new functionality
3. Update documentation as needed
4. Submit pull requests against the `master` branch
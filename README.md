# GenAI for Software Engineering — Course Materials

> Practical examples and Jupyter Notebooks covering the application of Generative AI techniques in Software Engineering contexts, including RAG, fine-tuning, tool calling, MCP integration, attention visualization, and AI-assisted TDD.

---

## Repository Structure

```
genai4se-course/
├── rag-example/              # Retrieval-Augmented Generation pipeline
├── fine-tuning-example/      # LLM fine-tuning workflow
├── tool-calling-example/     # Function/tool calling with LLMs
├── mcp-example/              # Model Context Protocol integration
├── attention-visualization/  # Transformer attention map visualization
└── assured-tdd/              # AI-assisted Test-Driven Development
```

Each directory is self-contained and includes its own notebook(s) and dependencies.

---

## Modules

### `rag-example`
Demonstrates a Retrieval-Augmented Generation pipeline: document ingestion, embedding generation, vector store indexing, and augmented query answering. Covers the core trade-offs between retrieval precision and generation quality.

### `fine-tuning-example`
End-to-end fine-tuning workflow for adapting a pre-trained LLM to a domain-specific task. Includes dataset preparation, training configuration, and evaluation against a baseline model.

### `tool-calling-example`
Illustrates how LLMs invoke structured external functions (tool calling / function calling). Shows schema definition, request/response handling, and multi-step tool orchestration patterns.

### `mcp-example`
Integration example using the Model Context Protocol (MCP), demonstrating how to expose and consume external tool capabilities through a standardized protocol interface.

### `attention-visualization`
Interactive visualization of transformer self-attention maps. Useful for interpreting model behavior, debugging attention heads, and building intuition about how tokens attend to each other across layers.

### `assured-tdd`
Explores the use of Generative AI to assist and automate Test-Driven Development cycles. Based on research into collaborative and fully automated TDD patterns using LLMs (see references below).

---

## Requirements

Python 3.10+ and Jupyter are required. Install dependencies per module:

```bash
pip install -r <module>/requirements.txt
```

If no `requirements.txt` is present in a given module, refer to the import statements at the top of the notebook for the necessary packages.

---



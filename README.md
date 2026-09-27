# RAG Learning Lab

A hands-on project built while learning **Retrieval-Augmented Generation (RAG)** — using a real project document as sample data to practice chunking, embeddings, vector search, retrieval tuning, and building a simple Q&A interface.

## What this is

This isn't a course copy — it uses my own project document (a workflow doc for a separate hackathon project) as the knowledge base, and includes real debugging I went through while building it.

## What it does

- Loads a PDF document
- Splits it into chunks using `RecursiveCharacterTextSplitter`
- Generates embeddings using `HuggingFaceEmbeddings` (`sentence-transformers/all-mpnet-base-v2`)
- Stores and searches vectors using `ChromaDB`
- Retrieves relevant chunks and generates grounded answers using `meta-llama/Llama-3.1-8B-Instruct` (via Hugging Face Inference Providers)
- Uses a custom prompt template to prevent hallucination (the model explicitly says "I don't know" when the answer isn't in the document)
- Wraps everything in a simple **Gradio** web interface for interactive Q&A

## Tech Stack

- LangChain (`langchain-community`, `langchain-huggingface`, `langchain-chroma`, `langchain-classic`)
- ChromaDB (vector store)
- Hugging Face Inference Providers (LLM + embeddings)
- Gradio (UI)

## What I learned / debugged

- How chunking strategy affects retrieval accuracy — naive character-based splitting can cut sentences in half, losing context
- How the `k` parameter (number of retrieved chunks) directly affects answer completeness — increasing `k` fixed cases where relevant information existed in the document but wasn't being retrieved
- Hugging Face's shift to an "Inference Providers" model-hosting system, and how token permissions, task types (`text-generation` vs `conversational`), and provider availability all affect whether a model actually works
- How a prompt template can be used to explicitly force an LLM to stay grounded in retrieved context instead of falling back on its own general knowledge

## How to run

1. Open `rag_lab.ipynb` in Google Colab
2. Get a free Hugging Face access token ([huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)) with the "Inference" permission preset
3. Replace `YOUR_HUGGINGFACE_TOKEN_HERE` in the notebook with your token
4. Run all cells in order
5. Use the Gradio link generated at the end to chat with the document

## Note

This is a learning/practice project, not a production system — built to solidify RAG fundamentals before applying them to a larger multi-agent project.

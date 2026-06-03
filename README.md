# RAG System Implementation

## Introduction
This project implements a Retrieval-Augmented Generation (RAG) system designed to answer user queries by leveraging information extracted from PDF documents. The system follows a defined workflow: input -> data (PDF) -> chunking & overlapping -> embedding -> vectorDB -> LLM -> output, ensuring responses are grounded in the provided document content.

## Features
- **Document Ingestion**: Processes PDF documents for information retrieval.
- **Text Processing**: Chunks documents with specified overlap for efficient embedding.
- **Semantic Search**: Utilizes `SentenceTransformer/all-MiniLM-L6-v2` for generating embeddings and FAISS for fast similarity search.
- **Hybrid Search**: Combines vector similarity search with keyword search for robust retrieval.
- **Generative AI**: Integrates Groq LLaMA 3.1 8B for generating concise and relevant answers based on retrieved context.
- **Structured Output**: Presents answers in a clearly defined XML-like structure, with the LLM's raw output cleaned of any residual XML tags before insertion.

## Tech Stack
- **Chunking**: Token-based chunking with 900-1200 tokens per chunk and 200-300 tokens overlap.
- **Embedding Model**: Hugging Face `SentenceTransformer/all-MiniLM-L6-v2`.
- **Vector Database**: FAISS (Facebook AI Similarity Search).
- **Large Language Model (LLM)**: Groq LLaMA 3.1 8B via API.
- **Search**: Hybrid search combining vector search and keyword search.
- **Similarity Metric**: Cosine similarity for ranking search results.

## Task Not To Do
- Do not train any models; utilize only pre-trained models.

## Setup and Installation
To set up the environment, you'll need Python and the following libraries. It's recommended to use a virtual environment.

```bash
pip install pypdf sentence-transformers faiss-cpu groq langchain langchain-community
```

### API Key Configuration
For the Groq LLM, you need to obtain an API key from [Groq](https://groq.com/). Store this key securely. In a Colab environment, use the Secrets manager (🔑 icon on the left panel) and name the secret `GROQ_API_KEY`. Alternatively, you can use a `.env` file for local development.

## Workflow
1. **Input**: User query.
2. **Data (PDF)**: Load and extract text from target PDF documents (e.g., `iso27001.pdf`).
3. **Chunking & Overlapping**: Split the extracted text into smaller, manageable chunks with specified overlap.
4. **Embedding**: Convert text chunks into numerical vector embeddings using `SentenceTransformer/all-MiniLM-L6-v2`.
5. **VectorDB**: Store these embeddings in a FAISS index for efficient retrieval.
6. **Retrieval (Hybrid Search)**: Upon a query, perform both vector search (FAISS) and keyword search to retrieve relevant chunks. Re-rank retrieved chunks using cosine similarity.
7. **LLM**: Pass the user query and the retrieved context to the Groq LLaMA 3.1 8B model.
8. **Output**: The LLM generates an answer, which is then cleaned of any extraneous XML tags and formatted according to the specified structure.

## Output Structure
The final output will adhere to the following structure:

```
   Topic: Document Knowledge Extraction:

Question: [User input question]


Answer: [Highly accurate answer extracted directly from the context, without any XML tags or extra formatting]
```

## Usage
(The code cells in this notebook demonstrate the implementation details for each step of the workflow. Users can execute those cells to run the RAG system and test with various queries.)

# MiniRAG_ApplicationMini RAG System (Retrieval-Augmented Generation)

This project implements a simple and transparent Retrieval-Augmented Generation (RAG) pipeline.
It retrieves relevant information from a small set of documents and generates grounded answers strictly based on retrieved content.

The goal is to demonstrate understanding of:

Document chunking and embeddings

Vector search using FAISS
LLM-based answer generation
Grounding and hallucination control

1. Embedding Model and LLM Used
Model: all-MiniLM-L6-v2 (Sentence Transformers)
Why this model:
Open-source and lightweight
Produces high-quality semantic embeddings
Fast and suitable for small-to-medium document collections
Commonly used in production-grade RAG systems

 Large Language Model (LLM)

LLM: OpenRouter-hosted  mistralai/devstral-2512:free

Why this model:
Reliable instruction-following behavior
Low latency and stable responses
Works well with strict prompts for grounded QA
Easily accessible through OpenRouter API

2. Document Chunking and Retrieval
What is Document Chunking?
Documents are split into small, meaningful chunks
Chunking is sentence-based with a fixed character limit
Overlap is used to preserve context between chunks

This ensures:

Better semantic embedding quality
More precise retrieval
Reduced noise during LLM generation
 Embedding & Vector Index
Each document chunk is converted into a vector embedding

A local FAISS index (IndexFlatL2) is built over all embeddings
This enables fast semantic similarity search

 Retrieval Strategy
For each user query:
The query is embedded
Top-k most relevant chunks are retrieved using FAISS
top_k is intentionally kept low (2–3) to avoid mixing unrelated documents

3. Grounding to Retrieved Context (Hallucination Control)

Grounding is enforced through three mechanisms:
1. Strict Prompt Instructions
The LLM is explicitly instructed to:
Answer only using the retrieved context
Avoid assumptions, explanations, or generalizations

Respond with
"Not specified in the provided documents"
if the answer is not explicitly present

 2. Low Temperature
LLM temperature is set to 0.0
This minimizes creative or speculative output

 3. Controlled Retrieval
Limited number of retrieved chunks
Prevents over-aggregation across FAQs, policies, and specifications

4. Transparency and Explainability
For every query, the system clearly displays:
The retrieved document chunks (with source and chunk ID)
The final generated answer
This makes it easy to:
Inspect why an answer was generated
Detect hallucinations or unsupported claims
Debug retrieval quality

5. How to Run the Project Locally
Python 3.8+
OpenRouter API key
pip install -r requirements.txt

 Run the Notebook
Open rag_notebook.ipynb in:
Jupyter Notebook, or
Google Colab
Add your OpenRouter API key in the configuration cell
Run all cells from top to bottom

6. Example Query
What factors affect construction project delays?
The system retrieves relevant chunks and produces a grounded answer strictly based on document content.

7. Notes on Limitations
Mild hallucinations may still occur if:
Retrieval returns mixed-topic chunks
Documents contain implicit information
These are common RAG challenges and are mitigated but not fully eliminated

8. Conclusion
This project demonstrates a clean, minimal, and explainable RAG pipeline using:
Open-source embeddings
Local FAISS vector search

Strictly grounded LLM-based answer generation

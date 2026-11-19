# Retrieval-Augmented Generation (RAG) & Hybrid Search Pipelines

A collection of end-to-end Retrieval-Augmented Generation (RAG) experiments, chunking strategies, and inference pipelines built and tested on Google Colab. This repository progresses from single-PDF document Q&A to multi-source hybrid search (Semantic Vector + BM25) over structured CSV catalogs and unstructured store policy PDFs.

---

## Repository Structure

| File | Pipeline Focus | Key Techniques |
| :--- | :--- | :--- |
| **`AYUSH_resume_pdf_RAG_implementation_usi...`** | Single-Document PDF Q&A | PDF text extraction, recursive text chunking, dense embeddings, and context-grounded QA inference. |
| **`bakery_rag_chatbot_supabase_vectorstoreinde...`** | Multi-Source Bakery RAG (`Supabase` + `pgvector`) | Ingestion across multiple `.csv` product catalogs and policy `.pdf` files, `pgvector` indexing, and hybrid search (Vector + Keyword/BM25). |
| **`BAKE_BOX_ASSISTENT.ipynb`** | BAKEBOX (CHOCO AI) Inference & Chunking | Row-level structured chunking (`544` bakery SKUs + store rules), metadata enrichment, image JSON mapping, and low-hallucination inference. |
| **`RAG_readme`** | Architecture Notes | Reference notes on hybrid search scoring and retrieval tuning. |

---

## Architecture & Engineering Progression

### 1. Document Q&A Pipeline (`PDF RAG`)
* **Ingestion:** Extracts raw text from PDF documents using `pypdf` / LlamaIndex loaders.
* **Chunking:** Splits documents using overlapping windows (`chunk_size=512`, `chunk_overlap=50`) to preserve cross-sentence context.
* **Retrieval:** Generates dense embeddings using Hugging Face sentence-transformer models (`BAAI/bge-small-en-v1.5` / `all-MiniLM-L6-v2`) and queries via cosine similarity.

### 2. Multi-Source Hybrid Search (`Supabase + pgvector`)
* **Hybrid Retrieval:** Combines dense vector similarity (`pgvector`) with lexical keyword matching (`BM25`) to handle both semantic queries (*"romantic cherry cake"*) and exact identifiers (*"BB-BLK-01"*, *"Khoaa"*).
* **Unified Storage:** Stores document chunks, embeddings, and source metadata inside PostgreSQL (`Supabase`) for unified filtering.

### 3. BAKEBOX Assistant (`Row-Level Chunking & Grounded Inference`)
* **Row-Level Serialization:** Converts structured product rows (SKU, flavor, weight, price, visual features, and image path) into self-contained retrieval chunks so product attributes and image paths stay intact during inference.
* **Hallucination Guardrails:** Enforces strict system-level grounding rules so responses only cite verified catalog prices, weights, and store policies.
* **Architectural Takeaway (SQL vs. Vector):** Demonstrated through benchmarking that while vector search excels on unstructured policy PDFs and visual descriptions, high-precision numeric/attribute filters (`price <= 500`, `weight_kg`, `flavor`) are best served via parameterized relational SQL queries alongside vector RAG.

---

## Tech Stack
* **Frameworks:** LlamaIndex, LangChain
* **Vector Stores & Databases:** PostgreSQL (`pgvector` via Supabase), FAISS , IBMDB2 
* **Embeddings & Retrieval:** Hugging Face Sentence Transformers, BM25 Hybrid Search 
* **LLM Inference:** Groq API, Google Gemini ,ollama
* **Environment:** Google Colab, Python 3.10+

---

## Running the Notebooks
1. Open any `.ipynb` notebook directly in **Google Colab**.
2. Add your API keys (`GROQ_API_KEY`, `SUPABASE_URL`, `SUPABASE_KEY`, or `HF_TOKEN`) in Colab **Secrets**.
3. Upload the target `.pdf` or `.csv` files when prompted and run all cells sequentially.

---

## Author
**Ayush Dhangar**  
B.Tech CSE (AI & ML) | Python Backend & AI Engineer  
[GitHub](https://github.com/AYUSHDHANGAR) • [LinkedIn](https://www.linkedin.com/in/ayush-dhangar-63b190442)
```[cite: 1]

---

### Ek Chota Pro-Tip (GitHub Polish ke liye):
* `README.md` commit karne ke baad, jo purani `RAG_readme` file repo mein padi hai use delete kar dena taaki repo clean dikhe[cite: 1].
* Right sidebar mein **About** section ke gear icon par click karke typo bhi fix kar lena: `"rag Inference..."` $\rightarrow$ `"End-to-end RAG, Hybrid Search (pgvector + BM25), and BAKEBOX inference pipelines on Google Colab"`[cite: 1].

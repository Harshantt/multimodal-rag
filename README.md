# Multimodal RAG with Qwen2-VL and FAISS

## Overview

This project implements a Multimodal Retrieval-Augmented Generation (RAG) system for answering questions from a company PDF report.

The system processes text, tables, images, charts, and diagrams.

Visual content is summarized using Qwen2-VL-2B. Sentence Transformers are used to create embeddings, and FAISS is used for local vector similarity search.

## Architecture

PDF Report
    |
    +--> Text extraction
    +--> Table extraction
    +--> Image extraction
              |
              v
        Qwen2-VL-2B
        Visual summaries
              |
              v
       LangChain Documents
              |
              v
    Sentence Transformers
              |
              v
            FAISS
              |
              v
        Similarity Retrieval
              |
              v
        Retrieved Evidence
              |
              v
        Qwen2-VL-2B
              |
              v
           Answer

## Technologies

- Python
- PyMuPDF
- LangChain
- Qwen2-VL-2B-Instruct
- Hugging Face Transformers
- Sentence Transformers
- FAISS
- Pandas
- Pillow
- Google Colab / NVIDIA T4 GPU

## Multimodal Processing

The PDF is processed page by page.

### Text

Text is extracted directly from each PDF page.

### Tables

Tables are detected and converted into Markdown-formatted text.

### Images and Charts

Images are extracted from the PDF and passed to Qwen2-VL.

The model generates concise factual descriptions of charts, diagrams, and other business visuals.

These descriptions are stored as LangChain documents together with metadata such as page number, modality, image path, and source PDF.

## Retrieval

The extracted documents are embedded using:

`sentence-transformers/all-MiniLM-L6-v2`

The embeddings are stored in a FAISS vector index.

For each question, the retriever returns the most relevant documents.

## Generation

The retrieved text, tables, visual summaries, and relevant images are provided to Qwen2-VL-2B.

The model generates an answer using the retrieved report evidence.

## Example Questions

1. Which region had the highest revenue growth?
2. What are the main components and flow shown in the supply-chain diagram?
3. What was NovaCore's revenue in Europe?
4. Which region had the highest customer satisfaction?
5. What are the main production and quality-control locations?
6. What is the company's electricity mix?

## Example Result

Question: Which region had the highest revenue growth?

Answer: Europe

The system retrieved relevant evidence from the report, including Page 4 table and visual content.

## Vector Store Persistence

The FAISS index can be saved locally and reloaded later.

Saved files:

- index.faiss
- index.pkl

This avoids rebuilding the vector store every time the notebook is restarted.

## Project Structure

```text
multimodal-rag/
|
+-- multimodal_rag.ipynb
+-- requirements.txt
+-- README.md
+-- data/
+-- novacore_extracted_images/
```

## Running the Project

1. Open the notebook in Google Colab.
2. Enable a GPU runtime.
3. Install the dependencies from requirements.txt.
4. Place the PDF report in the expected path.
5. Run the notebook cells in order.
6. The system extracts multimodal content.
7. The FAISS vector store is created.
8. Ask questions using ask_rag().

## Hardware

The project was tested using an NVIDIA Tesla T4 GPU in Google Colab.

Qwen2-VL-2B was selected to keep GPU memory usage practical while supporting multimodal processing.

## Future Improvements

- Add document chunking for larger reports.
- Add reranking for improved retrieval.
- Add conversational memory.
- Add a web or Streamlit interface.
- Add citations directly to generated answers.
- Use a stronger multimodal model when more GPU memory is available.
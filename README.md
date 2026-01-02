# AI-Powered Virtual Assistant for the NUS MSBA Website

This repository documents the design, configuration, and evaluation of a **Retrieval-Augmented Generation (RAG)** virtual assistant built on **Google Vertex AI Agent Builder** for the Master of Science in Business Analytics (MSBA) programme at the National University of Singapore.

The assistant answers prospective students’ queries about admissions, curriculum, tuition fees, scholarships, programme structure, and general MSBA details using **grounded information** drawn from official programme documents.

---

## 1. Project Overview

- **Goal:** Deliver an accurate, scalable, and citation-backed chatbot to reduce manual query load for the MSBA Programme Office and improve prospective student experience.
- **Platform:** Google Vertex AI Agent Builder (UI-driven configuration).
- **Knowledge Base:** MSBA brochure, FAQs, curated snippets, and structured MSBA website content stored in Google Cloud Storage.
- **Architecture:** A layered RAG pipeline including User Interaction, Processing, Retrieval, Response Generation, and Platform Services layers.

For full methodology and context, refer to:
- `docs/capstone_paper_msba_chatbot.pdf`
- `docs/poster_msba_chatbot.pdf`

---

## 2. Tech Stack

- **Platform:** Google Vertex AI Agent Builder  
- **LLM:** Gemini / PaLM (managed by Vertex AI)  
- **Architecture:** Retrieval-Augmented Generation (RAG)  
- **Embedding & Search:** Vertex AI managed vector store  
- **Storage:** Google Cloud Storage (GCS)  
- **Interface:** Vertex AI Web Messenger  
- **Languages / Tools:** Python (experiments), Markdown, Google Cloud Console  

---

## 3. System Architecture

Key components of the solution:

- **User Interaction Layer:** MSBA website integration using Vertex AI Messenger widget.
- **Processing Layer:** Model context, system prompt, agent instructions, tone, and fallback configuration.
- **Retrieval Layer:**  
  - Source documents (brochure, FAQ, curated text)  
  - Advanced chunking (~500 tokens with hierarchical headings)  
  - Semantic similarity search over a managed vector store  
- **Response Generation Layer:**  
  - Vertex AI LLM  
  - RAG output composer  
  - Citation logic and response formatting  
- **Platform Services Layer:**  
  - Google Cloud Storage  
  - IAM & access controls  
  - Domain whitelisting for deployment  

Architecture diagrams are included in the capstone paper and poster.

---

## 4. Vertex AI Configuration (UI-Driven)

Most of the implementation was done through the **Vertex AI Agent Builder UI**, with the following key settings:

### **Document Ingestion**
- OCR parsing for complex PDFs  
- Advanced chunking with section headers  
- Cleaned brochure + curated text to avoid OCR noise  
- Version-controlled document uploads

### **Retrieval & Embeddings**
- Vertex-managed vector DB  
- Semantic similarity search  
- Higher overlap threshold to reduce irrelevant matches  

### **LLM Behavior Controls**
- Strict “answer only from documents” grounding  
- Personality and tone tuned for clarity and professionalism  
- Limited LLM fallback to prevent hallucination  
- Scenario-based fallback messages for out-of-scope queries  

### **Citations & Logging**
- Source traces displayed with each answer  
- Feedback logging (thumbs up/down) enabled  
- Grounding diagnostics used heavily during testing  

Details, rationales, and screenshots are included in `docs/capstone_paper_msba_chatbot.pdf`.

---

## 5. Testing & Evaluation

Testing followed a structured methodology:

- ~50 benchmark queries across admissions, deadlines, curriculum, fees, contact information, and programme requirements.
- Evaluation criteria:
  - **Accuracy**
  - **Grounding / citation correctness**
  - **Clarity**
  - **Fallback behaviour**
  - **Retrieval quality**

### **Performance Improvements**
- Initial accuracy: **~76%**  
- After iterative fixes: **~88%**  
- Citation errors reduced to **0%**  
- Fallback behaviour became consistent and safe  

### Testing Artifacts
- Testing guide: `docs/testing_guide_vertex_ui.md`
- Performance log & improvement notes: `docs/testing_findings_and_improvements.md`

These provide examples of:
- incorrect responses & why they occurred  
- retrieval failures due to OCR or formatting  
- fallback misfires  
- content gaps requiring new corpus files  
- configuration changes that improved accuracy  

---

## 6. Repository Structure

msba-rag-chatbot-vertex-ai/
├─ docs/
│ ├─ capstone_paper_msba_chatbot.pdf
│ ├─ poster_msba_chatbot.pdf
│ ├─ testing_guide_vertex_ui.md
│ ├─ testing_findings_and_improvements.md
│
├─ notebooks/
│ ├─ rag_codebase.ipynb
│
├─ data/
│ ├─ sample_queries.csv 
└─ README.md


---

## 7. Notebook & Code

Although the chatbot was primarily built via the **Vertex UI**, this repository includes a Jupyter notebook for experimentation:

### `notebooks/rag_codebase.ipynb`
Contains:
- Embedding and retrieval experiments  
- Similarity scoring tests  
- Chunk size exploration  
- Trial RAG queries outside Vertex AI  
- Log analysis for incorrect responses  

This notebook complements the UI configuration by providing a flexible sandbox for testing retrieval logic.

---

## 8. How to Reproduce (High-Level)

Since most of the implementation is UI-driven, the steps below describe the workflow rather than runnable code:

1. Upload cleaned brochure, FAQ, and curated MSBA text to Google Cloud Storage.  
2. Create a new agent in **Vertex AI Agent Builder**.  
3. Configure ingestion settings:  
   - OCR, chunk size, metadata, headings.  
4. Configure grounding and retrieval:  
   - embeddings, similarity thresholds, and fallback logic.  
5. Add system instructions to enforce grounded, citation-based answers.  
6. Test using the console with benchmark queries.  
7. Iterate on:  
   - document quality,  
   - chunking,  
   - fallback rules,  
   - system prompts.  
8. Deploy using the Vertex AI Messenger widget.  

---

## 9. Demo

The chatbot was deployed through the MSBA website using the Vertex AI Messenger widget.  
Example interactions and screenshots are available in the project poster and capstone paper.

---

## 10. Potential Future Work

- Automate log export to BigQuery for performance monitoring  
- Build Looker dashboards for unresolved intents & popular queries  
- Add multilingual support (EN/CN/IN languages)  
- Integrate into NUS IT’s AI-Know enterprise framework  
- Enhance fuzzy matching and query reformulation  
- Integrate programme calendar and search-based capabilities  

---

**This repository represents the complete lifecycle of building an enterprise-grade RAG chatbot — from data ingestion and retrieval configuration to evaluation, error analysis, and deployment.**

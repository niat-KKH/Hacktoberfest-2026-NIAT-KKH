# LearnAnywhere

## Team / attendee

- Team name (if applicable): HackersofHactober
- Members and GitHub usernames: Nonitha Ganta (@nonithaganta)
- Profile links (optional): https://github.com/nonithaganta

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [ ] Best Use of Gemma 4

## Project links

- Public GitHub repository: https://github.com/nonithaganta/learnanywhere
- Open-source license (link to the license file): https://github.com/nonithaganta/learnanywhere/blob/main/LICENSE

## Problem and solution

College students frequently rely on generic cloud AI chat tools that suffer from critical drawbacks: they require continuous internet connectivity, transmit sensitive coursework and private notes to third-party cloud servers, and hallucinate answers without exact attribution to assigned textbooks or lecture slides.

**LearnAnywhere** solves this by providing a 100% offline-first AI study tutor. Students upload their course lecture PDFs, and the system extracts text locally using PyMuPDF, indexes passages across single or multiple documents into a local SQLite database, and runs local inference using Google's open-weights Gemma model through Ollama.

**Workflow (Input → Output):**
1. **Input**: Students select or drag-and-drop one or more course PDFs (lecture slides, chapters, notes) with support for multi-file selection.
2. **Processing**: PyMuPDF extracts text and page numbers completely offline; duplicate notes are identified and deduplicated; chunks are indexed for local BM25 ranking.
3. **Output**:
   - **Interactive Study Chat**: Grounded Q&A with expandable citations showing `[Document Name, Page X]` and exact source snippets. If a topic is not in the material, the assistant explicitly states so instead of hallucinating.
   - **Concept Explainer**: 4 tailored pedagogical modes (ELI5, Deep Dive, Summary, Exam Prep) breaking down complex academic topics with key takeaways and verified note citations.
   - **Revision Quiz Generator**: Custom 3, 5, or 10-question multiple-choice revision quizzes sampled across notes with instant grading and explanations referencing source pages.

## Approach and technologies

- **Frontend**: React 19, TypeScript, Tailwind CSS, Lucide React icons, and Vite. The frontend runs locally on `http://localhost:5173` with zero external CDN dependencies or tracking scripts.
- **Backend**: Python 3.10+ and FastAPI running locally on `http://127.0.0.1:8000`.
- **Local PDF Extraction**: PyMuPDF (`pymupdf`) for sub-second, memory-efficient local document extraction preserving exact page structures and detecting image-only/scanned documents.
- **Local Document Retrieval**: In-memory BM25 chunk retrieval scoring and ranking relevant passages across individual notes or the entire document library ("All Documents" mode) without bloating LLM context.
- **Local Inference Engine**: Ollama local HTTP API (`http://localhost:11434`) running Google's open-weights `gemma:2b` (2B parameter Q4_0 quantized model) on local CPU/GPU.
- **Persistence**: Local SQLite (`learnanywhere.db`) storing document metadata, page contents, chat sessions, and quiz records with zero cloud synchronization.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI component and its role**: 
  Google DeepMind's open-weights **Gemma 1 (2B)** (`gemma:2b`), running entirely offline via Ollama. It powers all language generation, grounded question answering, pedagogical explanations, and structured multiple-choice quiz generation with zero cloud API keys or external services.
- **Code link showing the integration**:
  - Local Ollama Service: [backend/app/ollama_service.py](https://github.com/nonithaganta/learnanywhere/blob/main/backend/app/ollama_service.py)
  - Grounded RAG & Multi-Document Retrieval: [backend/app/rag_service.py](https://github.com/nonithaganta/learnanywhere/blob/main/backend/app/rag_service.py)
  - Resilient Quiz Generation & JSON Schema Repair: [backend/app/quiz_service.py](https://github.com/nonithaganta/learnanywhere/blob/main/backend/app/quiz_service.py)
  - Multi-Document API Coordination: [backend/app/main.py](https://github.com/nonithaganta/learnanywhere/blob/main/backend/app/main.py)
- **Agent Skill Open Standard compliance (if applicable)**:
  N/A
- **Original harness implementation or meaningful changes**:
  Built an original, offline-first application harness that combines:
  1. Multi-document batch upload with duplicate prevention and independent page storage.
  2. Cross-document BM25 retrieval ranking across multiple distinct note sets without prompt bloat.
  3. Grounded attribution enforcement formatting citations as `[Document Name, Page X]`.
  4. Robust fallback and validation logic repairing imperfect local LLM JSON quiz responses.
  5. Air-gapped verification ensuring zero external network calls during runtime.

## Current status

- **What works**:
  - Multi-document PDF drag-and-drop and Ctrl/Shift-click upload queue with individual status badges.
  - Independent PyMuPDF text extraction with duplicate prevention.
  - Multi-document ("All Documents") and single-document study chat with expandable page citations.
  - 4-mode pedagogical Concept Explainer (ELI5, Deep Dive, Summary, Exam Prep).
  - Revision Quiz generator (3, 5, 10 questions) with automated grading and explanations.
  - SQLite persistence for documents, sessions, and quiz results.
  - 100% offline execution verified with local Ollama (`gemma:2b`).
  - Automated test suite (8/8 pytest tests passing).
- **Known limitations / incomplete features**:
  - Scanned/image-only PDFs currently trigger a warning because offline OCR (e.g. Tesseract) is not yet bundled to keep installation lightweight.
  - Inference generation latency depends directly on the host computer's CPU/GPU capabilities.
- **What you would improve next**:
  - Add optional local OCR using ONNX/Tesseract for scanned handwritten notes.
  - Implement spaced-repetition flashcards using the SM-2 algorithm.
  - Add concept mind-map visualizations linking related topics across uploaded documents.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

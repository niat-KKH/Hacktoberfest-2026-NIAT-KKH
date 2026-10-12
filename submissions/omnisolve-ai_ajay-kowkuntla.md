# OmniSolve AI

## Team / attendee

- Team name (if applicable): OmniSolve AI
- Members and GitHub usernames:
  - Ajay Kowkuntla ([@ajaykowkuntla-hash](https://github.com/ajaykowkuntla-hash)) — Frontend Development & Project Integration
  - Amay Raj ([@<amay-raj-github-username>](https://github.com/<amay-raj-github-username>)) — Backend Development & AI Integration
- Profile links (optional):
  - Ajay Kowkuntla: https://github.com/ajaykowkuntla-hash
  - Amay Raj: https://github.com/<amay-raj-github-username>

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [ ] Best Use of Gemma 4

*Note on Track Selection:* OmniSolve AI is submitted to the **Best Open-Source AI Project** challenge as an open-source, local-first engineering assistant pairing an open-weight Gemma 4 model with a tailored client-server harness. (If cross-track entry is eligible per organizer instructions, the local Gemma 4 integration architecture also provides evidence for the Gemma 4 category).

## Project links

- Public GitHub repository: https://github.com/ajaykowkuntla-hash/omnisolve
- Open-source license (link to the license file): https://github.com/ajaykowkuntla-hash/omnisolve/blob/main/LICENSE *(MIT License)*

## Problem and solution

### Target Audience
Engineering students, embedded hardware hobbyists, software developers, and technical learners who constantly context-switch across terminal logs, circuit schematics, and dense technical documentation.

### Problem Statement
Debugging technical problems is often fragmented and tedious:
1. **Code & Build Errors:** Stack traces, terminal build failures, and cryptic compiler errors require tedious manual searching and formatting.
2. **Circuit & Hardware Issues:** Analyzing breadboards, schematics, pinouts, and floating pull-ups/pull-downs requires cross-referencing multiple component datasheets.
3. **Technical Documentation:** Students struggle to extract high-yield conceptual explanations, mathematical formulas, and viva voce defense questions from dense PDF lab manuals.

### Proposed Solution
OmniSolve AI is an intelligent engineering assistant built to troubleshoot multidisciplinary technical problems through multimodal file analysis. Users select their domain, upload files (screenshots, source files, schematics, or PDFs), and receive structured, actionable diagnostic results with follow-up contextual conversation.

### Main Input → Output Workflow
```
[User Input]
  ├── Multi-file upload (Screenshots, Logs, Schematics, or PDFs)
  ├── Specific inquiry / prompt
  └── Analysis Mode Selection:
        ├── Code Debugger
        ├── Circuit Analyzer
        └── Document Explainer
       │
       ▼
[OmniSolve React + Vite Frontend]
  ├── Client-side MIME & Size Validation (up to 5 files / 50MB)
  ├── Local State & Session Management (Projects & Recent Chats)
  └── REST API Dispatch (`multipart/form-data`)
       │
       ▼
[FastAPI Backend & Inference Harness]
  ├── File Parser & Multi-part Ingestion
  ├── Prompt Orchestration & Schema Enforcement
  └── Local-First AI Engine: Gemma 4 via LM Studio
       │
       ▼
[Structured Output & Follow-up Chat]
  ├── Code Debugger: Root-cause analysis, formatted copyable code fix, verification checklist
  ├── Circuit Analyzer: Identified components/pinouts, hazard checks, multimeter test steps
  ├── Document Explainer: Executive summary, formula breakdowns, viva voce exam Q&As
  └── Contextual Chat: Pinned multi-turn conversation referencing the active files
```

## Approach and technologies

### Architecture Overview
OmniSolve AI implements a clean decoupled client-server architecture:
1. **Client Layer:** Single-Page Application (SPA) providing sub-second UI interactions, instant file drag-and-drop validation, persistent workspace projects, and responsive dark-mode styling.
2. **API Service Layer:** Typed REST client communicating with FastAPI endpoints (`/api/health`, `/api/analyze`, `/api/chat`).
3. **Backend & AI Harness:** FastAPI service orchestrating file parsing, domain-tailored system prompts, and local LLM execution. The architecture strictly isolates model credentials and local inference endpoints from the browser.

### Technology Stack
- **Frontend:**
  - Framework: React 18 with TypeScript
  - Build Tool: Vite
  - Styling: Custom design tokens and modern CSS with dark aesthetics, cyan illuminated glow accents, and responsive layout
  - Icons: Lucide React
  - State & Storage: React hooks, custom state orchestration, and local browser persistence for Projects and Recent Chats
- **Backend & AI Service:**
  - Framework: FastAPI (Python 3.10+)
  - AI Model: Gemma 4 open-weight model
  - Inference Harness: Local inference via LM Studio (OpenAI-compatible local server API) with planned fallback to Google Gemini API
- **Deployment (Planned):**
  - Frontend: Vercel (configuration-ready for static SPA build)
  - Backend: Containerized FastAPI deployment (planned separately)

### Reused Materials & Credits
- Design inspiration: Verity design aesthetic (dark theme, illuminated cyan accents, clean typography).
- Open-source packages: React, Vite, Lucide-react, TypeScript.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI component and its role:**
  OmniSolve AI uses Google's open-weight **Gemma 4** model running locally via LM Studio as its core technical reasoning engine. Rather than using AI merely for code completion during development, Gemma 4 is central to runtime application logic:
  - Performing root-cause analysis on compiler errors and stack traces.
  - Parsing pin configurations and electronic components from schematics.
  - Synthesizing complex engineering formulas and viva questions from technical documentation.
  - Maintaining multi-turn conversational context pinned to specific uploaded files.

- **Code link showing the integration:**
  - Frontend API Service Contract: [TRD.md — Section 3 API Contract Specification](https://github.com/ajaykowkuntla-hash/omnisolve/blob/main/TRD.md#3-api-contract-specification)
  - Frontend REST Client Implementation: [`src/services/api.ts`](https://github.com/ajaykowkuntla-hash/omnisolve/blob/main/src/services/api.ts)
  - Backend API & Model Harness Contract: [`TRD.md`](https://github.com/ajaykowkuntla-hash/omnisolve/blob/main/TRD.md) (FastAPI router connecting `/api/analyze` and `/api/chat` to LM Studio / Gemma 4)

- **Agent Skill Open Standard compliance (if applicable):**
  N/A (OmniSolve AI is built as a client-server web application and inference harness).

- **Original harness implementation or meaningful changes (if applicable):**
  OmniSolve AI incorporates an original domain-specific engineering harness:
  - **Domain Prompting & Schema Formatting:** Enforces structured schema returns (identifying potential hazards, solution code blocks, uncertainty disclaimers, and test steps) rather than unstructured free-form text.
  - **Graceful Connectivity Fallback:** Implements health checks (`/api/health`) and structured client-side mock fallback so the application functions seamlessly for UI testing even when the local AI runner is offline.
  - **Multi-File Context Staging:** Bundles multi-file payloads with domain-specific metadata before submission to the model.

## Current status

### What Works (Implemented & Verified)
- **Frontend User Interface:** Fully functional React + TypeScript + Vite application styled with custom dark-mode aesthetics, cyan glow accents, responsive layout, and Lucide icons.
- **Three Core Engineering Modes:**
  - *Code Debugger:* Supports code files, terminal logs, and error screenshots with syntax-highlighted code solutions and verification checklists.
  - *Circuit Analyzer:* Supports schematic and breadboard image uploads with component breakdown and safety alerts.
  - *Document Explainer:* Supports PDF, text, and markdown files with summaries, formula extractions, and viva exam question generation.
- **Projects & History System:**
  - Create, rename, switch, and delete engineering projects.
  - Project-isolated chat history and global independent chats.
  - Local browser persistence across page reloads.
- **Multi-File Upload Workflow:**
  - Simultaneous multi-file selection via file picker and drag-and-drop.
  - Staging area supporting up to 5 files or 50MB total.
  - Client-side MIME and extension validation per active mode with actionable error alerts.
- **API Integration & Fallback:**
  - Typed API client for FastAPI endpoints (`/api/health`, `/api/analyze`, `/api/chat`).
  - Active backend health monitoring pill (Online / Offline indicator).
  - Deterministic mock simulation mode for offline testing and evaluation.

### Known Limitations / Incomplete Features
- **Cloud Deployment:** Live frontend deployment to Vercel and hosted backend deployment are planned but not yet deployed; the project currently runs locally.
- **Local Model Dependency:** Local execution of Gemma 4 requires LM Studio running on the host machine (`http://localhost:1234` or configured host port).
- **Advanced Optical Circuit Recognition:** Breadboard component recognition is reliant on model vision resolution and prompt tuning; complex multi-layer PCB schematics may require manual component verification.

### What You Would Improve Next
1. **Live Cloud Deployment:** Deploy the React frontend to Vercel and containerize the FastAPI backend on Google Cloud Run with GPU/Gemini API integration.
2. **Interactive Circuit Simulation:** Integrate a WebAssembly-based SPICE engine to simulate circuits directly in the browser based on schematic analysis.
3. **Repository Analysis:** Expand Code Debugger to ingest full GitHub repositories via Git clone or zip archive for whole-project debugging.
4. **Live Code Runner:** Provide an in-browser sandbox (e.g., Pyodide / WebContainer) to execute and test recommended code solutions directly.

## Team Contributions

- **Ajay Kowkuntla ([@ajaykowkuntla-hash](https://github.com/ajaykowkuntla-hash)):**
  - Designed and implemented the complete React + TypeScript + Vite frontend application.
  - Engineered the ChatGPT-style Projects system, Recent Chats navigation, and local storage state persistence.
  - Built the multi-file drag-and-drop upload staging system with client-side validation rules.
  - Implemented the custom dark UI theme with illuminated cyan accents and responsive layout.
  - Developed the API client service layer, backend health monitoring, and realistic mock fallback engine.

- **Amay Raj ([@<amay-raj-github-username>](https://github.com/<amay-raj-github-username>)):**
  - Engineered the FastAPI backend service architecture and REST API routing (`/api/health`, `/api/analyze`, `/api/chat`).
  - Integrated the open-weight Gemma 4 model via local LM Studio harness.
  - Designed the prompt orchestration and structured JSON response schemas for Code Debugger, Circuit Analyzer, and Document Explainer modes.
  - Structured multi-turn session persistence and context handling for file-pinned chat conversations.

## Setup & Demo Instructions

### Prerequisites
- Node.js (v18.0.0 or higher, v20+ recommended)
- npm (v9.0.0 or higher)
- Python 3.10+ (for FastAPI backend)
- LM Studio with Gemma 4 model downloaded (for local AI inference)

### 1. Frontend Setup
```bash
# Clone the repository
git clone https://github.com/ajaykowkuntla-hash/omnisolve.git
cd omnisolve-ai

# Install dependencies
npm install

# Configure environment variables
cp .env.example .env
# Set VITE_API_BASE_URL=http://localhost:8000 in .env

# Run development server
npm run dev
```
The frontend interface will be available at `http://localhost:5173`.

### 2. Backend & Model Setup
```bash
# In the backend directory
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# Start LM Studio, load Gemma 4, and start local server on port 1234
# Run FastAPI server
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

### 3. Quick Demo Walkthrough (Offline / Mock Mode Supported)
1. Open `http://localhost:5173`. If the backend is not yet started, the connectivity pill indicates `Backend Offline` and OmniSolve operates seamlessly in Mock Demonstration Mode.
2. Select **Code Debugger**, click the sample prompt or drop an error log / code file, and click **Analyze & Debug**.
3. Review the root cause diagnosis, syntax-highlighted code patch, and verification checklist.
4. Use the Contextual Chat panel to ask a follow-up question regarding the fix.
5. Switch to **Circuit Analyzer** or **Document Explainer** to explore hardware schematic and PDF documentation workflows.
6. Open the sidebar to create a new Project and observe project-specific conversation isolation.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

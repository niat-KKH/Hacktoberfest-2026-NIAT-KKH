# RepoBlueprint AI — From Napkin Sketch to Production Repo

## Team / attendee

- Team name: Team Blueprint
- Members and GitHub usernames: @satvik310706 , @NIKHIL2K5 and team
- Hackathon: Hacktoberfest Hack Day 2026 · Hyderabad (NIAT-KKH × MLH)

## Challenge

- [x] Best Open-Source AI Project
- [x] Best Use of Gemma 4

## Project links

- Public GitHub repository: [RepoBlueprint-AI](https://github.com/satvik310706/RepoBlueprint-AI)
- Open-source license: [MIT License](https://github.com/satvik310706/RepoBlueprint-AI/blob/main/LICENSE)

## Problem and solution

Every developer and hackathon builder experiences the same moment: you have a burst of inspiration, grab a napkin, iPad, or whiteboard, sketch a few boxes and arrows, and then spend hours manually figuring out:
1. What tech stack fits best without over-engineering?
2. What are the free tiers vs production cloud costs?
3. What files, directories, and boilerplate code do I need to start writing real features?
4. Are there existing similar repositories on GitHub I can learn from or build upon?

**RepoBlueprint AI** is an autonomous AI Technical Co-Founder that turns any napkin sketch, chalkboard photo, or 2-sentence thought into a complete production open-source blueprint in seconds:
1. **Interactive In-Browser Whiteboard**: Sketch components directly on the digital blueprint canvas, or upload a photo of your paper/whiteboard.
2. **Multimodal Architecture Synthesis**: Google Gemma 4 & Gemini Vision trace every component, user flow, and database relation.
3. **AI Alignment Interview**: Conducts a technical co-founder alignment loop asking 3-4 targeted architecture questions to lock down database, auth, and scaling paths.
4. **Master Architecture Dashboard**:
   - 🌐 Interactive Mermaid.js system topology with zoom, pan, code export, and vector SVG download.
   - ⚙️ Recommended production stack with explicit trade-off justifications and alternative options.
   - 💰 Interactive hosting cost calculator with a live DAU slider (500 to 100k users) estimating compute, database, and token bills.
   - 🔎 Real-time GitHub intelligence searching public repositories in the space with stars, forks, and tags.
   - 📦 1-Click Starter Repo generator that packages real working boilerplate code and docker-compose into a downloadable `.zip`.
   - ⏱️ Sprint roadmap with interactive checkboxes to track 24-hour sprint milestones in real time.

## Approach and technologies

- **Frontend**: React 18, Vite, Vanilla CSS Design System with Cyber-Architectural Aesthetics and Glassmorphism.
- **Multimodal AI Reasoning & Speech**:
  - Google Gemma & Gemini API (`gemini-2.5-flash`, `gemini-2.0-flash`, `gemini-1.5-flash` with resilient automatic fallback).
  - Local Ollama Engine integration (`llama3.2:3b`, `gemma3:27b`, `mistral:7b`) with Vite reverse proxy and GPU acceleration for 100% private zero-cloud egress execution.
  - Native Web Speech API Voice Dictation engine allowing real-time speech transcription with live waveform visualization.
  - Zero-friction Instant Sandbox Demo engine with pre-computed production blueprints for instant presentation.
- **Visuals & Diagramming**: Mermaid.js dynamic dark-mode rendering with custom SVG export.
- **Repo Packaging**: JSZip and FileSaver for client-side multi-file project `.zip` bundle generation.
- **Live Search**: GitHub REST API v3 integration with debouncing and curated offline fallbacks.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI component**: Full native support for open-weight models including **Llama 3.2 (3B)**, Google's **Gemma 3 (27B/12B)**, and **Mistral (7B)** via local Ollama.
- **Code link showing integration**: [`src/services/gemini.js`](https://github.com/satvik310706/RepoBlueprint-AI/blob/main/src/services/gemini.js) (supports both local Ollama REST endpoints with JSON schema mode and Google GenAI SDK).
- **Client privacy guarantee**: API keys and prompt vectors are held strictly in browser client memory with zero remote telemetry.

### Best Use of Gemma 4

- **Model identifier & Gemini API integration**: Leverages Google GenAI with multimodal vision understanding (`gemini-2.5-flash` / Gemma open model harness).
- **Code link showing integration**: [`src/services/gemini.js`](https://github.com/satvik310706/RepoBlueprint-AI/blob/main/src/services/gemini.js) (multimodal inline image ingestion + architecture parser).
- **Multimodal Value**: Users can draw boxes and arrows on paper or the built-in napkin canvas, or speak their idea via voice dictation. The model parses the spatial relationships (e.g. `Client --> API Gateway --> PostgreSQL`) and converts raw visual doodles into structured JSON schemas and executable boilerplate files.

## Current status

### What works
- [x] Autonomous AI Co-Founder Copilot drawer accessible on every single stage with context-aware guidance, multilingual voice speech-out (TTS in 8 languages), and direct live DOM page action execution without raw JSON leakage.
- [x] Local Ollama integration with automatic model detection (`llama3.2:3b`, `gemma3:27b`) and GPU acceleration with zero-cloud egress.
- [x] In-browser Real-Time Voice Dictation supporting 8 languages (English, Telugu, Hindi, Spanish, French, German, Japanese, Chinese) with live Web Audio API VU meter and continuous pause recovery.
- [x] In-browser interactive Napkin Whiteboard with pen, component box, text / labeling tool, arrow connector, eraser, and 1-click architecture stencils (`Web UI`, `API Gateway`, `PostgreSQL`, `Redis`, `AI Worker`, `Auth`).
- [x] Whiteboard and chalkboard photo drag-and-drop upload with Gemini vision analysis.
- [x] Multimodal image & text idea understanding via Gemini & Gemma.
- [x] Interactive AI Co-founder Alignment Phase with smart questions, toggleable innovations, and 1040px spacious layout.
- [x] Interactive Mermaid.js architecture graph with dynamic light/dark mode adaptation, zoom controls, and SVG export.
- [x] Interactive cloud hosting cost calculator with dynamic DAU slider (500 to 100k users) and free tier launch matrix.
- [x] Live GitHub similar repository search via GitHub REST API with offline fallbacks.
- [x] 1-Click Starter Repo packaging and instant `.zip` download with working boilerplate code.
- [x] Interactive 24-hour sprint milestone checklist with velocity progress indicator.
- [x] Zero-friction Instant Demo mode for offline or judge evaluation without requiring an API key.
- [x] Claude.ai-inspired warm editorial design system (warm ivory light mode `#FAF9F5`, charcoal dark mode `#262624`, clay accent `#D97757`).

### Known limitations / incomplete features
- Direct push to a user's GitHub account requires personal access tokens (currently user downloads `.zip`).

### What we would improve next
- GitHub OAuth integration to automatically initialize a new public repository and commit files directly from the browser.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license (MIT License).
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

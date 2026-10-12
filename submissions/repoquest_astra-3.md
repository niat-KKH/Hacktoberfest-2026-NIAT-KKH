# RepoQuest — AI-Powered Open-Source Exploration & Adventure Game

## Team / attendee

- Team name (if applicable): Astra 3
- Members and GitHub usernames:
  - Abhishek Swargam — [@Abhishek391-glitch](https://github.com/Abhishek391-glitch) (Team Lead / Full-Stack & AI Engineer)
  - Prahas — [@SangamPrahas](https://github.com/SangamPrahas) (Frontend & Gameplay Developer)
  - Keerthan Jakkaraju — [@Keerthanjakkarajucodes](https://github.com/Keerthanjakkarajucodes) (Backend & Core Systems Developer)
  - *Unofficial Contributor / Project Mentor (Not part of competitive team)*: Nikhil — [@NIKHIL2K5](https://github.com/NIKHIL2K5) (Guided throughout ideation, architecture, planning, and execution)
- Profile links (optional):
  - Abhishek Swargam: https://github.com/Abhishek391-glitch
  - Prahas: https://github.com/SangamPrahas
  - Keerthan Jakkaraju: https://github.com/Keerthanjakkarajucodes
  - Nikhil (Mentor & Guide): https://github.com/NIKHIL2K5

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [ ] Best Use of Gemma 4

## Project links

- Public GitHub repository: https://github.com/Abhishek391-glitch/Repogame
- Open-source license (link to the license file): MIT License (https://github.com/Abhishek391-glitch/Repogame/blob/main/LICENSE)

## Problem and solution

### Who is this for?
First-time open-source contributors, student developers, and engineers who want to explore and understand unfamiliar codebases without feeling intimidated or lost in massive repositories.

### What problem dogit commit -m "Add project submission"es it solve?
Diving into open-source projects is daunting for newcomers:
1. **Intimidating Codebases**: Beginners struggle to know where execution starts, how modules communicate, or how configurations and test suites connect.
2. **Cloning & Setup Overhead**: Cloning gigabytes of code, configuring local toolchains, and pulling dependencies just to understand a repository is wasteful and high-friction.
3. **Sparse Architectural Documentation**: README files often assume prior domain knowledge and omit beginner-friendly component explanations.
4. **Dry Learning Experiences**: Existing learning tools are either passive documentation readers or quiz-based tests that quiz users rather than showing them how code functions.

**RepoQuest solves this by turning open-source exploration into an interactive game world.** It inspects any public GitHub repository without cloning or running untrusted code, creates an interactive 2D adventure game world and architecture map, and guides developers through evidence-grounded discoveries powered by local open-weight AI.

### Main input → output workflow:
1. **Input**: A public GitHub repository URL or identifier (e.g., `facebook/react`, `pallets/flask`, `fastapi/fastapi`).
2. **Processing**:
   - The FastAPI backend queries the GitHub REST API asynchronously to analyze repository trees, manifests, documentation, and source files.
   - Files are classified into structural stations (Entry Points, Build Manifests, Core Services, Test Suites, Contribution Guidelines).
   - Ingested files are passed to a locally hosted open-weight LLM (**Qwen 2.5 7B Instruct** via Ollama) with strict prompt boundaries to generate grounded file explanations and answer developer questions.
3. **Output & Interactive Experience**:
   - **Code Adventure 2D Game (Quests Tab)**: A top-down 2D canvas game built with Phaser. Players navigate an explorer character through repository stations, inspect key files in context, unlock zones, and earn XP and badges upon discovering milestones.
   - **Multilingual AI Guide with Voice (Ask Repo Tab)**: An intelligent conversational AI mentor that explains code architecture in plain language, supporting English, Hindi, Telugu, and Tamil with built-in voice speech recognition (mic input) and voice speech synthesis (audio playback).
   - **Interactive Architecture Map**: A visual dependency and folder graph built with React Flow (`@xyflow/react`).
   - **Contributor Readiness Dashboard**: Automated newcomer scorecard evaluating README quality, contribution docs, testing setups, issue templates, and licenses.

## Approach and technologies

### Implementation & Architecture
- **Frontend**: Built with **React 18**, **Vite**, and **TypeScript**. Utilizes **Phaser** for 2D game mechanics, **Tailwind CSS** for styling, **@xyflow/react (React Flow)** for interactive architectural maps, **Lucide React** for icons, **Canvas Confetti** for celebration effects, and the native **Web Speech API** for multilingual voice recognition and speech synthesis.
- **Backend**: Built with **Python 3.10+** and **FastAPI**. Employs asynchronous HTTP (`httpx`) to interface with GitHub's REST API, `pydantic-settings` for type safety, and `aiosqlite` for asynchronous SQLite persistence of user progress, discoveries, badges, and levels.
- **Open-Source AI Model**: **Qwen 2.5 7B Instruct** (`qwen2.5:7b-instruct`) executed locally via **Ollama** (`http://localhost:11434`).

### Why we chose these tools:
- **Local Open-Weight AI (Qwen 2.5 7B Instruct via Ollama)**: Eliminates dependence on proprietary cloud AI APIs, removes token subscription costs, ensures 100% developer privacy, and runs smoothly on standard consumer GPUs (such as 6GB VRAM) or CPU fallbacks.
- **Phaser 2D Canvas Engine**: Turns static code reading into a spatial exploration game that builds visual and conceptual memory of the codebase.
- **FastAPI + Async SQLite**: Delivers sub-millisecond API response times for gameplay events, maintains bounded context memory, and safely tracks exploration progress without modifying user files.

### Reused libraries, datasets, starter code, and AI-assisted development:
- **Open-Source Libraries**: Phaser, React, Vite, Tailwind CSS, `@xyflow/react`, FastAPI, `httpx`, `aiosqlite`, `pytest`.
- **Open-Weight AI Model**: Qwen 2.5 7B Instruct developed by the Qwen Team / Alibaba Cloud, served via Ollama.
- **Mentorship & Planning**: Architecture, scoping, and execution guidance provided by Nikhil ([@NIKHIL2K5](https://github.com/NIKHIL2K5)).
- **AI-Assisted Development**: Leveraged IDE AI tools during the hackathon for rapid component scaffolding, boilerplate reduction, and writing comprehensive test suites.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI component and its role**:
  - **Component**: **Qwen 2.5 7B Instruct** (`qwen2.5:7b-instruct`), an open-weight foundation LLM running locally via Ollama (`http://localhost:11434`).
  - **Role**: Powers the intelligence layer of RepoQuest:
    1. **Context-Grounded File Explanations**: Summarizes unfamiliar source code into concise beginner explanations, pointing out core logic, inputs/outputs, and how the file fits into the larger application.
    2. **Multilingual Architecture Guide**: Answers developer questions grounded directly in the inspected repository's code files across multiple regional languages (English, Hindi, Telugu, Tamil).
    3. **Hallucination-Proof Grounding**: Uses bounded context windows and strict retrieval prompts, refusing speculation and citing specific file paths.
- **Code link showing the integration**:
  - Local Ollama AI Client Service: [`backend/app/services/ollama_service.py`](https://github.com/Abhishek391-glitch/Repogame/blob/dev/backend/app/services/ollama_service.py)
  - AI File Explanation and Chat Endpoints: [`backend/app/api/routes_ai.py`](https://github.com/Abhishek391-glitch/Repogame/blob/dev/backend/app/api/routes_ai.py)
  - Multilingual AI Chat & Voice Interface: [`frontend/src/components/AskRepo.tsx`](https://github.com/Abhishek391-glitch/Repogame/blob/dev/frontend/src/components/AskRepo.tsx)
  - Repository Game Spec & Station Analyzer: [`backend/app/services/game_spec_service.py`](https://github.com/Abhishek391-glitch/Repogame/blob/dev/backend/app/services/game_spec_service.py)
- **Agent Skill Open Standard compliance (if applicable)**:
  - N/A (Project is an end-to-end open-weight application).
- **Original harness implementation or meaningful changes (if applicable)**:
  - Implemented a custom asynchronous Python inference and prompt harness (`ollama_service.py`) featuring:
    - **Bounded context safeguards**: Capped at 8,000 characters to prevent VRAM overflow on 6GB GPUs.
    - **Prompt injection defenses**: System boundaries treating repository file contents as untrusted raw strings.
    - **Graceful heuristic fallback**: When the local Ollama daemon is offline or starting up, the system switches automatically to deterministic heuristic explanations, guaranteeing 100% application uptime.
    - **Multilingual prompt orchestration**: Specialized system prompting supporting regional Indian languages (Hindi, Telugu, Tamil) while preserving programming terminology and code snippets in English.

## Current status

- **What works**:
  - Playable 2D Code Adventure world with player navigation, station beacons, discovery modals, persistent XP, badges, and completion fireworks.
  - Zero-clone repository analysis via the GitHub REST API for public repositories.
  - Local AI file explanations and conversational Q&A powered by Qwen 2.5 7B Instruct via Ollama.
  - Full voice support: speech-to-text microphone input and speech-to-text audio synthesis in the AI guide.
  - Interactive React Flow architectural graph view.
  - Automated contributor readiness scorecard.
  - Async SQLite database storing user progress, levels, discoveries, and achievements.
  - Automated testing suite: 17/17 pytest tests passing and zero TypeScript errors (`npm run build` succeeds).
- **Known limitations / incomplete features**:
  - Full AI intelligence requires Ollama running locally with `qwen2.5:7b` (gracefully degrades to offline heuristic explanations if unavailable).
  - Repositories larger than 400 files are bounded to prevent UI and memory bottlenecks.
  - Private repositories require configuring a GitHub personal access token in `.env`.
- **What you would improve next**:
  - In-browser sandbox challenges allowing users to write small test fixes and submit mock PRs directly in the game.
  - Multi-player cooperative repository exploration for hackathon teams and onboarding cohorts.
  - In-browser WebLLM support to run smaller open-weight models (e.g. Qwen 2.5 1.5B/3B) entirely inside the browser without needing Ollama installed.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

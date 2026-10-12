# DebugYourCode (DYC) — Dynamic 3D Execution IDE & AI Tutor

**Tagline:** Write It. Run It. See It. Understand It.

## Team / attendee

- Team name (if applicable): codebuddies
- Members and GitHub usernames: N Dinesh - https://github.com/nalliboinadinesh,  
                                M Abinav Sosa - https://github.com/abinav04, 
                                G Satish - https://github.com/Satish-2004
- Profile links (optional): 

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [] Best Use of Gemma 4

## Project links

- Public GitHub repository: https://github.com/nalliboinadinesh/debug_your_code
- Open-source license (link to the license file): 

## Problem and solution

### Who is this for?
Computer science students, educators, and Data Structures and Algorithms (DSA) learners who struggle to understand how abstract code executes line-by-line, how pointers move, and how memory evolves.

### What problem does it solve?
1. **Cryptic Debuggers:** Standard CLI debuggers (GDB/LLDB) are text-heavy, intimidating, and lack visual clarity for students.
2. **Static Diagrams:** Textbook slides and whiteboard diagrams cannot convey dynamic runtime state changes, loop mutations, or pointer traversals.
3. **AI Hallucinations:** When students paste code into general-purpose LLMs, the models frequently hallucinate variable states, miscalculate loop iterations, and guess memory values.

### Main Input → Output Workflow:
1. **Input:** The student writes or pastes C++ code (with optional custom input) into a modern code editor.
2. **Deterministic Tracing:** The backend compiles the program in an isolated environment and runs an automated GDB tracer that captures ground-truth runtime events (variables, vectors, arrays, pointers, call stack frames, and hash sets).
3. **Stage 3 Visualization Planner:** Trace events are converted into a versioned `VisualizationPlan` that coordinates objects and timeline actions.
4. **Interactive 3D Stage:** The frontend renders an interactive 3D WebGL scene (3D array cells, animated pointer tags like `low`, `mid`, `high`, call stack blocks, and sorting bars) that students can scrub backward and forward in time.
5. **Gemma 4 AI Tutor:** Powered locally by Gemma 4, an integrated AI tutor explains each step with zero hallucinations because its explanations are strictly grounded in verified GDB execution data.

## Approach and technologies

- **Backend & Sandbox:** FastAPI (Python 3.14), Linux/WSL GDB (GNU Debugger) with automated STL filter skipping (`skip -rfu ^std::`), GCC/G++ (`-g3 -O0 -std=c++17`), Pydantic v2 schemas.
- **Frontend & 3D Visualization:** React 19, TypeScript, Vite, Three.js, `@react-three/fiber`, `@react-three/drei`, Tailwind CSS, Lucide icons, responsive LeetCode-style draggable splitter layout.
- **Local AI Engine:** Local **Gemma 4 (`gemma4:12b`)** running via Ollama (`http://localhost:11434`), delivering private, low-latency on-device inference with deterministic trace grounding.
- **5-Stage Architecture:** 
  1. *Stage 1 (Code Understanding)* — AST analyzer & Gemma 4 pattern detection.
  2. *Stage 2 (Runtime Execution Tracer)* — Deterministic GDB step-by-step memory capture.
  3. *Stage 3 (Visualization Planner)* — Decoupled JSON specification bridging runtime state to visual objects.
  4. *Stage 4 (Dynamic 3D Visualizer)* — Three.js rendering engine with perspective autofit and active pointer tagging.
  5. *Stage 5 (AI Tutor)* — Interactive step explanation and Big-O derivation engine.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI component and its role:** 
  Uses the open-weight **Gemma 4 (12B)** model running locally via Ollama as an intelligent pedagogical tutor and code analyzer. It explains complex algorithm steps, justifies pointer movements, and explains mathematical time/space complexity derivations.
- **Code link showing the integration:** 
  - AI Adapter: [`backend/app/ai/adapter.py`](backend/app/ai/adapter.py)
  - Visualization Planner: [`backend/app/ai/planner.py`](backend/app/ai/planner.py)
  - Runtime GDB Tracer: [`backend/app/tracing/gdb_driver.py`](backend/app/tracing/gdb_driver.py)
- **Original harness implementation or meaningful changes:** 
  Implemented a dual-engine architecture where the LLM is never allowed to hallucinate variable values or array states. Instead, a custom GDB driver captures ground-truth memory states and feeds them as verified context into Gemma 4, combining the pedagogical reasoning of an LLM with the mathematical precision of a native debugger.

### Best Use of Gemma 4

- **Gemma 4 model identifier and API integration:** 
  Model: `gemma4:12b` (Q4_K_M, 11.9B parameters, 262K context window). Integrated through Ollama's local chat endpoint (`/api/chat`) with latency-optimized inference (`"think": False`, custom prompt templating, and sub-3-second responses).
- **Code link showing the integration:** 
  - [`backend/app/ai/adapter.py`](backend/app/ai/adapter.py) (Ollama connection & prompt formatting)
  - [`backend/app/ai/prompts.py`](backend/app/ai/prompts.py) (Pedagogical prompt templates)
  - [`backend/app/routes/ai.py`](backend/app/routes/ai.py) (Tutor explanation endpoints)
- **Input and useful output:**
  - *Input:* Current line of code, active variable state, mutation delta (e.g. `mid` updated from 32767 to 3), current array/set snapshot (`arr = [10, 20, 30, 40, 50, 60, 70]`), and the student's question.
  - *Output:* Concise 1–2 sentence pedagogical explanations explaining *why* variables changed, how search boundaries narrowed, and how time complexity $O(\log N)$ or $O(N)$ is derived.

## Current status

- **What works:**
  - Full C++ compile, execute, and step-by-step tracing (supports scalars, vectors, arrays, pointers, recursion, and sets/hash sets like `std::unordered_set`).
  - Interactive 3D visualization with auto-fitting perspective camera, dynamic pointer indicators (`low`, `mid`, `high`), cell highlight animations, and dual 2D/3D views.
  - LeetCode-style resizable draggable layout with vertical and horizontal splitters.
  - Instant algorithmic complexity analysis (Best, Average, Worst case $O(N)$, $O(\log N)$, $O(N^2)$, Auxiliary Space).
  - Local Gemma 4 AI tutor providing instant verified step explanations and complexity insights.
- **Known limitations / incomplete features:**
  - Currently optimized for C++ (Python and Java runtime tracing engines are in progress).
  - Graph and tree structures currently render in 2D cards while the 3D node-link graph layout is under active development.
- **What you would improve next:**
  - Multi-language execution support (Python, Java, Rust).
  - 3D heap memory visualization with pointer reference arrows and dynamic node allocation (`new` / `malloc`).
  - Interactive voice tutoring mode using Gemma.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

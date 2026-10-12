# CodeScope

## Team / attendee

- Team name (if applicable): astra 2.5
- Members and GitHub usernames: Nikhil K (@NIKHIL2K5)
- Profile links (optional): https://github.com/NIKHIL2K5

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [ ] Best Use of Gemma 4

CodeScope is built primarily for the **Best Open-Source AI Project** track as a 100% offline, privacy-first developer intelligence platform using local open-weight models via Ollama. It also supports Google's open-weight Gemma models such as `gemma2:2b` out of the box.

## Project links

- Public GitHub repository: https://github.com/NIKHIL2K5/codescope
- Open-source license (link to the license file): https://github.com/NIKHIL2K5/codescope/blob/main/LICENSE

## Problem and solution

### Who is this for?

Open-source contributors, software engineers, and engineering teams who need to onboard to unfamiliar codebases, understand complex architectures, analyze the blast radius of code changes, and safely open pull requests without breaking production or downstream dependencies.

### What problem does it solve?

1. **Onboarding Friction & Cognitive Overload**: New contributors struggle to grasp multi-file architectures, symbol hierarchies, and module boundaries in large repositories.
2. **Hidden Regressions & Blast Radius**: Making changes to functions or classes often triggers undetected regressions across dependent files and tests that developers are not aware of.
3. **Contribution Hurdles**: First-time contributors frequently miss project-specific contribution guidelines, forget unit tests, overlook uncommitted files, and struggle to format pull request descriptions.
4. **Privacy & Proprietary Code Leaks**: Cloud-hosted AI tools upload proprietary code and repository tokens to external servers. CodeScope runs 100% locally and offline with zero telemetry.

### Main input -> output workflow

- **Input**: A local directory path (for example, `C:\Projects\app` or `/home/user/app`) or a public GitHub clone URL (`https://github.com/owner/repo.git`).
- **Processing**:
  1. **AST Parsing**: An in-process WebAssembly Tree-sitter engine parses code into an Abstract Syntax Tree (AST), indexing functions, classes, imports, and definitions across TypeScript, JavaScript, and Python.
  2. **Git Version History Traversal**: Native Git CLI queries extract commit history, blame, uncommitted working tree diffs, historical bug-fix heuristic tags, and co-changed tests.
  3. **Blast Radius Traversal**: The PR Impact Radar calculates an explainable risk score (`LOW`, `MEDIUM`, `HIGH`) by walking the dependency call graph.
  4. **Local Open-Weight AI Reasoning**: LEO (Local Open-Weight AI Assistant via Ollama) consumes deterministic repository evidence to answer architectural questions, explain diff risks, and propose patches with line-level citations.
- **Output**:
  1. An interactive, navigable multi-file architecture DAG (`@xyflow/react`) synced with a real-time syntax viewer.
  2. Blast radius impact report with affected symbols, risk level, and suggested test suites.
  3. Codebase Time Machine showing historical bug fixes and co-changed test files.
  4. Real-time Contribution Cockpit with live `git status` verification, 7-step copyable terminal commands, interactive pre-push checklist, and a 1-click PR markdown generator.

## Approach and technologies

### Implementation, Models, and Architecture

- **Frontend**: Built with **React 18**, **TypeScript**, **Vite**, and **Tailwind CSS**. Graph visualization is powered by **`@xyflow/react`** for smooth pan, zoom, minimap, and node-edge interaction, paired with Lucide React icons.
- **Backend**: **Node.js** with Express and TypeScript, using Prisma ORM with SQLite for zero-configuration, local persistence of repository graphs, symbols, and conversations.
- **AST Parsing Engine**: **`web-tree-sitter`** with precompiled WebAssembly (WASM) grammars (`tree-sitter-wasms`) for high-speed, in-process syntax parsing of TypeScript, JavaScript, and Python without external runtimes.
- **Git Engine**: Direct Git CLI subprocess execution (`child_process.execFile`) with strict argument arrays to eliminate shell injection risks.
- **Local Open-Weight AI ("LEO")**: Powered by **Ollama** running locally on `http://127.0.0.1:11434` with open-weight models (for example, `qwen2.5:0.5b`, `qwen2.5:1.5b`, or Google's `gemma2:2b`). AI prompts are strictly grounded via a deterministic AST and Git retrieval service. Responses are constrained with Zod JSON schemas (`aiExplanationResponseSchema`). An automatic fallback ensures full system functionality even when Ollama is offline.
- **Testing**: **Vitest** for backend and frontend unit/integration testing with mocked repository fixtures.

### Reused Libraries, Starter Code & Credits

- `@xyflow/react`: Interactive DAG architecture diagramming.
- `web-tree-sitter` & `tree-sitter-wasms`: In-browser and Node.js WebAssembly AST parsing.
- `prisma` & `@prisma/client`: Type-safe SQLite database access.
- `ollama`: Local open-weight LLM inference runtime.
- No third-party template or starter boilerplate used; project built from scratch as a modular monorepo.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI component and its role**:
  - **LEO (Local Open-Weight AI Assistant)**: Integrates local open-weight models via Ollama (`qwen2.5:0.5b`, `qwen2.5:1.5b`, `gemma2:2b`).
  - **Role**: Serves as an on-device repository intelligence copilot. Performs grounded Q&A, explains PR blast radius risks, details test rationale, and generates candidate patch proposals without sending any source code to third-party cloud APIs.
- **Code link showing the integration**:
  - [Ollama Client & Schema-Constrained Inference Harness](https://github.com/NIKHIL2K5/codescope/blob/main/apps/api/src/ai/ollamaClient.ts)
  - [AI Assistant Service with Grounded Evidence & Offline Auto-Healing](https://github.com/NIKHIL2K5/codescope/blob/main/apps/api/src/services/aiAssistantService.ts)
  - [AST & Git Evidence Retrieval Service](https://github.com/NIKHIL2K5/codescope/blob/main/apps/api/src/services/retrievalService.ts)
  - [Candidate Patch Proposal Generator](https://github.com/NIKHIL2K5/codescope/blob/main/apps/api/src/services/patchService.ts)
- **Agent Skill Open Standard compliance (if applicable)**:
  - Structured tool calling and context grounding with strict input/output DTOs defined in `@codescope/shared`. Prompts are strictly assembled using deterministic AST symbols, line numbers, and Git blame records to avoid hallucinations.
- **Original harness implementation or meaningful changes**:
  - Implemented an original **Grounded Retrieval-Augmented Harness** combining WASM Tree-sitter symbol indexing with Git history traversal.
  - Engineered strict JSON extraction with Zod schema validation (`summary`, `keyRisks`, `testRationale`) and low inference temperature (0.1–0.2).
  - Built an offline fallback mechanism: if Ollama is unreachable, CodeScope automatically extracts and displays verified repository evidence citations without breaking the user experience.

### Best Use of Gemma 4

*(Complete if evaluated under Gemma track)*

- **Gemma 4 model identifier and Gemini API integration**:
  - CodeScope natively supports Google's open-weight Gemma family (for example, `gemma2:2b`, `gemma:2b`, or local Gemma 4 deployments) through its configurable Ollama inference engine (`OLLAMA_MODEL=gemma2:2b` or custom Gemma tag) running on `http://127.0.0.1:11434`.
- **Code link showing the integration**:
  - [Ollama Client Multi-Model Integration](https://github.com/NIKHIL2K5/codescope/blob/main/apps/api/src/ai/ollamaClient.ts#L13-L128)
  - [Environment Configuration for Model Swapping](https://github.com/NIKHIL2K5/codescope/blob/main/apps/api/src/config/index.ts)
- **Input and useful output; multimodal value where applicable**:
  - **Input**: Bounded AST symbol graphs, modified diff snippets, and historical fix heuristics.
  - **Output**: Structured risk analysis, regression warnings, test rationale, and localized patch proposals.

## Current status

- **What works**:
  - Ingestion of local project directories and public GitHub repositories.
  - WebAssembly Tree-sitter parsing for JavaScript, TypeScript, and Python.
  - Interactive multi-file architecture DAG (`@xyflow/react`) with symbol search, minimap, and node inspection.
  - Bi-directional synced source code viewer.
  - Two-commit revision comparison and uncommitted working tree diff visualizer.
  - Codebase Time Machine with historical bug-fix heuristics and co-changed test discovery.
  - PR Impact Radar with blast radius risk scoring (`LOW`, `MEDIUM`, `HIGH`) and sandboxed test execution with approval safeguards.
  - LEO local AI assistant with multi-stage thinking, grounded citations, conversation persistence, and offline fallback.
  - Real-time Step 3 Contribution Cockpit: Live working tree status (`git status`), 7-step copyable terminal workflow, interactive pre-push checklist, and 1-click GitHub PR markdown generator.
- **Known limitations / incomplete features**:
  - AST parsing currently focuses on JavaScript, TypeScript, and Python; languages such as Rust, Go, or Java require compiling additional Tree-sitter WASM grammar binaries.
  - Cross-module symbol resolution in large monorepos uses relative import paths rather than a full TypeScript type-checker compiler API.
  - Local AI inference speed is constrained by the host machine's hardware (CPU vs GPU availability).
- **What you would improve next**:
  - Add precompiled Tree-sitter WASM grammars for Go, Rust, Java, and C/C++.
  - Integrate hybrid semantic search using local vector embeddings (for example, `sqlite-vec`).
  - Direct GitHub API integration to push branches and open PRs directly from the web cockpit.
  - Multi-agent collaboration harness where one agent drafts refactoring patches and a secondary agent verifies AST invariants.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

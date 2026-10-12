# RepoPilot

## Team / attendee

- Team name (if applicable): RepoPilot
- Members and GitHub usernames:
  - Sri Pavan Tej Balam (@sripavantejb)
  - Supritha Gurazala (@Supritha-gurazala)
  - Hafsa (@hafsa2006)
- Profile links (optional):
  - https://github.com/sripavantejb
  - https://github.com/Supritha-gurazala
  - https://github.com/hafsa2006

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [x] Best Use of Gemma 4

## Project links

- Public GitHub repository: https://github.com/sripavantejb/repopilot
- Open-source license (link to the license file): https://github.com/sripavantejb/repopilot/blob/main/LICENSE
- Live Web Application: https://repopilot-orcin.vercel.app/

## Problem and solution

### Who is this for?
Software engineers, open-source maintainers, hackathon participants, and engineering leads who face cognitive overload when onboarding to large, unfamiliar codebases or triaging backlogs of pull requests.

### What problem does it solve?
1. **Codebase Opacity:** Reading source code file-by-file makes it painful to visualize architectural dependencies, module relationships, and the blast radius of changes.
2. **PR Review Fatigue:** Reviewing dozens of PRs with conventional web diffs is slow and high-friction, leading to review backlogs, delayed merges, and overlooked breaking changes.

### Main input → output workflow:
1. **Architecture Discovery (Gitora Engine):**
   - **Input:** GitHub repository URL (e.g. `owner/repo`).
   - **Processing:** The engine parses the repository structure, generates deterministic AST topologies and module relationships, and renders an interactive Mermaid dependency graph.
   - **Output:** Visual architectural map with module breakdowns, entry-point detection, and 1-click **"Explain with Gemma ✨"** contextual inspections.
2. **Grounded Codebase Intelligence (Gemma 4):**
   - **Input:** Selected AST node / module + developer question (e.g., *"How does authentication work here?"*, *"What breaks if I modify this module?"*).
   - **Processing:** Gemma 4 (`gemma-4-26b-a4b-it`) reasons over the extracted AST context, file relationships, and symbol references.
   - **Output:** Source-grounded explanations with direct file references, data-flow walkthroughs, and dependency risk assessments.
3. **High-Velocity PR Triage (RepoPilot / SwiPR Engine):**
   - **Input:** Open pull requests fetched via the GitHub Octokit REST API.
   - **Processing:** Analyzes diffs, computes blast radius, calculates confidence vs. impact risk scores, and checks CI pass/fail status.
   - **Output:** Keyboard-driven, swipe-based review deck (`Swipe Right / L` to Approve, `Swipe Left / H` to Request Changes, `J` to Auto-Merge), multi-dimensional risk matrix, and automated merging directly from the web or native Chrome side panel.

## Approach and technologies

### Implementation & Architecture
RepoPilot is built as a unified full-stack application featuring:
- **Next.js 16 (App Router) & React 19:** Server components and high-performance client state management for fast graph rendering and snappy card swipe animations.
- **TypeScript & Tailwind CSS v4:** Type-safe domain models and modern dark/light mode responsive UI.
- **Gemma 4 (`gemma-4-26b-a4b-it`) via Gemini API & Vercel AI SDK:** Open-weight LLM powering grounded codebase explanations, AST query answering, and PR risk synthesis.
- **GitHub Octokit REST API & Webhooks:** Pull request fetching, file diff extraction, and safe automated pull request merging.
- **Chrome Extension (Manifest V3):** Embedded side panel and DevTools panel for real-time triage directly while browsing GitHub.
- **Neon PostgreSQL & Drizzle ORM:** Serverless persistence for caching analysis and audit logging.

### Reused materials and AI assistance disclosure
- The architecture visualizer builds upon and meaningfully extends open-source AST graph techniques from GitDiagram, retaining full MIT attribution.
- AI-assisted pair programming was utilized during development for rapid UI component iteration, AST parser refinement, and test harnesses.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI component and its role:**
  - **Gemma 4 (`gemma-4-26b-a4b-it`):** Serves as the core open-weight reasoning engine over deterministic codebase AST representations. It powers contextual Q&A, explains architectural dependencies, computes blast-radius risk heuristics for pull requests, and summarizes code review sessions.
- **Code links showing the integration:**
  - AI provider setup & Gemma 4 initialization: [lib/ai.ts](https://github.com/sripavantejb/repopilot/blob/main/lib/ai.ts)
  - Codebase architectural Q&A & explanation API: [app/api/diagram/explain/route.ts](https://github.com/sripavantejb/repopilot/blob/main/app/api/diagram/explain/route.ts)
  - PR triage, risk scoring, and blast radius calculation: [app/api/prs/route.ts](https://github.com/sripavantejb/repopilot/blob/main/app/api/prs/route.ts)
  - Safe automated PR merge engine: [app/api/prs/merge/route.ts](https://github.com/sripavantejb/repopilot/blob/main/app/api/prs/merge/route.ts)
  - Chrome Extension integration: [extension/sidepanel.js](https://github.com/sripavantejb/repopilot/blob/main/extension/sidepanel.js)
- **Agent Skill Open Standard compliance (if applicable):**
  - Not applicable.
- **Original harness implementation or meaningful changes:**
  - Engineered a specialized model harness in [lib/ai.ts](https://github.com/sripavantejb/repopilot/blob/main/lib/ai.ts) and [app/api/diagram/explain/route.ts](https://github.com/sripavantejb/repopilot/blob/main/app/api/diagram/explain/route.ts) that extracts structured AST nodes, imports, and cross-file dependencies and injects them as structured context for Gemma 4.
  - Implemented an automated PR triage and safe merging harness with custom risk tolerance checks and Octokit REST execution.

### Best Use of Gemma 4

- **Gemma 4 model identifier and Gemini API integration:**
  - **Identifier:** `gemma-4-26b-a4b-it`
  - Integrated via `@ai-sdk/google` (`createGoogleGenerativeAI`) connecting to the Google Gemini API.
- **Code links showing the integration:**
  - Model configuration: [lib/ai.ts:L18-L45](https://github.com/sripavantejb/repopilot/blob/main/lib/ai.ts)
  - Grounded code explanation endpoint: [app/api/diagram/explain/route.ts](https://github.com/sripavantejb/repopilot/blob/main/app/api/diagram/explain/route.ts)
- **Input and useful output; multimodal & grounded value:**
  - **Input:** Developer provides a GitHub repository URL and selects a module/file node from the interactive Mermaid graph, asking questions such as *"Explain the data flow from request to response"* or *"What breaks if I change this file?"*.
  - **Useful Output:** Gemma 4 analyzes the file references, imports, and exported symbols to generate concise, grounded explanations citing actual source lines and callers.
  - **PR Risk Scoring:** Gemma 4 evaluates diff size, touched critical files, and test presence to assign risk scores and heuristic warnings displayed on the review cards.

## Demo instructions

### Live Web Demo (No Setup Required)
1. Open the deployed application at **https://repopilot-orcin.vercel.app/**.
2. **Architecture Visualizer (Gitora):**
   - Click on the Architecture / Visualizer tab or explore sample repositories.
   - Inspect the interactive Mermaid dependency graph and module cards.
   - Click any node and select **"Click to explain ✨"** or ask a custom question in the Gemma 4 sidebar.
3. **PR Triage & Auto-Merge (SwiPR / RepoPilot):**
   - Navigate to `/swipe` to load the high-velocity PR deck.
   - Use keyboard shortcuts (`L` to approve, `H` to request changes, `J` to toggle auto-merge).
   - Switch between **Swipe Cards**, **Risk Matrix**, **Contributor Trust**, and **Dependency Graph** views.

### Local Development Setup
```bash
# 1. Clone repository
git clone https://github.com/sripavantejb/repopilot.git
cd repopilot

# 2. Install dependencies
pnpm install

# 3. Configure environment variables
cp .env.example .env.local
# Add GEMINI_API_KEY and GITHUB_TOKEN (optional for private repos / merging)

# 4. Start local development server
pnpm dev
# Open http://localhost:3000
```

### Chrome Extension (Manifest V3) Setup
1. In RepoPilot, navigate to the footer or click the Chrome Extension download link to download the pre-packaged zip, or load from the `/extension` directory.
2. In Google Chrome, go to `chrome://extensions/` and enable **Developer mode**.
3. Click **Load unpacked** and select the `extension` folder.
4. Navigate to any GitHub repository or PR page; open the Chrome Side Panel to review PRs and view diagrams side-by-side with GitHub.

## Current status

- **What works:**
  - End-to-end repository ingestion and AST dependency graph synthesis.
  - Interactive Mermaid diagramming with responsive zooming and node selection.
  - Source-grounded Q&A and component explanations using Gemma 4 (`gemma-4-26b-a4b-it`).
  - Tinder-style swipeable PR review deck with keyboard shortcuts (`H`, `J`, `K`, `L`).
  - PR blast radius estimation, impact vs. confidence 2D risk matrix, and contributor trust scoring.
  - Automated pull request merging via Octokit REST API.
  - Manifest V3 Chrome Extension side panel and DevTools panel.
  - Full production deployment on Vercel.
- **Known limitations / incomplete features:**
  - Repositories exceeding 50,000 files are pruned to top 5 module depths to prevent Mermaid graph layout bottlenecks.
  - Automated merge requires a GitHub Personal Access Token with write permissions to the target repository.
  - In-browser local storage holds session history; team-wide multi-tenant collaboration is in progress.
- **What you would improve next:**
  - Multi-file symbol cross-indexing via Tree-sitter WASM directly in the client.
  - Real-time GitHub Action webhook listeners to automatically trigger triage on newly opened PRs.
  - Monorepo package filtering with workspace-specific dependency graphs.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

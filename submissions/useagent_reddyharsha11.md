# UseAgent

## Team / attendee

- Team name (if applicable): reddyharsha11
- Members and GitHub usernames: B Harshavardhan Reddy (@reddyharsha11)
- Profile links (optional): https://github.com/reddyharsha11

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [ ] Best Use of Gemma 4

## Project links

- Public GitHub repository: https://github.com/reddyharsha11/useagent
- Open-source license (link to the license file): https://github.com/reddyharsha11/useagent/blob/main/LICENSE

## Problem and solution

### Who is this for?
Software engineers, DevOps teams, and AI developers building, deploying, and supervising autonomous coding agents across multi-surface workspaces.

### What problem does it solve?
Modern developers struggle with fragmented coding assistants, lacking isolated sandbox environments, persistent memory, verifiable audit trails, and consistent sharing cards across workspace surfaces. UseAgent unifies these capabilities into an open-source, multi-surface cockpit.

### Main input → output workflow:
1. **Input:** User submits tasks via chat prompt, scheduled automation, or GitHub issue/PR webhook.
2. **Execution:** UseAgent runs the agent within an isolated sandbox environment (Docker, local, or Daytona runner) equipped with durable context and organization memory ledgers.
3. **Output:** Agent generates code changes, pull requests, artifacts, Kanban updates, and dynamic per-route OpenGraph preview cards for immediate team collaboration.

## Approach and technologies

- **Frontend:** Next.js (App Router), TypeScript, TailwindCSS / Vanilla design tokens, Lucide & Remix Icons.
- **Backend & Protocol:** Bun runtime, TypeScript, custom runner protocol, Better Auth for authentication, and secure edge proxy middleware.
- **Sandboxing & Harness:** Pluggable sandbox contracts (`packages/sandbox-contract`, `packages/agent-harness`) supporting isolated runner environments.
- **Metadata & Previews:** Dynamic request host resolution (`headers()`) and crawler-aware edge middleware serving dedicated route-level OpenGraph preview cards (`public/og/*.png`).
- **Models Supported:** Pluggable multi-provider architecture supporting open-weight models (Llama, DeepSeek, Mistral, Ollama) and hosted APIs (Gemini, Claude, OpenAI).
- **AI-Assisted Development:** Built and iterated with pair programming using Google Antigravity & Gemini 3.8 Flash.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI component and its role:** 
  An open-source model harness and agent execution runtime (`packages/agent-harness`) designed to supervise and run autonomous coding agents with open-weight models (Ollama, Llama, DeepSeek, Mistral) and standard developer tooling in isolated sandboxes.
- **Code link showing the integration:** 
  - Repository: https://github.com/reddyharsha11/useagent
  - Agent Harness: https://github.com/reddyharsha11/useagent/tree/main/packages/agent-harness
  - Edge Preview & Route Cards: https://github.com/reddyharsha11/useagent/tree/issue-124-preview-card
- **Agent Skill Open Standard compliance (if applicable):** 
  Implements standard agent execution interfaces, memory recall ledgers, and sandboxed runner protocol.
- **Original harness implementation or meaningful changes (if applicable):** 
  Full end-to-end multi-surface workspace with live session streaming, Kanban boards, knowledge galleries, and route-specific edge OpenGraph metadata injection for protected pages.

## Current status

- **What works:** 
  - Complete workspace UI with Dashboard, Bots, Apps, Settings, Skills, Tasks, Knowledge, and Wiki.
  - Edge middleware serving route-specific OpenGraph and Twitter cards for every route.
  - Autonomous agent harness running tasks in isolated execution environments.
  - All 14/14 frontend proxy and metadata test suites passing.
- **Known limitations / incomplete features:** 
  - Remote sandboxes require local Docker/Daytona daemon to be accessible.
- **What you would improve next:** 
  - Multi-tenant team roles and permissions.
  - Multi-agent debate and consensus coordination for complex refactoring.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

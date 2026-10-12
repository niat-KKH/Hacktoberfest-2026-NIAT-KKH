# Synapse OS

## Team / attendee

- Team name (if applicable): Synapse OS
- Members and GitHub usernames: Sachin Rana ([@Sachinva-karma](https://github.com/Sachinva-karma)), Mannat Sahu ([@mannatsahu55-oss](https://github.com/mannatsahu55-oss))
- Profile links (optional): [https://github.com/Sachinva-karma](https://github.com/Sachinva-karma), [https://github.com/mannatsahu55-oss](https://github.com/mannatsahu55-oss)

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [ ] Best Use of Gemma 4

## Project links

- Public GitHub repository: [https://github.com/Sachinva-karma/synapse-os](https://github.com/Sachinva-karma/synapse-os)
- Open-source license: [https://github.com/Sachinva-karma/synapse-os/blob/main/LICENSE](https://github.com/Sachinva-karma/synapse-os/blob/main/LICENSE) (MIT License)

## Problem and solution

**Target Audience:** Enterprise operators, engineering leads, and developers orchestrating autonomous multi-agent AI systems in production.

**The Problem:**
Enterprises are shifting from single-prompt chatbots to autonomous multi-agent swarms that plan, use tools, delegate tasks, and negotiate decisions across workflows. However, multi-agent systems introduce severe reliability risks:
- Hallucination cascades where one hallucinating agent corrupts downstream decision-makers.
- Runaway token and cost spikes caused by unconstrained reasoning and retry loops.
- Risky autonomous actions (e.g., high-budget purchase orders or logistics rerouting) executed without human validation.
- Traditional observability tools (e.g. standard APM) operate passively—explaining failures only after money has been spent or damage has been done.

**The Solution:**
Synapse OS is a real-time mission-control and active governance platform for AI agent swarms. It transforms passive logging into proactive control:
- **Real-Time Swarm Topology:** Visualizes agent states (`idle`, `processing`, `warning`, `critical`, `killed`), communication handoffs, and active execution paths on an interactive canvas.
- **3-Tier Hallucination & Runaway Detection:** Evaluates agent telemetry in real time using mathematical heuristics (Shannon entropy, n-gram repetition detection, latency Z-scores, and cost velocity derivative).
- **Active Risk Interception & HITL:** Auto-pauses agent chains when risk thresholds are exceeded and intercepts high-impact operations for human operator approval.
- **Operator Kill Switches:** Provides granular per-agent and swarm-wide kill switches to stop runaway workflows instantly.

**Core Workflow (Input → Output):**
1. **Scenario / Event Ingestion:** System receives an unstructured trigger (e.g., an invoice image/text, customer order via chat, or external supply disruption alert).
2. **Multi-Agent Orchestration:** Specialized agents (`inventory` → `pricing` → `finance` → `logistics`) execute tasks sequentially or collaboratively.
3. **Telemetry & Risk Analysis:** For each agent execution, the system captures real token counts, latency, and cost, computing a real-time Hallucination Risk Score (HRS).
4. **Interception & Governance Gate:** If the risk score breaches critical thresholds (HRS >= 0.85) or an action exceeds configured limits (e.g., PO > $10,000), execution halts and routes to the human-in-the-loop approval queue.
5. **Execution & Audit Trail:** Once approved by the operator (or when execution completes within nominal bounds), actions are committed and logged to the central audit stream.

## Approach and technologies

**Architecture:**
- **Frontend Observability Command Center:** Built with React 19, Vite, Tailwind CSS, and Framer Motion. Uses React Flow (`@xyflow/react`) for real-time node-edge topology mapping, status glows, and animated handoff transitions.
- **Real-Time Communication Layer:** Socket.IO bidirectional websocket engine streaming telemetry, status broadcasts, and alert notifications between server and UI.
- **Dual Execution Engine:**
  - *Live Multi-Agent Orchestrator:* Node.js backend (`AIAgentOrchestrator.js`) utilizing the `@google/generative-ai` SDK with specialized system prompts, real token calculation, and sequential context piping.
  - *Simulation & Swarm Engine:* In-browser and Node.js event-driven simulators (`AgentSimulator.js`) for reproducible disruption scenarios and chaos testing.
- **3-Tier Hallucination Detection Engine (`HallucinationDetector.js`):**
  - *Tier 1 (Mathematical Telemetry - Always On):* Evaluates output text Shannon entropy (randomness), n-gram overlap with prior outputs (loop detection), latency deviations (Z-score), and cost velocity ($\Delta \text{cost} / \Delta t$). Requires zero access to proprietary customer data.
  - *Tier 2 (Behavioral Contracts):* Validates outputs against user-configured schema, range, and keyword safety rules.
  - *Tier 3 (Verifier Receiver):* Consumes pass/fail validation verdicts from sandboxed verifier agents.
- **Data & Audit Persistence:** Supabase PostgreSQL integration for structured audit logs and scenario replay.

**Credits & Reused Material:**
- React Flow (`@xyflow/react`) for graph topology rendering.
- Google Generative AI SDK (`@google/generative-ai`) for LLM agent calls.
- Socket.IO and Express for real-time websocket server.
- Lucide React for UI iconography.
- AI-assisted pair programming was used for scaffolding frontend layouts and baseline test configurations.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI component and its role:**
  Synapse OS acts as an open-source observability and governance harness for AI agents. It integrates generative AI models as active swarm participants while wrapping each execution in an original mathematical and contract-based safety harness.
- **Code links showing the integration:**
  - Multi-Agent Orchestrator & Live Chain: [AIAgentOrchestrator.js](https://github.com/Sachinva-karma/synapse-os/blob/main/backend/synapse-observatory/server/AIAgentOrchestrator.js#L192-L316)
  - 3-Tier Hallucination & Runaway Detection Engine: [HallucinationDetector.js](https://github.com/Sachinva-karma/synapse-os/blob/main/backend/synapse-observatory/server/HallucinationDetector.js#L14-L215)
  - Event-Driven Swarm Simulator & Governance Bus: [AgentSimulator.js](https://github.com/Sachinva-karma/synapse-os/blob/main/src/engine/AgentSimulator.js#L6-L198)
  - Backend Kill Switches & Swarm Controller: [backend/synapse-observatory/server/index.js](https://github.com/Sachinva-karma/synapse-os/blob/main/backend/synapse-observatory/server/index.js#L52-L96)
- **Original harness implementation or meaningful changes:**
  Synapse OS implements an original multi-agent telemetry and safety harness:
  1. **Zero-Knowledge Mathematical Telemetry:** Combines text entropy, n-gram cyclic repetition, latency outlier detection (Z-scores), and cost acceleration derivative to detect runaway loops without inspecting private payload contents.
  2. **Circuit-Breaker Interception:** Automatically breaks execution chains before corrupted outputs propagate to downstream agents when the composite risk score crosses the auto-pause threshold (0.85).
  3. **Human Governance Gate:** Intercepts high-budget purchase orders and high-risk operational proposals into an interactive queue requiring human approval before dispatch.

## Current status

- **What works:**
  - Real-time agent swarm execution monitoring and visual topology mapping via React Flow.
  - Live multi-agent chain orchestration with token, latency, and cost telemetry per agent and step.
  - 3-tier hallucination scoring calculating entropy, repetition loops, and cost burn acceleration.
  - Automated circuit breaker (auto-pause) halting downstream agent delegation when risk thresholds are exceeded.
  - Swarm-wide and individual agent kill switch controls (`/api/kill-all`, `/api/kill/:agentId`) and resume functionality.
  - Human-in-the-loop approval workflow with interactive review, approval, and rejection handlers.
  - Ambient simulation engine demonstrating three end-to-end scenarios (Invoice OCR, WhatsApp Order, Self-Healing Supply Chain).
- **Known limitations / incomplete features:**
  - Fail-closed approval timeout is planned: pending approvals currently remain in the queue until an operator explicitly approves or rejects them, rather than auto-rejecting after a fixed time window.
  - Tool interception is implemented at the agent handoff and purchase order boundary; sandboxed proxying of arbitrary shell or HTTP tools is not yet implemented.
  - Production deployment currently requires hosting both the Vite frontend and Node.js Socket.IO backend.
- **What you would improve next:**
  - Implement configurable fail-closed timeouts for pending approval requests.
  - Add native OpenTelemetry (OTel) trace exporter to stream data into Datadog and Grafana.
  - Build fine-grained role-based access control (RBAC) and cryptographically signed audit logs.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge (MIT License).
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

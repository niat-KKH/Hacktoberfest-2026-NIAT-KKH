# MergeShield

## Team / attendee

- Team name (if applicable): MergeShield
- Members and GitHub usernames: Sachin Rana ([@Sachinva-karma](https://github.com/Sachinva-karma))
- Profile links (optional): [https://github.com/Sachinva-karma](https://github.com/Sachinva-karma)

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [ ] Best Use of Gemma 4

## Project links

- Public GitHub repository: [https://github.com/Sachinva-karma/mergeshield](https://github.com/Sachinva-karma/mergeshield)
- Open-source license: [https://github.com/Sachinva-karma/mergeshield/blob/main/LICENSE](https://github.com/Sachinva-karma/mergeshield/blob/main/LICENSE) (MIT License)

## Problem and solution

**Target Audience:** Open-source maintainers, developer teams, and code reviewers managing high-velocity repositories.

**The Problem:** Reviewing incoming pull requests under tight deadlines leads to missed edge cases, hidden regressions, and security blind spots. Generic AI coding assistants often hallucinate code, cite non-existent line numbers, or offer vague suggestions without reproduction tests.

**The Solution:** MergeShield provides an automated, evidence-backed PR risk auditor powered by hosted open-weight foundation models.

**Core Workflow (Input → Output):**
1. **Input:** User submits a public GitHub Pull Request URL (e.g. `https://github.com/pallets/flask/pull/5918`).
2. **Fetch & Bound:** Backend retrieves PR metadata and unified diff via the GitHub REST API, applying strict SSRF defenses and bounding diffs to 1,500 lines / 200 KB.
3. **Open-Weight Reasoning:** The open-weight model (`qwen/qwen3.8-27b`) analyzes code changes under strict anti-hallucination prompt constraints.
4. **Evidence Grounding Verification:** A deterministic diff parser validates every cited line range and code snippet against the real diff hunks. If a cited line is not in the diff, MergeShield flags it with an unverified caveat.
5. **Output:** An interactive risk report displaying overall risk severity (`HIGH`, `MEDIUM`, `LOW`), verbatim grounded code evidence, uncertainty disclosures, and concrete verification tests.

## Approach and technologies

- **Frontend:** React 19, Vite, Lucide Icons, Vanilla CSS with custom Google Stitch-inspired design tokens, full light/dark/system theme support.
- **Backend:** Python 3.11, FastAPI, Uvicorn, HTTPX for async network I/O, Pydantic for validation schemas.
- **Model & Hosting:** `qwen/qwen3.8-27b` (Apache 2.0 open weights) hosted via Groq Cloud API.
- **Diff Grounding Harness:** Custom deterministic unified diff parser and grounding validator (`diff_parser.py` and `analyzer.py`) that indexes hunks and checks line-level modifications.
- **Security:** Strict SSRF URL validation, prompt-injection isolation with XML delimiter tagging, and zero execution of untrusted pull request code.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI component and its role:**  
  `qwen/qwen3.8-27b` released under the OSI-approved **Apache 2.0 License**. The model serves as the core intelligence engine, performing structural diff reasoning, risk classification, uncertainty assessment, and test generation.
- **Code link showing the integration:**  
  [https://github.com/Sachinva-karma/mergeshield/blob/main/backend/app/model_adapter.py](https://github.com/Sachinva-karma/mergeshield/blob/main/backend/app/model_adapter.py)
- **Original harness implementation or meaningful changes:**  
  MergeShield implements an original evidence-grounding harness (`backend/app/analyzer.py` and `backend/app/diff_parser.py`):
  1. It sanitizes and bounds incoming diffs to prevent token exhaustion.
  2. It formats diff hunks with exact line numbering metadata.
  3. It intercepts the model's structured JSON findings and verifies every code snippet against the ground-truth parsed diff. Any citation referencing unverified or unmodified context lines is explicitly flagged, preventing AI hallucinations from reaching reviewers unchecked.

## Current status

- **What works:**
  - Real end-to-end pull request analysis tested live with public PRs (e.g. `pallets/flask/pull/5918`).
  - Automated 17-test backend suite covering SSRF defense, diff bounding, line grounding, license resolution, and rate limits.
  - Production-ready Vite frontend build with light/dark mode and responsive design.
  - Complete evidence-grounding engine flagging ungrounded lines.
  - Safe `.env` configuration template excluding secrets from version control.
- **Known limitations / incomplete features:**
  - Ingestion is currently restricted to public GitHub repositories.
  - Very large monorepo pull requests exceeding 1,500 lines are truncated to maintain deterministic latency.
  - Static reasoning only; does not spin up runtime test containers.
- **What you would improve next:**
  - GitHub App integration for automated PR review comments.
  - Multi-file interactive side-by-side split diff viewer.
  - Support for self-hosted Ollama / vLLM deployment instances.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge (MIT License).
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

# Project Name - Mark21

## Team / attendee

- Team name (if applicable): Spirits
- Members and GitHub usernames:
   T.Rohith-rohith198023
   K.Siddartha-siddu1205
   K.Vivek-KatakamVivek1210
   K.Thrailokya-Thrailokya1006
   C.Abhishek-abhishekreddychitukula
- Profile links (optional):

## Challenge

Select the challenge you are entering:

- [ ] Best Open-Source AI Project
- [ ] Best Use of Gemma 4

If listing both categories, confirm eligibility with the organizers and complete evidence for each.

## Project links

- Public GitHub repository: https://github.com/rohith198023/Project_Mark_21
- Open-source license: No license file is present in the repository snapshot reviewed; add a license before claiming open-source licensing.

## Problem and solution

JARVIS is a personal-assistant project for people who want to issue natural-language requests through one interface and route them to desktop, browser, memory, or voice services. A user submits a message in the dashboard; the API forwards it to the orchestrator, which uses a language-model planner (when configured) to turn the message into a structured intent and route it to a specialist. The service returns a response or action result to the dashboard.

The project is an in-progress foundation rather than a complete local-first assistant: some services still return mock data, and the planner's Groq-hosted inference requires network access and a configured API key.

## Approach and technologies

The project uses Python, FastAPI, Pydantic, and HTTPX for API services and typed service contracts; Docker Compose defines the multi-service development stack. The orchestrator's planner calls the Groq API using the configurable `GROQ_MODEL` setting, which defaults to `llama-3.3-70b-versatile`, and validates structured planner output before routing. A keyword-based router is available as a fallback. Playwright powers browser automation. PostgreSQL and Qdrant are included in the Compose configuration for planned memory storage, but persistent embedding and retrieval are not implemented yet. The voice service is also currently mocked.

The repository's Python requirements identify the reused frameworks and SDKs; no external datasets or starter-code attribution are identified in the files reviewed. Copilot AI assistance was used to prepare this submission text. The team should disclose any additional AI-assisted implementation and any datasets, starter code, or other substantial reused material actually used.

## Challenge evidence

The repository reviewed does not currently provide Gemma 4 integration evidence. Neither challenge checkbox is selected; confirm the organizers' eligibility rules and add the missing evidence before selecting a track.

### Best Open-Source AI Project

- Open-source/open-weight AI component and its role: The planner is configured to use Llama 3.3 70B Versatile by default through Groq-hosted inference. This is an open-weight model with its own license; it is not bundled or run locally by this project. Confirm that the event accepts this model and hosted inference for this category.
- Code link showing the integration: [planner_service.py](https://github.com/rohith198023/Project_Mark_21/blob/main/services/orchestrator/planner_service.py) and [settings.py](https://github.com/rohith198023/Project_Mark_21/blob/main/shared/config/settings.py).
- Agent Skill Open Standard compliance (if applicable): No Agent Skill Open Standard implementation or compliance evidence is present in the repository reviewed.
- Original harness implementation or meaningful changes (if applicable): The repository contains a custom FastAPI orchestrator, Pydantic service contracts, specialist-service routing, and risk/confirmation handling. See [orchestrator main.py](https://github.com/rohith198023/Project_Mark_21/blob/main/services/orchestrator/main.py), [shared contracts](https://github.com/rohith198023/Project_Mark_21/tree/main/shared/contracts), and [architecture documentation](https://github.com/rohith198023/Project_Mark_21/blob/main/docs/architecture.md).

### Best Use of Gemma 4

- Gemma 4 model identifier and Gemini API integration: Not implemented in the repository reviewed; no Gemma 4 model or Gemini API integration is configured.
- Code link showing the integration: Not available. The existing planner integration is Groq/Llama, not Gemma 4: [planner_service.py](https://github.com/rohith198023/Project_Mark_21/blob/main/services/orchestrator/planner_service.py).
- Input and useful output; multimodal value where applicable: No Gemma-powered input/output flow or multimodal capability is implemented. Do not submit under this track unless the integration is added and demonstrated.

## Current status

- What works: The repository includes a dashboard/API/orchestrator flow, a structured LLM-planner integration with a keyword-routing fallback, Playwright browser-service code, and desktop-service code. Docker Compose defines the service stack. Runtime behavior depends on configuration and platform; these components have not all been verified together as a complete end-to-end deployment for this submission.
- Known limitations / incomplete features: The planner requires a Groq API key and network access. The memory service currently returns mock matches and does not persist embeddings or records; the voice service returns mock synthesis/transcription results. Desktop actions run in mock mode in Docker, and the project has no Gemma 4 integration. No repository license file was found.
- What you would improve next: Add and test a Gemma 4/Gemini integration if entering that challenge; implement persistent memory and real speech-to-text/text-to-speech; validate the complete deployment on the supported platforms; and choose and add an appropriate open-source license.

## Submission checklist

- [ ] Project repository is public and links work.
- [ ] Required challenge evidence is included (Gemma 4 integration is currently absent).
- [ ] Project uses an open-source license where required by the challenge (no license file was found).
- [ ] Work and reused materials are represented honestly; verify team-specific AI-assistance and reuse disclosures before submission.
- [ ] No API keys, tokens, passwords, or private data are included.
- [ ] I followed the organizers' build window and submission instructions.
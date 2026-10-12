\# ClaimCheck



\## Team / attendee



\- Team name (if applicable): Individual submission

\- Members and GitHub usernames: Amogh Gummadavelly — @Ravindar-Amogh-Gummadavelly

\- Profile links (optional): https://github.com/Ravindar-Amogh-Gummadavelly



\## Challenge



Select the challenge you are entering:



\- \[x] Best Open-Source AI Project

\- \[ ] Best Use of Gemma 4



\## Project links



\- Public GitHub repository: https://github.com/Ravindar-Amogh-Gummadavelly/ClaimCheck

\- Open-source license: https://github.com/Ravindar-Amogh-Gummadavelly/ClaimCheck/blob/main/LICENSE



\## Problem and solution



ClaimCheck is a local-first health insurance claim pre-submission auditing tool. It helps users identify potential policy-related issues and missing information before submitting a claim.



Users provide claim details, available documents, and insurance policy information. The application extracts relevant policy conditions, applies supported deterministic checks, and presents potential issues with supporting quotations and page references when available.



The goal is to help users review room-rent limits, procedure sub-limits, applicable waiting periods, exclusions, and document requirements before submission. Findings depend on the supplied information and implemented rules; ClaimCheck does not guarantee claim approval or reimbursement.



\## Approach and technologies



\- Python for policy processing and deterministic rule checks.

\- FastAPI for the web application.

\- Jinja2, HTML, and CSS for the user interface.

\- Pydantic for structured data validation.

\- Ollama with the local `llama3.1:8b` model for optional AI-assisted policy extraction.

\- Pytest and synthetic datasets for testing and evaluation.



The application combines AI-assisted extraction with deterministic validation. The model assists with interpreting policy information, while application logic performs supported rule checks and calculations. Ollama is optional, with fallback behavior when AI-assisted processing is unavailable.



Development used AI coding assistance; the project also includes application logic, tests, and a runnable workflow.



\## Challenge evidence



\### Best Open-Source AI Project



\- Open-source/open-weight AI component and its role: The locally run `llama3.1:8b` model, accessed through Ollama, assists with policy extraction. This optional AI workflow complements the deterministic insurance-rule engine.

\- Code link showing the integration: https://github.com/Ravindar-Amogh-Gummadavelly/ClaimCheck/blob/main/claimcheck/ollama\_client.py and https://github.com/Ravindar-Amogh-Gummadavelly/ClaimCheck/blob/main/claimcheck/policy\_extractor.py

\- Agent Skill Open Standard compliance (if applicable): Not applicable; this project is an application, not a standalone agent skill.

\- Original harness implementation or meaningful changes (if applicable): Not submitted as a standalone model harness.



\## Current status



\- What works: FastAPI web interface, deterministic audit mode, optional Ollama-assisted processing, command-line interface, synthetic test data, evidence-oriented findings, and automated evaluation.

\- Known limitations / incomplete features: Coverage depends on implemented rules and the information supplied. Ambiguous policy language and incomplete evidence may require manual review. The tool is not an insurer's official decision system.

\- What you would improve next: Expand supported policy conditions, improve evidence traceability, and evaluate against a broader set of synthetic policy scenarios.



\## Submission checklist



\- \[x] Project repository is public and links are provided.

\- \[x] Required challenge evidence is included.

\- \[x] Project uses an open-source license where required by the challenge.

\- \[x] Work and reused materials are represented honestly.

\- \[x] No API keys, tokens, passwords, or private data are included.

\- \[ ] I followed the organizers' build window and submission instructions.




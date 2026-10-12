# ClaimCheck

## Team / attendee

- Team name (if applicable): Individual submission
- Members and GitHub usernames: Amogh Gummadavelly - @Ravindar-Amogh-Gummadavelly
- Profile links (optional): https://github.com/Ravindar-Amogh-Gummadavelly

## Challenge

- [x] Best Open-Source AI Project
- [ ] Best Use of Gemma 4

## Project links

- Public GitHub repository: https://github.com/Ravindar-Amogh-Gummadavelly/ClaimCheck
- Open-source license: https://github.com/Ravindar-Amogh-Gummadavelly/ClaimCheck/blob/main/LICENSE

## Problem and solution

ClaimCheck is a local-first health insurance claim auditing tool designed to help users review a claim before submission. It identifies potential issues such as missing documents, policy-rule mismatches, waiting-period concerns, procedure sub-limits, and room-rent-related deductions. It provides preliminary checks and evidence-oriented findings to help users prepare their documentation.

ClaimCheck is an educational tool, not a guarantee of insurer approval or reimbursement.

## Approach and technologies

The project combines Python-based deterministic rules with optional local AI-assisted policy extraction through Ollama and the Llama 3.1 8B model. FastAPI powers the web interface, with a CLI for additional access. Synthetic test scenarios and automated tests help validate expected behavior.

The deterministic checks are designed to produce explainable findings, while the optional AI component assists with extracting relevant information from policy documents.

## Challenge evidence

### Best Open-Source AI Project

- Open-source/open-weight AI component and its role: The optional local Llama 3.1 8B model assists with extracting policy information for the audit workflow.
- Code link showing the integration: https://github.com/Ravindar-Amogh-Gummadavelly/ClaimCheck/blob/main/claimcheck/ollama_client.py
- Policy extraction implementation: https://github.com/Ravindar-Amogh-Gummadavelly/ClaimCheck/blob/main/claimcheck/policy_extractor.py
- Agent Skill Open Standard compliance: Not claimed.
- Original harness implementation or meaningful changes: The project integrates local model-assisted policy extraction into a claim-audit workflow alongside deterministic rule checks.

### Best Use of Gemma 4

Not submitted for this category. ClaimCheck uses Llama 3.1 rather than Gemma 4.

## Current status

- What works: FastAPI interface, deterministic rule checks, optional Ollama-assisted extraction, CLI, synthetic test data, and automated tests.
- Known limitations / incomplete features: Findings depend on the quality and completeness of the supplied policy and claim information. The tool is not a substitute for insurer review or professional advice. Results on synthetic scenarios do not establish real-world accuracy.
- What you would improve next: Expand policy coverage, improve extraction validation, add more representative tests, and evaluate findings against reviewed real-world examples where appropriate permissions and privacy protections are in place.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included, subject to organizer confirmation of eligibility.
- [x] Project uses an open-source license where required by the challenge, provided the linked license is present.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [ ] I followed the organizers' build window and submission instructions - confirm eligibility with the organizers before checking this box.

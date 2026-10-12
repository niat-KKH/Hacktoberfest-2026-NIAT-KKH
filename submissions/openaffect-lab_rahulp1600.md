# OpenAffect Lab

## Team / attendee

- Team name (if applicable): N/A (Individual submission)
- Members and GitHub usernames: Pampuram Rahul (@rahulp1600)
- Profile links (optional): https://github.com/rahulp1600

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project

## Project links

- Public GitHub repository: https://github.com/rahulp1600/openaffect-lab
- Open-source license (link to the license file): https://github.com/rahulp1600/openaffect-lab/blob/main/LICENSE
- Verified commit hash: `c834ab7`

## Problem and solution

Who is this for? What problem does it solve? Describe the main input → output workflow.

Language models frequently offer unsolicited comfort, sympathy, or psychoanalysis even when a user explicitly requests practical help or states a clear boundary rejecting reassurance.

OpenAffect Lab provides an open-source evaluation framework for AI developers and researchers to systematically evaluate how local models handle conversational boundaries and emotional framing.

**Main Workflow**:
1. **Input**: Scripted multi-turn conversational scenarios from a JSONL file (`scenarios/core.jsonl`), including counterfactual scenario pairs where the same situation is presented with and without an explicit user boundary (e.g., asking for a driving test checklist vs. asking for the checklist with *"Please don't comfort me or tell me it's okay"*).
2. **Inference**: Sends scripted prompts to local open-weight models through Ollama's HTTP API (`/api/chat`) using deterministic settings (`temperature=0.0`, `seed=0`).
3. **Evaluation**: Evaluates responses using deterministic criterion-level checks (`forbidden_patterns`, `required_patterns`, `max_words`, `no_unsupported_emotion_claim`) and marks subjective tone criteria for human inspection (`manual_review`).
4. **Output**: Preserves verbatim text evidence and produces structured JSON reports with behavioral pass rates, latency, and category breakdowns, viewable via terminal formatting (`openaffect report` and `openaffect compare`).

## Approach and technologies

OpenAffect Lab is implemented in Python 3.11+ using the **Python standard library** and `pytest`. It introduces zero cloud API dependencies, zero databases, and zero paid tokens.

**Technologies**:
- **Python 3.11+**: Core runtime using standard library packages (`urllib.request`, `json`, `dataclasses`, `re`, `argparse`).
- **Ollama**: Local inference server hosting open-weight models (`phi3:mini` and `llama3.2:3b`).
- **pytest**: Test harness verifying parser validation, evaluation rules, and CLI behaviors (30 automated tests).

**Attribution & AI Disclosure**:
- **AI-Assisted Development**: Developed with assistance from ChatGPT (ideation and initial drafting) and Google Antigravity (implementation, test generation, and empirical report verification).
- **Initial Code Disclosure**: Initial prototype code was developed shortly before the hackathon and finalized, tested, expanded to 10 scenarios, benchmarked against real models, and documented during the event.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI component and its role**:
  - OpenAffect Lab evaluates open-weight models running locally through Ollama. We benchmarked Microsoft Phi-3 Mini (`phi3:mini`, licensed under MIT) and Meta Llama 3.2 3B (`llama3.2:3b`, licensed under the Llama 3.2 Community License Agreement).
- **Code link showing the integration**:
  - Ollama integration adapter: [src/openaffect/model_adapter.py](https://github.com/rahulp1600/openaffect-lab/blob/main/src/openaffect/model_adapter.py)
  - Scenario evaluation engine: [src/openaffect/evaluator.py](https://github.com/rahulp1600/openaffect-lab/blob/main/src/openaffect/evaluator.py)
  - Comparison runner: [src/openaffect/runner.py](https://github.com/rahulp1600/openaffect-lab/blob/main/src/openaffect/runner.py)
- **Original harness implementation or meaningful changes**:
  - **Original Evaluation Harness**: Built an original, zero-dependency command-line evaluation framework featuring:
    - Counterfactual scenario pairs (`pair_id` and `variant`) proving that identical supportive text can pass in baseline and fail under boundary constraints.
    - Preserved text evidence for every failed check.
    - Side-by-side comparison command (`openaffect compare --models A B`) evaluating models across four behavioral dimensions: Boundary respect, Context adaptation, Task responsiveness, and Unsupported emotion attribution.
    - Offline runnable demo script ([demo.py](https://github.com/rahulp1600/openaffect-lab/blob/main/demo.py)) demonstrating evaluation logic without needing a running Ollama server.
    - Real reproducible sample reports saved in [examples/](https://github.com/rahulp1600/openaffect-lab/tree/main/examples).
- **Empirical Findings from 10 Scenarios**:
  - `llama3.2:3b` scored 100% (6/6) on avoiding unsupported emotion claims, while `phi3:mini` scored 83% (5/6) because it assumed the user was "nervous" in `boundary-no-reassurance-001` despite the user explicitly requesting only a document checklist.
  - `phi3:mini` scored 91% (10/11) on task responsiveness, while `llama3.2:3b` scored 82% (9/11) due to verbosity exceeding word limits.
  - Both models achieved 100% (2/2) on phrase-based boundary checks and 100% (4/4) on context adaptation.

## Current status

- **What works**:
  - Complete 10-scenario dataset loaded and validated in `scenarios/core.jsonl`.
  - Full CLI with `run`, `report`, and `compare` commands.
  - Deterministic evaluation engine with 5 check types and evidence preservation.
  - 30 passing unit and CLI tests (`python -m pytest -q`).
  - Offline runnable demo script (`python demo.py`).
  - Real reproducible evaluation reports saved in `examples/`.
- **Known limitations / incomplete features**:
  - Heuristic checks use regular expressions; they cannot catch novel or semantically paraphrased reassurance.
  - Small dataset (10 scenarios); findings describe behavior on these specific scenarios and do not measure overall model capability or emotional intelligence.
  - Small local models (3B class) exhibit prompt sensitivity.
- **What you would improve next**:
  - Expand the counterfactual scenario dataset to 50+ pairs across diverse domains (clinical triage boundary, customer service, workplace feedback).
  - Add optional local embedding similarity checks alongside regex heuristics.
  - Add export options for HTML visualization and CSV metric exports.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

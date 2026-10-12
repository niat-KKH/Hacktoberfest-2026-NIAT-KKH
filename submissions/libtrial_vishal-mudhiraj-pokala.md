# LibTrial — Evidence-Backed Python Library Evaluation Suite

## Team / attendee

- Team name (if applicable): LibTrial
- Members and GitHub usernames: Vishal Mudhiraj Pokala (@vishalmudhirajpokala)
- Profile links (optional): https://github.com/vishalmudhirajpokala

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [x] Best Use of Gemma 4

*Note: Entering both tracks as LibTrial features deep Gemma 4 integration through the Google GenAI SDK alongside an original, open-source sandboxed testing harness published under the MIT License.*

## Project links

- Public GitHub repository: https://github.com/vishalmudhirajpokala/LibTrial
- Open-source license (link to the license file): https://github.com/vishalmudhirajpokala/LibTrial/blob/main/LICENSE

## Problem and solution

### The Problem
When developers consult Large Language Models for open-source library recommendations, models frequently hallucinate non-existent package names, suggest obsolete or abandoned projects, or recommend libraries that silently conflict with existing dependencies. Blindly running `pip install <package>` risks polluting the local development environment or introducing broken dependencies into production.

### The Solution
**LibTrial** solves this by establishing a strict boundary between AI suggestions and empirical facts:
1. **Hypothesis vs. Fact**: Model suggestions are treated strictly as unverified hypotheses.
2. **Registry Ground Truth**: Package names, versions, licenses, and `Requires-Python` constraints are verified live against the official PyPI JSON API.
3. **Isolated Integration Trials**: Before any code touches the developer's machine, Gemma generates a minimal integration probe that is AST-screened and executed inside a disposable, network-isolated Docker container (`--network none`, 256MB RAM).
4. **Audit Reports**: Produces instant, auditable PDF and Markdown evaluation reports.

**Main Workflow:**
```text
PROJECT (Static Scan) ──> DISCOVERY (Gemma 4) ──> VERIFY (PyPI API) ──> TRIAL (Docker Sandbox) ──> REPORT (PDF/MD)
```

## Approach and technologies

- **LLM & Reasoning**: Google Gemma 4 (`gemma-4-26b-a4b-it`) accessed through the official Google GenAI SDK (`google-genai`).
- **Registry & Metadata**: Direct HTTP queries to the official PyPI JSON API (`https://pypi.org/pypi/<pkg>/json`) and GitHub REST API via `httpx`.
- **Static Analysis & Safety**: Python `ast` parses generated probes to forbid dangerous calls (`os.system`, `subprocess`, `open(..., 'w')`, network sockets, `eval`/`exec`). `packaging.specifiers` evaluates PEP 440 target version compatibility.
- **Sandboxing**: Ephemeral Docker containers built with `--network none`, 256MB memory cap, 60-second timeouts, and zero host filesystem mounts.
- **User Interface**: Streamlit web application with responsive layout and state machine.
- **Reporting**: ReportLab vector PDF generator and GitHub-Flavored Markdown generator.
- **Testing**: 43 automated unit and integration tests with `pytest`.

*Reused libraries*: Standard open-source libraries (`streamlit`, `google-genai`, `reportlab`, `httpx`, `pydantic`, `packaging`). All core harness logic, AST scanning, and PyPI verification pipelines are original implementations.

## Challenge evidence

### Best Use of Gemma 4

- **Model Identifier & Integration**: Utilizes `gemma-4-26b-a4b-it` via the official Google GenAI SDK (`google.genai.Client`).
- **Code Links**:
  - Client configuration & retry wrapper: [src/libtrial/llm_client.py](https://github.com/vishalmudhirajpokala/LibTrial/blob/main/src/libtrial/llm_client.py)
  - Candidate discovery orchestrator: [src/libtrial/candidate_discovery.py](https://github.com/vishalmudhirajpokala/LibTrial/blob/main/src/libtrial/candidate_discovery.py)
  - Probe generation prompt & runner: [src/libtrial/trial_runner.py](https://github.com/vishalmudhirajpokala/LibTrial/blob/main/src/libtrial/trial_runner.py)
- **Input & Output Workflow**:
  - *Input*: Natural-language capability requirement combined with structured project profile context (`python_version`, `detected_frameworks`, and `declared_dependencies` from target `pyproject.toml`).
  - *Output*: Gemma generates structured candidate hypotheses with specific distribution names, expected imports, capabilities, and fit rationale. In Stage 4, Gemma writes a minimal 5-line integration probe testing basic initialization and DataFrame/API behavior against the target framework.

### Best Open-Source AI Project

- **Open-Weight AI Component**: Integrates Google Gemma 4 as the semantic recommendation and code generation engine.
- **Code Links**:
  - Full Repository: [https://github.com/vishalmudhirajpokala/LibTrial](https://github.com/vishalmudhirajpokala/LibTrial)
  - License (MIT): [https://github.com/vishalmudhirajpokala/LibTrial/blob/main/LICENSE](https://github.com/vishalmudhirajpokala/LibTrial/blob/main/LICENSE)
  - Integration Trial Harness: [src/libtrial/trial_runner.py](https://github.com/vishalmudhirajpokala/LibTrial/blob/main/src/libtrial/trial_runner.py)
- **Original Harness Implementation**:
  - Built an original, zero-host-execution evaluation harness that pairs model output with deterministic static analysis (AST validation) and disposable container execution.
  - Enforces strict security boundaries: probe execution requires explicit user consent, operates with network disabled (`--network none`), and captures exit codes, runtimes, and stdout/stderr logs for engineering audits.

## Current status

- **What works**:
  - Complete 5-stage interactive Streamlit workflow (Project Inspection → Gemma 4 Discovery → PyPI Verification → Docker Trial → Report Export).
  - Deterministic PyPI verification distinguishing model hypotheses from official registry facts.
  - AST static safety pre-flight screening.
  - Disposable Docker container trials with live output capturing.
  - Export of downloadable PDF reports (via ReportLab) and Markdown briefs.
  - 43 automated pytest tests passing with 100% success rate.
- **Known limitations**:
  - Dependency analysis evaluates direct declared dependencies; deep transitive sub-dependency trees are not fully resolved.
  - Trial execution requires local Docker daemon; safely aborts and flags as `NOT RUN` if Docker is absent (host environment is strictly protected).
- **What to improve next**:
  - Expand registry coverage to JavaScript (npm) and Rust (crates.io).
  - Implement full SAT-solver transitive dependency conflict checking.
  - Add automated performance profiling benchmarks during container trials.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

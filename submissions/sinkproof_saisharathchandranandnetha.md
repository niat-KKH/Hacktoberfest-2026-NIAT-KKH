# SinkProof — Hacktoberfest Hack Day 2026 Submission

> **Prove your telemetry pipeline before production.**

SinkProof is a pre-deployment behavioral verification harness for telemetry pipelines that turns security intent into executable contracts, runs them against a real OpenTelemetry Collector, and mutation-tests the pipeline to prove the tests can detect regressions.

---

## Team / attendee

- **Team name:** SinkProof
- **Member:** Saisharathchandranandnetha — @Saisharathchandranandnetha
- **Profile:** https://github.com/Saisharathchandranandnetha
- **Format:** Solo build
- **Event:** Hacktoberfest Hack Day 2026 — NIAT-KKH × MLH

---

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [x] Best Use of Gemma 4

> Dual-category eligibility will be confirmed with the event organizers. Evidence for both categories is included below.

---

## Project links

- **Public GitHub repository:** https://github.com/Saisharathchandranandnetha/SinkProof
- **Open-source license:** Apache-2.0 — https://github.com/Saisharathchandranandnetha/SinkProof/blob/main/LICENSE
- **Demo & supporting materials:** https://drive.google.com/drive/folders/1biwFHfSvOEO4fLZluHslEvkQuul63nsk?usp=drive_link

---

# Problem and solution

## The current problem

Telemetry pipelines have quietly become part of the security boundary.

A modern OpenTelemetry Collector may redact secrets, remove PII, preserve identifiers, filter noise, route security events, and transform attributes before data reaches an external backend.

Yet the standard deployment workflow remains:

```text
write YAML → validate config syntax → deploy → inspect production
```

Configuration validation (`otelcol validate`) proves only that a configuration can parse and load. It does **not** prove that the resulting pipeline preserves the security and data-integrity guarantees the team intended.

A telemetry pipeline configuration can be syntactically valid while its real behavior is wrong:

- secrets survive redaction,
- PII reaches an external exporter,
- `request.id` disappears,
- trace correlation breaks,
- ERROR events go to the wrong sink,
- filters silently remove important telemetry,
- security tests pass vacuously because nothing reached the destination.

Core thesis:

> **Configuration validity is not behavioral proof.**

## The solution

SinkProof introduces a pre-deployment proof step:

```text
Human Requirement / Architecture
  ↓
Gemma 4
  ↓
Executable Telemetry Contract IR
  ↓
Schema + Inventory Guard
  ↓
Real OpenTelemetry Collector
  ↓
Captured Output
  ↓
Deterministic Verification
  ↓
Evidence + Fault Localisation
  ↓
Mutation Testing
  ↓
Mutation Score
```

Instead of asking:

> "Does this YAML config look correct?"

SinkProof asks:

> **"Can this pipeline execute against real synthetic traffic and prove that the expected security and data-integrity properties hold?"**

And then:

> **"Would these tests still detect the problem if the pipeline configuration regressed?"**

Important architectural principle:

> **AI proposes. SinkProof verifies.**

The LLM is a contract proposal engine, never the final security oracle.

---

# Approach and technologies

## 1. Telemetry Contract IR

SinkProof defines a machine-checkable Intermediate Representation (IR) for telemetry invariants independent of Collector processor implementation details.

Supported contract types:

- `forbidden_value`: asserts that specific canary values or secret tokens never reach specified destinations.
- `must_reach`: asserts that matching telemetry records successfully arrive at a designated sink.
- `must_not_reach`: asserts that filtered or classified records never leak to unauthorized sinks.
- `preserve_field`: asserts that critical contextual keys (such as `request.id` or trace attributes) survive transformation.

Example:

```yaml
- id: no-secret-canaries
  type: forbidden_value
  sinks: [siem, analytics]
  canaries: [password, token]
  severity: critical

- id: request-id-survives
  type: preserve_field
  sink: analytics
  field:
    scope: log
    key: request.id
  severity: high
```

## 2. Real Collector execution

SinkProof runs the candidate configuration inside a real `otel/opentelemetry-collector-contrib` container.

The execution harness:

1. boots the actual Collector container,
2. injects synthetic OTLP log data via gRPC,
3. executes all real processors, filters, and routing pipelines,
4. captures actual exporter output from file/mock sinks,
5. evaluates contracts directly against captured telemetry batches.

The verdict measures actual runtime behavior rather than static YAML heuristics.

## 3. Deterministic verification

PASS / FAIL / INCONCLUSIVE decisions are computed strictly by deterministic Python verifier logic, never by the model.

The verifier evaluates:

- canary leakage across all monitored sinks,
- destination reachability and routing constraints,
- attribute retention and transformation integrity,
- non-vacuity of security assertions.

## 4. Non-vacuous verification

A negative assertion such as:

> "No password reached the SIEM"

must **not** pass simply because zero records reached the SIEM (e.g. if an upstream receiver failed or an overzealous filter dropped everything).

SinkProof verifies that applicable test cases actually exercised the sink path before accepting any negative security assertion.

## 5. Telemetry mutation testing

To test the test suite, SinkProof deliberately injects realistic faults into the Collector configuration and reruns verification.

Structure-aware mutation operators:

- `drop_processor`: drops a processor from a pipeline (e.g., removing a redactor)
- `drop_ottl_statement`: removes an OTTL statement inside a transform processor
- `perturb_key`: alters an attribute name to simulate a typo or naming mismatch
- `weaken_filter`: lowers or inverts a severity/condition filter

Each mutant outcome is classified into:

- `KILLED`: at least one contract failed, proving the test suite caught the regression
- `SURVIVED`: all contracts still passed despite the corruption, exposing a blind spot in the contract suite
- `INVALID`: configuration was syntactically rejected at Collector startup
- `INCONCLUSIVE`: execution failed to produce verifiable telemetry

```text
Mutation Score = killed / (killed + survived)
```

This provides empirical proof of contract suite sensitivity.

## 6. Evidence + static fault localisation

On contract failure, SinkProof generates an actionable evidence bundle:

- contract ID and severity level (`critical`, `high`, `medium`, `low`)
- destination sink where violation was observed
- test case ID and canary token or missing field
- pipeline path (`logs/siem`, `logs/analytics`)
- relevant processor evidence identifying the candidate faulty processor/statement

This turns policy failures into immediate, actionable debugging evidence.

## 7. Gemma 4 multimodal contract compilation

Gemma 4 receives:

- `architecture.png` (visual diagram of services, sinks, trust boundaries, and telemetry flows)
- `requirements.txt` (plain-English security and data-integrity requirements)
- deterministically generated pipeline inventory (extracted sinks, keys, and canary tokens)

Transformation flow:

```text
Architecture Diagram + Human Security Requirement
  ↓
Gemma 4 Candidate Telemetry Contracts
  ↓
Schema + Inventory Guard
  ↓
Executable Contract IR
```

**Why multimodal context matters:**
An architecture diagram encodes logical service relationships, egress destinations, trust boundaries, and telemetry flows that are not obvious from a single configuration fragment. Gemma 4 combines visual architecture context with textual policy intent.

The candidate output passes through a strict Pydantic schema and inventory allow-list guard before it can enter the verification harness.

## 8. Reproducible record/replay

Model responses are cached by SHA-256 hash of all multimodal inputs in `examples/checkout/captures/`. During offline demos or evaluations without API access, SinkProof deterministically replays the recorded Gemma 4 proposal. Replayed contracts still pass through the exact same schema and inventory guard before entering the execution harness.

---

# What we actually built in the hackathon window

We did not attempt to pretend that a production telemetry-security platform could be completed in a few hours. We implemented the smallest complete vertical slice needed to prove the architecture end-to-end.

### What we built:

- Telemetry Contract IR
- Real Collector execution (`otel/opentelemetry-collector-contrib` via Docker)
- OTLP synthetic fixtures
- Secret / PII-like synthetic canaries
- Deterministic verifier (PASS / FAIL / INCONCLUSIVE)
- Non-vacuous verification logic
- Structured evidence bundles
- Static fault localisation
- Telemetry mutation engine
- Mutation adequacy scoring
- Gemma 4 contract proposal (`gemma-4-26b-a4b-it` via Gemini API)
- Schema and inventory guard
- CLI (`sinkproof verify`, `sinkproof mutate`, `sinkproof compile`)
- Streamlit UI (`app.py`)
- CI workflow (`.github/workflows/ci.yml`)
- Reproducible end-to-end checkout scenario

### Current limitations:

- log telemetry is the primary implemented signal
- checkout is the main seeded end-to-end example
- some sink mappings remain example-specific
- remediation is a bounded prototype
- Docker is the primary execution backend
- SinkProof does not claim regulatory compliance certification

This transparency strengthens the technical credibility of the project: evidence over claims.

---

# Measured evidence from the included scenario

The checkout example is a seeded evaluation scenario.

## Vulnerable pipeline

Running the contract suite against `collector.vulnerable.yaml`:

```text
4 PASS
3 FAIL
0 INCONCLUSIVE
```

The failing contracts expose unredacted passwords and tokens reaching the SIEM and analytics sinks, plus loss of the required `request.id` attribute.

## Corrected pipeline

Running the same suite against `collector.fixed.yaml`:

```text
7 PASS
0 FAIL
0 INCONCLUSIVE
```

## Mutation adequacy experiment

Testing the contract suites against 12 configuration mutants:

Weak contracts:
```text
1 / 12 mutants killed
Mutation score: 8.3%
```

Strong contracts:
```text
12 / 12 mutants killed
Mutation score: 100%
```

> **Note on evaluation:** These results are from the included seeded evaluation scenario and demonstrate that the mutation mechanism distinguishes a weak suite from one sensitive to the injected regressions. They are not claimed as broad industry benchmarks.

---

# Challenge evidence

## Best Open-Source AI Project

### Open-source / open-weight AI component and its role

Open-weight model support is part of the specification layer.

SinkProof uses Gemma 4 (`gemma-4-26b-a4b-it`) through the Gemini API as well as an Ollama-compatible backend. The model translates human security requirements and visual architecture diagrams into structured, machine-checkable telemetry contracts.

The AI component is central because the difficult human-facing bottleneck is translating high-level policy intent into explicit, machine-checkable invariants. The AI proposes; deterministic execution verifies.

### Code links

- Gemma / model integration:
  https://github.com/Saisharathchandranandnetha/SinkProof/blob/main/pipewitness/ai/gemma.py
- Model guard:
  https://github.com/Saisharathchandranandnetha/SinkProof/blob/main/pipewitness/ai/guard.py
- Prompts:
  https://github.com/Saisharathchandranandnetha/SinkProof/blob/main/pipewitness/ai/prompts.py

### Agent Skill Open Standard compliance

**Not applicable.**
Do not evaluate as an Agent Skill. SinkProof is submitted as an original verification and model harness.

### Original harness implementation

The innovation is NOT simply calling Gemma. The original engineering is the verification harness around the model:

1. **Telemetry Contract IR:** [pipewitness/contracts.py](https://github.com/Saisharathchandranandnetha/SinkProof/blob/main/pipewitness/contracts.py)
2. **Real Collector execution runner:** [pipewitness/runner.py](https://github.com/Saisharathchandranandnetha/SinkProof/blob/main/pipewitness/runner.py)
3. **Deterministic contract verifier:** [pipewitness/verify.py](https://github.com/Saisharathchandranandnetha/SinkProof/blob/main/pipewitness/verify.py)
4. **Telemetry mutation testing engine:** [pipewitness/mutate.py](https://github.com/Saisharathchandranandnetha/SinkProof/blob/main/pipewitness/mutate.py)
5. **Evidence model:** [pipewitness/evidence.py](https://github.com/Saisharathchandranandnetha/SinkProof/blob/main/pipewitness/evidence.py)
6. **Static fault localisation:** [pipewitness/localize.py](https://github.com/Saisharathchandranandnetha/SinkProof/blob/main/pipewitness/localize.py)
7. **Schema and inventory guard:** [pipewitness/ai/guard.py](https://github.com/Saisharathchandranandnetha/SinkProof/blob/main/pipewitness/ai/guard.py)

---

## Best Use of Gemma 4

### Model identifier + Gemini API integration

Primary challenge model:
```text
gemma-4-26b-a4b-it
```

The integration is implemented with the official Google GenAI SDK (`google-genai`):

- Gemma 4 integration:
  https://github.com/Saisharathchandranandnetha/SinkProof/blob/main/pipewitness/ai/gemma.py
- Schema and inventory guard:
  https://github.com/Saisharathchandranandnetha/SinkProof/blob/main/pipewitness/ai/guard.py
- Prompt templates:
  https://github.com/Saisharathchandranandnetha/SinkProof/blob/main/pipewitness/ai/prompts.py

### Multimodal input

Gemma 4 receives:

- Architecture diagram: `examples/checkout/architecture.png`
- Plain-English security requirements: `examples/checkout/requirements.txt`
- Deterministic pipeline inventory: extracted from candidate Collector configuration and OTLP fixtures

### Useful output & multimodal value

```text
Architecture Diagram + Human Security Requirement
  ↓
Gemma 4 Candidate Telemetry Contracts
  ↓
Schema + Inventory Guard
  ↓
Executable Contract IR
```

**Why multimodal reasoning is essential:**
The architecture diagram encodes logical service relationships, egress destinations, trust boundaries, and telemetry flows that are not obvious from an isolated Collector YAML snippet. Gemma 4 combines visual architecture topology with human policy text to propose candidate contracts.

The model output is not a generic chat answer: it is parsed into structured contracts, validated against an inventory allow-list guard, and fed directly into the deterministic execution and verification engine.

---

# Why this is not another observability dashboard

SinkProof does not aim to replace:

- OpenTelemetry Collector,
- telemetry routing platforms,
- observability backends,
- application tracing tools.

It adds a verification layer around the pipeline.

The category we are exploring is:

> **Pre-deployment behavioral verification for telemetry infrastructure.**

Our core contribution is the combination of:

- executable telemetry contracts,
- real-runtime evaluation,
- non-vacuous security assertions,
- structure-aware configuration mutation,
- and deterministic evidence.

---

# Future roadmap

## Phase 1 — Generalized proof engine

- remove example-specific sink assumptions,
- dynamically derive sinks and exporters,
- pin Collector image versions and SHA-256 digests,
- add trace contracts and metric contracts,
- add more independent topologies,
- expand structure-aware mutation operators,
- emit structured JSON/SARIF evidence.

## Phase 2 — Verified remediation

Target design:

```text
Failure Evidence
      ↓
Gemma Patch Proposal
      ↓
Strict PatchOp IR
      ↓
Apply in Sandbox
      ↓
otelcol validate
      ↓
Real Collector Execution
      ↓
Contract Verification
      ↓
Mutation Verification
      ↓
VERIFIED FIX / REJECTED PATCH
```

The model can suggest a patch; it cannot declare the patch correct.

## Phase 3 — CI/CD policy gate

Target CLI usage:

```bash
sinkproof verify \
  --config otel-collector.yaml \
  --contracts telemetry.contracts.yaml
```

Future CI capabilities:

- PR annotations and comments,
- deployment blocking on contract failure,
- mutation-score thresholds,
- cryptographically signed evidence bundles,
- SARIF reporting for GitHub Security tab,
- baseline drift detection.

## Phase 4 — Adversarial telemetry generation

Add property-based and fuzz-generated telemetry fixtures:

- Unicode edge cases,
- deeply nested JSON structures,
- encoded and obscured secrets,
- malformed values and schema drift,
- large payloads,
- partial identifiers,
- high cardinality attributes.

This evolves SinkProof from a fixed fixture runner into an adversarial telemetry verification harness.

## Phase 5 — Cross-engine telemetry contracts

Explore adapters for other programmable telemetry platforms such as Vector and Fluent Bit.

Contracts describe what must remain true. Engine adapters prove whether each implementation satisfies them.

## Phase 6 — Research directions

- mutation testing for observability infrastructure,
- executable security contracts for telemetry pipelines,
- LLM-assisted specification with deterministic runtime verification,
- coverage-guided telemetry mutations,
- differential verification between Collector versions.

---

# Current status

## What works

- CLI (`verify`, `mutate`, `compile`)
- Streamlit interactive UI (`app.py`)
- Real Collector execution in Docker
- Deterministic contract evaluation
- Non-vacuous verification logic
- Structure-aware mutation testing and scoring
- Evidence generation and static fault localisation
- Multimodal Gemma 4 contract proposal (`gemma-4-26b-a4b-it`)
- Schema and inventory guard
- Recorded demo fallback for offline evaluation
- Automated CI workflow
- Apache-2.0 licensing

## Known limitations / incomplete features

- log telemetry is the primary implemented signal,
- checkout is the main seeded end-to-end example,
- some sink mappings remain example-specific,
- remediation is currently a bounded prototype,
- Docker is the primary execution backend,
- SinkProof verifies technical properties and does not claim regulatory certification.

We document these limitations because the project itself is built around one principle:

> **Evidence over claims.**

---

# Reproduction

## Installation

```bash
git clone https://github.com/Saisharathchandranandnetha/SinkProof.git
cd SinkProof

python3 -m venv .venv
source .venv/bin/activate
pip install -e .
```

## Verify vulnerable pipeline

```bash
sinkproof verify \
  -c examples/checkout/collector.vulnerable.yaml \
  -k examples/checkout/contracts.yaml \
  -t examples/checkout/telemetry.yaml
```

## Verify corrected pipeline

```bash
sinkproof verify \
  -c examples/checkout/collector.fixed.yaml \
  -k examples/checkout/contracts.yaml \
  -t examples/checkout/telemetry.yaml
```

## Mutation testing

```bash
sinkproof mutate \
  -c examples/checkout/collector.fixed.yaml \
  -k examples/checkout/contracts.yaml \
  -t examples/checkout/telemetry.yaml
```

## Gemma contract compilation

```bash
sinkproof compile \
  -i examples/checkout/architecture.png \
  -r examples/checkout/requirements.txt \
  -c examples/checkout/collector.fixed.yaml \
  -t examples/checkout/telemetry.yaml \
  -o compiled.yaml
```

## Interactive UI demo

```bash
streamlit run app.py
```

> **Demo walkthrough & presentation files:** [Google Drive Demo Folder](https://drive.google.com/drive/folders/1biwFHfSvOEO4fLZluHslEvkQuul63nsk?usp=drive_link)

---

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

---

# Final statement

SinkProof is not trying to prove that five hours is enough to build an enterprise observability security platform.

The five-hour build demonstrates a more fundamental idea:

> **Telemetry infrastructure should not be trusted because its configuration looks correct. It should be trusted because its behavior can be tested, challenged, and proven.**

That is the direction SinkProof is built to explore.

# IssuePilot

## Team / attendee

- Team name (if applicable): Vedham
- Members and GitHub usernames: Team Vedham (4 members; individual handles withheld)
- Profile links (optional):

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [ ] Best Use of Gemma 4

## Project links

- Public GitHub repository: https://github.com/shasudo/issuepilot-hackday
- Open-source license (link to the license file): https://github.com/shasudo/issuepilot-hackday/blob/main/LICENSE

## Problem and solution

Open-source newcomers drown in issues: they can't tell which one matches their stack and
level, and PRs take long to review because context is scattered across threads and diffs.
IssuePilot takes any GitHub link (issue, PR, repo, org) plus a user profile (tech stack,
skill level, interests) and returns a personalized triage card: what it is, how hard it is
(Easy/Medium/Difficult from an explicit weighted rubric), whether it fits (0–100 with
stated reasons), and where to start. Main workflow: link + profile → resolve → fetch →
multi-agent analysis → scored card, plus Compare view, opportunity Boards, and markdown
export.

Demo: `uv run streamlit run app.py`, pick the Beginner Python preset, click "Seeded
issue", then Compare and Boards tabs. Seeded: psf/requests#7634, psf/requests#7586,
backup pytest-dev/pytest#15124. Full 2-minute script: docs/PRESENTATION_SCRIPT.md in
the repo.

## Approach and technologies

Python + LangGraph pipeline (fetch → investigate → evidence → finalize) with typed
Pydantic state; every LLM call uses structured outputs, versioned YAML prompts, and a
SQLite trace row. A provider-agnostic factory selects the model from `.env`: the demo
runs the open-weights `openai/gpt-oss-120b` via Groq, and the same factory supports the
organizer-confirmed Gemma 4 ID via Google with a one-line change. Deterministic layers
(link resolver, regex reference extraction, rubric math, fit scoring) keep the model
honest. Cached/retried GitHub client with stale-cache fallback for event WiFi. Streamlit
4-tab UI + Typer CLI (`analyze`, `recommend`).

Credit: inspired by a prior private prototype by the team; all code in the submission
repo was written fresh during the Hack Day. Reused libraries: LangChain/LangGraph,
Streamlit, Typer, Rich, httpx, tenacity (see pyproject.toml). Significant AI-assisted
development: yes — built with an AI coding assistant; the open-weights model is central
to the product itself (analysis agents), not just the authoring process.

## Challenge evidence

### Best Open-Source AI Project

- Open-source/open-weight AI component and its role: open-weights `openai/gpt-oss-120b`
  (via Groq) powers all four analysis agents — issue investigation, PR review, repo path
  ranking, history relevance — through structured outputs. Without it there is no
  summary, no checklist, no evidence: it is the product's core, shown live in the
  demo's Analyze tab.
- Code link showing the integration: https://github.com/shasudo/issuepilot-hackday/blob/main/issuepilot/llm/factory.py (provider factory, open-weight models) and https://github.com/shasudo/issuepilot-hackday/blob/main/issuepilot/graph/builder.py (LangGraph orchestration) and https://github.com/shasudo/issuepilot-hackday/blob/main/issuepilot/graph/nodes/issue_investigation.py (structured-output agent).
- Agent Skill Open Standard compliance (if applicable): `issue-triage/SKILL.md` —
  directory named `issue-triage`, frontmatter `name` matches, lowercase-hyphenated,
  `description` states what + when-to-use (under 1024 chars), MIT `license` field,
  plus `scripts/` wrapper and `references/` doc. Link: https://github.com/shasudo/issuepilot-hackday/blob/main/issue-triage/SKILL.md
- Original harness implementation or meaningful changes (if applicable): N/A — this
  entry is an application + agent skill, not a model harness.

## Current status

- What works: universal link analysis (issue/PR/repo/org), rubric difficulty, profile
  fit scoring, repo path evidence, related-history notes, Compare view, Boards with
  drill-down, markdown export, CLI (`analyze`, `recommend`), skill wrapper, 95 tests.
- Known limitations / incomplete features: board ranks use fast label heuristics (full
  analysis runs per opened pick); PR path skips repo-evidence agents (diff is the
  evidence); public repos only; free-tier rate limits need paced demo clicks.
- What you would improve next: evidence-backed score refinement node, eval harness over
  seeded links, LangSmith tracing, private-repo support.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

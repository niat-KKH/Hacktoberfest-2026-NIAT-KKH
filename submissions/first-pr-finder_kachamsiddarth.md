# first-pr-finder

## Team / attendee

- Team name : RUBIX 
- Members and GitHub usernames:Siddarth Kacham- @kachamsiddarth  P Srikar Naidu-@https://github.com/srikar-naidu   Srujan Kaleru-https://github.com/srujankaleru2007


## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [ ] Best Use of Gemma 4

The open-source AI category explicitly accepts an Agent Skill. This project does not use Gemma 4 or the Gemini API, so it is not submitted for that category.

## Project links

- Public GitHub repository: https://github.com/kachamsiddarth/first-pr-finder
- Open-source license (link to the license file): https://github.com/kachamsiddarth/first-pr-finder/blob/main/LICENSE (verified MIT License)

## Problem and solution

Open-source contributors can spend time on issues that are already claimed, actively being worked on, rejected for unresolved reasons, or already fixed. `first-pr-finder` is an Agent Skill for evidence-based contribution pre-flight analysis. Given an issue or repository to explore, it guides an AI coding agent to investigate ownership claims and competing pull requests, prior attempts and resolution evidence, repository rules, the likely code and test paths, and contribution risks before recommending an implementation plan.

The workflow keeps the issue-status verdict separate from planning readiness. It uses the six defined issue verdicts (`AVAILABLE`, `POTENTIALLY_CLAIMED`, `ACTIVE_WORK`, `PREVIOUSLY_REJECTED`, `ALREADY_RESOLVED`, `UNKNOWN`) and the separate readiness outcomes (`GO`, `GO WITH ASSUMPTIONS`, `NEEDS CLARIFICATION`, `BLOCKED`). It calls for evidence, timestamps, confidence, search-coverage limits, and explicit unknowns rather than treating missing information as proof. Its default pre-flight analysis is read-only; it does not make code changes, commits, or pushes.

## Approach and technologies

The project packages instructions in Markdown with YAML skill metadata and supporting reference documents. A compatible AI coding agent loads the skill and applies its workflow using whatever GitHub, repository, and issue/PR access that host provides. The skill does not bundle or select a model, call a model API, or provide its own GitHub integration; available evidence and analysis depend on the host agent's tools and permissions.

The repository includes an illustrative report, scenario-based evaluation cases, an evaluation checklist, and a Node.js static compatibility test. The automated test checks skill metadata, required workflow/status terms, local links, report-schema headings, compatibility documentation, and canonical-source layout; it is not a test of an AI agent's live analysis.

## Challenge evidence

### Best Open-Source AI Project

- Open-source/open-weight AI component and its role: The project is an Agent Skill using the Agent Skills Open Standard. The skill instructions guide a compatible AI coding agent through contribution pre-flight analysis. The host supplies the model and tools; this repository does not include a model integration.
- Code link showing the integration:
  - [Skill instructions and metadata](https://github.com/kachamsiddarth/first-pr-finder/blob/main/SKILL.md)
  - [Pre-flight investigation workflow](https://github.com/kachamsiddarth/first-pr-finder/blob/main/references/preflight-workflow.md)
  - [Evidence and verdict rules](https://github.com/kachamsiddarth/first-pr-finder/blob/main/references/evidence-and-risk-rules.md)
  - [Planning-readiness gate](https://github.com/kachamsiddarth/first-pr-finder/blob/main/references/confidence-gate.md)
  - [Required analysis report schema](https://github.com/kachamsiddarth/first-pr-finder/blob/main/references/report-schema.md)
  - [Repository convention analysis](https://github.com/kachamsiddarth/first-pr-finder/blob/main/references/repo-norms.md), [impact analysis](https://github.com/kachamsiddarth/first-pr-finder/blob/main/references/impact-analysis.md), and [counterfactual analysis](https://github.com/kachamsiddarth/first-pr-finder/blob/main/references/counterfactual.md)
  - [Evaluation scenarios](https://github.com/kachamsiddarth/first-pr-finder/blob/main/tests/test-cases.md), [evaluation checklist](https://github.com/kachamsiddarth/first-pr-finder/blob/main/tests/evaluation-checklist.md), and [static compatibility tests](https://github.com/kachamsiddarth/first-pr-finder/blob/main/tests/compatibility.test.mjs)
- Agent Skill Open Standard compliance (if applicable): **Statically checked against the required metadata fields.** `SKILL.md` has a lowercase, hyphenated `name` (`first-pr-finder`) and a non-empty `description`; the repository is organized as the matching skill directory and includes optional references. This is not a formal `skills-ref validate` result, and loading by an agent has not been runtime-tested. See the [Agent Skills specification](https://agentskills.io/specification).
- Original harness implementation or meaningful changes (if applicable): Not applicable; this submission is an Agent Skill, not a model harness.

#### Reproducible demo instructions (not yet runtime-tested)

1. In a disposable target project, check for an existing skill with the same name, then install this public skill for an available agent, for example: `npx skills add kachamsiddarth/first-pr-finder --skill first-pr-finder --agent github-copilot --copy`. The README documents corresponding Codex and Claude Code options.
2. Open or refresh that project in the selected agent. Ask it to use `first-pr-finder` to pre-flight a public GitHub issue, providing the repository and issue URL.
3. Show the agent checking available issue/PR and repository evidence, then review its issue verdict, confidence and coverage caveats, separate readiness result, and report against the linked workflow and schema.

These are documented demonstration steps, not evidence that a live agent demo has been run. A successful demo also depends on the host agent discovering the installed skill and having access to relevant GitHub evidence.

## Current status

- What works: The public repository contains the documented three-mode workflow (analyze an issue, discover candidates, or review a contribution plan); status and readiness rules; ownership, competing-work, rejection, and resolution checks; repository-norm, code/test-path, impact, and risk analysis; and a report contract. The project also documents project-local installation paths for Copilot, Codex, and Claude Code. Static compatibility tests passed locally (8/8); this does not verify agent runtime behavior.
- Known limitations / incomplete features: GitHub coverage depends on each host's tools, permissions, and search limits. Findings are not a guarantee of maintainer acceptance or exhaustive discovery. No bundled model or GitHub client performs the analysis independently of an agent host. Copilot, Codex, and Claude Code placement has been CLI-smoke-tested, but loading and an actual pre-flight analysis have not been runtime-tested in those agents.
- What you would improve next: Run and document a live, reproducible pre-flight demo in each supported agent; validate with the official `skills-ref` validator; expand automated and agent-level evaluations for evidence coverage and report quality; and improve graceful handling and reporting when host GitHub access is limited.

## Submission checklist

- [x] Project repository is public and links work.
- [ ] Required challenge evidence is included. The skill and static standard metadata are linked; the challenge's live skill invocation and result still need a demo.
- [x] Project uses an open-source license where required by the challenge. The public repository's `LICENSE` is MIT.
- [x] Work and reused materials are represented honestly. No model integration, live agent test, or demo result is claimed.
- [x] No API keys, tokens, passwords, or private data are included. A full repository secrets/privacy audit was not performed.
- [x] I followed the organizers' build window and submission instructions. Build-window eligibility and any separate event-platform submission remain for attendee/organizer confirmation.

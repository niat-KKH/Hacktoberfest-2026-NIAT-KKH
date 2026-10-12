# SiteSentinel AI

## Team / attendee

- Team name : Pepper potts
- Members and GitHub usernames: @mallurivikas, @Wahaj-udn

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [x] Best Use of Gemma 4

If listing both categories, confirm eligibility with the organizers and complete evidence for each.

## Project links

- Public GitHub repository: https://github.com/mallurivikas/hacktoberfest_niat
- Open-source license (link to the license file): https://github.com/mallurivikas/hacktoberfest_niat/blob/main/LICENSE

## Problem and solution

SiteSentinel AI is for small businesses, solo developers, and anyone who ships a website using AI code generators, templates, or freelance work but has no technical team to verify whether the result is actually secure, fast, or able to handle real traffic. These users have a URL and no reliable way to know what's wrong with it.

**Workflow:** User enters a URL → the platform runs a security scan (header/cookie checks + OWASP ZAP baseline), a performance audit (PageSpeed Insights / Lighthouse metrics), and a concurrent load test (k6, with a Python fallback) → the combined structured findings are passed to Gemma, which generates a Security Score, Performance Score, Overall Score, and a prioritized, site-specific remediation plan → the result is rendered on a dashboard with visual breakdowns (radar chart, findings distribution, load profile) so a non-technical owner can understand what's wrong and what to fix first without reading raw scan output.

## Approach and technologies

- **Security layer**: custom header/cookie inspection (`security_check.py`) for CSP, HSTS, X-Frame-Options, X-Content-Type-Options, Referrer-Policy, Permissions-Policy, and cookie flags, combined with OWASP ZAP's baseline scanner run via Docker (`zap_scanner.py`) for broader passive vulnerability detection.
- **Performance layer**: direct HTTP timing/TTFB measurement (`perf_basic.py`) plus Google PageSpeed Insights API integration (`perf_psi.py`) for Lighthouse-grade metrics (LCP, FCP, Speed Index, TBT, CLS) — chosen over running Lighthouse locally to avoid a Chrome/Node dependency on the demo machine.
- **Reliability layer**: k6 load testing (`load_k6.py`) for concurrent-user throughput, p50/p95 latency, and failure rate, with an automatic multi-threaded Python fallback if k6 isn't installed.
- **AI layer**: Gemma (`ai_gemini.py`), via the Gemini API, takes the merged structured findings from all three layers and produces dual 0–100 scores plus a step-by-step, non-generic remediation plan.
- **Frontend**: Flask backend (`app.py`) serving a vanilla CSS/JS dashboard with Chart.js visualizations (radar chart, donut chart, benchmark bar chart, load profile).
- Built on established open-source tools (OWASP ZAP, k6, Chart.js) rather than reimplementing scanning logic from scratch, so the project's own code focuses on orchestration, the shared JSON contract between modules, and the AI synthesis layer.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI component and its role**: Gemma, called via the Gemini API, is the synthesis layer the entire pipeline is built around — it ingests the structured security, performance, and load-test findings and produces the Security/Performance/Overall scores and the remediation plan. Without this layer, the product is five tools' worth of disconnected technical output; the AI is what makes it usable by a non-expert.
- **Code link showing the integration**: [`ai_gemini.py`](https://github.com/mallurivikas/hacktoberfest_niat/blob/main/ai_gemini.py), called from [`scanner.py`](https://github.com/mallurivikas/hacktoberfest_niat/blob/main/scanner.py)
- **Agent Skill Open Standard compliance**: Not applicable — this is an application integration, not an agent skill.
- **Original harness implementation or meaningful changes**: Not applicable.

### Best Use of Gemma 4

- **Gemma 4 model identifier and Gemini API integration**: `gemma-4-26b-a4b-it`, called via the Gemini API in [`ai_gemini.py`](https://github.com/mallurivikas/hacktoberfest_niat/blob/main/ai_gemini.py); configured via `GEMINI_API_KEY` and `GEMINI_MODEL` in [`config.py`](https://github.com/mallurivikas/hacktoberfest_niat/blob/main/config.py).
- **Code link showing the integration**: [`ai_gemini.py`](https://github.com/mallurivikas/hacktoberfest_niat/blob/main/ai_gemini.py)
- **Input and useful output; multimodal value where applicable**: Input is structured text (JSON findings from the security, performance, and load-test scans). Output is dual 0–100 scores and a prioritized, site-specific remediation plan in plain English. Current implementation is text-only; it does not yet use multimodal input (e.g. a page screenshot), which is a known gap against this track's multimodal emphasis — see Known limitations below.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

# Submission: Already Built?

## Project name

**Already Built?**: check whether your project idea already exists before you build it.

## Team / attendee

- Team name (if applicable): Already Built? *(solo entry; edit if you have teammates)*
- Members and GitHub usernames: [@ronaksarda](https://github.com/ronaksarda)
- Profile links (optional): https://github.com/ronaksarda

## Challenge

- [x] Best Open-Source AI Project
- [x] Best Use of Gemma 4

Entering both categories. *(Confirm with the organizers that one project can enter both; evidence for each is below.)*

## Project links

- Public GitHub repository: https://github.com/ronaksarda/already-built
- Open-source license: [MIT](https://github.com/ronaksarda/already-built/blob/main/LICENSE)

## Problem and solution

**Who it is for:** students and hackathon builders who have an idea and need to know, before spending a weekend on it, whether it already exists. At events like Hacktoberfest people often build their idea, then find a near-copy on GitHub at demo time. Or they find one early and quit when they didn't need to.

**What it does:** you type or say your idea (or paste a GitHub link, or add a screenshot of a rival you found). Already Built? searches GitHub, Hugging Face (Spaces and Models) and Show HN, ranks what it finds by meaning, and tells you plainly:

- a **verdict**: already built / crowded / no close open-source match / search incomplete
- the **closest projects** (P1-P10) with links, stars, activity and a similarity score
- a **chart** placing each project by "same users and problem" vs "same features and approach"
- a **report**: what already exists, what your idea states differently, weak spots in existing projects, reality checks, and one next step. Every point cites a listed project.
- a **chat advisor** for follow-up questions (narrower niche, MVP scope, how to differ from P1)

**Workflow:** idea text, voice, link or screenshot → input guard (local) → **Gemma 4 plan** (queries, problem/solution axes, title, screenshot reading, safety) → parallel search of 3 sources → local embedding ranking → report (gpt-oss, Gemma 4 as backup) → verdict, chart, report, chat. A check takes about 15-25 s.

It guides; it does not build the project for you, and it does not flatter. If there is nothing close it says that closed apps and app stores were not searched, rather than calling the space open.

## Approach and technologies

| Part | Choice | Why |
|---|---|---|
| Planner, vision, safety | **Gemma 4** `gemma-4-26b-a4b-it` (fallback `gemma-4-31b-it`) via the Gemini API, `google-genai` SDK | Open-weight, multimodal: one call turns a messy idea (plus an optional screenshot) into structured search intent |
| Similarity | `sentence-transformers/all-MiniLM-L6-v2`, run **locally** | Open model, deterministic, free. It is the only thing that decides what counts as similar, so no LLM can invent a match |
| Report and chat | `openai/gpt-oss-120b` → `gpt-oss-20b` on Groq; Gemma 4 when Groq is down | Open-weight, fast; every claim must cite a fetched project, and invalid citations are removed |
| Speech | `whisper-large-v3-turbo` on Groq; browser speech recognition as fallback | Lets people speak the idea (English, Hindi, Telugu, Tamil, auto) |
| Sources | GitHub Search API, Hugging Face Hub API, HN Algolia API | Free, public; the searches run in parallel |
| Server | Python, FastAPI, uvicorn | Small, async-friendly; per-IP rate limits protect the free API quotas |
| Front end | Plain HTML/CSS/JS, no build step; WebGL background inspired by a shadergradient.co preset; Pixelon pixel font | Loads fast, works without a bundler |

**Credits and reuse:** FastAPI, uvicorn, requests, numpy, Pillow, sentence-transformers / PyTorch, google-genai, python-dotenv (see `requirements.txt`). Pixelon font from 1001fonts (licence in `web/fonts/`). Simplex-noise GLSL function adapted from Ashima Arts / Stefan Gustavson (MIT). Background colours taken from a shadergradient.co preset; the shader itself is our own WebGL code, not the shadergradient library. No datasets or starter code. **AI-assisted development:** significant parts of the code, tests, UI and docs were written with Claude Code (Anthropic) under our direction, and every feature was reviewed and tested by running it.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI components and their roles:**
  - Gemma 4 (open weights): plans every search, reads rival screenshots, refuses harmful ideas, and is the backup writer for the report and chat.
  - gpt-oss-120b / 20b (open weights): writes the grounded report and powers the chat.
  - all-MiniLM-L6-v2 (Apache-2.0, runs locally): decides similarity and computes the two chart axes. No closed model ranks anything.
  - Whisper large-v3-turbo (open weights): speech to text.
- **Code links showing the integration:**
  - Planning and vision: [core/gemma.py](https://github.com/ronaksarda/already-built/blob/main/core/gemma.py)
  - Local ranking and chart axes: [core/rank.py](https://github.com/ronaksarda/already-built/blob/main/core/rank.py)
  - Report and chat, gpt-oss with Gemma fallback: [core/report.py](https://github.com/ronaksarda/already-built/blob/main/core/report.py), [core/groq.py](https://github.com/ronaksarda/already-built/blob/main/core/groq.py)
  - Orchestration: [core/pipeline.py](https://github.com/ronaksarda/already-built/blob/main/core/pipeline.py)
- **Agent Skill Open Standard compliance:** [skills/already-built/SKILL.md](https://github.com/ronaksarda/already-built/blob/main/skills/already-built/SKILL.md). The directory name matches `name: already-built` (lowercase, hyphenated), the `description` says what the skill does and when to use it, and it has the optional `license`, `compatibility` and `metadata` fields. The bundled script `scripts/check.py` uses only the Python standard library: it calls the server, polls the job and returns Markdown or JSON (`--json`, `--image`, `--url`).
- **Original harness implementation:** [core/pipeline.py](https://github.com/ronaksarda/already-built/blob/main/core/pipeline.py) is an original multi-model harness:
  - **Hedged planning:** Gemma 4 starts first; gpt-oss joins after 6 s; Gemma's plan wins whenever it arrives within 10 s, or within 30 s when a screenshot is attached.
  - **Failure breaker:** Gemma is skipped for 5 minutes after repeated failures.
  - **Grounded report:** citations that are not in the fetched list are stripped; "the idea differs because it lacks X" claims are dropped; report safety refusals are honored.
  - **Rule-based fallback:** when every LLM is down, a report built from the data by fixed rules stands in.
  - **Per-source slots:** keep Show HN and Hugging Face results in the top list next to GitHub.

### Best Use of Gemma 4

- **Gemma 4 model identifier and Gemini API integration:** `gemma-4-26b-a4b-it` (primary), `gemma-4-31b-it` (second model), called with `google-genai` `client.models.generate_content` and `thinking_config=ThinkingConfig(thinking_level="minimal")`. That setting cut plan time from 20 s or more (timeouts) to about 5-9 s. Images go in as `types.Part.from_bytes`.
- **Code link showing the integration:** [core/gemma.py](https://github.com/ronaksarda/already-built/blob/main/core/gemma.py) (`plan()`, `chat()`, `_generate()`); used in [core/pipeline.py `_plan()`](https://github.com/ronaksarda/already-built/blob/main/core/pipeline.py) and [core/report.py `llm()`](https://github.com/ronaksarda/already-built/blob/main/core/report.py).
- **Input and useful output; multimodal value:**
  - **Input:** "A study tool that turns lecture slides into flashcards for nursing students", plus a screenshot of a rival product page.
  - **Gemma 4 output (real run):** title "Lecture Slide Flashcard Generator"; the problem and solution sentences that become the chart's x and y axes; search queries for GitHub, HF and HN; and from the image alone: *"SlideDeck2Anki is a web application that allows users to upload lecture PDFs or PowerPoints. It automatically generates Anki flashcards using cloze deletions within 30 seconds, specifically targeting…"*.
  - **Multimodal value:** people often meet a rival as an app page or a tweet screenshot, not as a repo. Gemma turns that picture into text, and the report then compares the idea against that rival as well as the open-source matches.
  - **Gemma also decides safety** before any search runs: ideas meant to hurt people are refused, while defensive versions (e.g. detecting stalkerware) pass.

## Demo instructions

1. Clone and install:
   ```bash
   git clone https://github.com/ronaksarda/already-built
   cd already-built
   pip install -r requirements.txt
   cp .env.example .env   # add GEMINI_API_KEY and GROQ_API_KEY (both free)
   python scripts/prefetch.py
   python server.py       # open http://localhost:8000
   ```
2. No keys? Click **Open a saved example** on the start page to see a full recorded result.
3. Things to try:
   - Type an idea, e.g. "An app called DupeRadar that shows open source maintainers which GitHub issues are duplicates before they triage". The project name becomes the title in the sidebar.
   - Click **Rival screenshot** and upload a screenshot of any app's landing page: Gemma 4 describes it under "Your screenshot", and the report compares against it.
   - Click **Speak** to dictate an idea, and **Read aloud** to hear the verdict; the dragon shows each sentence.
   - Ask the chat "How is my idea different from P1?", then reopen the check from the sidebar: the chat is saved.
   - Paste a GitHub repo link instead of an idea to find that repo's rivals.
4. Agent Skill: with the server running, `python skills/already-built/scripts/check.py "your idea"`.
5. Tests (offline, no keys): `python -m pytest -q tests`.

## Current status

- **What works:**
  - **Checking:** text, voice and GitHub-link input; rival screenshots read by Gemma 4; parallel search of GitHub, Hugging Face and Show HN; local ranking; verdict, quadrant chart, cited report and chat advisor.
  - **Dashboard:** Claude-style sidebar of past checks with titles and project names, saved chats, search and delete.
  - **Look:** light and dark themes, an animated background, and a pixel dragon that reads results aloud.
  - **Agent Skill:** `skills/already-built/`.
  - **Hardening:** private-address (SSRF) protection for pasted links, rate limits, security headers.
  - **Tests:** 81 automated tests, offline, no keys needed.
- **Known limitations / incomplete features:**
  - Closed apps, app stores and startups without code are not searched. The verdict says so.
  - GitHub search does not look inside READMEs, so a repo found only through its README text can be missed.
  - Similarity measures overlap in wording and purpose, not quality or traction.
  - Runs on free tiers (Gemini, Groq, GitHub), so heavy use hits rate limits; Gemma latency on the free tier varies.
  - Past checks are saved only in the browser.
  - No public deployment yet; runs locally with `python server.py`.
- **What you would improve next:**
  - Search Product Hunt and app stores.
  - Search README text through the GitHub code-search API.
  - Host a public instance (e.g. DigitalOcean App Platform).
  - Optional accounts to sync past checks.
  - A "watch this idea" alert when a new close rival appears.

## Submission checklist

- [x] Project repository is public and links work. *(Push the latest commit first; the code links above point to `main`.)*
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge (MIT).
- [x] Work and reused materials are represented honestly (credits and AI-assistance disclosure above).
- [x] No API keys, tokens, passwords, or private data are included (`.env` is gitignored; only `.env.example` with empty values is committed).
- [ ] I followed the organizers' build window and submission instructions. *(Only you can confirm this.)*

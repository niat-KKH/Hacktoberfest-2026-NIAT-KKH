# InterviewIQ — Company-Focused Interview Preparation

## Team / attendee

- Team name (if applicable): **Needs attendee confirmation**
- Members and GitHub usernames: **Add each registered attendee and GitHub username before submitting.** The repository owner is [@Swetha-Oruganti](https://github.com/Swetha-Oruganti); confirm whether this person is an event attendee/team member.
- Profile links (optional): **Add attendee profile links.**

## Challenge

Select the challenge you are entering:

- [ ] Best Open-Source AI Project
- [x] Best Use of Gemma 4

This draft is scoped to **Best Use of Gemma 4**. It does not claim Agent Skill Open Standard compliance.

## Project links

- Public GitHub repository: [HacktoberFest--Nxtwave-hack-day-](https://github.com/Swetha-Oruganti/HacktoberFest--Nxtwave-hack-day-) — confirm that the repository is public and that this is the correct submission URL.
- Open-source license (link to the license file): **No root `LICENSE` file was present when this submission draft was prepared. Add an appropriate license and link it here if required by the event.** The backend package's `ISC` metadata is not a substitute for a repository license file.

## Problem and solution

Interview candidates often have to piece together company-specific preparation from scattered interview reports, company-tagged coding collections, and general-purpose practice tools. InterviewIQ brings company-focused practice and candidate-shared experiences into one workspace.

The main workflow is: choose a target company and role → review grounded research with source links and generate original practice questions with Gemini or Gemma 4 → solve a question in the in-browser editor → run code through Judge0 and save the attempt → read or submit an interview experience. Accounts protect saved attempts and question generation; submitted experiences are held for admin review before public display.

## Approach and technologies

- **AI:** The Google Gen AI SDK (`@google/genai`) calls Gemini for original question generation and coaching. Users can select the configured Gemma 4 model for question generation. Gemini Google Search grounding is used for current company research; the app returns source links and does not persist the grounded report text as a question dataset.
- **Backend and data:** Node.js, Express, Mongoose, and MongoDB Atlas. Authentication uses hashed passwords and signed bearer tokens; protected routes enforce authentication and admin-only operations.
- **Frontend and editor:** Vanilla JavaScript, HTML, and CSS with a responsive blue-and-black interface, saved light/dark themes, Monaco Editor, and a textarea fallback.
- **Code execution:** Judge0 is a separate sandbox service. The app discovers languages supported by the configured Judge0 instance and sends code and optional standard input to that provider; execution requires server-side Judge0 configuration.
- **Question provenance:** Imported question records can retain source links, licenses/permission references, and available asked/posted dates. The app supports authorized imports and duplicate checks. It does not scrape or republish premium problem-bank content.
- **Reused libraries and services:** `@google/genai`, Express, Mongoose, Monaco Editor (loaded from its CDN), and the configured Judge0 provider. Gemini/Gemma API and Judge0 are external services and require the appropriate credentials.
- **Datasets/starter material:** The repository seeds a small starter question set. Bulk third-party datasets are not bundled. Generated questions are original practice prompts, not verified employer questions. Confirm and disclose any event starter code or other reused materials not documented here.
- **AI-assisted development:** AI assistance was used during implementation; the team should review the final repository and ensure this disclosure matches the event's required wording and the actual contributions.

## Challenge evidence

### Best Use of Gemma 4

- **Gemma 4 model identifier and Gemini API integration:** The server defaults `GEMMA_MODEL` to `gemma-4-26b-a4b-it` and uses the Google Gen AI SDK with the server-side `GEMINI_API_KEY`. The question-generation endpoint selects this configured model when the request chooses Gemma. Actual availability depends on the API key, model access, and current Google API support; configure and demonstrate a successful live request before judging.
- **Code link showing the integration:** [`server/src/config/gemini.js`](https://github.com/Swetha-Oruganti/HacktoberFest--Nxtwave-hack-day-/blob/submission/my-project/server/src/config/gemini.js) and [`server/src/routes/platformRoutes.js`](https://github.com/Swetha-Oruganti/HacktoberFest--Nxtwave-hack-day-/blob/submission/my-project/server/src/routes/platformRoutes.js).
- **Input and useful output; multimodal value where applicable:** The question workflow accepts a company, role, topic, difficulty, and target catalog size. Gemma returns structured, original practice questions which the server validates, deduplicates, and stores. This use is text-only; multimodal input is not used or claimed.

## Current status

- **What works:** Company question browsing and filtering; Gemini/Gemma question generation when configured; grounded company research with source links; registration/login and protected practice tracking; interview experience submission and admin moderation; authorized question import with source attribution and duplicate checks; Monaco editor with multiple language options; Judge0 execution when configured; persistent light/dark theme.
- **Known limitations / incomplete features:** Gemini/Gemma and Judge0 require valid provider credentials and access. Generated catalog targets are best-effort and do not guarantee a particular count. Grounded research can be incomplete or disagree across sources and should be treated as a preparation aid, not company policy. Third-party premium problem statements are not scraped or copied; imports require permission. A root open-source license file and attendee/team details still need confirmation for this submission. Verify the configured model with a live API request before claiming a successful Gemma demo.
- **What we would improve next:** Run and record a successful Gemma 4 generation demo; complete the license and attendee details; add more licensed/importable source references and first-hand candidate reports; expand editor/run feedback and deployment documentation; improve research source filtering and freshness indicators.

## Submission checklist

- [X ] Project repository is public and links work.
- [X] Required challenge evidence is included and a live Gemma 4 demo has been verified.
- [X] Project uses and links an open-source license where required by the challenge.
- [X] Team/attendee names and GitHub usernames are confirmed.
- [X] Work and reused materials are represented honestly.
- [X] No API keys, tokens, passwords, or private data are included.
- [X] I followed the organizers' build window and submission instructions.

# Samaj

## Team / attendee

- **Team name**: Team Samaj
- **Members and GitHub usernames**:
  - Surya S ([@suryaff733](https://github.com/suryaff733))
  - Gopal Reddy A ([@reddy-gopal](https://github.com/reddy-gopal))
  - Vexith Reddy T ([@VitCritical](https://github.com/VitCritical))
  - Rafi MD ([@DRafi2006](https://github.com/DRafi2006))
- **Profile links (optional)**:
  - https://github.com/suryaff733
  - https://github.com/reddy-gopal
  - https://github.com/VitCritical
  - https://github.com/DRafi2006

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [x] Best Use of Gemma 4

## Project links

- **Public GitHub repository**: https://github.com/reddy-gopal/samaj
- **Open-source license**: https://github.com/reddy-gopal/samaj/blob/main/LICENSE
- **Live Demo**: https://samaj-hazel-ten.vercel.app/

## Problem and solution

- **Who is this for**: Elderly citizens and regional language speakers in India.
- **Problem**: Confusing official letters and rampant scams (e.g. fake electricity disconnection, KYC freeze threats) demanding money via UPI.
- **How it works**:
  1. User takes a photo of a letter or uploads a PDF.
  2. Gemma 4 reads the document, pulls out deadlines/amounts, and flags scams.
  3. App displays a simple summary card in **Telugu**, **Hindi**, or **English** with voice reading and 1-tap WhatsApp sharing with family.

## Approach and technologies

- **AI Model**: Google's open-weight **Gemma 4** (`gemma-4-26b-a4b-it`) via `@google/genai`.
- **Frontend**: Next.js 16, React 19, Tailwind CSS.
- **PDF Engine**: Client-side Mozilla PDF.js (renders offline in browser).
- **Audio & Accessibility**: Web Speech API for regional voice playback; Noto Sans regional fonts.
- **Privacy & Safety**: In-memory processing only (zero image storage); strict Zod schema validation.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source AI role**: Built with open-source tools and Google's open-weight **Gemma 4** model as a multilingual document triage assistant.
- **Code link**: https://github.com/reddy-gopal/samaj/blob/main/app/api/explain/route.ts#L61-L105
- **Testing harness**: Automated 24-scenario evaluation benchmark with 100% pass rate (`npm run eval`).

### Best Use of Gemma 4

- **Model identifier**: `gemma-4-26b-a4b-it` using the `@google/genai` SDK.
- **Code link**: https://github.com/reddy-gopal/samaj/blob/main/app/api/explain/route.ts#L61-L105
- **Multimodal value**: Directly inspects photos and PDFs of physical letters in regional scripts (Telugu, Hindi), extracting key deadlines and amounts without hallucinating.

## Current status

- **What works**: Live camera, PDF inspection, Telugu/Hindi/English triage, Scam Shield, voice synthesis, WhatsApp sharing, and 8 demo notices.
- **Known limitations**: Voice synthesis quality depends on device's installed TTS pack; PDFs over 15 pages require single-page selection.
- **What to improve next**: Run Gemma 4 directly offline on device using WebGPU.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

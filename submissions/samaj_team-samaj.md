# Samaj

## Team / attendee

- **Team name (if applicable)**: Team Samaj
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

If listing both categories, confirm eligibility with the organizers and complete evidence for each.

## Project links

- **Public GitHub repository**: https://github.com/reddy-gopal/samaj
- **Open-source license (link to the license file)**: https://github.com/reddy-gopal/samaj/blob/main/LICENSE
- **Live Demo**: https://samaj-hazel-ten.vercel.app/

## Problem and solution

### Who is this for?
Elderly citizens, first-generation English readers, and small business owners in India who receive confusing, intimidating, or fraudulent official-looking notices.

### What problem does it solve?
In India, millions of households fall victim to extortion scams (e.g., electricity disconnection threats, fake bank KYC account freeze notices, and spoofed court summons) that demand money via personal UPI handles or threaten immediate arrest. Genuine critical notices also go unaddressed due to dense bureaucratic jargon.

### Main input → output workflow:
1. **Input**: User takes a photo of a physical document via the live camera viewfinder, uploads an image, or uploads a multi-page PDF notice.
2. **AI Processing**: Google Gemma 4 analyzes the document in-memory through the multimodal Gemini API, extracting core meaning, explicit deadlines, amounts due, and detecting scam indicators.
3. **Output**: The user receives an instant, high-contrast plain-language triage card in their native language (**Telugu**, **Hindi**, or **English**) with:
   - What the letter is and who issued it.
   - What immediate action to take (e.g., pay online, visit branch, ignore).
   - Strict visible deadlines and amounts due (evaluates strictly to `null` if not visible; never guessed).
   - Automated Scam Shield alert if suspicious demands (OTP, personal UPI, threats) are detected.
   - Regional voice readout (Web Speech TTS in `te-IN`, `hi-IN`, `en-IN`).
   - 1-tap WhatsApp family share button (`wa.me`) to consult trusted relatives before acting.

## Approach and technologies

### Implementation & Architecture:
- **Multimodal AI Engine**: Google's open-weight **Gemma 4** (`gemma-4-26b-a4b-it` and `gemma-4-31b-it`) via the `@google/genai` SDK.
- **Frontend / Framework**: Next.js 16 (Turbopack, App Router), React 19, TypeScript (Strict Mode), and Tailwind CSS.
- **PDF Processing**: Mozilla PDF.js (v3.11) locally vendored in `/public/pdfjs/` for 100% offline, privacy-first, client-side rendering with zero SSR issues.
- **Validation**: Zod runtime schema enforcement validating both user payloads and AI structured JSON outputs.
- **Localization & Typography**: `next/font/google` loading `Noto Sans Telugu` and `Noto Sans Devanagari` to eliminate broken regional font glyphs.
- **Audio & Sharing**: Browser Web Speech API (`SpeechSynthesis`) and WhatsApp deep links (`wa.me`).
- **Privacy Guarantee**: Zero persistent storage—images and PDFs are processed strictly in-memory and never saved to any database or bucket.

### Reused materials & libraries:
- `@google/genai` (Google Gen AI SDK)
- `pdfjs-dist` (Mozilla PDF.js client engine)
- `lucide-react` (icons)
- `zod` (schema validation)

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI component and its role**:
  Powered entirely by open-source tools and Google's open-weight **Gemma 4** models (`gemma-4-26b-a4b-it` and `gemma-4-31b-it`). The model acts as an empathetic, rigorous multilingual safety triage agent that parses physical document photos and regional language scripts without hallucination.
- **Code link showing the integration**:
  - Backend Gemma 4 integration: https://github.com/reddy-gopal/samaj/blob/main/app/api/explain/route.ts#L61-L105
  - Strict Zod schema contract: https://github.com/reddy-gopal/samaj/blob/main/lib/schema.ts
  - Security prompt-injection guard & zero-hallucination prompt: https://github.com/reddy-gopal/samaj/blob/main/lib/prompt.ts
- **Agent Skill Open Standard compliance / Harness**:
  Implemented an automated evaluation benchmark suite (`scripts/eval.ts`) testing 24 document scenarios across Telugu, Hindi, and English with an automated 100% pass rate (`npm run eval`).

### Best Use of Gemma 4

- **Gemma 4 model identifier and Gemini API integration**:
  - Model identifier: `gemma-4-26b-a4b-it` (primary), `gemma-4-31b-it` (secondary), with resilient fallback to `gemini-2.5-flash`.
  - Integrated via `@google/genai` SDK using `ai.models.generateContent` with inline Base64 multimodal image parts and strict JSON system prompts.
- **Code link showing the integration**:
  https://github.com/reddy-gopal/samaj/blob/main/app/api/explain/route.ts#L61-L105
- **Input and useful output; multimodal value where applicable**:
  - **Multimodal Value**: Gemma 4 directly inspects photographs of crumpled, folded, or multi-page paper documents, bills with official stamps, bank notices, and legal summons across complex Indian scripts (Telugu, Devanagari).
  - **Useful Output**: Converts dense, intimidating official text into a structured, plain-language breakdown (Document Type, Scam Risk, Core Meaning, Immediate Steps, Visible Deadline, Amount Due) in the recipient's chosen regional language. Delivers zero-hallucination extraction (strict `null` if data is missing) and amber uncertainty fallback for blurry photos.

## Current status

- **What works**:
  - Live camera capture, file picker, and desktop drag-and-drop.
  - Multi-page PDF client-side rendering with interactive page-by-page navigation.
  - Multilingual triage across Telugu, Hindi, and English.
  - Automated Scam Shield detecting OTP phishing and personal UPI extortion.
  - Web Speech API regional text-to-speech (`te-IN`, `hi-IN`, `en-IN`).
  - 1-tap WhatsApp formatted family sharing.
  - 8 preloaded interactive demo notices for 1-click evaluation.
  - Automated benchmark suite with 24/24 tests passing (100%).
- **Known limitations / incomplete features**:
  - Web Speech synthesis quality depends on the operating system's installed regional language voice packs (fallback to standard voice if Telugu pack is missing on the client device).
  - Documents over 15 PDF pages require single-page selection to stay within browser memory limits.
- **What you would improve next**:
  - Native offline edge inference using Gemma 4 via WebGPU/ONNX Runtime Web.
  - Direct integration with national cybercrime helpline reporting (`cybercrime.gov.in` / 1930) for flagged high-risk scam notices.
  - Expansion to additional regional languages (Tamil, Kannada, Bengali, Marathi).

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [x] Work and reused materials are represented honestly.
- [x] No API keys, tokens, passwords, or private data are included.
- [x] I followed the organizers' build window and submission instructions.

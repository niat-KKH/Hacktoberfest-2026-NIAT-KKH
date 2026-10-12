# Gemma Lens

## Team / attendee

- Team name (if applicable): N/A (solo)
- Members and GitHub usernames: YOUR NAME (@molletisanthoshkumar19-crypto)
- Profile links (optional): https://github.com/molletisanthoshkumar19-crypto

## Challenge

Select the challenge you are entering:

- [ ] Best Open-Source AI Project
- [x] Best Use of Gemma 4

If listing both categories, confirm eligibility with the organizers and complete evidence for each.

## Project links

- Public GitHub repository: https://github.com/molletisanthoshkumar19-crypto/gemma-lens
- Open-source license (link to the license file): https://github.com/molletisanthoshkumar19-crypto/gemma-lens/blob/main/LICENSE

## Problem and solution

Gemma Lens is for people who have information trapped in images, such as a beginner staring at an error screenshot, a student with a confusing diagram, or someone with a chart or notice they don't understand.

Workflow: the user uploads an image, types a question about it, and Gemma 4 reads the image and answers. The user can then ask follow-up questions about the same image, start new chats, and reopen earlier conversations from the history sidebar.

## Approach and technologies

- Frontend: React + Vite, with a dark/light theme, drag-and-drop upload, a chat view, and chat history saved in the browser (localStorage).
- Backend: Python + FastAPI. It validates the uploaded file, builds the request from the prompt, the image, and the earlier messages in that conversation, and calls Gemma 4 through the Gemini API. The API key stays on the backend in an environment variable.
- Model: Gemma 4 through the Gemini API, chosen because it accepts image and text input, which is what this use case needs.
- Interface language selector (English, Telugu, Hindi) is built into the frontend.
- Libraries used: React, Vite, FastAPI, lucide-react icons, and the Google Gen AI SDK.
- AI-assisted development: I used Claude (Anthropic) as a coding assistant for guidance, code, and debugging during this project.

## Challenge evidence

### Best Use of Gemma 4

- Gemma 4 model identifier and Gemini API integration: `gemma-4-26b-a4b-it` through the Gemini API
- Code link showing the integration: https://github.com/molletisanthoshkumar19-crypto/gemma-lens/blob/main/backend/main.py
- Input and useful output; multimodal value where applicable: Input is an image (for example a screenshot of a Python error) plus a text question. Output is a plain-language explanation of what the image shows, such as the cause of the error and the steps to fix it. Gemma reads the image directly, so users do not have to retype or search for the error text.

## Current status

- What works: image upload (drag and drop or file picker) with validation; asking a question about an image; Gemma 4 answers; follow-up questions in the same conversation; new chats; chat history that survives a refresh; dark/light theme; copy button for answers.
- Known limitations / incomplete features: the language selector currently translates only part of the interface, and Gemma's replies in Telugu and Hindi are not finished yet. Testing so far has focused on image inputs. Chat history is stored only in the browser.
- What you would improve next: full Telugu and Hindi support, including Gemma replying in the chosen language; more test coverage; more example prompts.

## Submission checklist

- [x] Project repository is public and links work.
- [x] Required challenge evidence is included.
- [x] Project uses an open-source license where required by the challenge.
- [ ] Work and reused materials are represented honestly.
- [ ] No API keys, tokens, passwords, or private data are included.
- [ ] I followed the organizers' build window and submission instructions.

# AI Prompts Used

This project was built with AI assistance. The prompts below document the main prompts and follow-up prompts used while building the MVP.

## 1. Initial Build Prompt

```text
Build a complete MVP repository called edgetex-agent.

Project goal:
Create a lightweight AI-assisted LaTeX editor built for the Cloudflare AI app assignment. It should feel like a very small Overleaf-style editor, but focused on AI-assisted editing rather than full LaTeX compilation.

The app name is: EdgeTex Agent

Use this stack:
- React + Vite + TypeScript frontend
- Cloudflare Workers backend API
- Cloudflare Workers AI binding for LLM calls
- Cloudflare D1 for persistence
- Wrangler config
- No Next.js
- No full pdflatex compiler
- No MCP integration for this MVP

Core UI:
- Header: "EdgeTex Agent"
- Left panel: LaTeX editor textarea
- Right panel: rendered preview
- Bottom or side panel: chat assistant
- Action buttons: Generate LaTeX, Improve Writing, Fix LaTeX, Make Academic, Review Formatting, Save Document

Backend API:
- POST /api/ai/edit
- POST /api/documents
- GET /api/documents/:id
- PUT /api/documents/:id
- GET /api/documents
- POST /api/messages
- GET /api/messages/:documentId

D1 schema:
- documents
- messages
- preferences

README requirements:
- Project title and description
- Assignment alignment
- Features
- Architecture diagram
- Local development instructions
- Cloudflare setup instructions
- Example use cases
- Limitations
- Future work
```

## 2. Runtime LLM Prompt Used By The App

The Worker uses this system prompt when calling Workers AI:

```text
You are EdgeTex Agent, an AI assistant for LaTeX and academic writing. The user is editing a LaTeX document. Follow the instruction while preserving valid LaTeX structure. Do not remove important content unless asked. Return JSON only with updatedContent, summary, and issues.
```

The user prompt sent to Workers AI includes:

````text
Mode: generate | improve | fix | academic | review
Document ID: current document id or "not saved yet"
Instruction: user's chat instruction
Mode-specific guidance
Return JSON only. The JSON shape must be:
{"updatedContent":"...","summary":"...","issues":["..."]}
Current LaTeX document:
```latex
...
```
````

Mode-specific guidance:

```text
Review mode: do not rewrite the whole document unless needed. Focus on formatting, clarity, structure, possible LaTeX syntax issues, readability, and academic tone.

Fix mode: fix obvious LaTeX syntax problems, preserve meaning, and explain what changed.

Academic mode: make the prose more formal and scholarly while keeping the document structure intact.

Generate mode: produce a coherent LaTeX document or section that follows the instruction.

Improve mode: improve clarity, concision, and flow while preserving the user's content.
```

## 3. Follow-Up Development Prompts

These smaller prompts were used to iterate on the project after the initial MVP was working:

```text
Push the repository to GitHub and explain how to test it locally and on Cloudflare.
```

```text
Explain how to connect Cloudflare Workers AI and D1, and help deploy the Worker.
```

```text
Add GitHub Actions CI/CD using Cloudflare API token and account id secrets.
```

```text
Improve the chat UX: clear the input after send, make Enter send the message, and format assistant issues more cleanly.
```

```text
Add document controls for new document, open saved document, import .tex, download .tex, and lightweight PDF export.
```

```text
Refine the editor UI so it feels less generated: improve toolbar grouping, panel headers, confirmation dialogs, Save/Send buttons, and tinted editor/assistant panels.
```

```text
Add a clear-chat trash button and replace browser confirm dialogs with an in-app confirmation modal.
```

## 4. Human Decisions Made During The Build

- Kept the app as a focused MVP instead of a full Overleaf clone.
- Avoided full LaTeX/PDF compilation and documented the preview limitation clearly.
- Added deterministic local AI fallback so local development works without a Workers AI binding.
- Used D1 for document and message persistence.
- Kept authentication out of scope for the MVP.
- Added CI/CD after deployment worked manually.
- Iterated on the UI layout to make it feel more like a practical editor and less like a generated demo page.

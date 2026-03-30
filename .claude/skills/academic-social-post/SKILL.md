---
name: academic-social-post
description: "Use when generating social media posts (Twitter/X or Xiaohongshu/RedNote) for academic paper promotion or Call for Papers (CFP) announcements, in English or Chinese"
---

# Academic Social Post Generator

Generate platform-specific social media posts for academic content.

## Input

Accepts one or more of:
- **PDF file path** — reads and extracts text from the PDF
- **URL** — fetches the webpage (arXiv, workshop site, etc.)
- **Inline text** — paper abstract, CFP details, or any description

If multiple inputs are provided, combine all as source material.

## Parameters

Infer these from the user's message. Only ask if genuinely ambiguous.

| Parameter | Options | How to infer |
|---|---|---|
| `platform` | `twitter`, `xiaohongshu`, `formal` | "X post"/"twitter" → twitter; "rednote"/"小红书" → xiaohongshu; "formal"/"professor"/"linkedin" → formal |
| `language` | `en`, `zh` | twitter defaults to `en`; xiaohongshu defaults to `zh`; user can override |
| `content_type` | `paper`, `cfp` | "CFP"/"call for papers"/"征稿" → cfp; otherwise → paper |

## Workflow

1. **Read source material**
   - If a file path ending in `.pdf` is provided: use the Read tool to read the PDF
   - If a URL is provided: use WebFetch to retrieve the page content
   - If inline text is provided: use it directly
   - Combine all sources into one block of source material
   - **Source priority:** When both a URL and PDF are provided, the URL/website is the authoritative source for factual details (dates, speakers, page limits). The PDF provides background context only. Do not use PDF details that contradict the website.

2. **Determine parameters**
   - Scan the user's message for platform, language, and content type signals
   - Apply defaults: twitter→en, xiaohongshu→zh, content_type→paper
   - If platform is unclear, ask the user (one question only)

3. **Load the prompt guide**
   - Read `references/prompts/{content_type}-{platform}-{language}.md`
   - This file contains the role, style rules, structure, and example for the target post

4. **Generate the post**
   - Follow the loaded prompt guide exactly
   - Use the source material as your information base
   - Output ONLY the final, ready-to-publish post text

5. **Deliver output**
   - Print the post directly to the terminal
   - If the user specified an output file path, also write it using the Write tool

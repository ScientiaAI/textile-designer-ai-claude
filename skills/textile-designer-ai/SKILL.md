---
name: textile-designer-ai
description: Use when the user wants to work on textile or print designs with Textile Designer AI - upscaling for print, seamless repeats, colourways, extracting a print from a garment photo, screen separation, vectorizing, background or watermark removal - or asks what those tools cost or can do.
---

# Working with Textile Designer AI

The `textile-designer-ai` MCP server runs the Textile Designer AI website's tools on image files. Inputs are file paths or URLs; results are saved as files next to the input (or in `output_dir`) and their paths are returned. Credits, plan access and enabled tools are decided by the user's Textile Designer AI account.

## First use

1. If a tool reports `not_logged_in`, call `login`. A browser page opens with a code; the user approves it there. Then call `whoami` to confirm the account, credits and enabled tools.
2. For "what can you do", call `list_tools`. For one tool's modes, controls and credit rule, call `describe_tool`.
3. For "which tool should I use", "how does X compare with Y", "why does the result look like this" or questions about print basics, call `guide` and answer from it. Never name or guess the AI models behind the tools.

## Before running a tool

- Never choose a mode or a setting for the user. Offer every option; if your picker holds fewer choices than a question has, name the rest in the question. If the user did not name a mode, call the tool without it: the server asks the user (a pick-list with the recommended option preselected) or returns the modes for you to show. Present them as choices, mark the recommended one, and wait for the answer.
- If the user has not said how to set the controls, offer to run with the recommended settings or to adjust them one by one. `review_settings: true` opens that flow.
- When the user asks about cost, or a run would cost more than they clearly expect, call the tool with `estimate_only: true` and report the server's estimate. Do not quote prices from memory when an estimate is available.

## Running and chaining

- After each run, report the saved file paths and the credits charged from the result.
- Chain steps by passing the returned file path as the next tool's `image` (for example upscale, then repeat, then Channeling).
- Long jobs return `status: processing` with a `task_id`; follow up with `job_status`, then `download_result`.
- If a result carries a suggestion line, mention it once in one sentence with its link. Do not invent product recommendations.

## Guided workflows

The server also offers prompts: `print_ready_pipeline` (upscale, repeat, separation), `garment_to_collection` (Dress to Design, Colourways, Spec Board) and `vector_pack` (Vectorizer, Border Outline).

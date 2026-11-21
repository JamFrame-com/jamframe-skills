---
name: render-templates
description: Render JamFrame templates into images, carousels, PDFs or MP4 videos. Use when the user wants to find one of their JamFrame templates (or a template-library design), see which values it takes, fill it with their text, photos or brand assets, render it, follow a video render, or check render credits and past renders.
license: MIT
compatibility: Needs the JamFrame MCP server (https://app.jamframe.com/mcp) connected to the agent.
metadata:
  openclaw:
    homepage: https://jamframe.com
---

# Render templates with JamFrame

JamFrame renders designs the user made once (templates) with new values. Every tool works in the one JamFrame workspace the user chose when connecting. Renders come from the template's own layout; no AI model is involved. To design a new template or change one, see the design-templates skill.

## Find and inspect a template

1. `list_templates` lists the workspace's templates first, then the template library. Pass `source` (`workspace`, `library` or `all`) and `query` (part of the name) to narrow it down, and `cursor` with the previous `nextCursor` for the next page.
2. Match the user's wording to a template by name. If several could match, ask which one they mean.
3. `get_template` returns its pages (size, video duration), whether it is animated, its parameters and `exampleModifications`. Use the parameter keys exactly as listed.

## Render

- Call `render_template` with `templateId`, `modifications` and `format`:
  - `png` (default), `jpeg` or `webp`: one URL per page, so a multi-page template gives a carousel.
  - `pdf`: one file with every page.
  - `mp4`: one video, the pages playing as scenes. Suggest it for animated templates.
- Modification keys: `title` sets every layer with that key; `page2@title` only page 2; `title.color`, `title.fontSize` or `badge.hidden` override a style; `audio` replaces an MP4's soundtrack and `audio.volume` (0–1) sets its volume.
- `includePages` renders only some pages, `scale` (1–3) sharpens images, `quality` (1–100) sets JPEG and WebP compression.
- Keys that match nothing come back in `warnings`; mention them so the user can fix a typo.
- Give the user the URLs and the credits used.

## Videos that take longer

An MP4 that takes more than about a minute comes back with `status` `queued` or `running` and a `jobId`. Call `get_render_job` with it every 15–30 seconds until `status` is `succeeded` (then give the URL and `duration`) or `failed` (then give the `error`; a failed render costs nothing).

## Brand assets

- `list_brand_assets` lists logos and images, videos, audio, fonts and brand colours. Filter with `kind` or `tag` (for example `logo`, `brand-color`, or a website domain), or search with `query`.
- Use an image URL as an image parameter's value, a video URL for a video parameter, an audio URL as `audio`, a colour's `hex` as a `.color` (text) or `.fill` (shape) override, and a font's `family` as `.fontFamily`.
- `upload_brand_asset_from_url` adds a public file to the library. Only use it when the user asks to add a file.

## Credits and history

- Each image or PDF page uses 1 render credit and each second of video 1 credit. Before a large render (many pages or a long video), say what it will cost.
- `get_workspace_status` gives the plan and `remaining` credits (`null` means no limit) and when the period ends.
- `list_renders` lists past renders with their files, newest first; filter by `status` or `templateId`.

## What these tools don't do

- They don't delete templates, connect social accounts or change plans and billing. The user does those in the JamFrame app.
- They don't generate pictures, video or audio from a prompt. If the user asks for one, offer to design a template or fill one of theirs instead.

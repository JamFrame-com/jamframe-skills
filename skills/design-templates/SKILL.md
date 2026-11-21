---
name: design-templates
description: Design a new JamFrame template from a description, or change an existing one from a plain-language instruction, with JamFrame's AI design agent, using the workspace's brand colours, fonts and logo. Use when the user wants a new template (a social post, carousel, story, thumbnail, document or animated video) or wants one restyled, resized, rewritten, extended with slides or animated.
license: MIT
compatibility: Needs the JamFrame MCP server (https://app.jamframe.com/mcp) connected to the agent.
metadata:
  openclaw:
    homepage: https://jamframe.com
---

# Design templates with JamFrame

JamFrame's design agent lays out text, shapes and the user's own images (brand logo, brand images, a reference image) into a template, and saves it in the workspace. It doesn't generate pictures, video or audio: photo slots get a stand-in image that the user fills in when rendering.

## Design a new template

1. Call `generate_template` with a `prompt` that says what it's for, the content and the style, for example "Instagram post announcing our summer sale, 30% off, bold and playful".
   - `size`: a preset id such as `instagram-post`, `instagram-story`, `linkedin-post`, `youtube-thumbnail` or `a4-portrait`, or `WIDTHxHEIGHT`. The default is 1080x1350.
   - `pages` for a carousel, `video: true` for an animated template (render it as `mp4`).
   - `brand`: the workspace's brand library is used by default; pass `false` for none, or explicit `colors`, `fonts` and `logo`.
   - `referenceImageUrl`: a public image to recreate or take inspiration from, only when the user gives one.
2. It takes 15–30 AI credits and up to about 100 seconds. If the answer has a `jobId` instead of a template, call `generate_template` again with only that `jobId`.
3. Tell the user the template's name, its parameters (the values they can change when rendering), the AI credits used, and the editor link. To get files, render it with `render_template`.

## Change a template

- Call `edit_template` with the `templateId` and an `instruction` ("make the headline bigger and use our brand colours", "turn it into a 5-slide carousel", "animate it as a video"). `page` says which page the instruction is about.
- It saves over the workspace template. Unless the user clearly wants the original replaced, confirm first or pass `saveAsCopy: true`. Library templates are always saved as a copy.
- It takes 10–30 AI credits. Parameter keys stay the same unless the instruction is about them.

## What these tools don't do

- They don't delete templates or change plans and billing; the user does those in the JamFrame app.
- They don't make photos or illustrations from a prompt. When the user only wants a picture, don't call them. If they want a post or banner around a picture, offer a design with a photo slot they can fill.

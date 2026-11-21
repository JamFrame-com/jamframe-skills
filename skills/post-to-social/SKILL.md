---
name: post-to-social
description: Post JamFrame renders to the user's connected social accounts (for example LinkedIn and Bluesky, and Instagram, Facebook, X, TikTok and more through Buffer), now, at a set time or as a draft, check each account's result, and report how posts performed. Use when the user asks to publish, schedule or draft a social post with a JamFrame template, render or media URLs, or asks how their posts did.
license: MIT
compatibility: Needs the JamFrame MCP server (https://app.jamframe.com/mcp) connected to the agent.
metadata:
  openclaw:
    homepage: https://jamframe.com
---

# Post to social accounts with JamFrame

JamFrame posts to social accounts the user connected in the JamFrame app. These tools can't connect new accounts.

## Before posting

1. `list_social_accounts` lists the connected accounts with their `id`, platform, name and `status`. An account with `needs_reconnect` can't post until the user reconnects it in the app. A `sandbox` account only simulates posting.
2. Pick the media:
   - a template rendered as part of the post: `templateId` and `modifications`, as for `render_template` (several pages post as a carousel where the platform has one; a `pdf` posts as a LinkedIn document);
   - an earlier render: `renderId` from `render_template` or `list_renders`;
   - public image, video or PDF URLs: `mediaUrls`.
3. Confirm with the user, in one message, the accounts, the caption (and any per-platform caption), the media, and when it goes out (now, `scheduledAt` with `timezone`, or a draft). A post can't be taken back once a platform has published it.

## Post

- Call `publish_post` with `accountIds`, `caption` and the media. Add `scheduledAt` (ISO 8601; without an offset it is read in `timezone`, such as `Europe/Paris`) to schedule it, or `draft: true` to save it in the app without posting.
- Platform options go in `options`, for example `{"linkedin": {"firstComment": "…"}}` or `{"youtube": {"title": "…", "privacy": "unlisted"}}`. TikTok needs `privacyLevel`; ask the user which one.
- Every account is checked first. If any account can't take the post, nothing is posted and the answer lists every problem: relay them and suggest the fix (for example `format: "jpeg"` for Instagram, or a shorter caption for X).
- Posting costs no credits; rendering a template as part of the post costs the usual render credits.

## After posting

Posting is asynchronous. `get_social_post` with the `postId` shows each account as `queued`, `rendering`, `publishing`, `published` (with its link) or `failed` (with the reason). One account failing doesn't stop the others. Canceling or retrying a post is done in the JamFrame app.

## How posts did

`get_social_analytics` gives totals for a date range (28 days up to today by default): impressions or views, reach, likes, comments, shares, saves, clicks and engagement, each with the change against the previous period. `include: ["daily"]` adds one row per day; the default `insights` add the top posts (with links), the best weekday and hour to post, and a per-platform comparison. Filter with `platform` or `accountIds`. A `null` metric means the platform doesn't report it, not zero. The range is limited to the plan's history.

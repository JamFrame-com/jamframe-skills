# JamFrame agent skills

Agent skills that teach AI agents (Claude, ChatGPT, Cursor, OpenClaw and others that read `SKILL.md` files) to use [JamFrame](https://jamframe.com) through its MCP server.

JamFrame turns one design into every post, flyer and video. Design a template once, then render images, carousels, PDFs and short MP4 videos from it, design new templates with AI, run spreadsheet workflows and post the results to your social accounts.

## Skills

| Skill | What it does |
| --- | --- |
| [render-templates](skills/render-templates/SKILL.md) | Find a template, see which values it takes, and render it as PNG, JPEG, WebP, PDF or MP4 |
| [design-templates](skills/design-templates/SKILL.md) | Design a new template from a description, or change one, with JamFrame's AI design agent and your brand kit |
| [post-to-social](skills/post-to-social/SKILL.md) | Post renders to your connected social accounts, now, scheduled or as drafts, and see how posts did |
| [run-workflows](skills/run-workflows/SKILL.md) | Run a workflow (a render for each row of a sheet, file, Airtable table or webhook) and report each row |

## 1. Connect the MCP server

The skills call the tools of JamFrame's remote MCP server:

```
https://app.jamframe.com/mcp
```

You sign in with your JamFrame account and pick a workspace. A [free account](https://app.jamframe.com/signup) works.

- **Claude:** Settings → Connectors → Add custom connector, with the URL above ([guide](https://jamframe.com/docs/ai-apps/claude/)).
- **ChatGPT:** [guide](https://jamframe.com/docs/ai-apps/chatgpt/).
- **Claude Code:** `claude mcp add --transport http jamframe https://app.jamframe.com/mcp`
- **Cursor and other clients:** this repo's [`.mcp.json`](.mcp.json).

## 2. Add the skills

```sh
npx skills add JamFrame-com/jamframe-skills
```

Or copy the `skills/` folders into your agent's skills folder.

## Links

- Website: https://jamframe.com
- AI apps guide: https://jamframe.com/docs/ai-apps/
- API reference: https://app.jamframe.com/docs/api
- SDKs: [Node.js](https://github.com/JamFrame-com/jamframe-node), [Python](https://github.com/JamFrame-com/jamframe-python)
- n8n node: [n8n-nodes-jamframe](https://github.com/JamFrame-com/n8n-nodes-jamframe)
- Support: support@jamframe.com

## License

MIT

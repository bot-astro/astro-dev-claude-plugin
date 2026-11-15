---
name: astro-discord-api-docs
description: Offline copy of the Discord developer docs (REST endpoints, gateway events, objects, permissions, interactions, components, OAuth2, Activities). Use when you need an exact Discord API reference before writing or changing code that calls or models the Discord API.
---

# Discord API docs

`references/llms-full.txt` is a snapshot of https://docs.discord.com/developers, taken from Discord's own `llms-full.txt`. Each page starts with a `# Title` line followed by a `Source:` URL.

**It can go stale.** Discord changes its API without notice and this copy is only refreshed by hand. For anything version-sensitive (new fields, limits, deprecations, new features), confirm against the live page given in the `Source:` line before relying on it. Prefer the live page when the two disagree.

## How to use it

- Don't read the whole file, it is huge. Grep for the endpoint, object, event or permission name, then read only the matching section.
- Find a page: `grep -n '^# ' references/llms-full.txt`, then read from that line.
- Find the live page: the `Source:` line right under the title.
- JDA wraps this API, so check how JDA names things as well. For how Astro builds messages, see `astro-discord-components` and `astro-discord-markdown`.

## Refresh

```bash
f=skills/astro-discord-api-docs/references/llms-full.txt
curl -sL https://docs.discord.com/llms-full.txt -o $f
sed -i '' -E 's/Bot [A-Za-z0-9_-]{20,}\.[A-Za-z0-9_-]{5,}\.[A-Za-z0-9_-]{20,}/Bot <BOT_TOKEN>/g; s/Basic [A-Za-z0-9+\/=]{30,}/Basic <BASE64_CREDENTIALS>/g' $f
```

The `sed` redacts the example credentials in Discord's docs, which GitHub push protection otherwise blocks. Review the diff, then commit it. `https://docs.discord.com/llms.txt` is the page index.

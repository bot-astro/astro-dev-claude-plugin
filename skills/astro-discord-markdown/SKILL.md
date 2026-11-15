---
name: astro-discord-markdown
description: Cheatsheet of the Discord markdown syntax available in Astro bot messages (text styles, headings, subtext, lists, quotes, code, links, mentions, emojis, timestamps, escaping). Use when writing or editing message text, i18n strings, or anything a user reads in Discord.
---

# Discord markdown

Syntax for the text inside Astro messages (text displays, i18n strings). For how to lay messages out, see `astro-discord-components`.

## Text styles

| Syntax | Result |
|---|---|
| `**text**` | bold |
| `*text*` / `_text_` | italic |
| `***text***` | bold italic |
| `__text__` | underline |
| `~~text~~` | strikethrough |
| `\|\|text\|\|` | spoiler |

Styles combine: `__***bold italic underline***__`.

## Headings and subtext

Only at the start of a line: `# H1`, `## H2`, `### H3`, and `-# small grey caption` (subtext, good for footers and hints).

## Lists and quotes

- Bullets: `- item` (nest with two leading spaces). Numbered: `1. item`.
- `> text` quotes one line; `>>> text` quotes everything after it, to the end of the message.
- To keep an empty line inside a quote, use a lone `>` line.

## Code

- Inline: `` `code` ``.
- Block: three backticks on their own lines, optionally with a language (`js`, `py`, `diff`, ...) for highlighting.

## Links

- Masked: `[label](https://example.com)`.
- `<https://example.com>` suppresses the link preview.

## Mentions and chips

| Syntax | Result |
|---|---|
| `<@userId>` | user mention |
| `<#channelId>` | channel mention |
| `<@&roleId>` | role mention |
| `@everyone`, `@here` | broadcast mentions |
| `</command sub:commandId>` | clickable slash command (subcommand after a space) |
| `<:name:emojiId>` | custom emoji |
| `<a:name:emojiId>` | animated custom emoji |
| `:smile:` | standard emoji (prefer the unicode character) |

Mentions in bot messages ping unless allowed-mentions is restricted, so set it deliberately.

## Timestamps

`<t:unixSeconds:flag>`, rendered in each viewer's timezone and locale:

| Flag | Shows |
|---|---|
| `t` / `T` | short / long time |
| `d` / `D` | short / long date |
| `f` / `F` | short / long date-time (with weekday) |
| `R` | relative, updates live ("in 2 hours") |

Prefer these over formatting dates in the bot.

## Escaping

Prefix a marker with `\` to show it literally: `\*not italic\*`, `` \` ``. Escape user-provided text before interpolating it so it can't inject formatting or mentions.

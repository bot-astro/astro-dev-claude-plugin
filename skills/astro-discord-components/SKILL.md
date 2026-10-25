---
name: astro-discord-components
description: How Astro builds Discord messages with Components V2 (containers, sections, separators, action rows) and the layout options available. Use when creating or changing any bot message, welcome/help/error replies, or deciding how to arrange content and buttons.
---

# Discord messages: Components V2

All bot messages use Components V2 (JDA `net.dv8tion.jda.api.components`), never `content` or embeds. The only exception is the voice channel interface, which still uses embeds. V2 messages can't carry `content`/`embeds`, and the flag can't be removed from a message once sent.

Build messages with the shared UI helpers in the `bot` module (`ui` package) so the look (colors, headings, fields, footers) changes from one place. Check JDA's `components` sources and the Discord component reference for exact limits.

## What nests in what

- **Top level:** container, text display, section, media gallery, file display, separator, action row.
- **Container children:** action row, text display, section, media gallery, separator, file display. No container inside a container.
- **Section:** a few text displays plus exactly one accessory, a thumbnail or a button (never a select).
- **Action row:** a handful of buttons, or a single select.
- The whole message has a cap on total components, containers and children included. Keep dense messages lean.

## Layout options

1. **Single container (default).** One accented card holding header section, fields, dividers and buttons. Closest to the old embed; use for most messages.
2. **Several containers.** Separate cards, each with its own accent color, for distinct topics or severities (e.g. result + warning).
3. **Container + loose components.** Put an action row or small text outside the card so buttons/footnotes read as detached from the content.
4. **No container.** Plain top-level text, separators and media for lightweight replies without a card.
5. **Section for side-by-side.** Text with a thumbnail (icon/avatar header) or with a button (one row per item, e.g. a list with an action each).
6. **Media / files.** A media gallery inside or outside a container; file displays only reference attachments.

## Conventions

- Accent color: brand purple by default, red for errors; the container accent is the only color signal.
- Heading → body → divider → fields → buttons is the usual order inside a card; footers are small grayed text.
- Use dividers between logical groups, invisible separators when you only need spacing.
- Disabling a finished prompt: disable the whole container instead of editing buttons one by one.
- All user-facing text goes through the i18n layer, never hardcoded.

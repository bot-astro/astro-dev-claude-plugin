# AstroDevToolkit

Private Claude Code plugin shared by the Astro repos (`astro`, `astro-bot.space`, `infra`).  
It is not published to any marketplace: each repo's `.claude/settings.json` registers this folder as a local `astro-dev` marketplace and enables the plugin.

## Layout

- `skills/astro-*`: Astro's own skills. Edit them here.
- `skills/*` (everything else): vendor skills (Crowdin, Infisical, Chargebee), tracked in `skills-lock.json`. Don't edit them by hand.

## Use it in a repo

Clone this repo next to the others (`bot-astro/astro-dev-claude-plugin`) and add to `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "astro-dev": { "source": { "source": "directory", "path": "../astro-dev-claude-plugin" } }
  },
  "enabledPlugins": { "AstroDevToolkit@astro-dev": true }
}
```

After editing a skill, run `/reload-plugins` in the session.

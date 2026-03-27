# zepphyr

Claude Code plugin — ZeppOS platform skills, ZeRoUI skills, and zeppos-reviewer agent for building zepp watch apps.

## Install

Add the marketplace and install the plugin (once per machine):

```
/plugin marketplace add bug-breeder/zepphyr
/plugin install zepphyr@zepphyr
```

Or, for project-level auto-install, add to `.claude/settings.json` in your repo:

```json
{
  "extraKnownMarketplaces": {
    "zepphyr": {
      "source": { "source": "github", "repo": "bug-breeder/zepphyr" }
    }
  },
  "enabledPlugins": { "zepphyr@zepphyr": true }
}
```

## What's included

| Name | Type | Trigger |
|------|------|---------|
| `zeppos` | Skill | Auto — ZeppOS code, `@zos/*` imports, platform questions |
| `zeroui` | Skill | Auto (agent-preloaded) — ZeRoUI code, `@bug-breeder/zeroui` usage |
| `zeppos-reviewer` | Agent | Via `/review` command in consuming repos |

## Update

```
/plugin marketplace update
```

## References

- [Create plugins](https://code.claude.com/docs/en/plugins)
- [Create and distribute a plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces)
- [Discover and install plugins](https://code.claude.com/docs/en/discover-plugins)
- [Plugins reference](https://code.claude.com/docs/en/plugins-reference)
- [Agent Skills](https://code.claude.com/docs/en/skills)
- [Subagents](https://code.claude.com/docs/en/sub-agents)

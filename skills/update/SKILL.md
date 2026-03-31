---
name: update
description: Update the zepphyr plugin to the latest version from GitHub. Only activate when explicitly invoked.
disable-model-invocation: true
---

Update zepphyr to the latest version:

1. Clear the local marketplace cache so Claude Code re-fetches from GitHub:
   ```bash
   rm -rf ~/.claude/plugins/marketplaces/zepphyr/
   ```

2. Tell the user: "Marketplace cache cleared. Now run `/plugin update zepphyr@zepphyr` to install the latest version."

---
name: update
description: Update the zepphyr plugin to the latest version from GitHub. Only activate when explicitly invoked.
disable-model-invocation: true
---

Update zepphyr to the latest version:

1. Pull the latest marketplace metadata from GitHub:
   ```bash
   git -C ~/.claude/plugins/marketplaces/zepphyr pull
   ```

2. Tell the user: "Marketplace updated. Now run `/plugin update zepphyr@zepphyr` to install the latest version."

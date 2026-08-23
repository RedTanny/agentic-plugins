<!--
  Catalog fragment — maintain via create-collection workflow (assistant + maintainer + PR review).
  Golden sources: skills/*/SKILL.md, README.md, AGENTS.md
-->

## Deploy and use
**Note:** This skill pack is released as Developer Preview. Developer Preview features provide early access to functionality in advance of possible inclusion in a Red Hat product offering. For more information about the support scope of Red Hat Developer Preview features, see [Developer Preview Support Scope](https://access.redhat.com/support/offerings/devpreview).

### Prerequisites

- At least one supported AI coding assistant:
  - [Claude Code](https://claude.com/product/claude-code) (CLI or IDE extension)
  - [GitHub Copilot](https://github.com/features/copilot) (CLI or VS Code)
  - [Cursor](https://www.cursor.com/)
  - [OpenCode](https://opencode.ai/)
- [Lola](https://github.com/LobsterTrap/lola) CLI installed
- [Podman](https://podman.io/) (or Docker) — the MCP servers run as containers

### Step 1: Install the skill pack

```bash
# Add the Red Hat Agentic marketplace (one-time setup)
lola market add rh-agentic-plugins https://raw.githubusercontent.com/RHEcosystemAppEng/agentic-catalog/main/marketplace/rh-agentic-collection.yml

# Install the rh-sre pack (replace claude-code with your AI assistant)
# Valid targets: claude-code, copilot-cli, copilot-vscode, cursor, opencode
lola install rh-sre -a claude-code
```

This installs the skills, the instructions file, and the MCP server definitions into your project.

Verify the installation:

```bash
lola list
```

### Step 2: Configure environment variables

The pack uses three MCP servers that require credentials passed as environment variables. **Never hardcode tokens — always use environment variables.**

**For CVE discovery and remediation** (`lightspeed-mcp`):

1. Create a Red Hat Lightspeed service account at [console.redhat.com](https://console.redhat.com/)
2. Export the credentials:

```bash
export LIGHTSPEED_CLIENT_ID="<your-client-id>"
export LIGHTSPEED_CLIENT_SECRET="<your-client-secret>"
```

**For Ansible Automation Platform playbook execution** (optional — `aap-mcp-job-management`, `aap-mcp-inventory-management`):

```bash
export AAP_MCP_SERVER="<your-aap-controller-hostname>"
export AAP_API_TOKEN="<your-api-token>"
```

### Step 3: Use the skills

The pack provides 13 skills. See the [rh-sre README](../README.md) for the full list with descriptions and usage examples.

### Uninstall

Remove the skill pack from your project:

```bash
lola uninstall rh-sre
```

To also remove the marketplace registry:

```bash
lola market rm rh-agentic-plugins
```

<!--
  Deploy & Use instructions for the ocp-admin skill pack.
  Golden sources: skills/*/SKILL.md, README.md, AGENTS.md, mcps.json
-->

## Deploy and use
**Note:** This skill pack is released as Developer Preview. Developer Preview features provide early access to functionality in advance of possible inclusion in a Red Hat product offering. For more information about the support scope of Red Hat Developer Preview features, see Developer Preview Support Scope.

### Prerequisites

- At least one supported AI coding assistant:
  - [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (CLI or IDE extension)
  - [GitHub Copilot](https://docs.github.com/en/copilot) (CLI or VS Code)
  - [Cursor](https://www.cursor.com/)
  - [Gemini CLI](https://github.com/google-gemini/gemini-cli)
  - [OpenClaw](https://github.com/openclaw/openclaw)
  - [OpenCode](https://github.com/opencode-ai/opencode)
- [Lola](https://github.com/LobsterTrap/lola) CLI installed
- [Podman](https://podman.io/) (or Docker) — the MCP servers run as containers (see [OS-specific setup](#os-specific-setup))
- A Red Hat account with access to [cloud.redhat.com](https://cloud.redhat.com)

### Step 1: Install the skill pack

```bash
# Add the Red Hat Agentic marketplace (one-time setup)
lola market add rh-agentic-collection https://raw.githubusercontent.com/RHEcosystemAppEng/agentic-catalog/main/marketplace/rh-agentic-collection.yml

# Install the ocp-admin pack (replace claude-code with your AI assistant)
# Valid targets: claude-code, copilot-cli, copilot-vscode, cursor, gemini-cli, openclaw, opencode
lola install ocp-admin -a claude-code
```

This installs the skills, the `AGENTS.md` routing file, and the `mcps.json` MCP server definitions into your project.

Verify the installation:

```bash
lola list
```

### Step 2: Configure environment variables

The pack uses three MCP servers, each requiring specific credentials passed as environment variables. **Never hardcode tokens or paths — always use environment variables.**

**For cluster creation and inventory** (`openshift-self-managed`, `openshift-ocm-managed`):

1. Go to [https://cloud.redhat.com/openshift/token](https://cloud.redhat.com/openshift/token)
2. Click **Load token** → **Copy to clipboard**
3. Export it:

```bash
export OFFLINE_TOKEN="<your-token>"
```

**For cluster operations and reporting** (`openshift-administration`):

```bash
export KUBECONFIG="/path/to/your/kubeconfig"
```

To make these persistent, add them to your shell profile (`~/.bashrc`, `~/.zshrc`).

### Step 3: Use the skills

The pack provides 7 skills. See the [ocp-admin README](../README.md) for the full list with descriptions and usage examples.

### OS-specific setup

#### Linux (Fedora / RHEL)

**Podman** (required for MCP servers):

```bash
sudo dnf install -y podman
```

**Additional tools for security skills** (`/container-cve-validator`, `/coreos-cve-validator`, `/image-inspect`):

```bash
# Required
pip install requests

# regctl
curl -L https://github.com/regclient/regclient/releases/latest/download/regctl-linux-amd64 -o ~/.local/bin/regctl
chmod +x ~/.local/bin/regctl

# cosign
curl -L https://github.com/sigstore/cosign/releases/latest/download/cosign-linux-amd64 -o ~/.local/bin/cosign
chmod +x ~/.local/bin/cosign

# Optional (fallback SBOM generation)
curl -sSfL https://raw.githubusercontent.com/anchore/syft/main/install.sh | sh -s -- -b ~/.local/bin
```

#### macOS

**Podman** (required for MCP servers):

```bash
brew install podman
podman machine init
podman machine start
```

**Additional tools for security skills** (`/container-cve-validator`, `/coreos-cve-validator`, `/image-inspect`):

```bash
# Required
pip install requests
brew install regclient/tap/regctl
brew install sigstore/tap/cosign

# Optional (fallback SBOM generation)
brew install anchore/syft/syft
```

### Uninstall

Remove the skill pack from your project:

```bash
lola uninstall ocp-admin
```

To also remove the marketplace registry:

```bash
lola market rm rh-agentic-collection
```

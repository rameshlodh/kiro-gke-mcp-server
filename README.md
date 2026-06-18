# GKE MCP Server Setup for Kiro CLI

Connect Kiro CLI to your Google Kubernetes Engine clusters using the GKE MCP server.

## Prerequisites

- macOS (Apple Silicon or Intel)
- Google Cloud SDK (`gcloud`) installed and configured
- `gke-gcloud-auth-plugin` installed (required for cluster authentication)
- GCP project with GKE clusters
- Authenticated with GCP: `gcloud auth application-default login`

## Installation

### Install GKE MCP Server

```bash
curl -sSL https://raw.githubusercontent.com/GoogleCloudPlatform/gke-mcp/main/install.sh | bash
```

Verify installation:

```bash
gke-mcp --help
```

### Install gke-gcloud-auth-plugin

Required for authenticating to GKE clusters:

```bash
gcloud components install gke-gcloud-auth-plugin
```

Verify:

```bash
gke-gcloud-auth-plugin --version
```

If you get `command not found`, the plugin is installed but not in your PATH. Add it based on your setup:

**Apple Silicon (Homebrew install):**
```bash
export PATH="/opt/homebrew/share/google-cloud-sdk/bin:$PATH"
```

**Intel Mac or manual install:**
```bash
export PATH="/usr/local/share/google-cloud-sdk/bin:$PATH"
```

Make it permanent by adding the appropriate line to `~/.zshrc`:

```bash
echo 'export PATH="/opt/homebrew/share/google-cloud-sdk/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### Kiro MCP Configuration

The MCP config is located at `.kiro/settings/mcp.json`:

```json
{
  "mcpServers": {
    "gke": {
      "command": "gke-mcp",
      "args": [],
      "transportType": "stdio"
    }
  }
}
```

## Usage

Once configured, restart Kiro in this project directory. You can then interact with your GKE clusters through natural language.

### Example Prompts

- "List my GKE clusters"
- "Get details of cluster X in project Y"
- "Show pods in namespace default"
- "Query logs for my cluster"
- "Create a new Autopilot cluster"
- "Deploy a workload to my cluster"
- "Get recommendations for my cluster"

## Available MCP Tools

| Tool | Description |
|------|-------------|
| `list_clusters` | List GKE clusters |
| `get_cluster` | Get detailed info about a cluster |
| `create_cluster` | Create a new GKE cluster (defaults to Autopilot) |
| `update_cluster` | Update a GKE cluster |
| `get_kubeconfig` | Configure kubeconfig for a cluster |
| `list_node_pools` | List node pools in a cluster |
| `get_node_pool` | Get details for a node pool |
| `create_node_pool` | Create a new node pool |
| `update_node_pool` | Update a node pool |
| `get_k8s_resource` | Get Kubernetes resources from a cluster |
| `describe_k8s_resource` | Describe a Kubernetes resource in detail |
| `patch_k8s_resource` | Patch a Kubernetes resource |
| `delete_k8s_resource` | Delete a Kubernetes resource |
| `apply_k8s_manifest` | Apply a manifest using server-side apply |
| `list_k8s_events` | Retrieve events from a cluster |
| `list_k8s_api_resources` | List available API resources |
| `get_k8s_logs` | Get logs from a container in a pod |
| `get_k8s_version` | Get Kubernetes server version |
| `get_k8s_cluster_info` | Get cluster endpoint info |
| `get_k8s_rollout_status` | Check rollout status of a resource |
| `check_k8s_auth` | Check if an action is allowed on a resource |
| `gke_deploy` | Deploy a workload using a config file |
| `generate_manifest` | Generate a K8s manifest using Vertex AI |
| `query_logs` | Query GCP logs using LQL |
| `get_log_schema` | Get schema for a GKE log type |
| `list_recommendations` | List recommendations for clusters |
| `get_k8s_changelog` | Get Kubernetes changelog for upgrades |
| `get_gke_release_notes` | Get GKE release notes |

## Troubleshooting

### `gke-mcp: command not found`

Ensure `/usr/local/bin` is in your PATH:

```bash
export PATH="/usr/local/bin:$PATH"
```

### Authentication errors

Re-authenticate with GCP:

```bash
gcloud auth application-default login
```

### `.zshrc` errors about google-cloud-sdk

If you see `no such file or directory: /opt/homebrew/share/google-cloud-sdk/path.bash.inc`, verify your SDK location and update `~/.zshrc` accordingly. Common locations:

- Apple Silicon (Homebrew): `/opt/homebrew/share/google-cloud-sdk`
- Intel Mac / manual: `/usr/local/share/google-cloud-sdk`

## References

- [GKE MCP Server (GitHub)](https://github.com/GoogleCloudPlatform/gke-mcp)
- [GKE Remote MCP Server (Google Cloud)](https://cloud.google.com/kubernetes-engine/docs/how-to/use-gke-mcp)
- [Model Context Protocol](https://modelcontextprotocol.io)

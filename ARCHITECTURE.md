# Architecture

## Network and workspace

```mermaid
flowchart LR
    Device["Tailscale device"] --> VPN["Tailscale DNS / VPN"] --> Mini["Mac mini"]
    Mini --> Paseo --> Workspace["Paseo workspace\n(repository or worktree)"]
    Workspace --> Safehouse["agent-safehouse"] --> Pi["Pi agent"]

    Pi -->|"select one"| OMLX["OMLX"]
    Pi -->|"select one"| Kapitsa["Kapitsa"]
    Pi -->|"select one"| Copilot["GitHub Copilot"]
```

**Paseo setup:** Register the repository on the mini, create a local or worktree workspace per task, then launch Pi through `agent-safehouse`. Ensure Paseo can resolve the wrapper defined in `.zshrc`.

**Providers:** GitHub Copilot is this harness's default. Configure OMLX or Kapitsa on the mini before selecting it in Pi.

## Jira and engineering flow

```mermaid
flowchart TD
    Start["Jira issue or engineering idea"] --> Intake["Pi: ticket-to-spec"]
    Intake <--> Jira["Jira via MCP"]
    Intake --> Spec["Write/update spec"]
    Spec --> Build["build-loop"]
    Build --> Tests["Test red → change → green"]
    Tests --> Review["code-review"]
    Review --> Done["Commit and report"]
```

Configure the Jira MCP server as `jira`; keep credentials private. New Jira issues require confirmation.

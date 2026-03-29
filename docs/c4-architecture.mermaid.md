# C4 Architecture

Projections from `docs/c4-events.ndjson` (frozen through pass 4).

## L1 — System context

```mermaid
flowchart TB
  actor_integrator["actor:integrator<br/>Connector integrator"]
  actor_maintainer["actor:maintainer<br/>Repository maintainer"]
  ext_github["ext:github<br/>GitHub"]
  ext_npm["ext:npm_ecosystem<br/>Node/npm/Yarn ecosystem"]
  sys_decreen["sys:decreen_connectors<br/>Decreen Connectors"]
  actor_integrator -->|"edge:integrator_github<br/>obtain repository"| ext_github
  actor_maintainer -->|"edge:maintainer_github<br/>git operations"| ext_github
  actor_maintainer -->|"edge:maintainer_workspace<br/>edit working tree"| sys_decreen
  sys_decreen -->|"edge:workspace_remote<br/>git remote sync"| ext_github
```

## L2 — Containers

```mermaid
flowchart TB
  subgraph sys_decreen["sys:decreen_connectors"]
    c_workspace["container:local_repo_workspace<br/>Local repository workspace"]
  end
  actor_maintainer["actor:maintainer<br/>Repository maintainer"]
  ext_github["ext:github<br/>GitHub"]
  actor_maintainer -->|"edge:maintainer_workspace<br/>edit working tree"| c_workspace
  c_workspace -->|"edge:workspace_remote<br/>git remote sync"| ext_github
```

## L3 — `container:local_repo_workspace`

```mermaid
flowchart TB
  %% SCOPE: urn:c4:container:container:local_repo_workspace
  subgraph c_workspace["container:local_repo_workspace"]
    direction TB
    comp_git["component:git_remote_integration<br/>Git remote integration"]
    comp_ignore["component:repo_ignore_policy<br/>Repository ignore policy"]
  end
  ext_github["ext:github<br/>GitHub"]
  ext_npm["ext:npm_ecosystem<br/>Node/npm/Yarn ecosystem"]
  comp_git -->|"edge:git_remote_github<br/>origin remote"| ext_github
  comp_ignore -->|"edge:ignore_npm_paths<br/>excludes node_modules and package caches"| ext_npm
```

### L3 kind annotations (from stream)

| ID | KIND (primary) |
|----|----------------|
| `component:git_remote_integration` | `integration` |
| `component:repo_ignore_policy` | `boundary` |

## Full containment map

| Parent | Child |
|--------|--------|
| `sys:decreen_connectors` | `container:local_repo_workspace` |
| `container:local_repo_workspace` | `component:git_remote_integration` |
| `container:local_repo_workspace` | `component:repo_ignore_policy` |

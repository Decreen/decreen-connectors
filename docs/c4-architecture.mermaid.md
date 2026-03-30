# C4 Architecture

Projections from `docs/c4-events.ndjson` (append-only). Pass 4 adds no new elements.

## L1 — System Context

```mermaid
flowchart TB
  actor_dev["actor:dev — Developer"]
  ext_github["ext:github — GitHub (git remote host)"]
  sys_repo["sys:connectors-repo — Decreen Connectors (repository)"]
  actor_dev -->|"edge:dev-sys — Maintains / edits"| sys_repo
  sys_repo -->|"edge:sys-github — Remote origin / collaboration"| ext_github
  actor_dev -->|"edge:dev-github — Git push / pull / browse"| ext_github
```

## L2 — Container diagram

```mermaid
flowchart TB
  actor_dev["actor:dev — Developer"]
  ext_github["ext:github — GitHub (git remote host)"]
  subgraph sys_connectors["%% SCOPE: urn:c4:container:sys:connectors-repo<br/>sys:connectors-repo — Decreen Connectors (repository)"]
    container_tracked["container:tracked-config — Tracked configuration and repository metadata"]
  end
  actor_dev -->|"edge:dev-sys — Maintains / edits"| sys_connectors
  sys_connectors -->|"edge:sys-github — Remote origin / collaboration"| ext_github
  actor_dev -->|"edge:dev-github — Git push / pull / browse"| ext_github
```

## L3 — container:tracked-config

```mermaid
flowchart TB
  actor_dev["actor:dev — Developer"]
  ext_github["ext:github — GitHub (git remote host)"]
  subgraph sys_connectors["%% SCOPE: urn:c4:container:sys:connectors-repo"]
    subgraph container_tracked["%% SCOPE: urn:c4:container:container:tracked-config<br/>container:tracked-config — Tracked configuration and repository metadata"]
      component_ignore["%% KIND: boundary<br/>component:ignore-rules — Path ignore rules"]
      component_origin["%% KIND: integration<br/>component:origin-link — Git remote origin configuration"]
    end
  end
  actor_dev -->|"edge:dev-sys — Maintains / edits"| sys_connectors
  sys_connectors -->|"edge:sys-github — Remote origin / collaboration"| ext_github
  actor_dev -->|"edge:dev-github — Git push / pull / browse"| ext_github
  actor_dev -->|"edge:dev-ignore — Edits ignore patterns"| component_ignore
  component_origin -->|"edge:origin-github — Remote URL / fetch target"| ext_github
  component_ignore -->|"edge:ignore-in-container — Part of"| container_tracked
  component_origin -->|"edge:origin-in-container — Part of"| container_tracked
```

## Containment map

| Element ID | Type | Contained in |
|------------|------|----------------|
| `sys:connectors-repo` | system | — |
| `container:tracked-config` | container | `sys:connectors-repo` |
| `component:ignore-rules` | component | `container:tracked-config` |
| `component:origin-link` | component | `container:tracked-config` |

# C4 Architecture

Derived from `docs/c4-events.ndjson` (Pass 4 projection; no new elements).

## L1 — System context

```mermaid
flowchart TB
  "actor:integrator"["Integrator"]
  "actor:maintainer"["Maintainer"]
  "ext:git_remote"["Git hosting"]
  "sys:connectors_repo"["Decreen connectors repository"]
  "ext:git_remote" -->|"Hosts"| "sys:connectors_repo"
  "actor:integrator" -->|"Consumes"| "sys:connectors_repo"
  "actor:maintainer" -->|"Maintains"| "sys:connectors_repo"
  "actor:integrator" -->|"Clone / pull"| "ext:git_remote"
  "actor:maintainer" -->|"Push / maintain"| "ext:git_remote"
```

## L2 — Containers

```mermaid
flowchart TB
  "sys:connectors_repo"["Decreen connectors repository"]
  "container:source_tree"["Version-controlled source tree"]
  "sys:connectors_repo" --> "container:source_tree"
```

## L3 — `container:source_tree`

```mermaid
flowchart TB
  subgraph scope_src["%% SCOPE: urn:c4:container:source_tree"]
    "container:source_tree"["Version-controlled source tree"]
    "component:vcs_ignore_policy"["%% KIND: boundary,storage<br/>VCS ignore policy"]
    "component:license_terms"["%% KIND: integration,storage<br/>License terms bundle"]
    "component:declared_dependencies"["%% KIND: integration,boundary<br/>Declared dependency manifests (none present)"]
  end
  "container:source_tree" -->|"Defines"| "component:vcs_ignore_policy"
  "container:source_tree" -->|"Includes"| "component:license_terms"
  "container:source_tree" -->|"Would host"| "component:declared_dependencies"
```

## Containment map

| Element | ID | Contained in |
| --- | --- | --- |
| System | `sys:connectors_repo` | — |
| Container | `container:source_tree` | `sys:connectors_repo` |
| Component | `component:vcs_ignore_policy` | `container:source_tree` |
| Component | `component:license_terms` | `container:source_tree` |
| Component | `component:declared_dependencies` | `container:source_tree` |

# C4 Architecture

Final projection from `docs/c4-events.ndjson` (replayed through Pass 4 freeze).

## L1 — System context

```mermaid
flowchart TD
  actor_developer["Developer"]
  ext_github["GitHub"]
  sys_decreen_connectors["Decreen connectors (VCS-hosted repository)"]
  actor_developer -->|"push / pull"| ext_github
  ext_github -->|"hosts"| sys_decreen_connectors
  actor_developer -->|"clone / contribute"| sys_decreen_connectors
```

## L2 — Container diagram

```mermaid
flowchart TB
  actor_developer["Developer"]
  ext_github["GitHub"]
  subgraph sys_decreen_connectors["Decreen connectors (VCS-hosted repository)"]
    container_repository_tree["Repository tree<br/><small>filesystem / VCS working tree</small>"]
  end
  actor_developer -->|"push / pull"| ext_github
  ext_github -->|"hosts"| sys_decreen_connectors
  actor_developer -->|"clone / contribute"| sys_decreen_connectors
```

## L3 — Repository tree

```mermaid
flowchart TB
  subgraph container_repository_tree["Repository tree"]
    direction TB
    %% SCOPE: urn:c4:container:repository_tree
    component_boundary_gitignore[".gitignore rules"]
    %% KIND: boundary
    component_storage_license["LICENSE text"]
    %% KIND: storage
    component_integration_gitignore["VCS ignore integration"]
    %% KIND: integration
    component_boundary_gitignore -->|"co-located artifacts"| component_storage_license
  end
```

## Containment Map

```json
{
  "parentToChildren": {
    "sys_decreen_connectors": ["container_repository_tree"],
    "container_repository_tree": [
      "component_boundary_gitignore",
      "component_storage_license",
      "component_integration_gitignore"
    ]
  },
  "childToParent": {
    "container_repository_tree": "sys_decreen_connectors",
    "component_boundary_gitignore": "container_repository_tree",
    "component_storage_license": "container_repository_tree",
    "component_integration_gitignore": "container_repository_tree"
  }
}
```

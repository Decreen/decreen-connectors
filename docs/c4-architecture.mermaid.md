# C4 Architecture

Derived from `docs/c4-events.ndjson` (replay projection). Evidence scope: committed source and configuration only; repository currently has no application runtime beyond workspace and toolchain surfaces implied by `.gitignore`.

## L1 — System context

```mermaid
flowchart LR
  actor_contributor["actor:contributor<br/>Contributor"]
  ext_npm["ext:npm_registry<br/>npm registry"]
  sys_dc["sys:decreen_connectors<br/>Decreen connectors (repository-only workspace)"]
  actor_contributor -->|"consumes_packages"| ext_npm
  actor_contributor -->|"develops"| sys_dc
  sys_dc -->|"resolves_dependencies_via"| ext_npm
```

## L2 — Containers

```mermaid
flowchart TB
  sys_dc["sys:decreen_connectors<br/>Decreen connectors (repository-only workspace)"]
  c_repo["container:repository_workspace<br/>Git-tracked workspace"]
  c_npm["container:npm_toolchain_surface<br/>npm or Yarn dependency surface"]
  sys_dc -->|"contains"| c_repo
  sys_dc -->|"contains"| c_npm
```

## L3 — `container:repository_workspace`

```mermaid
flowchart TB
  %% SCOPE: urn:c4:container:repository_workspace
  c_repo["container:repository_workspace<br/>Git-tracked workspace"]
  comp_ign["component:ignore_policy_surface<br/>Ignore policy surface<br/>%% KIND: boundary"]
  comp_tree["component:versioned_tree_surface<br/>Versioned tree surface<br/>%% KIND: storage"]
  c_repo -->|"contains"| comp_ign
  c_repo -->|"contains"| comp_tree
```

## L3 — `container:npm_toolchain_surface`

```mermaid
flowchart TB
  %% SCOPE: urn:c4:container:npm_toolchain_surface
  c_npm["container:npm_toolchain_surface<br/>npm or Yarn dependency surface"]
  ext_npm["ext:npm_registry<br/>npm registry"]
  comp_reg["component:registry_resolution_surface<br/>Registry resolution surface<br/>%% KIND: integration"]
  comp_local["component:local_install_tree_surface<br/>Local install tree surface<br/>%% KIND: storage"]
  c_npm -->|"contains"| comp_reg
  c_npm -->|"contains"| comp_local
  comp_reg -->|"uses"| ext_npm
```

## Full containment map

| Element | Contained in |
|---------|----------------|
| `sys:decreen_connectors` | _(system boundary)_ |
| `container:repository_workspace` | `sys:decreen_connectors` |
| `container:npm_toolchain_surface` | `sys:decreen_connectors` |
| `component:ignore_policy_surface` | `container:repository_workspace` |
| `component:versioned_tree_surface` | `container:repository_workspace` |
| `component:registry_resolution_surface` | `container:npm_toolchain_surface` |
| `component:local_install_tree_surface` | `container:npm_toolchain_surface` |

External: `ext:npm_registry` (not inside the system boundary).

Edges (from stream): `edge:e1` actor→ext; `edge:e2` actor→sys; `edge:e3` sys→ext; `edge:e4`–`e5` sys→containers; `edge:e6`–`e9` container→component containment; `edge:e10` component→ext.

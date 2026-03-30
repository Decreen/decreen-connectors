# C4 Architecture

Derived from `docs/c4-events.ndjson` (append-only stream). Pass 4 adds no new elements.

## L1 — System context

```mermaid
flowchart LR
  actor_developer["Developer"]
  ext_github["GitHub"]
  ext_npm["npm package registry"]
  ext_license["Apache License 2.0 terms"]
  sys_boundary["Decreen Connectors (repository)"]
  actor_developer -->|"uses Git remote"| ext_github
  actor_developer -->|"consumes packages (implied by ignore rules)"| ext_npm
  actor_developer -->|"maintains"| sys_boundary
```

## L2 — Containers

```mermaid
flowchart TB
  sys_boundary["Decreen Connectors (repository)"]
  ctr_bootstrap["Repository bootstrap (config-only)"]
  ext_github["GitHub"]
  ext_license["Apache License 2.0 terms"]
  sys_boundary --- ctr_bootstrap
  ctr_bootstrap -->|"source hosted at"| ext_github
  ctr_bootstrap -->|"distributes under"| ext_license
```

## L3 — Components (container: `repo_bootstrap`)

```mermaid
flowchart TB
  subgraph repo_bootstrap["%% SCOPE: urn:c4:container:repo_bootstrap"]
    comp_npm["%% KIND: integration\nnpm/Node ecosystem integration"]
    comp_build["%% KIND: pipeline\nBuild output path exclusions"]
  end
```

## Containment Map

```json
{
  "parentToChildren": {
    "decreen_connectors": ["repo_bootstrap"],
    "repo_bootstrap": ["npm_ecosystem_integration", "build_output_pipeline"]
  },
  "childToParent": {
    "repo_bootstrap": "decreen_connectors",
    "npm_ecosystem_integration": "repo_bootstrap",
    "build_output_pipeline": "repo_bootstrap"
  }
}
```

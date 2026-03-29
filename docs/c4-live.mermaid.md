# Live C4 Preview

```mermaid
flowchart TB
  subgraph L1["Actors & externals"]
    "actor:integrator"["Integrator"]
    "actor:maintainer"["Maintainer"]
    "ext:git_remote"["Git hosting"]
  end
  subgraph L2["Containers"]
    "sys:connectors_repo"["Decreen connectors repository"]
    subgraph src_scope["%% SCOPE: urn:c4:container:source_tree"]
      "container:source_tree"["Version-controlled source tree"]
      "component:vcs_ignore_policy"["%% KIND: boundary,storage<br/>VCS ignore policy"]
      "component:license_terms"["%% KIND: integration,storage<br/>License terms bundle"]
      "component:declared_dependencies"["%% KIND: integration,boundary<br/>Declared dependency manifests (none present)"]
    end
  end
  "ext:git_remote" -->|"Hosts"| "sys:connectors_repo"
  "actor:integrator" -->|"Consumes"| "sys:connectors_repo"
  "actor:maintainer" -->|"Maintains"| "sys:connectors_repo"
  "sys:connectors_repo" --> "container:source_tree"
  "container:source_tree" -->|"Defines"| "component:vcs_ignore_policy"
  "container:source_tree" -->|"Includes"| "component:license_terms"
  "container:source_tree" -->|"Would host"| "component:declared_dependencies"
```

_Pass 4 complete — see `docs/c4-architecture.mermaid.md` for frozen projection._

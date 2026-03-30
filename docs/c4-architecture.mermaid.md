# C4 Architecture

## L1 — System context

```mermaid
flowchart LR
  dev(("actor:developer\nDeveloper"))
  gh["ext:github_hosting\nGitHub"]
  sys["sys:decreen_connectors\nDecreen connectors platform repository"]
  dev -->|"edge:dev_maintains_repo\nmaintains repository"| sys
  gh -->|"edge:github_hosts_repo\nhosts remote repository"| sys
  dev -->|"edge:dev_git_remote\ngit clone/fetch/push"| gh
```

## L2 — Container diagram

```mermaid
flowchart TB
  subgraph sys_boundary["sys:decreen_connectors — Decreen connectors platform repository"]
    ctr_src["container:tracked_sources\nTracked source files"]
    ctr_git["container:scm_integration\nGit / SCM integration"]
  end
```

## L3 — container:tracked_sources

```mermaid
flowchart TB
  subgraph scoped["%% SCOPE: urn:c4:container:tracked_sources"]
    c1["component:tracked_license_and_ignore\n%% KIND: storage"]
    c2["component:repository_tree_boundary\n%% KIND: boundary"]
  end
```

## L3 — container:scm_integration

```mermaid
flowchart TB
  subgraph scoped["%% SCOPE: urn:c4:container:scm_integration"]
    c3["component:git_origin_remote\n%% KIND: integration"]
    c4["component:git_hooks_path\n%% KIND: scheduler"]
    c4 -->|"edge:hooks_invoke_remote_ops"| c3
  end
  c3 -->|"edge:scm_views_tracked_tree"| c2["component:repository_tree_boundary\n%% KIND: boundary"]
```

## Containment Map

```json
{
  "parentToChildren": {
    "sys:decreen_connectors": ["container:tracked_sources", "container:scm_integration"],
    "container:tracked_sources": ["component:tracked_license_and_ignore", "component:repository_tree_boundary"],
    "container:scm_integration": ["component:git_origin_remote", "component:git_hooks_path"]
  },
  "childToParent": {
    "container:tracked_sources": "sys:decreen_connectors",
    "container:scm_integration": "sys:decreen_connectors",
    "component:tracked_license_and_ignore": "container:tracked_sources",
    "component:repository_tree_boundary": "container:tracked_sources",
    "component:git_origin_remote": "container:scm_integration",
    "component:git_hooks_path": "container:scm_integration"
  }
}
```

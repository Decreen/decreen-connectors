# Live C4 Preview

```mermaid
flowchart TB
  subgraph L1["L1 System Context"]
    A_DEV["actor:developer\nDeveloper"]
    EXT_GH["ext:github\nGitHub"]
    A_DEV -->|"edge:developer_github"| EXT_GH
  end
```

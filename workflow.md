---
config:
  theme: mc
---

```mermaid
flowchart TD
    A@{ label: "<i class=\"fa fa-play-circle\"></i> Start" } --> B@{ label: "<i class=\"fa fa-jenkins\"></i> Jenkins" }
    B <--> C@{ label: "<i class=\"fa fa-github\"></i> TodoApp Repository" }
    B --> D@{ label: "<i class=\"fa fa-server\"></i> Worker Node" }
    D --> E["<i class='fa fa-docker'></i> Docker Build"]
    E --> F["<i class='fa fa-react'></i> React Build<br>Multi-stage"]
    F --> G["<i class='fa fa-nginx'></i> Nginx Container<br>Port: 1080"]
    G --> H["<i class='fa fa-globe'></i> Todo Application"]
    
    A@{ shape: rect}
    B@{ shape: rect}
    C@{ shape: rect}
    D@{ shape: rect}
    E@{ shape: rect}
    F@{ shape: rect}
    G@{ shape: rect}
    H@{ shape: rect}

    A:::start
    B:::jenkins
    C:::github
    D:::worker
    E:::docker
    F:::build
    G:::nginx
    H:::prod

    classDef start fill:#FF6B6B:#FF8787,stroke:#FF5252,stroke-width:2px,rx:8,ry:8
    classDef jenkins fill:#4ECDC4:#45B7AF,stroke:#40A69F,stroke-width:2px,rx:8,ry:8
    classDef github fill:#6C5CE7:#5850EC,stroke:#4C40E0,stroke-width:2px,rx:8,ry:8
    classDef worker fill:#74B9FF:#63A4FF,stroke:#5899FF,stroke-width:2px,rx:8,ry:8
    classDef docker fill:#A8E6CF:#8CD3B6,stroke:#7EC2A5,stroke-width:2px,rx:8,ry:8
    classDef build fill:#FFD93D:#FFD023,stroke:#FFC60A,stroke-width:2px,rx:8,ry:8
    classDef nginx fill:#00CEC9:#00B5B1,stroke:#009E9A,stroke-width:2px,rx:8,ry:8
    classDef prod fill:#2ECC71:#27AE60,stroke:#219A52,stroke-width:2px,rx:8,ry:8
```

### Workflow Description:
1. Pipeline starts when triggered
2. Jenkins checks out code from TodoApp repository
3. Worker node receives deployment tasks
4. Docker builds multi-stage image:
   - Stage 1: React build with Node.js
   - Stage 2: Nginx with built files
5. Final container serves app on port 1080
</rewritten_file>

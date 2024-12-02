# Zerops x Elastic APM

[Elastic APM](https://www.elastic.co/what-is/application-performance-monitoring) is a performance monitoring tool that hooks into your app to track latency, errors, and transactions across your stack, giving you real-time insights for debugging and optimizing code.

## Deploy to Zerops

Paste the following yml to Zerops GUI.

```yaml
project:
  name: recipe-elastic-apm
  tags:
    - zerops-recipe
services:
  - hostname: elasticsearch
    type: elasticsearch@8.16
    mode: NON_HA
    priority: 10

  - hostname: kibana
    type: ubuntu@24.04
    buildFromGit: https://github.com/zeropsio/recipe-elastic-apm
    enableSubdomainAccess: true
    minContainers: 1
    maxContainers: 1

  - hostname: apmserver
    type: ubuntu@24.04
    buildFromGit: https://github.com/zeropsio/recipe-elastic-apm
    minContainers: 1
    maxContainers: 1
```

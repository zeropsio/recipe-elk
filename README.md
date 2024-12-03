# Zerops x Elastic Stack

## Elastic ELK

TODO: Description.

Paste the following yml to Zerops GUI:
```yaml
project:
  name: recipe-elastic-elk
  tags:
    - zerops-recipe
services:
  - hostname: elasticsearch
    type: elasticsearch@8.16
    mode: NON_HA
    priority: 10

  - hostname: kibana
    type: ubuntu@24.04
    buildFromGit: https://github.com/zeropsio/recipe-elastic-stack
    enableSubdomainAccess: true
    verticalAutoscaling:
      minRam: 1
    minContainers: 1
    maxContainers: 1

  - hostname: logstash
    type: ubuntu@24.04
    buildFromGit: https://github.com/zeropsio/recipe-elastic-stack
    verticalAutoscaling:
      minRam: 1
    minContainers: 1
    maxContainers: 1
```

```
destination destination_logstash_udp {
    tcp("logstash" port(1514));
};

log {
    source(s_sys); destination(d_logstash);
};
```

## Elastic APM

[Elastic APM](https://www.elastic.co/what-is/application-performance-monitoring) is a performance monitoring tool that hooks into your app to track latency, errors, and transactions across your stack, giving you real-time insights for debugging and optimizing code.

Paste the following yml to Zerops GUI:
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
    buildFromGit: https://github.com/zeropsio/recipe-elastic-stack
    enableSubdomainAccess: true
    verticalAutoscaling:
      minRam: 1
    minContainers: 1
    maxContainers: 1

  - hostname: apmserver
    type: ubuntu@24.04
    buildFromGit: https://github.com/zeropsio/recipe-elastic-stack
    minContainers: 1
    maxContainers: 1
```

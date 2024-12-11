# Zerops x Elastic Stack

Elastic is a distributed search and analytics engine at the core of the Elastic Stack, designed for storing, searching, and analyzing large volumes of structured and unstructured data in near real-time.

![elastic](https://github.com/zeropsio/recipe-shared-assets/blob/main/covers/svg/cover-elastic.svg)

<br/>

## Elastic ELK

[ELK (Elastic Stack)](https://www.elastic.co/elastic-stack/) is a powerful open-source suite for centralized logging, monitoring, and analytics, consisting of Elasticsearch for indexing and searching, Logstash for data processing, and Kibana for visualizing the data.

Paste the following yml to Zerops GUI:
```yaml
project:
  name: recipe-elastic-stack
services:
  - hostname: elasticsearch
    type: elasticsearch@8.16
    mode: NON_HA
    priority: 10

  - hostname: kibana
    type: ubuntu@24.04
    buildFromGit: https://github.com/zeropsio/recipe-elastic-stack
    enableSubdomainAccess: true
    envSecrets:
      ELASTICSEARCH_URL: http://elasticsearch:9200 # Change, if your Elasticsearch runs on different hostname.
      PUBLIC_BASE_URL: $zeropsSubdomain
    verticalAutoscaling:
      minRam: 1
    minContainers: 1
    maxContainers: 1

  - hostname: logstash
    type: ubuntu@24.04
    buildFromGit: https://github.com/zeropsio/recipe-elastic-stack
    envSecrets:
      ELASTICSEARCH_URL: http://elasticsearch:9200 # Change, if your Elasticsearch runs on different hostname.
    verticalAutoscaling:
      minRam: 1
    minContainers: 1
    maxContainers: 1

```

To collect all Zerops logs with Logstash, set the following custom log forwarding (through GUI):
```
destination d_logstash {
  udp("logstash" port(1514));
};

log {
  source(s_src); destination(d_logstash);
};
```

<br/>

## Elastic APM

[Elastic APM](https://www.elastic.co/what-is/application-performance-monitoring) is a performance monitoring tool that hooks into your app to track latency, errors, and transactions across your stack, giving you real-time insights for debugging and optimizing code.

Paste the following yml to Zerops GUI:
```yaml
project:
  name: recipe-elastic-stack
services:
  - hostname: elasticsearch
    type: elasticsearch@8.16
    mode: NON_HA
    priority: 10

  - hostname: kibana
    type: ubuntu@24.04
    buildFromGit: https://github.com/zeropsio/recipe-elastic-stack
    enableSubdomainAccess: true
    envSecrets:
      ELASTICSEARCH_URL: http://elasticsearch:9200 # Change, if your Elasticsearch runs on different hostname.
      PUBLIC_BASE_URL: $zeropsSubdomain
    verticalAutoscaling:
      minRam: 1
    minContainers: 1
    maxContainers: 1

  - hostname: apmserver
    type: ubuntu@24.04
    buildFromGit: https://github.com/zeropsio/recipe-elastic-stack
    envSecrets:
      ELASTICSEARCH_URL: http://elasticsearch:9200 # Change, if your Elasticsearch runs on different hostname.    
    minContainers: 1
    maxContainers: 1

```

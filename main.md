name: inverse
layout: true
class: center, middle, inverse

---
## Kubernetes Monitoring
## Introduction

---
layout: false
## whoami

.left-small[
    ![image](https://pbs.twimg.com/profile_images/994762110792953856/EheEvqBY_400x400.jpg)
]

.right-large[
- Kyohei Mizumoto(@kyohmizu)

- C# Software Engineer

- Interests
    - Docker/Kubernetes
    - Go
    - Security
]

---
### Required

- Kubernetesの概要程度の知識

### Targets

- Kubernetes Monitoring未経験者

---
### Contents

1. Monitoring Overview

1. Datadog

1. Prometheus

1. Demo

---
class: center, middle, inverse
# Monitoring Overview

---
### Monitoring

- Problem Detection  
ダッシュボード、アラート

- Problem Resolution  
根本原因の特定、トラブルシュート

- Continuous Improvement  
キャパシティ、コスト最適化

---
### Observability

A measure of how well internal states of a system can be inferred from knowledge of its external outputs

---
class: header-margin
### Landscape
<center><img src=landscape.png width=58%></center>

---
### Tools

- Datadog

- Prometheus

- Managed Service
    - Azure Monitor for containers

    - Stackdriver(for GKE)

---
### Tools

- Datadog

- Prometheus

.color-gray[
- Managed Service
    - Azure Monitor for containers

    - Stackdriver(for GKE)
]

---
class: center, middle, inverse
# Datadog

---
class: header-margin
### Datadog

.right-small[
<center><img src="https://datadog-prod.imgix.net/img/presskit/DDlogo.jpg" width=100%></center>
]

.left-large[
- SaaS型の監視＆解析ツール

- 課金制

- 様々なサービスのメトリクスをシームレスに集約
]

---
class: header-margin
### Dashboard

<center><img src="https://datadog-prod.imgix.net/img/blog/monitoring-kubernetes-with-datadog/kubernetes-dashboard-small.png" width=100%></center>

---
### Metrics

- ホストのCPU使用率、ディスク使用率

- ノード上のコンテナのCPU使用率

- Deployment のPod数、要求起動数

- Jobの成功数、失敗数

etc...

---
class: center, middle, inverse
# Prometheus

---
class: header-margin
### Prometheus

.right-small[
<center><img src="https://upload.wikimedia.org/wikipedia/en/thumb/3/38/Prometheus_software_logo.svg/1033px-Prometheus_software_logo.svg.png" width=90%></center>
]

.left-large[
- CNCFがホストするOSSの監視ツール

- 時系列でメトリクスを収集

- Reliability
]

---
class: header-margin
### Architecture

<center><img src="https://camo.githubusercontent.com/78b3b29d22cea8eee673e34fd204818ea532c171/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f70726f6d6574686575732f70726f6d65746865757340633334323537643036396336333036383564613335626365663038343633326666643564363230392f646f63756d656e746174696f6e2f696d616765732f6172636869746563747572652e737667" width=80%></center>

---
### Architecture

- Prometheus Server  
メトリクスの収集(Pull型)、保存

- Alert Manager  

- Exporter  
要求に応じてメトリクスを送信

- Push Gateway  
メトリクスをPushしておく

---
class: center, middle, blue
# Get Started

---
### Use Helm

Helmを使用してマニフェストを作成  
公式サイト：<u><https://helm.sh/></u>

```console
# Chartのダウンロード
$ helm fetch stable/prometheus --version 8.8.0

# Chartを元にマニフェストを作成
$ helm template --name sample-prometheus \
    prometheus-8.8.0.tgz \
    > sample-prometheus.yaml
```

---
### Values

LBとして外部に公開する場合は、valuesファイルを用意しておく

```yaml
alertmanager:
  service:
    type: LoadBalancer
pushgateway:
  service:
    type: LoadBalancer
server:
  service:
    type: LoadBalancer
```

---
### Deploy

サンプルをデプロイ

.zoom1[
<u><https://github.com/Kyohei-M/slide-k8s-monitoring/blob/master/sample-prometheus.yaml></u>
]

```console
# Prometheusを起動
$ kubectl apply -f sample-prometheus.yaml
```

---
class: center, middle, inverse
# Demo

---
class: header-margin
### Prometheus Server

<center><img src="prometheus-server1.png" width=100%></center>

---
class: header-margin
### Prometheus Server

<center><img src="prometheus-server2.png" width=100%></center>

---
### PromQL

Prometheus Query Language

```sql
# Example
# apiserverのhttpリクエスト合計
http_requests_total{job="apiserver"}

# 直近5分のhttpリクエストの増加率
rate(http_requests_total[5m])[30m:1m]

# 直近1時間での空きメモリ量の差
delta(node_memory_MemFree_bytes[1h])
```

---
### Grafana

.zoom2[
OSSのデータ可視化ツール
]

<center><img src="grafana.png" width=73%></center>

---
### Grafana

helmでインストール

```console
$ helm install stable/grafana

```

---
### Books

<center><img src="https://images-na.ssl-images-amazon.com/images/I/91VNghpL1EL.jpg" width=45%></center>

---
### Links

.zoom2[
Monitoring and Observability  
<u><https://thenewstack.io/monitoring-and-observability-whats-the-difference-and-why-does-it-matter/></u>

Datadog  
<u><https://www.datadoghq.com/></u>

Prometheus  
<u><https://prometheus.io/></u>

Grafana  
<u><https://grafana.com/></u>
]

---
class: center, middle, blue
# Thank you!

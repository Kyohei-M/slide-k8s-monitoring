name: inverse
layout: true
class: center, middle, inverse

---
## Kubernetes入門

Monitoring

---
layout: false
### Assumptions

- Kubernetesの概要程度の知識

### Targets

- Monitoring未経験者

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

> a measure of how well internal states of a system can be inferred from knowledge of its external outputs.

---
<center><img src=landscape.png width=80%></center>

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
### Architecture

<center><img src="https://camo.githubusercontent.com/78b3b29d22cea8eee673e34fd204818ea532c171/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f70726f6d6574686575732f70726f6d65746865757340633334323537643036396336333036383564613335626365663038343633326666643564363230392f646f63756d656e746174696f6e2f696d616765732f6172636869746563747572652e737667" width=85%></center>

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
### Install

Helmを使用してマニフェストを作成  
[公式サイト](https://helm.sh/)

```console
# Chartのダウンロード
$ helm fetch stable/prometheus --version 8.8.0

# Chartを元にマニフェストを作成
$ helm template --name sample-prometheus \
    prometheus-8.8.0.tgz \
    > sample-prometheus.yaml
```

---
### Install

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
### Install

[sample-prometheus.yaml](https://github.com/Kyohei-M/slide-k8s-monitoring/blob/master/sample-prometheus.yaml)

```console
# Prometheusを起動
$ kubectl apply -f sample-prometheus.yaml
```

---
class: center, middle, inverse
# Demo

---
### Prometheus Server

<center><img src="prometheus-server1.png" width=100%></center>

---
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

.zoom1[
OSSのデータ可視化ツール
]

<center><img src="grafana.png" width=80%></center>

---
### Grafana

helmでインストール

```console
$ helm install -n sample-grafana stable/grafana

```

---
### Books

<center><img src="https://images-na.ssl-images-amazon.com/images/I/91VNghpL1EL.jpg" width=45%></center>

---
### Website

[Monitoring and Observability](https://thenewstack.io/monitoring-and-observability-whats-the-difference-and-why-does-it-matter/)

[Datadog - 公式](https://www.datadoghq.com/)

[Prometheus - 公式](https://prometheus.io/)

[Grafana - 公式](https://grafana.com/)
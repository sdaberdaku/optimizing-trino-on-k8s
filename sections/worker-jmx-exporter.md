### Enabling JMX Exporter on Trino Workers
Trino exposes a large number of different metrics via Java Management Extensions:
* JVM metrics (heap size, thread count)
* Trino cluster and node statistics
* Trino query metrics (number of active, queued, failed, etc.)
* Trino task metrics (input data bytes and rows)
* Connector metrics

Initially, the JMX Exporter could be enabled only on the Coordinator.
* JMX Exporter support was also added to Workers.

-vertical

### Testing JMX Exporter
* Install Prometheus in `kind` K8s cluster.
* Install Trino with JMX Exporter feature enabled for both coordinator and workers.
* Verify that JMX metrics are collected by Prometheus.

-vertical

### Future work: 
#### Worker Autoscaling with KEDA

<div style="font-size: 30px;">

```yaml
apiVersion: keda.sh/v1alpha1
kind: ScaledObject
metadata:
  name: trino-worker
spec:
  scaleTargetRef:
    name: trino-worker
  minReplicaCount: 1
  maxReplicaCount: 5
...
  triggers:
    - type: prometheus
      metricType: Value
      metadata:
        serverAddress: "http://prometheus.example.com"
        threshold: "1"
        metricName: queued_queries
        query: sum by (job) (avg_over_time(trino_queued_queries{job="trino"}[30s]))
        authModes: "basic"
...
```
</div>
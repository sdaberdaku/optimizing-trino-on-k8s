### Future work: 
Worker Autoscaling with KEDA
* The JMX Export and Worker graceful shutdown can be used to implement advanced worker autoscaling.

<div style="font-size: 20px;">

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
### Future work: 
Worker Autoscaling with KEDA
* The JMX Export, fault-tolerant query execution, and worker graceful shutdown can be used to implement advanced worker autoscaling.

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

-notes

As future work, I plan to add the Worker KEDA support. Scaling based on CPU and memory utilization is not necessarily a good approach for Trino workers, since as you may know, Trino will always try to use all the resources available on the node it runs on, and thus these metrics might not indicate resource exhaustion. On the other hand, the number of queued queries could be a better indicator of the cluster's utilization. If the number of queued queries increases, then this could be a better indicator that the cluster could use more workers. 

In the provided example, I am showing a KEDA ScaledObject resource with a possible trigger based on the number of queued queries. This metric can be exposed using the JMX Export feature and be collected in Prometheus. KEDA can then run the provided query on Prometheus and make the decision whether to scale the workers up or down. KEDA can also be used to scale the workers all the way down to zero for further cost saving. 
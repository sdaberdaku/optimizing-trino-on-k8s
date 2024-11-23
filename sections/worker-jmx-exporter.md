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

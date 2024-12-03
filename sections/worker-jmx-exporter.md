### Enabling JMX Exporter on Trino Workers
Trino exposes a large number of different metrics via Java Management Extensions:
* JVM metrics (heap size, thread count)
* Trino cluster and node statistics
* Trino query metrics (number of active, queued, failed, etc.)
* Trino task metrics (input data bytes and rows)
* Connector metrics

Initially, the JMX Exporter could be enabled only on the Coordinator.
* JMX Exporter support was also added to Workers.

-notes

Enabling the JMX Exporter on Trino Worker nodes is another feature that was introduced to the Helm Chart. JMX can be used to collect a large variety of metrics, including the ones shown in the current slide.

Some of these metrics are worth monitoring also on the worker nodes. The feature, when enabled, will enable the JMX export configuration, add the JMX exporter sidecar container to the Coordinator/Worker nodes, and enable Prometheus scraping by creating a Service Monitor CRD. For scraping the metrics from the Worker nodes, a headless service is also created, which serves as a target to the corresponding Service Monitor.

-vertical

### Testing JMX Exporter
* Install Prometheus in `kind` K8s cluster.
* Install Trino with JMX Exporter feature enabled for both coordinator and workers.
* Verify that JMX metrics are collected by Prometheus.

-notes

To test this feature, the existing JMX tests were expanded to account for the metrics of the worker nodes.

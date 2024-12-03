### Rendering default TPC-H and TPC-DS catalogs optional

* Initially, the TPC-H and TPC-DS catalogs were always created by the Helm Chart.
* These catalogs are useful for benchmarking a given Trino deployment configuration. After configuring the Trino cluster, users might want to drop these catalogs.
* Mounting static catalogs from a `configmap` conflicts with the Dynamic Catalogs feature which allows users to create catalogs at runtime with SQL statements which must be persisted on a mounted volume.
* Now, these catalogs can be easily disabled by Helm Chart users if they wish to do so.

-notes

The first contribution to the Helm Chart I am going to present is rendering the default TPC-H and TPC-DS catalogs optional. By default, the Helm Chart will create these catalogs providing a set of schemas to support the corresponding database benchmarks with the same name. These connectors can be used to test the capabilities and query syntax of Trino without configuring access to an external data source. 

Although useful, users might still want to disable these catalogs after they are done experimenting and fine-tuning their cluster.

Moreover, having default catalogs automatically mounted from a configmap conflicts with the Dynamic Catalog creation feature which allows users to create catalogs at runtime without having to restart the Trino Cluster.

-vertical

### Disabling TPC-H and TPC-DS catalogs

```yaml
# values.yaml
catalogs:
  tpch: |
    connector.name=tpch
    tpch.splits-per-node=4
  tpcds: |
    connector.name=tpcds
    ptcds.splits-per-node=4
```

-notes

With this new development, these catalogs are still enabled by default for backwards compatibility, but can also be disabled if the user wishes to do so, as shown in the next slide.

-vertical

### Disabling TPC-H and TPC-DS catalogs

```yaml
# values.yaml
catalogs:
  tpch: null
  tpcds: null
```

### Rendering default TPC-H and TPC-DS catalogs optional

* Initially, the TPC-H and TPC-DS catalogs were always created by the Helm Chart.
* These catalogs are useful for benchmarking a given Trino deployment configuration. After configuring the Trino cluster, users might want to drop these catalogs.
* Mounting static catalogs from a configmap conflicts with the Dynamic Catalogs feature which allows users to create catalogs at runtime with SQL statements which must be persisted on a mounted volume.
* Now, these catalogs can be easily disabled by Helm Chart users if they wish to do so.

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

-vertical

### Disabling TPC-H and TPC-DS catalogs

```yaml
# values.yaml
catalogs:
  tpch: null
  tpcds: null
```

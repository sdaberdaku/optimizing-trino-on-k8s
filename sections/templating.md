### Templating support for `additionalConfigFiles`

* The `additionalConfigFiles` properties on the coordinator and worker configurations allow users to add additional config files in the corresponding default configuration directories. 
* These configurations are now templated:

<div style="font-size: 30px;">

```yaml
# configmap-coordinator.yaml
apiVersion: v1
kind: ConfigMap
...
data:
...
{{- range $fileName, $fileContent := .Values.coordinator.additionalConfigFiles }}
  {{ $fileName }}: |
    {{- tpl $fileContent $ | nindent 4 }}
{{- end }}
```

</div> 

-vertical

### File group provider example
* The group file must contain a list of groups and members, one per line, separated by a colon. Users are separated by a comma.

```yaml
# values.yaml
auth:
  refreshPeriod: 60s
  groups: |-
    admin:admin.example.com
    group1:user1.example.com,user2.example.com
```

-vertical

### File group provider example

<div style="font-size: 30px;">


```yaml
# values.yaml
coordinator:
 additionalConfigFiles:
   group-provider.properties: |-
     group-provider.name=file
     file.group-file={{- .Values.server.config.path -}}/group-provider.db
     file.refresh-period={{- .Values.groupProvider.refreshPeriod }}
   group-provider.db: |-
     {{- range $k, $v := .Values.groupProvider.groups }}
     {{- printf "%s:%s\n" $k (join "," $v) }}
     {{- end }}
...
groupProvider:
  refreshPeriod: 60s
  groups: 
    admin:
      - admin.example.com
    group1:
      - user1.example.com
      - user2.example.com
```

</div> 


-vertical

### Resulting group-provider.db

```yaml
# group-provider.db
admin:admin@example.com
group1:user1@example.com,user2@example.com
```
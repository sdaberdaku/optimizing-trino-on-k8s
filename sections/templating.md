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

-notes

Templating support for `additionalConfigFiles` is another interesting functionality that was introduced. This configuration allows users to add custom configuration files to the Trino configuration directory, both on coordinator and workers alike.

Now these configuration files can be templated (as can be seen in the second-last row of this slide).

-vertical

### File group provider example
* The group file must contain a list of groups and members, one per line, separated by a colon. Users are separated by a comma.

```yaml
# values.yaml
auth:
  refreshPeriod: 60s
  groups: |-
    admin:admin@example.com
    group1:user1@example.com,user2@example.com
```

-notes

A notable example where this feature can be useful is managing the file group provider configuration. Trino requires the user to provide the user group assignment with a list of groups in a file, one per line, separated by a colon. Users are separated by a comma.

The problem with this configuration is that it becomes difficult to manage when handling a large number of users and groups.

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
      - admin@example.com
    group1:
      - user1@example.com
      - user2@example.com
```

</div> 

-notes

It would be much more convenient if the users and groups could be handled as YAML objects (i.e. lists). In this example, we can manage groups as actual lists of users, with the possibility now to use the editor capability to collapse the user lists and keep them ordered alphabetically.

-vertical

### Resulting group-provider.db

```yaml
# group-provider.db
admin:admin@example.com
group1:user1@example.com,user2@example.com
```
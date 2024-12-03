### `NetworkPolicy` protection

* Trino supports multiple authentication types to ensure all users of the system are authenticated. Different authenticators allow user management in one or more systems.
* All authentication requires secure connections using TLS and HTTPS or process forwarding enabled.
* To configure Trino with TLS there are two alternatives:
    1. **Use a TLS-terminated Load Balancer or proxy (preferred)**;
    2. Secure Trino directly with valid a TLS certificate (must manage certificates, increased CPU usage).

-notes

Trino supports multiple authentication mechanisms, all of which require secure connections using TLS and HTTPS or process forwarding enabled. 

To configure Trino with TLS there are two alternatives:
  1. Use a TLS-terminated Load Balancer or proxy, which is also the preferred way;
  2. Secure Trino directly with valid a TLS certificate, which requires users to manage certificates (if they are self-signed they must be installed in every component etc.), and ultimately this will result in an increased CPU utilization for the encoding/decoding activity (CPU cycles that could go towards query processing).


-vertical

### `NetworkPolicy` protection
* To prevent unauthorized connections to Trino from within the Kubernetes cluster, a `NetworkPolicy` can be used.
* On EKS requires VPC CNI Plugin.
* Can be used to only allow ingress traffic to Trino from Pods with certain labels or from given CIDR blocks.

-notes

In case the user decides to go with the TLS-terminated Load Balancer or proxy solution, the Trino cluster would still be unprotected from unauthorized access from within the Kubernetes cluster. 

Other Pods could try to connect to the Trino Service or to the Trino Pods directly using the internal Kubernetes DNS names, and they would not be required to provide a password, being able to impersonate any user they wish.

Just to make an example, if you have Apache Superset deployed on the same Kubernetes cluster as Trino, a user could create a connection to Trino by guessing the DNS name of the service `trino.trino.svc.cluster.local:8080`.

NetworkPolicies can be very helpful in these situations, since they can provide protection for unauthorized access from within the Kubernetes cluster itself. Specifically to our use case, they can be used to only allow ingress traffic to Trino Pods from Pods with certain labels or from given CIDR blocks. 

-vertical

##### TLS-terminated ALB with `NetworkPolicy` protection

![](assets/trino-network-policy.drawio.png)  <!-- .element width="70%" style="float: none; background-color: white; border: 5px solid white;" title="Trino NetworkPolicy" -->

-vertical

### Testing `NetworkPolicy` protection
Connections from unauthorized Pods will time-out.

<div style="font-size: 25px;">

```yaml
# test-networkpolicy.yaml
apiVersion: v1
kind: Pod
...
  containers:
    - name: check-connection
      image: {{ include "trino.image" . }}
      command: [ "/bin/bash", "-c" ]
      args:
        - >-
          curl
          {{ include "trino.fullname" . }}.{{ .Release.Namespace }}:{{ .Values.service.port }}
          --head
          --fail
          --connect-timeout 10
          --max-time 10
          2>&1 | grep -q "timed out"
...
```
</div>
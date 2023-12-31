# crd-to-cr
This tool reads a Kubernetes Custom Resource Definition (CRD) and outputs a commented, full example Custom Resource (CR).

# Usage
To generate a full CR of the [Tempo Operator CRD](https://raw.githubusercontent.com/grafana/tempo-operator/5a79e619f268dac0fefd6cc394555582b17de520/bundle/community/manifests/tempo.grafana.com_tempostacks.yaml):
```bash
./crd-to-cr.py < tempo.grafana.com_tempostacks.yaml > tempostack.yaml
```

# Example Output
```yaml
apiVersion: tempo.grafana.com/v1alpha1   # APIVersion defines the versioned schema of this representation of an object. Servers should convert recognized schemas to the latest internal value, and may reject unrecognized values. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources
kind: TempoStack                         # Kind is a string value representing the REST resource this object represents. Servers may infer this from the endpoint the client submits requests to. Cannot be updated. In CamelCase. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds
metadata:
  name: example
spec:                                    # TempoStackSpec defines the desired state of TempoStack.
  hashRing:                              # HashRing defines the spec for the distributed hash ring configuration.
    memberlist:                          # MemberList configuration spec
      enableIPv6: false                  # EnableIPv6 enables IPv6 support for the memberlist based hash ring.
  images:                                # Images defines the image for each container.
    tempo: ""                            # Tempo defines the tempo container image.
    tempoGateway: ""                     # TempoGateway defines the tempo-gateway container image.
    tempoGatewayOpa: ""                  # TempoGatewayOpa defines the OPA sidecar container for TempoGateway.
    tempoQuery: ""                       # TempoQuery defines the tempo-query container image.
  limits:                                # LimitSpec is used to limit ingestion and querying rates.
    global:                              # Global is used to define global rate limits.
      ingestion:                         # Ingestion is used to define ingestion rate limits.
        ingestionBurstSizeBytes: 0       # IngestionBurstSizeBytes defines the burst size (bytes) used in ingestion.
        ingestionRateLimitBytes: 0       # IngestionRateLimitBytes defines the Per-user ingestion rate limit (bytes) used in ingestion.
        maxBytesPerTrace: 0              # MaxBytesPerTrace defines the maximum number of bytes of an acceptable trace.
        maxTracesPerUser: 0              # MaxTracesPerUser defines the maximum number of traces a user can send.
      query:                             # Query is used to define query rate limits.
        maxBytesPerTagValues: 0          # MaxBytesPerTagValues defines the maximum size in bytes of a tag-values query.
        maxSearchBytesPerTrace: 0        # DEPRECATED. MaxSearchBytesPerTrace defines the maximum size of search data for a single trace in bytes. default: `0` to disable.
        maxSearchDuration: ""            # MaxSearchDuration defines the maximum allowed time range for a search. If this value is not set, then spec.search.maxDuration is used.
    perTenant:                           # PerTenant is used to define rate limits per tenant.
      "key":
        ingestion:                       # Ingestion is used to define ingestion rate limits.
          ingestionBurstSizeBytes: 0     # IngestionBurstSizeBytes defines the burst size (bytes) used in ingestion.
          ingestionRateLimitBytes: 0     # IngestionRateLimitBytes defines the Per-user ingestion rate limit (bytes) used in ingestion.
          maxBytesPerTrace: 0            # MaxBytesPerTrace defines the maximum number of bytes of an acceptable trace.
          maxTracesPerUser: 0            # MaxTracesPerUser defines the maximum number of traces a user can send.
        query:                           # Query is used to define query rate limits.
          maxBytesPerTagValues: 0        # MaxBytesPerTagValues defines the maximum size in bytes of a tag-values query.
          maxSearchBytesPerTrace: 0      # DEPRECATED. MaxSearchBytesPerTrace defines the maximum size of search data for a single trace in bytes. default: `0` to disable.
          maxSearchDuration: ""          # MaxSearchDuration defines the maximum allowed time range for a search. If this value is not set, then spec.search.maxDuration is used.
  managementState: ""                    # ManagementState defines if the CR should be managed by the operator or not. Default is managed.
  observability:                         # ObservabilitySpec defines how telemetry data gets handled.
    grafana:                             # Grafana defines the Grafana configuration for operands.
      createDatasource: false            # CreateDatasource specifies if a Grafana Datasource should be created for Tempo.
      instanceSelector:                  # InstanceSelector specifies the Grafana instance where the datasource should be created.
        matchExpressions:                # matchExpressions is a list of label selector requirements. The requirements are ANDed.
        - key: ""                        # key is the label key that the selector applies to.
          operator: ""                   # operator represents a key's relationship to a set of values. Valid operators are In, NotIn, Exists and DoesNotExist.
          values:                        # values is an array of string values. If the operator is In or NotIn, the values array must be non-empty. If the operator is Exists or DoesNotExist, the values array must be empty. This array is replaced during a strategic merge patch.
          - ""
        matchLabels:                     # matchLabels is a map of {key,value} pairs. A single {key,value} in the matchLabels map is equivalent to an element of matchExpressions, whose key field is "key", the operator is "In", and the values array contains only "value". The requirements are ANDed.
          "key": ""
    metrics:                             # Metrics defines the metrics configuration for operands.
      createPrometheusRules: false       # CreatePrometheusRules specifies if Prometheus rules for alerts should be created for Tempo components.
      createServiceMonitors: false       # CreateServiceMonitors specifies if ServiceMonitors should be created for Tempo components.
    tracing:                             # Tracing defines a config for operands.
      jaeger_agent_endpoint: ""          # JaegerAgentEndpoint defines the jaeger endpoint data gets send to.
      sampling_fraction: ""              # SamplingFraction defines the sampling ratio. Valid values are 0 to 1.
  replicationFactor: 0                   # NOTE: currently this field is not considered. ReplicationFactor is used to define how many component replicas should exist.
  resources:                             # Resources defines resources configuration.
    total:                               # The total amount of resources for Tempo instance. The operator autonomously splits resources between deployed Tempo components. Only limits are supported, the operator calculates requests automatically. See http://github.com/grafana/tempo/issues/1540.
      claims:                            # Claims lists the names of resources, defined in spec.resourceClaims, that are used by this container.   This is an alpha field and requires enabling the DynamicResourceAllocation feature gate.   This field is immutable. It can only be set for containers.
      - name: ""                         # Name must match the name of one entry in pod.spec.resourceClaims of the Pod where this field is used. It makes that resource available inside a container.
      limits:                            # Limits describes the maximum amount of compute resources allowed. More info: https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
        "key": 1Gi
      requests:                          # Requests describes the minimum amount of compute resources required. If Requests is omitted for a container, it defaults to Limits if that is explicitly specified, otherwise to an implementation-defined value. Requests cannot exceed Limits. More info: https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
        "key": 1Gi
  retention:                             # NOTE: currently this field is not considered. Retention period defined by dataset. User can specify how long data should be stored.
    global:                              # Global is used to configure global retention.
      traces: ""                         # Traces defines retention period. Supported parameter suffixes are "s", "m" and "h". example: 336h default: value is 48h.
    perTenant:                           # PerTenant is used to configure retention per tenant.
      "key":
        traces: ""                       # Traces defines retention period. Supported parameter suffixes are "s", "m" and "h". example: 336h default: value is 48h.
  search:                                # SearchSpec control the configuration for the search capabilities.
    defaultResultLimit: 0                # Limit used for search requests if none is set by the caller (default: 20)
    maxDuration: ""                      # The maximum allowed time range for a search, default: 0s which means unlimited.
    maxResultLimit: 0                    # The maximum allowed value of the limit parameter on search requests. If the search request limit parameter exceeds the value configured here it will be set to the value configured here. The default value of 0 disables this limit.
  serviceAccount: ""                     # ServiceAccount defines the service account to use for all tempo components.
  storage:                               # Storage defines the spec for the object storage endpoint to store traces. User is required to create secret and supply it.
    secret:                              # Secret for object storage authentication. Name of a secret in the same namespace as the TempoStack custom resource.
      name: ""                           # Name of a secret in the namespace configured for object storage secrets.
      type: ""                           # Type of object storage that should be used
    tls:                                 # TLS configuration for reaching the object storage endpoint.
      caName: ""                         # CA is the name of a ConfigMap containing a `ca.crt` key with a CA certificate. It needs to be in the same namespace as the TempoStack custom resource.
  storageClassName: ""                   # StorageClassName for PVCs used by ingester. Defaults to nil (default storage class in the cluster).
  storageSize: 1Gi                       # StorageSize for PVCs used by ingester. Defaults to 10Gi.
  template:                              # Template defines requirements for a set of tempo components.
    compactor:                           # Compactor defines the tempo compactor component spec.
      nodeSelector:                      # NodeSelector is the simplest recommended form of node selection constraint.
        "key": ""
      replicas: 0                        # Replicas represents the number of replicas to create for this component.
      tolerations:                       # Tolerations defines component specific pod tolerations.
      - effect: ""                       # Effect indicates the taint effect to match. Empty means match all taint effects. When specified, allowed values are NoSchedule, PreferNoSchedule and NoExecute.
        key: ""                          # Key is the taint key that the toleration applies to. Empty means match all taint keys. If the key is empty, operator must be Exists; this combination means to match all values and all keys.
        operator: ""                     # Operator represents a key's relationship to the value. Valid operators are Exists and Equal. Defaults to Equal. Exists is equivalent to wildcard for value, so that a pod can tolerate all taints of a particular category.
        tolerationSeconds: 0             # TolerationSeconds represents the period of time the toleration (which must be of effect NoExecute, otherwise this field is ignored) tolerates the taint. By default, it is not set, which means tolerate the taint forever (do not evict). Zero and negative values will be treated as 0 (evict immediately) by the system.
        value: ""                        # Value is the taint value the toleration matches to. If the operator is Exists, the value should be empty, otherwise just a regular string.
    distributor:                         # Distributor defines the distributor component spec.
      component:                         # TempoComponentSpec is embedded to extend this definition with further options.   Currently, there is no way to inline this field. See: https://github.com/golang/go/issues/6213
        nodeSelector:                    # NodeSelector is the simplest recommended form of node selection constraint.
          "key": ""
        replicas: 0                      # Replicas represents the number of replicas to create for this component.
        tolerations:                     # Tolerations defines component specific pod tolerations.
        - effect: ""                     # Effect indicates the taint effect to match. Empty means match all taint effects. When specified, allowed values are NoSchedule, PreferNoSchedule and NoExecute.
          key: ""                        # Key is the taint key that the toleration applies to. Empty means match all taint keys. If the key is empty, operator must be Exists; this combination means to match all values and all keys.
          operator: ""                   # Operator represents a key's relationship to the value. Valid operators are Exists and Equal. Defaults to Equal. Exists is equivalent to wildcard for value, so that a pod can tolerate all taints of a particular category.
          tolerationSeconds: 0           # TolerationSeconds represents the period of time the toleration (which must be of effect NoExecute, otherwise this field is ignored) tolerates the taint. By default, it is not set, which means tolerate the taint forever (do not evict). Zero and negative values will be treated as 0 (evict immediately) by the system.
          value: ""                      # Value is the taint value the toleration matches to. If the operator is Exists, the value should be empty, otherwise just a regular string.
      tls:                               # TLS defines TLS configuration for distributor receivers
        caName: ""                       # caName is the name of a ConfigMap containing a CA certificate. It needs to be in the same namespace as the Tempo custom resource.
        certName: ""                     # certName is the name of a Secret containing a certificate and the private key It needs to be in the same namespace as the Tempo custom resource.
        enabled: false
        minVersion: ""                   # minVersion is the name of a Secret containing a certificate and the private key It needs to be in the same namespace as the Tempo custom resource.
    gateway:                             # Gateway defines the tempo gateway spec.
      component:                         # TempoComponentSpec is embedded to extend this definition with further options.   Currently there is no way to inline this field. See: https://github.com/golang/go/issues/6213
        nodeSelector:                    # NodeSelector is the simplest recommended form of node selection constraint.
          "key": ""
        replicas: 0                      # Replicas represents the number of replicas to create for this component.
        tolerations:                     # Tolerations defines component specific pod tolerations.
        - effect: ""                     # Effect indicates the taint effect to match. Empty means match all taint effects. When specified, allowed values are NoSchedule, PreferNoSchedule and NoExecute.
          key: ""                        # Key is the taint key that the toleration applies to. Empty means match all taint keys. If the key is empty, operator must be Exists; this combination means to match all values and all keys.
          operator: ""                   # Operator represents a key's relationship to the value. Valid operators are Exists and Equal. Defaults to Equal. Exists is equivalent to wildcard for value, so that a pod can tolerate all taints of a particular category.
          tolerationSeconds: 0           # TolerationSeconds represents the period of time the toleration (which must be of effect NoExecute, otherwise this field is ignored) tolerates the taint. By default, it is not set, which means tolerate the taint forever (do not evict). Zero and negative values will be treated as 0 (evict immediately) by the system.
          value: ""                      # Value is the taint value the toleration matches to. If the operator is Exists, the value should be empty, otherwise just a regular string.
      enabled: false
      ingress:                           # Ingress defines gateway Ingress options.
        annotations:                     # Annotations defines the annotations of the Ingress object.
          "key": ""
        host: ""                         # Host defines the hostname of the Ingress object.
        ingressClassName: ""             # IngressClassName is the name of an IngressClass cluster resource. Ingress controller implementations use this field to know whether they should be serving this Ingress resource.
        route:                           # Route defines OpenShift Route specific options.
          termination: ""                # Termination specifies the termination type. By default "edge" is used.
        type: ""                         # Type defines the type of Ingress for the Jaeger Query UI. Currently ingress, route and none are supported.
    ingester:                            # Ingester defines the ingester component spec.
      nodeSelector:                      # NodeSelector is the simplest recommended form of node selection constraint.
        "key": ""
      replicas: 0                        # Replicas represents the number of replicas to create for this component.
      tolerations:                       # Tolerations defines component specific pod tolerations.
      - effect: ""                       # Effect indicates the taint effect to match. Empty means match all taint effects. When specified, allowed values are NoSchedule, PreferNoSchedule and NoExecute.
        key: ""                          # Key is the taint key that the toleration applies to. Empty means match all taint keys. If the key is empty, operator must be Exists; this combination means to match all values and all keys.
        operator: ""                     # Operator represents a key's relationship to the value. Valid operators are Exists and Equal. Defaults to Equal. Exists is equivalent to wildcard for value, so that a pod can tolerate all taints of a particular category.
        tolerationSeconds: 0             # TolerationSeconds represents the period of time the toleration (which must be of effect NoExecute, otherwise this field is ignored) tolerates the taint. By default, it is not set, which means tolerate the taint forever (do not evict). Zero and negative values will be treated as 0 (evict immediately) by the system.
        value: ""                        # Value is the taint value the toleration matches to. If the operator is Exists, the value should be empty, otherwise just a regular string.
    querier:                             # Querier defines the querier component spec.
      nodeSelector:                      # NodeSelector is the simplest recommended form of node selection constraint.
        "key": ""
      replicas: 0                        # Replicas represents the number of replicas to create for this component.
      tolerations:                       # Tolerations defines component specific pod tolerations.
      - effect: ""                       # Effect indicates the taint effect to match. Empty means match all taint effects. When specified, allowed values are NoSchedule, PreferNoSchedule and NoExecute.
        key: ""                          # Key is the taint key that the toleration applies to. Empty means match all taint keys. If the key is empty, operator must be Exists; this combination means to match all values and all keys.
        operator: ""                     # Operator represents a key's relationship to the value. Valid operators are Exists and Equal. Defaults to Equal. Exists is equivalent to wildcard for value, so that a pod can tolerate all taints of a particular category.
        tolerationSeconds: 0             # TolerationSeconds represents the period of time the toleration (which must be of effect NoExecute, otherwise this field is ignored) tolerates the taint. By default, it is not set, which means tolerate the taint forever (do not evict). Zero and negative values will be treated as 0 (evict immediately) by the system.
        value: ""                        # Value is the taint value the toleration matches to. If the operator is Exists, the value should be empty, otherwise just a regular string.
    queryFrontend:                       # TempoQueryFrontendSpec defines the query frontend spec.
      component:                         # TempoComponentSpec is embedded to extend this definition with further options.   Currently there is no way to inline this field. See: https://github.com/golang/go/issues/6213
        nodeSelector:                    # NodeSelector is the simplest recommended form of node selection constraint.
          "key": ""
        replicas: 0                      # Replicas represents the number of replicas to create for this component.
        tolerations:                     # Tolerations defines component specific pod tolerations.
        - effect: ""                     # Effect indicates the taint effect to match. Empty means match all taint effects. When specified, allowed values are NoSchedule, PreferNoSchedule and NoExecute.
          key: ""                        # Key is the taint key that the toleration applies to. Empty means match all taint keys. If the key is empty, operator must be Exists; this combination means to match all values and all keys.
          operator: ""                   # Operator represents a key's relationship to the value. Valid operators are Exists and Equal. Defaults to Equal. Exists is equivalent to wildcard for value, so that a pod can tolerate all taints of a particular category.
          tolerationSeconds: 0           # TolerationSeconds represents the period of time the toleration (which must be of effect NoExecute, otherwise this field is ignored) tolerates the taint. By default, it is not set, which means tolerate the taint forever (do not evict). Zero and negative values will be treated as 0 (evict immediately) by the system.
          value: ""                      # Value is the taint value the toleration matches to. If the operator is Exists, the value should be empty, otherwise just a regular string.
      jaegerQuery:                       # JaegerQuerySpec defines Jaeger Query specific options.
        enabled: false                   # Enabled is used to define if Jaeger Query component should be created.
        ingress:                         # Ingress defines Jaeger Query Ingress options.
          annotations:                   # Annotations defines the annotations of the Ingress object.
            "key": ""
          host: ""                       # Host defines the hostname of the Ingress object.
          ingressClassName: ""           # IngressClassName is the name of an IngressClass cluster resource. Ingress controller implementations use this field to know whether they should be serving this Ingress resource.
          route:                         # Route defines OpenShift Route specific options.
            termination: ""              # Termination specifies the termination type. By default "edge" is used.
          type: ""                       # Type defines the type of Ingress for the Jaeger Query UI. Currently ingress, route and none are supported.
        monitorTab:                      # MonitorTab defines monitor tab configuration.
          enabled: false                 # Enabled enables monitoring tab in Jaeger console. PrometheusEndpoint needs to be set to enable the feature.
          prometheusEndpoint: ""         # PrometheusEndpoint configures endpoint to the Prometheus that contains span RED metrics. For instance on OpenShift this is set to https://thanos-querier.openshift-monitoring.svc.cluster.local:9091
  tenants:                               # Tenants defines the per-tenant authentication and authorization spec.
    authentication:                      # Authentication defines the tempo-gateway component authentication configuration spec per tenant.
    - oidc:                              # OIDC defines the spec for the OIDC tenant's authentication.
        groupClaim: ""                   # Group claim field from ID Token
        issuerURL: ""                    # IssuerURL defines the URL for issuer.
        redirectURL: ""                  # RedirectURL defines the URL for redirect.
        secret:                          # Secret defines the spec for the clientID, clientSecret and issuerCAPath for tenant's authentication.
          name: ""                       # Name of a secret in the namespace configured for tenant secrets.
        usernameClaim: ""                # User claim field from ID Token
      tenantId: ""                       # TenantID defines the id of the tenant.
      tenantName: ""                     # TenantName defines the name of the tenant. The value of this field should be sent in X-Scope-OrgID header to identify the tenant.
    authorization:                       # Authorization defines the tempo-gateway component authorization configuration spec per tenant.
      roleBindings:                      # RoleBindings defines configuration to bind a set of roles to a set of subjects.
      - name: ""
        roles:
        - ""
        subjects:
        - kind: ""                       # SubjectKind is a kind of Tempo Gateway RBAC subject.
          name: ""
      roles:                             # Roles defines a set of permissions to interact with a tenant.
      - name: ""
        permissions:
        - ""                             # PermissionType is a Tempo Gateway RBAC permission.
        resources:
        - ""
        tenants:
        - ""
    mode: ""                             # Mode defines the multitenancy mode.
status:                                  # TempoStackStatus defines the observed state of TempoStack.
  components:                            # Components provides summary of all Tempo pod status grouped per component.
    compactor:                           # Compactor is a map to the pod status of the compactor pod.
      "key":
      - ""
    distributor:                         # Distributor is a map to the per pod status of the distributor deployment
      "key":
      - ""
    gateway:                             # Gateway is a map to the per pod status of the query frontend deployment
      "key":
      - ""
    ingester:                            # Ingester is a map to the per pod status of the ingester statefulset
      "key":
      - ""
    querier:                             # Querier is a map to the per pod status of the querier deployment
      "key":
      - ""
    queryFrontend:                       # QueryFrontend is a map to the per pod status of the query frontend deployment
      "key":
      - ""
  conditions:                            # Conditions of the Tempo deployment health.
  - lastTransitionTime: ""               # lastTransitionTime is the last time the condition transitioned from one status to another. This should be when the underlying condition changed.  If that is not known, then using the time when the API field changed is acceptable.
    message: ""                          # message is a human readable message indicating details about the transition. This may be an empty string.
    observedGeneration: 0                # observedGeneration represents the .metadata.generation that the condition was set based upon. For instance, if .metadata.generation is currently 12, but the .status.conditions[x].observedGeneration is 9, the condition is out of date with respect to the current state of the instance.
    reason: ""                           # reason contains a programmatic identifier indicating the reason for the condition's last transition. Producers of specific condition types may define expected values and meanings for this field, and whether the values are considered a guaranteed API. The value should be a CamelCase string. This field may not be empty.
    status: ""                           # status of the condition, one of True, False, Unknown.
    type: ""                             # type of condition in CamelCase or in foo.example.com/CamelCase. --- Many .condition.type values are consistent across resources like Available, but because arbitrary conditions can be useful (see .node.status.conditions), the ability to deconflict is important. The regex it matches is (dns1123SubdomainFmt/)?(qualifiedNameFmt)
  operatorVersion: ""                    # Version of the Tempo Operator.
  tempoQueryVersion: ""                  # DEPRECATED. Version of the Tempo Query component used.
  tempoVersion: ""                       # Version of the managed Tempo instance.
```

# Configure kernel parameters

The Sysctl interface allows to modify kernel parameters at runtime and in the pod can be specified under `securityContext.sysctls`. If kernel parameters in the pod are to be modified, should be handled cautiously, and policy with rules restricting these options will be helpful. We can control minimum and maximum port that a network connection can use as its source(local) port by checking net.ipv4.ip_local_port_range

## Policy YAML

[policy_validate_sysctl_configs.yaml](more/policy_validate_sysctl_configs.yaml)

````yaml
apiVersion: kyverno.io/v1alpha1
kind: ClusterPolicy
metadata:
  name: validate-allow-portrange-with-sysctl
spec:
  rules:
  - name: allow-portrange-with-sysctl
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: "Allowed port range is from 1024 to 65535"
      pattern:
        spec:
          securityContext:
            sysctls: 
            - name: net.ipv4.ip_local_port_range
              value: "1024 65535"
````


## Additional Information

* [List of supported namespaced sysctl interfaces](https://kubernetes.io/docs/tasks/administer-cluster/sysctl-cluster/) 
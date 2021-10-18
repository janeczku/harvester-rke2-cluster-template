# Example RKE2 cluster template for Harvester

This project contains an example RKE2 cluster template helm chart, which can be applied with values.yaml to create clusters on Harvester infrastructure.

### How to use

See the [upstream documentation](https://rancher.com/docs/rancher/v2.6/en/admin-settings/cluster-templates/#rke2-cluster-template) on importing and using cluster templates in Rancher.

This template contains several parameters that allow you to customize the cluster provisioning. These are are available through [values.yaml](./charts/values.yaml).

```yaml
# specify cloud credential secret name
cloudCredentialSecretName: ""

# specify Rancher agent environment variables
# agentEnvVars:
#   - name: "HTTP_PROXY"
#     value: "https://proxy.com"

# cluster specific values
cluster:
  # specify cluster name
  name: example-cluster

  # specify cluster labels
  labels: {}

  # specify cluster annotations
  annotations: {}

  # kubernetesVersion: ""
  defaultPodSecurityPolicyTemplateName: unrestricted
  
  # enable local auth endpoint
  localClusterAuthEndpoint: 
    enabled: false
  # fqdn: foo.bar.example
  # caCerts: ""

# specify user principal ids to be assiged as cluster members
# clusterMembers: 
# - principalName: "local://u-z8zl5"
#   roleTemplateName: "cluster-member"

# Cloud Provider Config contains the Kubeconfig configuration
# to use for the Harvester Cloud Provider
# It can be obtained like so:
# curl 'https://<RANCHER-SERVER>/k8s/clusters/c-m-hswtcmdj/v1/harvester/kubeconfig' \
#   -H 'Content-Type: application/json;charset=UTF-8' \
#   -H 'Accept: application/json' \
#   --data-raw '{"clusterRoleName":"harvesterhci.io:cloudprovider","namespace":"<VM NAMESPACE>","serviceAccountName":"<CLUSTERNAME>"}' \
#   --insecure \
#   -u <APIKEY>:<APISECRET>

# cloudProviderConfig: "apiVersion: v1\nclusters:\n- cluster:\n    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ...IENFUlRJRklDQVRFLS0tLS0K\n    server: https://192.168.32.21:6443\n  name: default\ncontexts:\n- context:\n    cluster: default\n    namespace: user-mf9ss\n    user: default\n  name: default\ncurrent-context: default\nkind: Config\npreferences: {}\nusers:\n- name: default\n  user:\n    token: eyJhbGciOiJSUzI1NiIsImtpZCI...1aIWxYpDig\n"

# Specify nodepool options. Can add multiple node groups, specify etcd, controlplane and worker roles.
nodepools:
- name: master-pool-1
  etcd: true
  controlplane: true
  worker: true
  # specify node labels
  labels: {}
  # specify node taints
  taints: {}
  # specify nodepool size
  quantity: 1
  # Pause node pool
  paused: false
  # specify displayName
  displayName: "Master Pool 1"
  # specify rolling update mechanism
  # rollingUpdate:
  #   The maximum number of machines that can be unavailable during the update.
  #   Value can be an absolute number (ex: 5) or a percentage of desired
  #   machines (ex: 10%).
  #   Absolute number is calculated from percentage by rounding down.
  #   This can not be 0 if MaxSurge is 0.
  #   Defaults to 0.
  #   Example: when this is set to 30%, the old MachineSet can be scaled
  #   down to 70% of desired machines immediately when the rolling update
  #   starts. Once new machines are ready, old MachineSet can be scaled
  #   down further, followed by scaling up the new MachineSet, ensuring
  #   that the total number of machines available at all times
  #   during the update is at least 70% of desired machines.
  #   maxUnavailable: "5"
  #   The maximum number of machines that can be scheduled above the
  #   desired number of machines.
  #   Value can be an absolute number (ex: 5) or a percentage of
  #   desired machines (ex: 10%).
  #   This can not be 0 if MaxUnavailable is 0.
  #   Absolute number is calculated from percentage by rounding up.
  #   Defaults to 1.
  #   Example: when this is set to 30%, the new MachineSet can be scaled
  #   up immediately when the rolling update starts, such that the total
  #   number of old and new machines do not exceed 130% of desired
  #   machines. Once old machines have been killed, new MachineSet can
  #   be scaled up further, ensuring that total number of machines running
  #   at any time during the update is at most 130% of desired machines.
  #   maxSurge: "1"

  # specify machineDeployment Labels
  # machineDeploymentLabels: {}

  # specify machineDeployment annotations
  # machineDeploymentAnnotations: {}

  # VM Image reference, <namespace/name>
  imageName: "default/image-pc6x9"

  # VM CPU Count
  cpuCount: 2

  # VM Memory in GB
  memorySize: 4

  # VM disk size in GB
  diskSize: 40

  # Virtual network name reference, <namespace/name>
  networkName: "default/vlan-40"

  # Network IPAM type
  networkType: dhcp

  # SSH username
  sshUser: root

  # VM namespace
  vmNamespace: "default"

  # File contents for userdata
  # userData: |-
  #   #cloud-config
  #   package_upgrade: true

  # File contents for network data
  # networkData: |-
  #   version: 2
  #   ethernets:
  #     eth0:
  #       dhcp4: true
  #       addresses: [ fd10:0:2::2/120 ]
  #       gateway6: fd10:0:2::1

```

To provide your own configuration, modify the original values.yaml and create your own version, and pass it to helm. For example:

```bash
helm install --namespace fleet-default --values ./charts/your-own-values.yaml do-cluster ./charts
```

### Version control

The version control is implemented as helm release history and can easily be implemented by UI to provide operation history and rollback.

# License

Copyright (c) 2014-2021 [Rancher Labs, Inc.](http://rancher.com)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
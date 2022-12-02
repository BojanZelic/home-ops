# Zelic.io K8s Cluster

Hello :wave:

Welcome to my Homelab cluster. Feel free to poke around

## OnPrem Tech
| Hardware                | Description |
| ----------------------  | ----------- |
| Rasberry Pi CM4         | 8GB         |
| Raspberry Pi 4 Model B  | 2GB         |

## Architecture
* I expose my services using Cloudflare Argo Tunnels
* I 


## Supported Kubernetes versions

The project should be used with a [supported version of Kubernetes cluster](https://docs.gitlab.com/ee/user/project/clusters/#supported-cluster-versions).

## Enabling Fluentd as syslog forwarder

Fluentd can be deployed as a central service to forward syslog messages to SIEM:

* Enable the Fluentd Helm chart:

    ```yaml
    helmfiles:
    - path: applications/fluentd/helmfile.yaml
    ```

* The above results in the `fluentd.gitlab-managed-apps` service, which accepts
  syslog messages on port 5140.

* To forward to the Elasticsearch service of the `elastic-stack` chart,
  uncomment the output in applications/fluentd/values.yaml:

    ```yaml
    04_outputs.conf: |-
      <label @OUTPUT>
        # Route all events to Elasticsearch.
        <match **>
          @type elasticsearch
          host "elastic-stack-elasticsearch-master.gitlab-managed-apps"
          port 9200
        </match>
      </label>
    ```

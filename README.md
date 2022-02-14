# Cluster management template project

Example [cluster management](https://docs.gitlab.com/ee/user/clusters/management_project.html) project.

This project is based on a GitLab [Project Template](https://docs.gitlab.com/ee/gitlab-basics/create-project.html).

For more information, see [the documentation for this template](https://docs.gitlab.com/ee/user/clusters/management_project_template.html).

Improvements can be proposed in the [original project](https://gitlab.com/gitlab-org/project-templates/cluster-management).

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
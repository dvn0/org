
# Table of Contents

1.  [deprecated: Paths](#org1f3207e)
2.  [Deprecated: delete Parameters](#org671d1fd)
3.  [DEPRECATED definitions](#orged5cf85)

This is an exploration of the openAPI definition for kubernetes:
<https://github.com/kubernetes/kubernetes/blob/master/api/openapi-spec/swagger.json>

There were three styles of deprecation marks within the description fields.

-   deprecated: paths w/ watch
-   Deprecated: all delete actions orphanDependents parameters
-   DEPRECATED various deprecated definitions

Note the various case and presence of colons&#x2026;

After this dive, I'm confident we can exclude at least 189 endpoints (all watch related) from our calculations.
As for the others, I'm unsure, but suspect the deprecated parameters and definitions have little bearing for now.

    cd ~/go/src/k8s.io/kubernetes
    cat ./api/openapi-spec/swagger.json \
    | jq .paths \
    | gron | grep deprecated: | gron --ungron \
    | jq 'keys[]' -r \
    | sort -r | wc -l

    189


<a id="org1f3207e"></a>

# deprecated: Paths

There are full on paths that are deprecated, we see this info in the description
as DEPRECATED and deprecated in the description field.

The recent deprecation of watches look like this:

    cd ~/go/src/k8s.io/kubernetes
    cat ./api/openapi-spec/swagger.json \
      | jq '.paths["/api/v1/watch/nodes/{name}"] |.get'

    {
      "description": "watch changes to an object of kind Node. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.",
      "consumes": [
        "*/*"
      ],
      "produces": [
        "application/json",
        "application/yaml",
        "application/vnd.kubernetes.protobuf",
        "application/json;stream=watch",
        "application/vnd.kubernetes.protobuf;stream=watch"
      ],
      "schemes": [
        "https"
      ],
      "tags": [
        "core_v1"
      ],
      "operationId": "watchCoreV1Node",
      "responses": {
        "200": {
          "description": "OK",
          "schema": {
            "$ref": "#/definitions/io.k8s.apimachinery.pkg.apis.meta.v1.WatchEvent"
          }
        },
        "401": {
          "description": "Unauthorized"
        }
      },
      "x-kubernetes-action": "watch",
      "x-kubernetes-group-version-kind": {
        "group": "",
        "kind": "Node",
        "version": "v1"
      }
    }

If we look for all the unique descriptions, we can see that all of them are 'watch' related:

    cd ~/go/src/k8s.io/kubernetes
    cat ./api/openapi-spec/swagger.json \
    | jq .paths \
    | gron | grep DEPRECATED\\\|deprecated: | gron --ungron \
    | jq '.[].get.description' -r \
    | sort -r | uniq | cat

    watch individual changes to a list of VolumeAttachment. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of ValidatingWebhookConfiguration. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of StorageClass. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of StatefulSet. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of Service. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of ServiceAccount. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of Secret. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of Role. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of RoleBinding. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of ResourceQuota. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of ReplicationController. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of ReplicaSet. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of PriorityClass. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of PodTemplate. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of PodSecurityPolicy. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of PodPreset. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of PodDisruptionBudget. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of Pod. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of PersistentVolume. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of PersistentVolumeClaim. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of Node. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of NetworkPolicy. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of Namespace. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of MutatingWebhookConfiguration. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of LimitRange. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of Lease. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of Job. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of InitializerConfiguration. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of Ingress. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of HorizontalPodAutoscaler. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of Event. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of Endpoints. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of Deployment. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of DaemonSet. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of CustomResourceDefinition. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of CronJob. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of ControllerRevision. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of ConfigMap. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of ClusterRole. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of ClusterRoleBinding. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of CertificateSigningRequest. deprecated: use the 'watch' parameter with a list operation instead.
    watch individual changes to a list of APIService. deprecated: use the 'watch' parameter with a list operation instead.
    watch changes to an object of kind VolumeAttachment. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind ValidatingWebhookConfiguration. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind StorageClass. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind StatefulSet. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind Service. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind ServiceAccount. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind Secret. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind Role. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind RoleBinding. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind ResourceQuota. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind ReplicationController. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind ReplicaSet. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind PriorityClass. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind PodTemplate. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind PodSecurityPolicy. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind PodPreset. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind PodDisruptionBudget. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind Pod. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind PersistentVolume. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind PersistentVolumeClaim. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind Node. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind NetworkPolicy. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind Namespace. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind MutatingWebhookConfiguration. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind LimitRange. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind Lease. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind Job. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind InitializerConfiguration. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind Ingress. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind HorizontalPodAutoscaler. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind Event. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind Endpoints. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind Deployment. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind DaemonSet. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind CustomResourceDefinition. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind CronJob. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind ControllerRevision. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind ConfigMap. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind ClusterRole. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind ClusterRoleBinding. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind CertificateSigningRequest. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.
    watch changes to an object of kind APIService. deprecated: use the 'watch' parameter with a list operation instead, filtered to a single item with the 'fieldSelector' parameter.

This gives us our specific list of deprecated paths:

    cd ~/go/src/k8s.io/kubernetes
    cat ./api/openapi-spec/swagger.json \
    | jq .paths \
    | gron | grep deprecated: | gron --ungron \
    | jq 'keys[]' -r \
    | sort -r | cat

    /api/v1/watch/services
    /api/v1/watch/serviceaccounts
    /api/v1/watch/secrets
    /api/v1/watch/resourcequotas
    /api/v1/watch/replicationcontrollers
    /api/v1/watch/podtemplates
    /api/v1/watch/pods
    /api/v1/watch/persistentvolumes/{name}
    /api/v1/watch/persistentvolumes
    /api/v1/watch/persistentvolumeclaims
    /api/v1/watch/nodes/{name}
    /api/v1/watch/nodes
    /api/v1/watch/namespaces/{namespace}/services/{name}
    /api/v1/watch/namespaces/{namespace}/services
    /api/v1/watch/namespaces/{namespace}/serviceaccounts/{name}
    /api/v1/watch/namespaces/{namespace}/serviceaccounts
    /api/v1/watch/namespaces/{namespace}/secrets/{name}
    /api/v1/watch/namespaces/{namespace}/secrets
    /api/v1/watch/namespaces/{namespace}/resourcequotas/{name}
    /api/v1/watch/namespaces/{namespace}/resourcequotas
    /api/v1/watch/namespaces/{namespace}/replicationcontrollers/{name}
    /api/v1/watch/namespaces/{namespace}/replicationcontrollers
    /api/v1/watch/namespaces/{namespace}/podtemplates/{name}
    /api/v1/watch/namespaces/{namespace}/podtemplates
    /api/v1/watch/namespaces/{namespace}/pods/{name}
    /api/v1/watch/namespaces/{namespace}/pods
    /api/v1/watch/namespaces/{namespace}/persistentvolumeclaims/{name}
    /api/v1/watch/namespaces/{namespace}/persistentvolumeclaims
    /api/v1/watch/namespaces/{namespace}/limitranges/{name}
    /api/v1/watch/namespaces/{namespace}/limitranges
    /api/v1/watch/namespaces/{namespace}/events/{name}
    /api/v1/watch/namespaces/{namespace}/events
    /api/v1/watch/namespaces/{namespace}/endpoints/{name}
    /api/v1/watch/namespaces/{namespace}/endpoints
    /api/v1/watch/namespaces/{namespace}/configmaps/{name}
    /api/v1/watch/namespaces/{namespace}/configmaps
    /api/v1/watch/namespaces/{name}
    /api/v1/watch/namespaces
    /api/v1/watch/limitranges
    /api/v1/watch/events
    /api/v1/watch/endpoints
    /api/v1/watch/configmaps
    /apis/storage.k8s.io/v1/watch/storageclasses/{name}
    /apis/storage.k8s.io/v1/watch/storageclasses
    /apis/storage.k8s.io/v1beta1/watch/volumeattachments/{name}
    /apis/storage.k8s.io/v1beta1/watch/volumeattachments
    /apis/storage.k8s.io/v1beta1/watch/storageclasses/{name}
    /apis/storage.k8s.io/v1beta1/watch/storageclasses
    /apis/storage.k8s.io/v1alpha1/watch/volumeattachments/{name}
    /apis/storage.k8s.io/v1alpha1/watch/volumeattachments
    /apis/settings.k8s.io/v1alpha1/watch/podpresets
    /apis/settings.k8s.io/v1alpha1/watch/namespaces/{namespace}/podpresets/{name}
    /apis/settings.k8s.io/v1alpha1/watch/namespaces/{namespace}/podpresets
    /apis/scheduling.k8s.io/v1beta1/watch/priorityclasses/{name}
    /apis/scheduling.k8s.io/v1beta1/watch/priorityclasses
    /apis/scheduling.k8s.io/v1alpha1/watch/priorityclasses/{name}
    /apis/scheduling.k8s.io/v1alpha1/watch/priorityclasses
    /apis/rbac.authorization.k8s.io/v1/watch/roles
    /apis/rbac.authorization.k8s.io/v1/watch/rolebindings
    /apis/rbac.authorization.k8s.io/v1/watch/namespaces/{namespace}/roles/{name}
    /apis/rbac.authorization.k8s.io/v1/watch/namespaces/{namespace}/roles
    /apis/rbac.authorization.k8s.io/v1/watch/namespaces/{namespace}/rolebindings/{name}
    /apis/rbac.authorization.k8s.io/v1/watch/namespaces/{namespace}/rolebindings
    /apis/rbac.authorization.k8s.io/v1/watch/clusterroles/{name}
    /apis/rbac.authorization.k8s.io/v1/watch/clusterroles
    /apis/rbac.authorization.k8s.io/v1/watch/clusterrolebindings/{name}
    /apis/rbac.authorization.k8s.io/v1/watch/clusterrolebindings
    /apis/rbac.authorization.k8s.io/v1beta1/watch/roles
    /apis/rbac.authorization.k8s.io/v1beta1/watch/rolebindings
    /apis/rbac.authorization.k8s.io/v1beta1/watch/namespaces/{namespace}/roles/{name}
    /apis/rbac.authorization.k8s.io/v1beta1/watch/namespaces/{namespace}/roles
    /apis/rbac.authorization.k8s.io/v1beta1/watch/namespaces/{namespace}/rolebindings/{name}
    /apis/rbac.authorization.k8s.io/v1beta1/watch/namespaces/{namespace}/rolebindings
    /apis/rbac.authorization.k8s.io/v1beta1/watch/clusterroles/{name}
    /apis/rbac.authorization.k8s.io/v1beta1/watch/clusterroles
    /apis/rbac.authorization.k8s.io/v1beta1/watch/clusterrolebindings/{name}
    /apis/rbac.authorization.k8s.io/v1beta1/watch/clusterrolebindings
    /apis/rbac.authorization.k8s.io/v1alpha1/watch/roles
    /apis/rbac.authorization.k8s.io/v1alpha1/watch/rolebindings
    /apis/rbac.authorization.k8s.io/v1alpha1/watch/namespaces/{namespace}/roles/{name}
    /apis/rbac.authorization.k8s.io/v1alpha1/watch/namespaces/{namespace}/roles
    /apis/rbac.authorization.k8s.io/v1alpha1/watch/namespaces/{namespace}/rolebindings/{name}
    /apis/rbac.authorization.k8s.io/v1alpha1/watch/namespaces/{namespace}/rolebindings
    /apis/rbac.authorization.k8s.io/v1alpha1/watch/clusterroles/{name}
    /apis/rbac.authorization.k8s.io/v1alpha1/watch/clusterroles
    /apis/rbac.authorization.k8s.io/v1alpha1/watch/clusterrolebindings/{name}
    /apis/rbac.authorization.k8s.io/v1alpha1/watch/clusterrolebindings
    /apis/policy/v1beta1/watch/podsecuritypolicies/{name}
    /apis/policy/v1beta1/watch/podsecuritypolicies
    /apis/policy/v1beta1/watch/poddisruptionbudgets
    /apis/policy/v1beta1/watch/namespaces/{namespace}/poddisruptionbudgets/{name}
    /apis/policy/v1beta1/watch/namespaces/{namespace}/poddisruptionbudgets
    /apis/networking.k8s.io/v1/watch/networkpolicies
    /apis/networking.k8s.io/v1/watch/namespaces/{namespace}/networkpolicies/{name}
    /apis/networking.k8s.io/v1/watch/namespaces/{namespace}/networkpolicies
    /apis/extensions/v1beta1/watch/replicasets
    /apis/extensions/v1beta1/watch/podsecuritypolicies/{name}
    /apis/extensions/v1beta1/watch/podsecuritypolicies
    /apis/extensions/v1beta1/watch/networkpolicies
    /apis/extensions/v1beta1/watch/namespaces/{namespace}/replicasets/{name}
    /apis/extensions/v1beta1/watch/namespaces/{namespace}/replicasets
    /apis/extensions/v1beta1/watch/namespaces/{namespace}/networkpolicies/{name}
    /apis/extensions/v1beta1/watch/namespaces/{namespace}/networkpolicies
    /apis/extensions/v1beta1/watch/namespaces/{namespace}/ingresses/{name}
    /apis/extensions/v1beta1/watch/namespaces/{namespace}/ingresses
    /apis/extensions/v1beta1/watch/namespaces/{namespace}/deployments/{name}
    /apis/extensions/v1beta1/watch/namespaces/{namespace}/deployments
    /apis/extensions/v1beta1/watch/namespaces/{namespace}/daemonsets/{name}
    /apis/extensions/v1beta1/watch/namespaces/{namespace}/daemonsets
    /apis/extensions/v1beta1/watch/ingresses
    /apis/extensions/v1beta1/watch/deployments
    /apis/extensions/v1beta1/watch/daemonsets
    /apis/events.k8s.io/v1beta1/watch/namespaces/{namespace}/events/{name}
    /apis/events.k8s.io/v1beta1/watch/namespaces/{namespace}/events
    /apis/events.k8s.io/v1beta1/watch/events
    /apis/coordination.k8s.io/v1beta1/watch/namespaces/{namespace}/leases/{name}
    /apis/coordination.k8s.io/v1beta1/watch/namespaces/{namespace}/leases
    /apis/coordination.k8s.io/v1beta1/watch/leases
    /apis/certificates.k8s.io/v1beta1/watch/certificatesigningrequests/{name}
    /apis/certificates.k8s.io/v1beta1/watch/certificatesigningrequests
    /apis/batch/v2alpha1/watch/namespaces/{namespace}/cronjobs/{name}
    /apis/batch/v2alpha1/watch/namespaces/{namespace}/cronjobs
    /apis/batch/v2alpha1/watch/cronjobs
    /apis/batch/v1/watch/namespaces/{namespace}/jobs/{name}
    /apis/batch/v1/watch/namespaces/{namespace}/jobs
    /apis/batch/v1/watch/jobs
    /apis/batch/v1beta1/watch/namespaces/{namespace}/cronjobs/{name}
    /apis/batch/v1beta1/watch/namespaces/{namespace}/cronjobs
    /apis/batch/v1beta1/watch/cronjobs
    /apis/autoscaling/v2beta2/watch/namespaces/{namespace}/horizontalpodautoscalers/{name}
    /apis/autoscaling/v2beta2/watch/namespaces/{namespace}/horizontalpodautoscalers
    /apis/autoscaling/v2beta2/watch/horizontalpodautoscalers
    /apis/autoscaling/v2beta1/watch/namespaces/{namespace}/horizontalpodautoscalers/{name}
    /apis/autoscaling/v2beta1/watch/namespaces/{namespace}/horizontalpodautoscalers
    /apis/autoscaling/v2beta1/watch/horizontalpodautoscalers
    /apis/autoscaling/v1/watch/namespaces/{namespace}/horizontalpodautoscalers/{name}
    /apis/autoscaling/v1/watch/namespaces/{namespace}/horizontalpodautoscalers
    /apis/autoscaling/v1/watch/horizontalpodautoscalers
    /apis/apps/v1/watch/statefulsets
    /apis/apps/v1/watch/replicasets
    /apis/apps/v1/watch/namespaces/{namespace}/statefulsets/{name}
    /apis/apps/v1/watch/namespaces/{namespace}/statefulsets
    /apis/apps/v1/watch/namespaces/{namespace}/replicasets/{name}
    /apis/apps/v1/watch/namespaces/{namespace}/replicasets
    /apis/apps/v1/watch/namespaces/{namespace}/deployments/{name}
    /apis/apps/v1/watch/namespaces/{namespace}/deployments
    /apis/apps/v1/watch/namespaces/{namespace}/daemonsets/{name}
    /apis/apps/v1/watch/namespaces/{namespace}/daemonsets
    /apis/apps/v1/watch/namespaces/{namespace}/controllerrevisions/{name}
    /apis/apps/v1/watch/namespaces/{namespace}/controllerrevisions
    /apis/apps/v1/watch/deployments
    /apis/apps/v1/watch/daemonsets
    /apis/apps/v1/watch/controllerrevisions
    /apis/apps/v1beta2/watch/statefulsets
    /apis/apps/v1beta2/watch/replicasets
    /apis/apps/v1beta2/watch/namespaces/{namespace}/statefulsets/{name}
    /apis/apps/v1beta2/watch/namespaces/{namespace}/statefulsets
    /apis/apps/v1beta2/watch/namespaces/{namespace}/replicasets/{name}
    /apis/apps/v1beta2/watch/namespaces/{namespace}/replicasets
    /apis/apps/v1beta2/watch/namespaces/{namespace}/deployments/{name}
    /apis/apps/v1beta2/watch/namespaces/{namespace}/deployments
    /apis/apps/v1beta2/watch/namespaces/{namespace}/daemonsets/{name}
    /apis/apps/v1beta2/watch/namespaces/{namespace}/daemonsets
    /apis/apps/v1beta2/watch/namespaces/{namespace}/controllerrevisions/{name}
    /apis/apps/v1beta2/watch/namespaces/{namespace}/controllerrevisions
    /apis/apps/v1beta2/watch/deployments
    /apis/apps/v1beta2/watch/daemonsets
    /apis/apps/v1beta2/watch/controllerrevisions
    /apis/apps/v1beta1/watch/statefulsets
    /apis/apps/v1beta1/watch/namespaces/{namespace}/statefulsets/{name}
    /apis/apps/v1beta1/watch/namespaces/{namespace}/statefulsets
    /apis/apps/v1beta1/watch/namespaces/{namespace}/deployments/{name}
    /apis/apps/v1beta1/watch/namespaces/{namespace}/deployments
    /apis/apps/v1beta1/watch/namespaces/{namespace}/controllerrevisions/{name}
    /apis/apps/v1beta1/watch/namespaces/{namespace}/controllerrevisions
    /apis/apps/v1beta1/watch/deployments
    /apis/apps/v1beta1/watch/controllerrevisions
    /apis/apiregistration.k8s.io/v1/watch/apiservices/{name}
    /apis/apiregistration.k8s.io/v1/watch/apiservices
    /apis/apiregistration.k8s.io/v1beta1/watch/apiservices/{name}
    /apis/apiregistration.k8s.io/v1beta1/watch/apiservices
    /apis/apiextensions.k8s.io/v1beta1/watch/customresourcedefinitions/{name}
    /apis/apiextensions.k8s.io/v1beta1/watch/customresourcedefinitions
    /apis/admissionregistration.k8s.io/v1beta1/watch/validatingwebhookconfigurations/{name}
    /apis/admissionregistration.k8s.io/v1beta1/watch/validatingwebhookconfigurations
    /apis/admissionregistration.k8s.io/v1beta1/watch/mutatingwebhookconfigurations/{name}
    /apis/admissionregistration.k8s.io/v1beta1/watch/mutatingwebhookconfigurations
    /apis/admissionregistration.k8s.io/v1alpha1/watch/initializerconfigurations/{name}
    /apis/admissionregistration.k8s.io/v1alpha1/watch/initializerconfigurations


<a id="org671d1fd"></a>

# Deprecated: delete Parameters

Some endpoints have deprecated parameters (orphanDependents)

    cd ~/go/src/k8s.io/kubernetes
    cat ./api/openapi-spec/swagger.json \
      | jq -c '.paths["/apis/storage.k8s.io/v1beta1/volumeattachments/{name}"] | .delete.parameters' \
      | jq .[3]

    {
      "uniqueItems": true,
      "type": "boolean",
      "description": "Deprecated: please use the PropagationPolicy, this field will be deprecated in 1.7. Should the dependent objects be orphaned. If true/false, the \"orphan\" finalizer will be added to/removed from the object's finalizers list. Either this field or PropagationPolicy may be set, but not both.",
      "name": "orphanDependents",
      "in": "query"
    }

All of these endpoints no longer support the orphanDependents, however this
should not affect coverage percentage.

    cd ~/go/src/k8s.io/kubernetes
    cat ./api/openapi-spec/swagger.json \
      | jq -c '.paths | to_entries[]' \
      | grep Deprecated: \
      | jq .key
    echo # https://necromuralist.github.io/posts/org-babel-stderr-results/

    "/api/v1/namespaces/{namespace}/configmaps/{name}"
    "/api/v1/namespaces/{namespace}/endpoints/{name}"
    "/api/v1/namespaces/{namespace}/events/{name}"
    "/api/v1/namespaces/{namespace}/limitranges/{name}"
    "/api/v1/namespaces/{namespace}/persistentvolumeclaims/{name}"
    "/api/v1/namespaces/{namespace}/pods/{name}"
    "/api/v1/namespaces/{namespace}/podtemplates/{name}"
    "/api/v1/namespaces/{namespace}/replicationcontrollers/{name}"
    "/api/v1/namespaces/{namespace}/resourcequotas/{name}"
    "/api/v1/namespaces/{namespace}/secrets/{name}"
    "/api/v1/namespaces/{namespace}/serviceaccounts/{name}"
    "/api/v1/namespaces/{namespace}/services/{name}"
    "/api/v1/namespaces/{name}"
    "/api/v1/nodes/{name}"
    "/api/v1/persistentvolumes/{name}"
    "/apis/admissionregistration.k8s.io/v1alpha1/initializerconfigurations/{name}"
    "/apis/admissionregistration.k8s.io/v1beta1/mutatingwebhookconfigurations/{name}"
    "/apis/admissionregistration.k8s.io/v1beta1/validatingwebhookconfigurations/{name}"
    "/apis/apiextensions.k8s.io/v1beta1/customresourcedefinitions/{name}"
    "/apis/apiregistration.k8s.io/v1/apiservices/{name}"
    "/apis/apiregistration.k8s.io/v1beta1/apiservices/{name}"
    "/apis/apps/v1/namespaces/{namespace}/controllerrevisions/{name}"
    "/apis/apps/v1/namespaces/{namespace}/daemonsets/{name}"
    "/apis/apps/v1/namespaces/{namespace}/deployments/{name}"
    "/apis/apps/v1/namespaces/{namespace}/replicasets/{name}"
    "/apis/apps/v1/namespaces/{namespace}/statefulsets/{name}"
    "/apis/apps/v1beta1/namespaces/{namespace}/controllerrevisions/{name}"
    "/apis/apps/v1beta1/namespaces/{namespace}/deployments/{name}"
    "/apis/apps/v1beta1/namespaces/{namespace}/statefulsets/{name}"
    "/apis/apps/v1beta2/namespaces/{namespace}/controllerrevisions/{name}"
    "/apis/apps/v1beta2/namespaces/{namespace}/daemonsets/{name}"
    "/apis/apps/v1beta2/namespaces/{namespace}/deployments/{name}"
    "/apis/apps/v1beta2/namespaces/{namespace}/replicasets/{name}"
    "/apis/apps/v1beta2/namespaces/{namespace}/statefulsets/{name}"
    "/apis/autoscaling/v1/namespaces/{namespace}/horizontalpodautoscalers/{name}"
    "/apis/autoscaling/v2beta1/namespaces/{namespace}/horizontalpodautoscalers/{name}"
    "/apis/autoscaling/v2beta2/namespaces/{namespace}/horizontalpodautoscalers/{name}"
    "/apis/batch/v1/namespaces/{namespace}/jobs/{name}"
    "/apis/batch/v1beta1/namespaces/{namespace}/cronjobs/{name}"
    "/apis/batch/v2alpha1/namespaces/{namespace}/cronjobs/{name}"
    "/apis/certificates.k8s.io/v1beta1/certificatesigningrequests/{name}"
    "/apis/coordination.k8s.io/v1beta1/namespaces/{namespace}/leases/{name}"
    "/apis/events.k8s.io/v1beta1/namespaces/{namespace}/events/{name}"
    "/apis/extensions/v1beta1/namespaces/{namespace}/daemonsets/{name}"
    "/apis/extensions/v1beta1/namespaces/{namespace}/deployments/{name}"
    "/apis/extensions/v1beta1/namespaces/{namespace}/ingresses/{name}"
    "/apis/extensions/v1beta1/namespaces/{namespace}/networkpolicies/{name}"
    "/apis/extensions/v1beta1/namespaces/{namespace}/replicasets/{name}"
    "/apis/extensions/v1beta1/podsecuritypolicies/{name}"
    "/apis/networking.k8s.io/v1/namespaces/{namespace}/networkpolicies/{name}"
    "/apis/policy/v1beta1/namespaces/{namespace}/poddisruptionbudgets/{name}"
    "/apis/policy/v1beta1/podsecuritypolicies/{name}"
    "/apis/rbac.authorization.k8s.io/v1/clusterrolebindings/{name}"
    "/apis/rbac.authorization.k8s.io/v1/clusterroles/{name}"
    "/apis/rbac.authorization.k8s.io/v1/namespaces/{namespace}/rolebindings/{name}"
    "/apis/rbac.authorization.k8s.io/v1/namespaces/{namespace}/roles/{name}"
    "/apis/rbac.authorization.k8s.io/v1alpha1/clusterrolebindings/{name}"
    "/apis/rbac.authorization.k8s.io/v1alpha1/clusterroles/{name}"
    "/apis/rbac.authorization.k8s.io/v1alpha1/namespaces/{namespace}/rolebindings/{name}"
    "/apis/rbac.authorization.k8s.io/v1alpha1/namespaces/{namespace}/roles/{name}"
    "/apis/rbac.authorization.k8s.io/v1beta1/clusterrolebindings/{name}"
    "/apis/rbac.authorization.k8s.io/v1beta1/clusterroles/{name}"
    "/apis/rbac.authorization.k8s.io/v1beta1/namespaces/{namespace}/rolebindings/{name}"
    "/apis/rbac.authorization.k8s.io/v1beta1/namespaces/{namespace}/roles/{name}"
    "/apis/scheduling.k8s.io/v1alpha1/priorityclasses/{name}"
    "/apis/scheduling.k8s.io/v1beta1/priorityclasses/{name}"
    "/apis/settings.k8s.io/v1alpha1/namespaces/{namespace}/podpresets/{name}"
    "/apis/storage.k8s.io/v1/storageclasses/{name}"
    "/apis/storage.k8s.io/v1alpha1/volumeattachments/{name}"
    "/apis/storage.k8s.io/v1beta1/storageclasses/{name}"
    "/apis/storage.k8s.io/v1beta1/volumeattachments/{name}"


<a id="orged5cf85"></a>

# DEPRECATED definitions

There were quite a few definitions dropped, but again this shouldn't affect test coverage.

      cd ~/go/src/k8s.io/kubernetes
      cat ./api/openapi-spec/swagger.json \
      | jq .definitions \
      | gron | grep DEPRECATED | gron --ungron \
      | jq . 
    #\
    #  | sort -r | uniq | cat
    #'.[].get.description' -r \
      echo # https://necromuralist.github.io/posts/org-babel-stderr-results/

    {
      "io.k8s.api.apps.v1beta1.ControllerRevision": {
        "description": "DEPRECATED - This group version of ControllerRevision is deprecated by apps/v1beta2/ControllerRevision. See the release notes for more information. ControllerRevision implements an immutable snapshot of state data. Clients are responsible for serializing and deserializing the objects that contain their internal state. Once a ControllerRevision has been successfully created, it can not be updated. The API Server will fail validation of all requests that attempt to mutate the Data field. ControllerRevisions may, however, be deleted. Note that, due to its use by both the DaemonSet and StatefulSet controllers for update and rollback, this object is beta. However, it may be subject to name and representation changes in future releases, and clients should not depend on its stability. It is primarily for internal use by controllers."
      },
      "io.k8s.api.apps.v1beta1.Deployment": {
        "description": "DEPRECATED - This group version of Deployment is deprecated by apps/v1beta2/Deployment. See the release notes for more information. Deployment enables declarative updates for Pods and ReplicaSets."
      },
      "io.k8s.api.apps.v1beta1.DeploymentRollback": {
        "description": "DEPRECATED. DeploymentRollback stores the information required to rollback a deployment."
      },
      "io.k8s.api.apps.v1beta1.DeploymentSpec": {
        "properties": {
          "rollbackTo": {
            "description": "DEPRECATED. The config this deployment is rolling back to. Will be cleared after rollback is done."
          }
        }
      },
      "io.k8s.api.apps.v1beta1.RollbackConfig": {
        "description": "DEPRECATED."
      },
      "io.k8s.api.apps.v1beta1.StatefulSet": {
        "description": "DEPRECATED - This group version of StatefulSet is deprecated by apps/v1beta2/StatefulSet. See the release notes for more information. StatefulSet represents a set of pods with consistent identities. Identities are defined as:\n - Network: A single stable DNS and hostname.\n - Storage: As many VolumeClaims as requested.\nThe StatefulSet guarantees that a given network identity will always map to the same storage identity."
      },
      "io.k8s.api.apps.v1beta2.ControllerRevision": {
        "description": "DEPRECATED - This group version of ControllerRevision is deprecated by apps/v1/ControllerRevision. See the release notes for more information. ControllerRevision implements an immutable snapshot of state data. Clients are responsible for serializing and deserializing the objects that contain their internal state. Once a ControllerRevision has been successfully created, it can not be updated. The API Server will fail validation of all requests that attempt to mutate the Data field. ControllerRevisions may, however, be deleted. Note that, due to its use by both the DaemonSet and StatefulSet controllers for update and rollback, this object is beta. However, it may be subject to name and representation changes in future releases, and clients should not depend on its stability. It is primarily for internal use by controllers."
      },
      "io.k8s.api.apps.v1beta2.DaemonSet": {
        "description": "DEPRECATED - This group version of DaemonSet is deprecated by apps/v1/DaemonSet. See the release notes for more information. DaemonSet represents the configuration of a daemon set."
      },
      "io.k8s.api.apps.v1beta2.Deployment": {
        "description": "DEPRECATED - This group version of Deployment is deprecated by apps/v1/Deployment. See the release notes for more information. Deployment enables declarative updates for Pods and ReplicaSets."
      },
      "io.k8s.api.apps.v1beta2.ReplicaSet": {
        "description": "DEPRECATED - This group version of ReplicaSet is deprecated by apps/v1/ReplicaSet. See the release notes for more information. ReplicaSet ensures that a specified number of pod replicas are running at any given time."
      },
      "io.k8s.api.apps.v1beta2.StatefulSet": {
        "description": "DEPRECATED - This group version of StatefulSet is deprecated by apps/v1/StatefulSet. See the release notes for more information. StatefulSet represents a set of pods with consistent identities. Identities are defined as:\n - Network: A single stable DNS and hostname.\n - Storage: As many VolumeClaims as requested.\nThe StatefulSet guarantees that a given network identity will always map to the same storage identity."
      },
      "io.k8s.api.core.v1.GitRepoVolumeSource": {
        "description": "Represents a volume that is populated with the contents of a git repository. Git repo volumes do not support ownership management. Git repo volumes support SELinux relabeling.\n\nDEPRECATED: GitRepo is deprecated. To provision a container with a git repo, mount an EmptyDir into an InitContainer that clones the repo using git, then mount the EmptyDir into the Pod's container."
      },
      "io.k8s.api.core.v1.Volume": {
        "properties": {
          "gitRepo": {
            "description": "GitRepo represents a git repository at a particular revision. DEPRECATED: GitRepo is deprecated. To provision a container with a git repo, mount an EmptyDir into an InitContainer that clones the repo using git, then mount the EmptyDir into the Pod's container."
          }
        }
      },
      "io.k8s.api.extensions.v1beta1.DaemonSet": {
        "description": "DEPRECATED - This group version of DaemonSet is deprecated by apps/v1beta2/DaemonSet. See the release notes for more information. DaemonSet represents the configuration of a daemon set."
      },
      "io.k8s.api.extensions.v1beta1.DaemonSetSpec": {
        "properties": {
          "templateGeneration": {
            "description": "DEPRECATED. A sequence number representing a specific generation of the template. Populated by the system. It can be set only during the creation."
          }
        }
      },
      "io.k8s.api.extensions.v1beta1.Deployment": {
        "description": "DEPRECATED - This group version of Deployment is deprecated by apps/v1beta2/Deployment. See the release notes for more information. Deployment enables declarative updates for Pods and ReplicaSets."
      },
      "io.k8s.api.extensions.v1beta1.DeploymentRollback": {
        "description": "DEPRECATED. DeploymentRollback stores the information required to rollback a deployment."
      },
      "io.k8s.api.extensions.v1beta1.DeploymentSpec": {
        "properties": {
          "rollbackTo": {
            "description": "DEPRECATED. The config this deployment is rolling back to. Will be cleared after rollback is done."
          }
        }
      },
      "io.k8s.api.extensions.v1beta1.IPBlock": {
        "description": "DEPRECATED 1.9 - This group version of IPBlock is deprecated by networking/v1/IPBlock. IPBlock describes a particular CIDR (Ex. \"192.168.1.1/24\") that is allowed to the pods matched by a NetworkPolicySpec's podSelector. The except entry describes CIDRs that should not be included within this rule."
      },
      "io.k8s.api.extensions.v1beta1.NetworkPolicy": {
        "description": "DEPRECATED 1.9 - This group version of NetworkPolicy is deprecated by networking/v1/NetworkPolicy. NetworkPolicy describes what network traffic is allowed for a set of Pods"
      },
      "io.k8s.api.extensions.v1beta1.NetworkPolicyEgressRule": {
        "description": "DEPRECATED 1.9 - This group version of NetworkPolicyEgressRule is deprecated by networking/v1/NetworkPolicyEgressRule. NetworkPolicyEgressRule describes a particular set of traffic that is allowed out of pods matched by a NetworkPolicySpec's podSelector. The traffic must match both ports and to. This type is beta-level in 1.8"
      },
      "io.k8s.api.extensions.v1beta1.NetworkPolicyIngressRule": {
        "description": "DEPRECATED 1.9 - This group version of NetworkPolicyIngressRule is deprecated by networking/v1/NetworkPolicyIngressRule. This NetworkPolicyIngressRule matches traffic if and only if the traffic matches both ports AND from."
      },
      "io.k8s.api.extensions.v1beta1.NetworkPolicyList": {
        "description": "DEPRECATED 1.9 - This group version of NetworkPolicyList is deprecated by networking/v1/NetworkPolicyList. Network Policy List is a list of NetworkPolicy objects."
      },
      "io.k8s.api.extensions.v1beta1.NetworkPolicyPeer": {
        "description": "DEPRECATED 1.9 - This group version of NetworkPolicyPeer is deprecated by networking/v1/NetworkPolicyPeer."
      },
      "io.k8s.api.extensions.v1beta1.NetworkPolicyPort": {
        "description": "DEPRECATED 1.9 - This group version of NetworkPolicyPort is deprecated by networking/v1/NetworkPolicyPort."
      },
      "io.k8s.api.extensions.v1beta1.NetworkPolicySpec": {
        "description": "DEPRECATED 1.9 - This group version of NetworkPolicySpec is deprecated by networking/v1/NetworkPolicySpec."
      },
      "io.k8s.api.extensions.v1beta1.ReplicaSet": {
        "description": "DEPRECATED - This group version of ReplicaSet is deprecated by apps/v1beta2/ReplicaSet. See the release notes for more information. ReplicaSet ensures that a specified number of pod replicas are running at any given time."
      },
      "io.k8s.api.extensions.v1beta1.RollbackConfig": {
        "description": "DEPRECATED."
      }
    }


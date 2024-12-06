---install kind
curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.25.0/kind-windows-amd64
.\kind.exe create cluster
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

contains:  https://artifacthub.io/packages/search?repo=prometheus-community&sort=relevance&page=1

.\helm.exe install kind-prometheus prometheus-community/kube-prometheus-stack --namespace monitoring --set prometheus.service.nodePort=30000 --set prometheus.service.type=NodePort --set grafana.service.nodePort=31000 --set grafana.service.type=NodePort --set alertmanager.service.nodePort=32000 --set alertmanager.service.type=NodePort --set prometheus-node-exporter.service.nodePort=32001 --set prometheus-node-exporter.service.type=NodePort


What is a resource
The objects we create in K8 i.e. pods, services.. etc
All resources can be seen by 
PS D:\kind\python-prom-exporter> kubectl api-resources
NAME                                SHORTNAMES   APIVERSION                        NAMESPACED   KIND
bindings                                         v1                                true         Binding
componentstatuses                   cs           v1                                false        ComponentStatus
configmaps                          cm           v1                                true         ConfigMap
endpoints                           ep           v1                                true         Endpoints
events                              ev           v1                                true         Event
limitranges                         limits       v1                                true         LimitRange
namespaces                          ns           v1                                false        Namespace
nodes                               no           v1                                false        Node
persistentvolumeclaims              pvc          v1                                true         PersistentVolumeClaim
persistentvolumes                   pv           v1                                false        PersistentVolume
pods                                po           v1                                true         Pod
podtemplates                                     v1                                true         PodTemplate
replicationcontrollers              rc           v1                                true         ReplicationController
resourcequotas                      quota        v1                                true         ResourceQuota
secrets                                          v1                                true         Secret
serviceaccounts                     sa           v1                                true         ServiceAccount
services                            svc          v1                                true         Service
mutatingwebhookconfigurations                    admissionregistration.k8s.io/v1   false        MutatingWebhookConfiguration
validatingadmissionpolicies                      admissionregistration.k8s.io/v1   false        ValidatingAdmissionPolicy
validatingadmissionpolicybindings                admissionregistration.k8s.io/v1   false        ValidatingAdmissionPolicyBinding
validatingwebhookconfigurations                  admissionregistration.k8s.io/v1   false        ValidatingWebhookConfiguration
customresourcedefinitions           crd,crds     apiextensions.k8s.io/v1           false        CustomResourceDefinition
apiservices                                      apiregistration.k8s.io/v1         false        APIService
controllerrevisions                              apps/v1                           true         ControllerRevision
daemonsets                          ds           apps/v1                           true         DaemonSet
deployments                         deploy       apps/v1                           true         Deployment
replicasets                         rs           apps/v1                           true         ReplicaSet
statefulsets                        sts          apps/v1                           true         StatefulSet

in above output we can see that there are different endpoint groups - v1, apps/v1 

What is a custom resource:
A user defined resource, which is not avalable above, it can be modification/extension of any of resources above, as per users requirements

to create this cutom resource we need to define it first in  yaml declarative syntax, which becomes a CRD

To manage the lifecycle of crd, create, update delete , we need a custom controller
https://medium.com/@minimaldevops/crds-in-kubernetes-c38037315548

custom kubernets controller - nginx ingress, argocd, istio..etc - when ever we want o extend functionality of k8,
controller will manage lifecycle of the custom resource created

ko = bundle package and manage your k8 custom controllers

difference between using helm charts vs operators
helm charts will be a package of all yamls, secrets, values.yaml and helm will update all these values in the kubernetes

But if a installed resource is modified - port chaned then helm will not be able to reconciliate or roll back that change


operator is also an bundle but structure is different, 
bundle will be installe 

it continuously watches the state
and they ahave the reconcilisation logic which can autofix the changes

the operator creator has to write the reconciliation logic

helm chart we have to download and install it

but operator can automatically install new version

operator also provides telemtry

how to write operator: https://www.youtube.com/watch?v=VAojjIYVhGk

use operator framework
  go based operator
  ansible based 
  helm based ope

  - operator sdk is the framework which we have to use
  - use controller manager
  - boot strapping code is given - watchers are created automatically
  - the reconciliation logic
  - crete the resources and autoheal resouces
  - 

operator hub.io all k8 operator are stored here

how to install the operator
install operator lifecycle manager olm


---
if you want to write operator, find the controller which does not have an operator
  -----------------------
  deploying prometheus
  https://observability.thomasriley.co.uk/prometheus/deploying-prometheus/

  https://operatorhub.io/operator/prometheus
Three Options
Lets look at these three options available for deploying Prometheus to Kubernetes.

Prometheus Operator
This is a Kubernetes Operator that provides several Custom Resource Definitions (CRDs) that will allow us to define and configure instances of Prometheus via Kubernetes resources. The Operator contains all the logic for managing the deployment and automated configuration of Prometheus based on the YAML configuration the user deployments to Kubernetes.

kube-prometheus
This project acts as a jssonet library for deploying Prometheus Operator and an entire Prometheus monitoring stack.

Community Helm Chart
This is similar to the kube-prometheus project however the deployment is done via Helm. This is a community driven chart in the stable Helm chart repository.

following services are installed via helm kube stack
PS D:\kind\python-prom-exporter> kubectl get all -n monitoring
NAME                                                         READY   STATUS    RESTARTS   AGE
pod/alertmanager-kind-prometheus-kube-prome-alertmanager-0   2/2     Running   0          4h48m
pod/kind-prometheus-grafana-7c557d9994-gmgww                 3/3     Running   0          4h49m
pod/kind-prometheus-kube-prome-operator-ddb649d89-d7knx      1/1     Running   0          4h49m
pod/kind-prometheus-kube-state-metrics-64b56896c5-jdc5s      1/1     Running   0          4h49m
pod/kind-prometheus-prometheus-node-exporter-bxbf7           1/1     Running   0          4h49m
pod/prometheus-kind-prometheus-kube-prome-prometheus-0       2/2     Running   0          4h48m

NAME                                               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                         AGE
service/alertmanager-operated                      ClusterIP   None            <none>        9093/TCP,9094/TCP,9094/UDP      4h48m
service/kind-prometheus-grafana                    NodePort    10.96.197.169   <none>        80:31000/TCP                    4h49m
service/kind-prometheus-kube-prome-alertmanager    NodePort    10.96.81.187    <none>        9093:32000/TCP,8080:30131/TCP   4h49m
service/kind-prometheus-kube-prome-operator        ClusterIP   10.96.30.213    <none>        443/TCP                         4h49m
service/kind-prometheus-kube-prome-prometheus      NodePort    10.96.71.114    <none>        9090:30000/TCP,8080:30091/TCP   4h49m
service/kind-prometheus-kube-state-metrics         ClusterIP   10.96.177.132   <none>        8080/TCP                        4h49m
service/kind-prometheus-prometheus-node-exporter   NodePort    10.96.51.134    <none>        9100:32001/TCP                  4h49m
service/prometheus-operated                        ClusterIP   None            <none>        9090/TCP                        4h48m

NAME                                                      DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
daemonset.apps/kind-prometheus-prometheus-node-exporter   1         1         1       1            1           kubernetes.io/os=linux   4h49m

NAME                                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/kind-prometheus-grafana               1/1     1            1           4h49m
deployment.apps/kind-prometheus-kube-prome-operator   1/1     1            1           4h49m
deployment.apps/kind-prometheus-kube-state-metrics    1/1     1            1           4h49m

NAME                                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/kind-prometheus-grafana-7c557d9994              1         1         1       4h49m
replicaset.apps/kind-prometheus-kube-prome-operator-ddb649d89   1         1         1       4h49m
replicaset.apps/kind-prometheus-kube-state-metrics-64b56896c5   1         1         1       4h49m

NAME                                                                    READY   AGE
statefulset.apps/alertmanager-kind-prometheus-kube-prome-alertmanager   1/1     4h48m
statefulset.apps/prometheus-kind-prometheus-kube-prome-prometheus       1/1     4h48m
PS D:\kind\python-prom-exporter>

PS D:\kind\python-prom-exporter> kubectl get customresourcedefinitions
NAME                                        CREATED AT
alertmanagerconfigs.monitoring.coreos.com   2024-12-06T14:21:43Z
alertmanagers.monitoring.coreos.com         2024-12-06T14:21:44Z
podmonitors.monitoring.coreos.com           2024-12-06T14:21:44Z
probes.monitoring.coreos.com                2024-12-06T14:21:44Z
prometheusagents.monitoring.coreos.com      2024-12-06T14:21:45Z
prometheuses.monitoring.coreos.com          2024-12-06T14:21:45Z
prometheusrules.monitoring.coreos.com       2024-12-06T14:21:45Z
scrapeconfigs.monitoring.coreos.com         2024-12-06T14:21:46Z
servicemonitors.monitoring.coreos.com       2024-12-06T14:21:46Z
thanosrulers.monitoring.coreos.com          2024-12-06T14:21:46Z

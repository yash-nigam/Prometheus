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


  -----------------------
  deploying prometheus
  https://observability.thomasriley.co.uk/prometheus/deploying-prometheus/

  


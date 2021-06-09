# Argo CD 1.1.2: OutOfSync Error for additional Argo CD Instances

## Issue

When you use the default instance of Argo CD that is deployed in the `openshift-gitops` namespace to create a new Argo CD intance in another namespace (e.g. `argocd`), the `Application` in `openshift-gitops` will remain "OutOfSync" due to a secret that is created.

This was issue is present in the Tech Preview version of OpenShift GitOps (1.0.0), but seemed to have been fixed in the GA version (1.1.0 and 1.1.1).

## How to Reproduce

1. Install the OpenShift GitOps operator in your cluster.

2. Create an Argo CD project that is responsible for creating the new namespace where the new Argo CD instance will be deployed.

```
oc apply -k 01-namespace
```

3. Create an Argo CD project that will deploy a new instance of Argo CD into the new namespace.

```
oc apply -k 02-argocd
```

4. The `openshift-gitops` instance of Argo CD will properly deploy a new instance of Argo CD into the `argocd` namespace, but the `Application` will remain "OutOfSync".  If you view the details of this application, you will see there is a `Secret` that was created that is the cause of the OutOfSync issue.


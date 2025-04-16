# NKP Custom Catalog Application: Kasten K10

To add this to a project simply create a gitrepository resource as shown below to the given project

```
#export a NAMESPACE variable pointing to the workspace on the management cluster.
#to find the workspace namespace, use the `nkp get workspaces` command
export WORKSPACE_NAMESPACE=lazarus-workspace
```

```
kubectl apply -f - <<EOF
apiVersion: source.toolkit.fluxcd.io/v1beta1
kind: GitRepository
metadata:
  name: kasten-k10
  namespace: ${WORKSPACE_NAMESPACE}
  labels:
    kommander.d2iq.io/gitapps-gitrepository-type: catalog
    kommander.d2iq.io/gitrepository-type: catalog
spec:
  interval: 1m0s
  ref:
    branch: main
  timeout: 20s
  url: https://github.com/vedw/nkp-kasten-catalog.git
EOF

``` 


Install the App from the App catalog

## configure k8s cluster for kasten

* get kubeconfig for workload cluster
* annotate volumesnapshotclass for kasten
  get volumesnapshotclasses

  ```
  kubectl get volumesnapshotclasses

  NAME                     DRIVER            DELETIONPOLICY   AGE
  nutanix-snapshot-class   csi.nutanix.com   Retain           26d
  ```

  ```
  kubectl annotate volumesnapshotclasses nutanix-snapshot-class "k10.kasten.io/is-snapshot-class"=true
  ``` 

* generate token for authentication

  ```
  kubectl --namespace kasten-io create token k10-k10 --duration=24h
  ```


use the generated token to access the kasten dashboard.



# fluxErrorRepo


Flux attempts to install the secret at `fails/my-secret.yaml` with error

```
NAME   AGE     READY   STATUS
demo   2m16s   False   Secret/foobar/my-secret forbidden, error: data values must be of type string...
```

The kustomization (`apply/kustomization.yaml`) valid, and the encrypted manifest is also valid:

```
$ export SOPS_AGE_KEY_FILE=$PWD/age.agekey
$ sops -d fails/my-secret.yaml
apiVersion: v1
stringData:
    foobar: drink your ovaltine
kind: Secret
metadata:
    name: my-secret
    namespace: foobar
```

## To Replicate

Flux version 0.31.1

```
minikube start
flux install
kubectl apply -f ./apply/
kubectl get kustomization -n flux-system
```


```
$ minikube start
...
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

$ flux install
✚ generating manifests
..
✔ install finished

$ kubectl apply -f ./apply/
gitrepository.source.toolkit.fluxcd.io/flux-error-repo created
kustomization.kustomize.toolkit.fluxcd.io/demo created
namespace/foobar created
serviceaccount/minikoob created
secret/sops-age created

$ kubectl get kustomization -n flux-system
NAME   AGE     READY   STATUS
demo   2m16s   False   Secret/foobar/my-secret forbidden, error: data values must be of type string...

```

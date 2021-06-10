# cert-manager

`01-cert-manager-arm.yaml` is a copy of the original [v1.3.1/cert-manager.yaml](https://github.com/jetstack/cert-manager/releases/download/v1.3.1/cert-manager.yaml) with the images modified to ARM specific images.

```
curl -sL \
https://github.com/jetstack/cert-manager/releases/download/v1.3.1/cert-manager.yaml |\
sed -r 's/(image:.*):(v.*)$/\1-arm:\2/g' > cert-manager-arm.yaml
```

`02-self-signed-issuer.yaml` creates a self-signed `ClusterIsssuer`.

Reference:

* [Make SSL certs easy with k3s](https://opensource.com/article/20/3/ssl-letsencrypt-k3s)
* [Setting Up Self-Signed HTTPS Access To Local Dev K8s Cluster in Minikube](https://itnext.io/setting-up-self-signed-https-access-to-local-dev-k8s-cluster-in-minikube-539bc62ad62f?gi=5a934e917bda
)

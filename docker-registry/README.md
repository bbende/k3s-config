# Private Docker Registry

1) Create the namespace with `01-registry-namespace.yaml`.

2) Create a basic auth credentials file:
```
htpasswd -Bc htpasswd registry
```

3) Create a secret to store the credentials file:
```
kubectl create secret generic docker-registry-htpasswd --from-file ./htpasswd --namespace docker-registry
```

4) Proceed with remaining steps through `05-registry-ingress.yaml`.

5) Edit `/etc/hosts` on laptop to map `docker.registry.private`:

```
192.168.1.244 rpi-1 longhorn-ui docker.registry.private
192.168.1.245 rpi-2
192.168.1.246 rpi-3
```

## Test Push/Pull

From laptop:
```
docker pull arm32v7/nginx
docker tag arm32v7/nginx:latest docker.registry.private/arm32v7/nginx:latest

docker login https://docker.registry.private
docker push docker.registry.private/arm32v7/nginx:latest

docker rmi arm32v7/nginx docker.registry.private/arm32v7/nginx
docker images

curl -k -X GET --basic -u registry https://docker.registry.private/v2/_catalog | python -m json.tool
```

## Configure k3s

1) Edit `/etc/hosts` on each node so that `docker.registry.private` resolves to the current node. Example from master node:
```
192.168.1.244 rpi-1 docker.registry.private
192.168.1.245 rpi-2
192.168.1.246 rpi-3
```

2) Create `/etc/rancher/k3s/registries.yaml` on master node:

```
mirrors:
  docker.registry.private:
    endpoint:
      - "https://docker.registry.private"
configs:
  "docker.registry.private":
    auth:
      username: registry
      password: xxxxxx
    tls:
      insecure_skip_verify: true
```

Replace `xxxxxx` with the password used to secure the registry.

```
sudo chmod go-r /etc/rancher/k3s/registries.yaml
sudo systemctl restart k3s
```

3) Create `/etc/rancher/k3s` directory on each worker node
```
cd /etc/rancher
sudo mkdir k3s
```

4) Create `/etc/rancher/k3s/registries.yaml` on each worker node with same content from step 2.

```
sudo chmod go-r /etc/rancher/k3s/registries.yaml
sudo systemctl restart k3s-node
```

5) Create a pod using the image previously pushed to the private registry:

```
kubectl create -f 06-registry-pod-test.yaml
```

## References

* [https://carpie.net/articles/installing-docker-registry-on-k3s](https://carpie.net/articles/installing-docker-registry-on-k3s)
* [https://itnext.io/how-to-setup-a-private-registry-on-k3s-d9283906d16](https://itnext.io/how-to-setup-a-private-registry-on-k3s-d9283906d16)

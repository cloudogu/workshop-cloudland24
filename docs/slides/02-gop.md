# Meet GOP <!-- .element style="font-size: 200%" -->
<!-- .slide: style="font-size:70%"  -->

a GitOps-based operational stack / platform

<span style="font-size: 250%">
<i class="fab fa-linux" style="color: #FFD133;"></i> 
<i class="fab fa-windows" style="color: #2279D1;"></i> 
<i class="fab fa-apple" style="color: black;"></i>
<span style="margin: 0 30px">➕</span>
<i class="fab fa-docker" style="color: #3696EA;"></i></span>

```bash
COMMIT='42e5f80' 
bash <(curl -s \
  "https://raw.githubusercontent.com/cloudogu/gitops-playground/$COMMIT/scripts/init-cluster.sh") \
  --bind-ingress-port=80 \
   && docker run --rm -t -u $(id -u) \
    -v ~/.config/k3d/kubeconfig-gitops-playground.yaml:/home/.kube/config \
    --net=host \
    ghcr.io/cloudogu/gitops-playground:$COMMIT --yes --base-url=http://localhost --ingress-nginx \
      --argocd --monitoring --vault=dev --mailhog
```

<i class="fab fa-github"></i> [cloudogu/gitops-playground](https://github.com/cloudogu/gitops-playground)



<!-- .slide: data-background-image="images/gitops-playground-local.drawio.svg" data-background-size="contain" -->



<!-- .slide: data-background-image="images/gitops-playground-features.drawio.svg" data-background-size="contain" -->



<!-- .slide: style="font-size:80%"  -->
## `scripts/init-cluster.sh`

```bash
k3d cluster create gitops-playground \
  # Mount port for ingress
  -p 80:80@server:0:direct \
  # Pin image for reproducibility
  --image=rancher/k3s:v1.29.1-k3s2 \
  # Disable built-in ingress controller, because we want to use the same one locally and in prod
  --k3s-arg=--disable=traefik@server:0 \
  # Allow node ports < 30000
  --k3s-arg=--kube-apiserver-arg=service-node-port-range=8010-65535@server:0 \
  # Hacks to make Docker available in Jenkins
  -v /var/run/docker.sock:/var/run/docker.sock@server:0 \
  -v /etc/group:/etc/group@server:0 -v /tmp:/tmp@server:0 \
  -p 30000:30000@server:0:direct

# Write kubeconfig to ~/.config/k3d/kubeconfig-gitops-playground.yaml
k3d kubeconfig write gitops-playground
```



## `docker run`
```bash
docker run
  # Remove container after running, keeping your device clean 
  # (remove in case of error to preserve logs)
  --rm 
  # Colorful output, please
  -t
  # Run as current user, so files written to /tmp are accessible for you
  -u $(id -u) \
  # Mount kubeconfig for k3d
  -v ~/.config/k3d/kubeconfig-gitops-playground.yaml:/home/.kube/config \
  # Make k3d cluster available
  --net=host \
ghcr.io/cloudogu/gitops-playground:$COMMIT #Params for image go here
```



## `ghcr.io/cloudogu/gitops-playground`


* OCI image <img data-src="images/OCI-logo.svg" class="floatRight" style="height: 2em; vertical-align: middle;" />
* Contains logic to install and configure the tools <img src="images/Groovy-logo.svg" class="floatRight" style="height: 2em; vertical-align: middle;" />
* App written in Groovy (and bash 😰) 
* Additional resources to run e.g. in air-gapped envs <img src="images/Gnu-bash-logo.svg" class="floatRight" style="height: 2em; vertical-align: middle;" />



## Your turn
<!-- .slide: style="text-align: center;" -->

<div style="margin-top: 20px">
    <img style="border-radius: 5px;" width="40%" data-src="images/hacking-cat.webp"/>
    <div style="font-size: 10%"><a href="https://giphy.com/gifs/JIX9t2j0ZTN9S">🌐 giphy.com/gifs/JIX9t2j0ZTN9S</a></div>

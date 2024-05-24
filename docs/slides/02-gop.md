<!-- .slide: style="font-size:70%"  -->
# Meet GOP <!-- .element style="font-size: 200%" -->
<span style="font-size: 250%">
<i class="fab fa-linux" style="color: #FFD133;"></i> 
<i class="fab fa-windows" style="color: #2279D1;"></i> 
<i class="fab fa-apple" style="color: black;"></i>
<span style="margin: 0 30px">➕</span>
<i class="fab fa-docker" style="color: #3696EA;"></i></span>

```bash
COMMIT='cddaaed' 
bash <(curl -s \
  "https://raw.githubusercontent.com/cloudogu/gitops-playground/$COMMIT/scripts/init-cluster.sh") \
  --bind-ingress-port=80 \
   && docker run --rm -t --pull=always -u $(id -u) \
    -v ~/.config/k3d/kubeconfig-gitops-playground.yaml:/home/.kube/config \
    --net=host \
    ghcr.io/cloudogu/gitops-playground --yes --base-url=http://localhost --ingress-nginx \
      --argocd --monitoring --vault=dev --mailhog
```

<i class="fab fa-github"></i> [cloudogu/gitops-playground](https://github.com/cloudogu/gitops-playground")



<!-- .slide: data-background-image="images/gitops-playground-local.drawio.svg" data-background-size="contain" -->



<!-- .slide: data-background-image="images/gitops-playground-features.drawio.svg" data-background-size="contain" -->

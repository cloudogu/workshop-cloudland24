<!-- .slide: id="exercise-secrets" -->
## [🏋️](#exercises) Exercise: Secrets Management  <img data-src="images/vault-logo.svg" style="height: 1.2em; vertical-align: middle;"/> <img data-src="images/eso-round-logo.svg" style="height: 1.2em; vertical-align: middle;"/> 
Integrate secrets into app, propagate updates automatically

* [Warmup](#secrets-warmup) 🤓
* [Mount secret into app](#secrets-advanced) 🚀



### Warmup 🤓
<!-- .slide: id="secrets-warmup" -->

* Secret exposed via HTTP 😅  
  <span style="font-size: 100%">🌐 <a href="http://staging.nginx-helm.nginx.localhost/secret/">staging.nginx-helm.nginx.localhost/secret</a>
* Change in Vault:  
  <span style="font-size: 80%"><img data-src="images/vault-logo.svg" style="height: 1em; vertical-align: middle;"/> <a  target="_blank"  href="http://vault.localhost/ui/vault/auth?redirect_to=%2Fvault%2Fsecrets%2Fsecret%2Fedit%2Fstaging%2Fnginx-helm-jenkins&with=userpass">vault.localhost/ui/vault/secrets/secret/edit/staging/nginx-helm-jenkins</a>
* Watch it propagate automatically  (<2 min)  
  Either reload Browser or:
```bash
while ; do  echo -n "$(date '+%Y-%m-%d %H:%M:%S'):  " ; \
   curl staging.nginx-helm.nginx.localhost/secret/  ; echo; sleep 1; done
```
Note:
* This usually takes between a couple of seconds and 1-2 minutes.  
* This time consists of `ExternalSecret`'s `refreshInterval`, as well as the [kubelet sync period](https://v1-25.docs.kubernetes.io/docs/concepts/configuration/configmap/#mounted-configmaps-are-updated-automatically)
(defaults to [1 Minute](https://kubernetes.io/docs/reference/config-api/kubelet-config.v1beta1/#kubelet-config-k8s-io-v1beta1-KubeletConfiguration))
+ cache propagation delay



#### 📽️ Warmup in time-lapse

<a href="https://user-images.githubusercontent.com/1824962/215204174-eadf180b-2a82-4273-8cbb-6e7c187267c6.mp4">
  <video controls loop data-autoplay width="80%">
    <source data-src="https://user-images.githubusercontent.com/1824962/215204174-eadf180b-2a82-4273-8cbb-6e7c187267c6.mp4" type="video/mp4">
  </video>
</a>



### Mount secret into app 🚀
<!-- .slide: id="secrets-advanced" -->

TODO summarize and link steps



<!-- .slide: style="text-align: center;" -->
### External Secrets Operator with Vault

<img data-src="images/External-Secret-Operator-Flow.svg" width="120%"/>



<!-- .slide: style="text-align: center;" data-background-image="images/External-Secret-Operator-CRs.svg" data-background-size="contain" -->



<!-- .slide: style="font-size:70%" -->
### ESO+Vault config in GOP

* SecretStore per Namespace:  
  <span style="font-size: 90%"><i><img data-src="images/Git-Icon-1788C.svg" style="height: 1.2em; vertical-align: middle;"/>  <a href="http://scmm.localhost/scm/repo/argocd/cluster-resources/code/sources/main/misc/secrets/secret-store-staging.yaml/">scmm.localhost/scm/repo/argocd/cluster-resources/code/sources/main/misc/secrets/secret-store-staging.yaml</a>
* Example External Secret:  
  <span style="font-size: 90%"><i><img data-src="images/Git-Icon-1788C.svg" style="height: 1.2em; vertical-align: middle;"/>  <a href="http://scmm.localhost/scm/repo/argocd/nginx-helm-jenkins/code/sources/main/k8s/staging/external-secret.yaml">scmm.localhost/scm/repo/argocd/nginx-helm-jenkins/code/sources/main/k8s/staging/external-secret.yaml</a>
* Mounted into app:  
  <span style="font-size: 90%"><i><img data-src="images/Git-Icon-1788C.svg" style="height: 1.2em; vertical-align: middle;"/>  <a href="http://scmm.localhost/scm/repo/argocd/nginx-helm-jenkins/code/sources/main/k8s/values-shared.yaml">scmm.localhost/scm/repo/argocd/nginx-helm-jenkins/code/sources/main/k8s/values-shared.yaml</a>



##### TODO


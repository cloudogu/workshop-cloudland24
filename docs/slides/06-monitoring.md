<!-- .slide: id="exercise-monitoring" -->
## [🏋️](#exercises) Exercise: Monitoring   <img data-src="images/prometheus-logo.svg" style="height: 1.2em; vertical-align: middle;"/> <img data-src="images/grafana.svg" style="height: 1.2em; vertical-align: middle;" /> 
Deploy a Grafana dashboard for an app using GitOps

1. [Expose metrics](#exercise-monitoring-1)
2. [Create specific Grafana dashboard JSON](#exercise-monitoring-1)
3. [Deploy dashboard via GitOps](#exercise-monitoring-3)
4. [Watch metrics](#exercise-monitoring-4)




### 1. Expose metrics
<!-- .slide: id="exercise-monitoring-1" -->

* Enable metrics export on nginx via GitOps  
   <span style="font-size: 50%"><img data-src="images/Git-Icon-1788C.svg" style="height: 1.2em; vertical-align: middle;"/>  <a href="http://scmm.localhost/scm/repo/argocd/example-apps/code/sourceext/edit/main/apps/nginx-helm-umbrella/values.yaml">scmm.localhost/scm/repo/argocd/example-apps/code/sourceext/edit/main/apps/nginx-helm-umbrella/values.yaml</a> 
```yaml
nginx:
  metrics:
    enabled: true
    serviceMonitor:
      enabled: true
#      labels:
#        release: kube-prometheus-stack
```
* Go to <span style="font-size: 55%"><img data-src="images/argo-icon.svg" style="height: 1.2em; vertical-align: middle;"/> <a href="http://argocd.localhost/applications/example-apps-production/nginx-helm-umbrella">argocd.localhost/applications/example-apps-production/nginx-helm-umbrella</a><span>, click  <button class="argo-button argo-button--base" style="margin-right: 2px;"><i class="fa fa-sync" style="margin-left: -5px; margin-right: 5px;"></i><span class="show-for-medium">Sync</span></div></button>
* Check if `servicemonitor` was created



### 2. Create specific Grafana dashboard JSON
<!-- .slide: id="exercise-monitoring-2" -->

* <img data-src="images/grafana.svg" style="height: 1.2em; vertical-align: middle;" /> [grafana.localhost/dashboard/import](http://grafana.localhost/dashboard/import)
* Paste content from here 👇️ and click <button class="css-td06pi-button"><span class="css-1riaxdn">Load</span></button>  
  <span style="font-size: 65%"><i class="fab fa-github"></i> <a href="https://github.com/nginxinc/nginx-prometheus-exporter/blob/v1.2.0/grafana/dashboard.json">github.com/nginxinc/nginx-prometheus-exporter/blob/v1.2.0/grafana/dashboard.json</a></span>
* `Name`: `nginx-helm-umbrella`
* Click `Select a Prometheus data source`: `Prometheus`
* Click <button class="css-td06pi-button"><span class="css-1riaxdn">Import</span></button>



### 3. Deploy dashboard via GitOps
<!-- .slide: id="exercise-monitoring-3" -->
<!-- .slide: style="font-size:80%" -->

* Copy JSON from <span style="font-size: 75%"><img data-src="images/grafana.svg" style="height: 1.2em; vertical-align: middle;" /> <a href="http://grafana.localhost/d/MsjffzSZz?editview=dashboard_json">grafana.localhost/d/MsjffzSZz?editview=dashboard_json</a></span>
* to <span style="font-size: 70%"><img data-src="images/Git-Icon-1788C.svg" style="height: 1.2em; vertical-align: middle;"/>  <a href="http://scmm.localhost/scm/repo/argocd/example-apps/code/sourceext/create/main/apps/nginx-helm-umbrella">scmm.localhost/scm/repo/argocd/example-apps/code/sourceext/create/main/apps/nginx-helm-umbrella</a>
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-helm-umbrella-dashboard
  labels:
    grafana_dashboard: "1"
data:
  dashboard.json: |-
    # Paste your JSON, INDENTED BY 4 SPACES here
    {...
```
* `Path`: **Add** `/templates`
* Enter `Filename`: `dashboard.yaml` + commit message, click <button type="button" class="button is-primary">Commit</button>
* Go to <span style="font-size: 75%"><img data-src="images/argo-icon.svg" style="height: 1.2em; vertical-align: middle;"/> <a href="http://argocd.localhost/applications/example-apps-production/nginx-helm-umbrella">argocd.localhost/applications/example-apps-production/nginx-helm-umbrella</a><span>, click <button class="argo-button argo-button--base" style="margin-right: 2px;"><i class="fa fa-sync" style="margin-left: -5px; margin-right: 5px;"></i><span class="show-for-medium">Sync</span></div></button>
* Check if <img data-src="images/cm.svg" style="height: 1.2em; vertical-align: middle;"/> `configmap` was created



### 4. Watch metrics
<!-- .slide: id="exercise-monitoring-4" -->

* Follow <img data-src="images/ing.svg" style="height: 1.2em; vertical-align: middle;"/> `ingress` [<i class="fa fa-external-link-alt"></i>](http://production.nginx-helm-umbrella.nginx.localhost/) link to open app in browser
* Generate traffic by <i class="fas fa-sync"></i> reloading
* Enjoy your dashboard  
  <img data-src="images/grafana.svg" style="height: 1.2em; vertical-align: middle;" /> <a href="http://grafana.localhost/d/MsjffzSZz">grafana.localhost/d/MsjffzSZz</a> 🥳

Note:
* Plain JSON  http://grafana.localhost/api/dashboards/uid/MsjffzSZz
* Click <span class="css-1riaxdn">Delete Dashboard</span> at the end of  
  http://grafana.localhost/d/MsjffzSZz/nginx-helm-umbrella?editview=settings
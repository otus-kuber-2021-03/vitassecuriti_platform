# HW 15: GitOps

gitlab URI - https://gitlab.com/vitassecuriti/microservicesdemo.git

1. Подготовил образы с тегом v0.0.1
2. Создал кластер из 4х нод а GKE с поддержкой Istio
3. Установил flux https://github.com/fluxcd/helm-operator-get-started т.к. в инструкции ссылк не рабочая
4. Обновил с учетом своего репозитория
   ```bash
   helm upgrade -i flux fluxcd/flux --wait \
   --namespace fluxcd \
   --set git.url=git@gitlab.com:vitassecuriti/microservicesdemo.git
   ```
  5. Установил helm-operator
  6. Устновил на локальный хость fluxctl
  7. получил ключ для доступа по ssh и добавил в свой профиль gitlab
  8. Добавил файл с неймспейсом microservices-demo
     ```
     ts=2021-07-23T21:53:41.339690169Z caller=sync.go:606 method=Sync cmd="kubectl apply -f -" took=946.202521ms err=null output="namespace/microservices-demo created\nnamespace/production
     ```
9.  В папку deploy/release добавил файл frontend.yaml   
10. Так как ресурс helmrelease не поднимался в в describe ресурса была ошибка
   ```
   Warning  FailedReleaseSync  18m                       helm-operator  synchronization of release 'frontend' in namespace 'microservices-demo' failed: failed to prepare chart for release: chart not ready: no existing git mirror found
  Warning  FailedReleaseSync  <invalid> (x12 over 18m)  helm-operator  synchronization of release 'frontend' in namespace 'microservices-demo' failed: installation failed: unable to build kubernetes objects from release manifest: unable to recognize "": no matches for kind "ServiceMonitor" in version "monitoring.coreos.com/v1"
  ```
  Дополнительно поставил prometheus-operator из проекта https://github.com/prometheus-operator/prometheus-operator.git
  
  ```
  $ kubectl get helmrelease -n microservices-demo
  NAME       RELEASE    PHASE       RELEASESTATUS   MESSAGE                                                                       AGE
  frontend   frontend   Succeeded   deployed        Release was successful for Helm release 'frontend' in 'microservices-demo'.   21h
  ```
11. Обновил образ на версию v0.0.2 обновил манифест frontend.yaml. Сервис перевыкатился
   ```
   $ helm history frontend -n microservices-demo
   REVISION	UPDATED                 	STATUS    	CHART          	APP VERSION	DESCRIPTION     
   1       	Sat Jul 24 20:28:15 2021	superseded	frontend-0.21.0	1.16.0     	Install complete
   2       	Sat Jul 24 20:49:18 2021	deployed  	frontend-0.21.0	1.16.0     	Upgrade complete
   
   $ helm list -n microservices-demo
   NAME    	NAMESPACE         	REVISION	UPDATED                                	STATUS  	CHART          	APP VERSION
   frontend	microservices-demo	2       	2021-07-24 20:49:18.191169468 +0000 UTC	deployed	frontend-0.21.0	1.16.0 
   ```
12. Переименовал наименование deployment
   ```
   $ kubectl logs helm-operator-654b5bf9fc-pszkg -n fluxcd | grep hipster
   ts=2021-07-24T21:01:11.81867106Z caller=helm.go:69 component=helm version=v3 info="Created a new Deployment called
   \"frontend-hipster\" in microservices-demo\n" targetNamespace=microservices-demo release=frontend
   ```
13. Добавил манифесты для остальных сервисов
   ```
   $ kubectl get pods -n microservices-demo
  NAME                                     READY   STATUS     RESTARTS   AGE
  adservice-84fb69d8cc-pss7d               1/1     Running    0          3m20s
  checkoutservice-7995988d9c-q7xqk         1/1     Running    0          3m20s
  currencyservice-765464d5f-7bp9h          1/1     Running    0          3m20s
  emailservice-7c7fd7c87c-6j585            1/1     Running    0          3m19s
  frontend-hipster-86d845f998-xfgnj        1/1     Running    0          34m
  loadgenerator-6f4489f665-rh897           1/1     Running    0          3m14s
  paymentservice-759c55f864-mhkg4          1/1     Running    0          110s
  productcatalogservice-7f5955b57c-r8mdw   1/1     Running    0          3m11s
  recommendationservice-579495b989-bmpgh   1/1     Running    0          3m10s
  shippingservice-55b8c4969c-f4vck         1/1     Running    0          3m6s

  helm list -n microservices-demo
  NAME                   	NAMESPACE         	REVISION	UPDATED                                	STATUS  	CHART                        	APP VERSION
  adservice              	microservices-demo	1       	2021-07-24 21:32:21.566002632 +0000 UTC	deployed	adservice-0.5.0              	1.16.0     
  checkoutservice        	microservices-demo	1       	2021-07-24 21:32:21.932789467 +0000 UTC	deployed	checkoutservice-0.4.0        	1.16.0     
  currencyservice        	microservices-demo	1       	2021-07-24 21:32:21.772945996 +0000 UTC	deployed	currencyservice-0.4.0        	1.16.0     
  emailservice           	microservices-demo	1       	2021-07-24 21:32:23.89093537 +0000 UTC 	deployed	emailservice-0.4.0           	1.16.0     
  frontend               	microservices-demo	3       	2021-07-24 21:01:11.496245605 +0000 UTC	deployed	frontend-0.21.0              	1.16.0     
  grafana-load-dashboards	microservices-demo	1       	2021-07-24 21:32:24.606287942 +0000 UTC	deployed	grafana-load-dashboards-0.0.3	           
  loadgenerator          	microservices-demo	1       	2021-07-24 21:32:29.00886585 +0000 UTC 	deployed	loadgenerator-0.4.0          	1.16.0     
  paymentservice         	microservices-demo	2       	2021-07-24 21:33:52.714721397 +0000 UTC	deployed	paymentservice-0.3.0         	1.16.0     
  productcatalogservice  	microservices-demo	1       	2021-07-24 21:32:31.767926349 +0000 UTC	deployed	productcatalogservice-0.3.0  	1.16.0     
  recommendationservice  	microservices-demo	1       	2021-07-24 21:32:32.787781918 +0000 UTC	deployed	recommendationservice-0.3.0  	1.16.0     
  shippingservice        	microservices-demo	1       	2021-07-24 21:32:36.722941644 +0000 UTC	deployed	shippingservice-0.3.0        	1.16.0
   ```
14. Установил Istio
15. Установил Flagger
16. Изменил ранее созданный манифест с неймспейсом, добавил label
   ```
   $ kubectl get ns microservices-demo --show-labels
   NAME                 STATUS   AGE   LABELS
   microservices-demo   Active   22h   istio-injection=enabled
   ```
17. Удалил все поды
   ```bash
   kubectl delete pods --all -n microservices-demo
   ```
18. Проверил, что в подах создались sidecar
   ```
   $kubectl describe pod -l app=frontend -n microservices-demo
containers:
  ...
  istio-proxy:
  ...
    Args:
      proxy
      sidecar
      ...
   ```
19. Добавил манифесты c VirtualService и Gateway
   ```
   kubectl get gateway -n microservices-demo
   NAME               AGE
   frontend-gateway   127m

$ kubectl get svc istio-ingressgateway -n istio-system

NAME                   TYPE           CLUSTER-IP    EXTERNAL-IP     PORT
istio-ingressgateway   LoadBalancer   10.84.14.98   35.188.27.116   15020:30301/TCP,80:31870...
   ```
20. Добавил манифестcanary.yaml
   ```
   $ kubectl get canaries -n microservices-demo 
   NAME       STATUS      WEIGHT   LASTTRANSITIONTIME
   frontend   Succeeded   0        2021-07-24T22:44:37Z
   
   $ kubectl get pods -n microservices-demo 
  NAME                                        READY   STATUS    RESTARTS   AGE
  ...
  frontend-hipster-primary-5bbf57d9cf-tx8mb   2/2     Running   0          3m
  ```
  
21.  Собрал образ с тегом v0.0.3. Имитация релиза
   ```
   kubectl describe canary frontend -n microservices-demo 
   ```
   ```
   Events:
  Type     Reason  Age              From     Message
  ----     ------  ----             ----     -------
  Warning  Synced  13m              flagger  frontend-hipster-primary.microservices-demo not ready: waiting for rollout to finish: observed deployment generation less then desired generation
  Warning  Synced  13m              flagger  frontend-hipster-primary.microservices-demo not ready: waiting for rollout to finish: 0 of 1 updated replicas are available
  Normal   Synced  12m              flagger  Initialization done! frontend.microservices-demo
  Normal   Synced  10m              flagger  New revision detected! Scaling up frontend-hipster.microservices-demo
  Normal   Synced  10m              flagger  Starting canary analysis for frontend-hipster.microservices-demo
  Normal   Synced  10m              flagger  Advance frontend.microservices-demo canary weight 5
  Normal   Synced  9m30s            flagger  Advance frontend.microservices-demo canary weight 10
  Normal   Synced  9m               flagger  Advance frontend.microservices-demo canary weight 15
  Normal   Synced  8m30s            flagger  Advance frontend.microservices-demo canary weight 20
  Normal   Synced  8m               flagger  Advance frontend.microservices-demo canary weight 25
  Normal   Synced  7m30s            flagger  Advance frontend.microservices-demo canary weight 30
  Normal   Synced  6m (x3 over 7m)  flagger  (combined from similar events): Promotion completed! Scaling down frontend-hipster.microservices-demo
  ```
  ```
  $ kubectl get canaries -n microservices-demo 
  NAME       STATUS      WEIGHT   LASTTRANSITIONTIME
  frontend   Succeeded   0        2021-07-25T00:03:45Z
  ```

    
   
 

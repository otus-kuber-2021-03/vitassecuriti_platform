```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get pods -l app=frontend
NAME             READY   STATUS    RESTARTS   AGE
frontend-crm7b   1/1     Running   0          107s
```


```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get rs
NAME       DESIRED   CURRENT   READY   AGE
frontend   1         1         1       2m15s
```
```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl scale replicaset frontend --replicas=3
replicaset.apps/frontend scaled
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get rs
NAME       DESIRED   CURRENT   READY   AGE
frontend   3         3         1       2m24s
```


```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl apply -f frontend-replicaset.yaml
replicaset.apps/frontend configured
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get rs
NAME       DESIRED   CURRENT   READY   AGE
frontend   1         1         1       3m11s
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get pods -l app=frontend
NAME             READY   STATUS        RESTARTS   AGE
frontend-crm7b   1/1     Running       0          3m14s
frontend-pkr6l   0/1     Terminating   0          54s
```


```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ nano frontend-replicaset.yaml
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl apply -f frontend-replicaset.yaml
replicaset.apps/frontend configured
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get rs
NAME       DESIRED   CURRENT   READY   AGE
frontend   3         3         3       4m20s
```


`talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ docker push vkryukov/kube-homework-front:contrl`

```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get replicaset frontend -o=jsonpath='{.spec.template.spec.containers[0].image}'
vkryukov/kube-homework-front:contrl
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get pods -l app=frontend -o=jsonpath='{.items[0:3].spec.containers[0].image}'
vkryukov/kube-homework-front:intro 
vkryukov/kube-homework-front:intro 
vkryukov/kube-homework-front:intro
```

```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl delete pods -l app=frontend
pod "frontend-64wsc" deleted
pod "frontend-cmfrk" deleted
pod "frontend-crm7b" deleted
```

```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get pods -l app=frontend -o=jsonpath='{.items[0:3].spec.containers[0].image}'
vkryukov/kube-homework-front:contrl 
vkryukov/kube-homework-front:contrl 
vkryukov/kube-homework-front:contrl
```

**ReplicaSet отслеживает состояние запушенных под управлением поды, но не следить за настройками самого пода. Под создается из шаблона при создании. Поэтому, обновление автоматом не происходит.**

```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl apply -f paymentservice-replicaset.yaml
replicaset.apps/payment created
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get pods
NAME             READY   STATUS    RESTARTS   AGE
frontend-5zpzt   1/1     Running   0          57m
frontend-8dnwr   1/1     Running   0          57m
frontend-glbbl   1/1     Running   0          57m
payment-d429w    1/1     Running   0          34s
payment-j4c4h    1/1     Running   0          34s
payment-zc6vv    1/1     Running   0          34s
```

```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl apply -f paymentservice-deployment.yaml
deployment.apps/payment created

talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get pods -l app=paymentservice -o=jsonpath='{.items[0:3].spec.containers[0].image}'
vkryukov/paymentservice:v0.0.1 
vkryukov/paymentservice:v0.0.1 
vkryukov/paymentservice:v0.0.1
```

```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl apply -f paymentservice-deployment.yaml

talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get pods -l app=paymentservice -o=jsonpath='{.items[0:3].spec.containers[0].image}'
vkryukov/paymentservice:v0.0.2 
vkryukov/paymentservice:v0.0.2 
vkryukov/paymentservice:v0.0.2
```

```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl rollout history deployment payment
deployment.apps/payment 
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
```

`kubectl rollout undo deployment payment --to-revision=1 | kubectl get rs -l app=paymentservice -w`


```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get pods -l app=paymentservice -o=jsonpath='{.items[0:3].spec.containers[0].image}'
vkryukov/paymentservice:v0.0.1 
vkryukov/paymentservice:v0.0.1 
vkryukov/paymentservice:v0.0.1
```

```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl apply -f frontend-deployment.yaml
deployment.apps/frontend configured

talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get rs
NAME                  DESIRED   CURRENT   READY   AGE
frontend-6886c487dd   3         3         3       31s
```

```
talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl apply -f node-exporter-daemonset.yaml
daemonset.apps/node-exporter created

talik@osboxes:~/vitassecuriti_platform/kubernetes-controllers$ kubectl get pods
NAME                        READY   STATUS    RESTARTS   AGE
node-exporter-962g2         1/1     Running   0          42s
node-exporter-jvc7n         1/1     Running   0          42s
node-exporter-mgwjl         1/1     Running   0          42s
node-exporter-r62n6         1/1     Running   0          42s
node-exporter-rfx9w         1/1     Running   0          42s
node-exporter-svz2c         1/1     Running   0          43s
```

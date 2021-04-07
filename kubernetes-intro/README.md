## Почему поды и контейнеры восстанавливались?
Потому что их запускал kubelet. 
**Как проверить** в случае с minikube нужно зайти на саму вм minikube
`minikube ssh`
остановить службу kubelet
`docker@minikube:~$ sudo systemctl stop kubelet`
Проверить статус
```
docker@minikube:~$ sudo systemctl status kubelet
● kubelet.service - kubelet: The Kubernetes Node Agent
     Loaded: loaded (/lib/systemd/system/kubelet.service; disabled; vendor preset: enabled)
    Drop-In: /etc/systemd/system/kubelet.service.d
             └─10-kubeadm.conf
     Active: inactive (dead)
       Docs: http://kubernetes.io/docs/
```
Удалить контейнеры или поды через kubectl
```
docker@minikube:~$ docker rm -f $(docker ps --all -q)
6e9d3c3425ef
5e3009a61a3f
92b84e72e4f7
b883e32614fa
69d12b1ebec7
c15976b20a01
dc3db12b66ce
501555801d6b
ceb9a15e844f
7da6e94132e0
e5e4395229ce
c24323ade23e
8b106c6f0d7e
2a8b43a1062f
3186be00d8b7
b8f2e79ebe5f
90930e0fed6d
55d92854b8b1
```
Проверяем через какое-то время
```
docker@minikube:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
после запуска службы контейнеры восстанавливаются

api-server запускается потому что он описан как static pod
```
docker@minikube:/etc/kubernetes/manifests$ ls
etcd.yaml  kube-apiserver.yaml  kube-controller-manager.yaml  kube-scheduler.yaml
```
И его  kubelet запускает всегда

core-dns создан как Deployment и работу контролирует replicaSet
```
talik@osboxes:~$ kubectl get rs -n kube-system -o wide
NAME                DESIRED   CURRENT   READY   AGE     CONTAINERS   IMAGES                     SELECTOR
coredns-74ff55c5b   1         1         1       3h23m   coredns      k8s.gcr.io/coredns:1.7.0   k8s-app=kube-dns,pod-template-hash=74ff55c5b
talik@osboxes:~$ kubectl get deployment -n kube-system -o wide
NAME      READY   UP-TO-DATE   AVAILABLE   AGE     CONTAINERS   IMAGES                     SELECTOR
coredns   1/1     1            1           3h24m   coredns      k8s.gcr.io/coredns:1.7.0   k8s-app=kube-dns
```

## Отчет о ДЗ

- на локальную ВМ установлен minikube
- Проведен разбор причин поднятия подов ответ выше
- Создан Dockerfile и образ на его основе docker pull vkryukov/kuber-homework
- написан и применен манифест создания пода с образом из докерфайла
- проведен анализ описания пода
- добавлен initContainer изучена информация с офсайт о инит контейнерах
- добавлены описания volume в описании контейнеров и спецификации пода
- запущен под и проверен результат, в рамках которого чере curl localhost:8000/index.htm получил страницу полученного в инит контейнере
- Склонирован репозиторий проект microservices-demo
- Создан образ на основании докерфайла docker pull vkryukov/kube-homework-front
- Создан под через ad-hoc
- на основании логов контейнера определена неисправность и в манифест добавил недостающие переменные
- новый манифест был применен, ошибок не наблюдается

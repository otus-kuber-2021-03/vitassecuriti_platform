Так как выполнение ДЗ будет не в облаке, в результате выполнения возникли ряд проблем:
- Добавить репозиторий не получается
   ```bash
      $ helm repo add stable https://kubernetes-charts.storage.googleapis.com
      Error: repo "https://kubernetes-charts.storage.googleapis.com" is no longer available; try "https://charts.helm.sh/stable" instead
   ```
   Используем рекомендованный репозиторий https://charts.helm.sh/stable
   ```bash
      helm repo add stable https://charts.helm.sh/stable
   ```                                           
- При установке nginx-ingress возникала ошибка по таймауту, flag --debug показал
   ```bash
      wait.go:225: [debug] Service does not have load balancer ingress IP address: nginx-ingress/nginx-ingress-controller
   ```
   При просмотре даннх по чарту обнаружил ссылку на код
   ```bash
      helm show chart stable/nginx-ingress
      apiVersion: v1
      appVersion: v0.34.1
      deprecated: true
      description: DEPRECATED! An nginx Ingress controller that uses ConfigMap to store the nginx configuration.
      home: https://github.com/kubernetes/ingress-nginx
      icon: https://upload.wikimedia.org/wikipedia/commons/thumb/c/c5/Nginx_logo.svg/500px-Nginx_logo.svg.png
      keywords:
      - ingress
      - nginx
      kubeVersion: '>=1.10.0-0'
      name: nginx-ingress
      sources:
      - https://github.com/kubernetes/ingress-nginx
      version: 1.41.3
   ```
   В описании был указан новый репозиторий 
   ```bash
   helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
   helm repo update
   helm install ingress-nginx ingress-nginx/ingress-nginx -n nginx-ingress
   ```
-  В ДЗ версии cert-manager указана старая версия
   ```bash
      kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.3.1/cert-manager.crds.yaml
   ```
   Для удачной установки чарта не хватало  Namespace и ClusterIssuers
   Дополнительно, нужно поставить LoadBalancer от mettalb иначе externalip не будет выдан


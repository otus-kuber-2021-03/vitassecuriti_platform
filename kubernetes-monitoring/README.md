# HW monitoring

Для выполнения ДЗ использовался оф образ nginx и kind-кластер
На его основе создан deployments
Для проверки работоспособности nginx в контейнеры помещен index.html
Создан service

Создан namespace monitoring
Задеплоен prometheus-operator
```bash
   helm upgrade --install  prometheus-operator stable/prometheus-operator -n monitoring
```

Создан servicemonitor
Для проверки работоспособности

```bash
   $ kubectl port-forward service/nginx 8080:8080
   Forwarding from 127.0.0.1:8080 -> 8080
   ....
   $ curl 127.0.0.1:8080
   <html>
   <h1> HomeWork monitoring </h1>
   <body>
     <p> Index for nginx </p>
   </body>
   </html>   
   ....
   $ kubectl port-forward service/nginx 9113:9113
   Forwarding from 127.0.0.1:9113 -> 9113   
   ....
   $ curl 127.0.0.1:9113
   <!DOCTYPE html>
   <title>NGINX Exporter</title>
   <h1>NGINX Exporter</h1>
   <p><a href="/metrics">Metrics</a></p>
   ....
   $ curl 127.0.0.1:9113/metrics
   # HELP nginx_connections_accepted Accepted client connections
   # TYPE nginx_connections_accepted counter
   nginx_connections_accepted 417
   # HELP nginx_connections_active Active client connections
   # TYPE nginx_connections_active gauge
   nginx_connections_active 1
   # HELP nginx_connections_handled Handled client connections
   # TYPE nginx_connections_handled counter
   nginx_connections_handled 417
   # HELP nginx_connections_reading Connections where NGINX is reading the request header
   # TYPE nginx_connections_reading gauge
   nginx_connections_reading 0
   # HELP nginx_connections_waiting Idle client connections
   # TYPE nginx_connections_waiting gauge
   nginx_connections_waiting 0
   # HELP nginx_connections_writing Connections where NGINX is writing the response back to the client
   # TYPE nginx_connections_writing gauge
   nginx_connections_writing 1
   # HELP nginx_http_requests_total Total http requests
   # TYPE nginx_http_requests_total counter
   nginx_http_requests_total 210
   # HELP nginx_up Status of the last metric scrape
   # TYPE nginx_up gauge
   nginx_up 1
   # HELP nginxexporter_build_info Exporter build information
   # TYPE nginxexporter_build_info gauge
   nginxexporter_build_info{commit="5f88afbd906baae02edfbab4f5715e06d88538a0",date="2021-03-22T20:16:09Z",version="0.9.0"} 1
```

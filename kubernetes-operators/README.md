### HomeWork Операторы,CustomResourceDefinition

1. Запустить minikube (minikube start)
2. Создал ветку kubernetes-operators и одноименную папку в корне проекта в созданной ветке
3. Создал папку kubernetes-operators/deploy
4. Создал манифест cr.yaml
5. Попытался применить манифест, получил ожидаемую ошибку ```error: unable to recognize "deploy/cr.yml": no matches for kind "MySQL" in version "otus.homework/v1"```
6. Создал манифест для описание ресурса deploy/crd.yaml
7. Применил оба манифеста, описание ресураса и сам ресурс создались. ```customresourcedefinition.apiextensions.k8s.io/mysqls.otus.homework created
mysql.otus.homework/mysql-instance created```
8. Проверил наличие данных о созданных сущностях 
    ```bash
       $ kubectl get crd
       NAME                   CREATED AT
       mysqls.otus.homework   2021-07-19T19:58:42Z
       $ kubectl get mysqls.otus.homework
       NAME             AGE
       mysql-instance   2m7s
       $ kubectl describe mysqls.otus.homework mysql-instance
       Name:         mysql-instance
       Namespace:    default
       ...
    ```
9.   Удалили созданный CR ```kubectl delete mysqls.otus.homework mysql-instance```
10.  В манифест deploy/crd.yaml добавил секцию validation
11.  Применил манифесты ``` customresourcedefinition.apiextensions.k8s.io/mysqls.otus.homework configured``` ```mysql.otus.homework/mysql-instance created```
12.  Добавил в манифестк deploy.yaml секции required
13.  Создали манифесты для:
- service-account.yml
- role.yml
- role-binding.yml
- deploy-operator.yml   
14. Применили манифесты 
    ```bash
       $ kubectl apply -f deploy/service-account.yml
       serviceaccount/mysql-operator created
       $ kubectl apply -f deploy/role.yml
       clusterrole.rbac.authorization.k8s.io/mysql-operator created
       $ kubectl apply -f deploy/role-binding.yml
       clusterrolebinding.rbac.authorization.k8s.io/workshop-operator created
       $ kubectl apply -f deploy/deploy-operator.yml
       deployment.apps/mysql-operator created
    ```   
15. Применил манифест deploy/cr.yaml ```$ kubectl apply -f deploy/cr.yml```
16. Проверяем радоту оператора
    ```bash
       kubectl get pvc
    ```   
        | NAME                      | STATUS | VOLUME  | CAPACITY | ACCESS MODES | STORAGECLASS |  AGE |
        |---------------------------|--------|---------|----------|--------------|--------------|------|
        | backup-mysql-instance-pvc | Bound  | pvc-... |   1Gi    |      RWO     | standard     | 112s |
        | mysql-instance-pvc        | Bound  | pvc-... |   1Gi    |      RWO     | standard     | 114s |

17. Заполнили поднятую БД
18. Проверели, что данные внесены
   ```bash
      $ kubectl exec -it $MYSQLPOD -- mysql -potuspassword -e "select * from test;" otus-database
   ```
   ```sql
        +----+-------------+
        | id | name        |
        +----+-------------+
        |  1 | some data   |
        |  2 | some data-2 |
        +----+-------------+
```    
19. Удалили cr
20. Проверяем, что зм больше нет
21. Создали заново cr
22. Проверили, что данные в бд остались

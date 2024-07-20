# Домашнее задание к занятию «Базовые объекты K8S»

### Цель задания

В тестовой среде для работы с Kubernetes, установленной в предыдущем ДЗ, необходимо развернуть Pod с приложением и подключиться к нему со своего локального компьютера. 

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключенным Git-репозиторием.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. Описание [Pod](https://kubernetes.io/docs/concepts/workloads/pods/) и примеры манифестов.
2. Описание [Service](https://kubernetes.io/docs/concepts/services-networking/service/).

------

### Задание 1. Создать Pod с именем hello-world

1. Создать манифест (yaml-конфигурацию) Pod.
2. Использовать image - gcr.io/kubernetes-e2e-test-images/echoserver:2.2.
3. Подключиться локально к Pod с помощью `kubectl port-forward` и вывести значение (curl или в браузере).

### Ответ:
[pod.yml](https://github.com/PatKolzin/kuber-1.2/blob/main/pod.yml)
```
pat@olZion:~/kuber$ kubectl apply -f pod.yml
pod/hello-world created

pat@olZion:~/kuber$ kubectl port-forward hello-world 8080:8080 --address='0.0.0.0'
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
Handling connection for 8080

pat@olZion:~$ curl --insecure localhost:8080


Hostname: hello-world

Pod Information:
        -no pod information available-

Server values:
        server_version=nginx: 1.12.2 - lua: 10010

Request Information:
        client_address=127.0.0.1
        method=GET
        real path=/
        query=
        request_version=1.1
        request_scheme=http
        request_uri=http://localhost:8080/

Request Headers:
        accept=*/*
        host=localhost:8080
        user-agent=curl/7.81.0

Request Body:
        -no body in request-


```
![hello-word](https://github.com/PatKolzin/kuber-1.2/blob/main/images/kub1.png)
------

### Задание 2. Создать Service и подключить его к Pod

1. Создать Pod с именем netology-web.
2. Использовать image — gcr.io/kubernetes-e2e-test-images/echoserver:2.2.
3. Создать Service с именем netology-svc и подключить к netology-web.
4. Подключиться локально к Service с помощью `kubectl port-forward` и вывести значение (curl или в браузере).

### Ответ:
[netology-web.yml](https://github.com/PatKolzin/kuber-1.2/blob/main/netology-web.yml)  
[netology-svc.yml](https://github.com/PatKolzin/kuber-1.2/blob/main/netology-svc.yml)  
```
pat@olZion:~/kuber$ kubectl apply -f netology-web.yml
pod/netology-web created
pat@olZion:~/kuber$ kubectl apply -f netology-svc.yml
service/netology-svc created

pat@olZion:~/kuber$ kubectl port-forward service/netology-svc 8081:80 --address='0.0.0.0'
Forwarding from 127.0.0.1:8081 -> 8080
Forwarding from [::1]:8081 -> 8080
Handling connection for 8081

pat@olZion:~/kuber$ curl -k localhost:8081


Hostname: hello-world

Pod Information:
        -no pod information available-

Server values:
        server_version=nginx: 1.12.2 - lua: 10010

Request Information:
        client_address=127.0.0.1
        method=GET
        real path=/
        query=
        request_version=1.1
        request_scheme=http
        request_uri=http://localhost:8080/

Request Headers:
        accept=*/*
        host=localhost:8081
        user-agent=curl/7.81.0

Request Body:
        -no body in request-

```
```
pat@olZion:~/kuber$ kubectl get pods
NAME           READY   STATUS    RESTARTS   AGE
hello-world    1/1     Running   0          90m
netology-web   1/1     Running   0          32m

pat@olZion:~/kuber$ kubectl get service -o wide
NAME           TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE     SELECTOR
kubernetes     ClusterIP   10.152.183.1     <none>        443/TCP   4d23h   <none>
netology-svc   ClusterIP   10.152.183.150   <none>        80/TCP    32m     app=netology

```
![service](https://github.com/PatKolzin/kuber-1.2/blob/main/images/kub2.png)
------

### Правила приёма работы

1. Домашняя работа оформляется в своем Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода команд `kubectl get pods`, а также скриншот результата подключения.
3. Репозиторий должен содержать файлы манифестов и ссылки на них в файле README.md.

------

### Критерии оценки
Зачёт — выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку — задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.

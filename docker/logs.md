### Добавим нджинкс в кластер

```shell
kubectl run nginx --image=nginx --port=80
```
### Добавим альпайн контейнер, чтобы выполнять курл на нджинкс

```shell
kubectl run alpine --image=alpine --command -- sh -c "sleep infinity"
```

### Получаем ip nginx

```shell
kubectl get pod nginx --template '{{.status.podIP}}'
```

### Подключаемся к альпайну и запускаем курл на нджинкс

```shell
k exec alpine -- sh -c "apk add --no-cache curl; while true; do curl -I 10.244.0.3; sleep 10; done"
```

### Проверим логи nginx

```shell
k logs -f nginx
```

### Получим имя nginx контейнера

```shell
sudo nsenter --target $(docker inspect -f '{{.State.Pid}}' minikube) -m -p docker ps --format '{{.Names}}' | grep -m 1 nginx
```

### Посмотрим путь до лог файла в контейнере nginx

```shell
sudo nsenter --target $(docker inspect -f '{{.State.Pid}}' minikube) -m -p docker inspect -f '{{.LogPath}}' k8s_my-nginx_my-nginx_default_9026f47c-7fb6-49ca-b3b8-1ee9aa21e834_0
```

### Читать логи

```shell
sudo nsenter --target $(docker inspect -f '{{.State.Pid}}' minikube) -m -p tail -f /var/lib/docker/containers/6ba8c93d48f45cd0d9ea6f951b7fa62ef08b0bd719471d06bb78b2622a9b14d6/6ba8c93d48f45cd0d9ea6f951b7fa62ef08b0bd719471d06bb78b2622a9b14d6-json.log
```
1. filebeat chart
2. logstash chart
3. python generate 10mb file in log
4. Check nsenter /var/log/containers/id/log/* folder with watch
filebeat -> logstash
5. check logstash stdout|file

root@548d895e470d:/# ls -l /var/log/nginx/
total 0
lrwxrwxrwx 1 root root 11 Feb  1 16:18 access.log -> /dev/stdout
lrwxrwxrwx 1 root root 11 Feb  1 16:18 error.log -> /dev/stderr


# Домашнее задание к занятию "5.3. Введение. Экосистема. Архитектура. Жизненный цикл Docker контейнера"

## Задача 1

![image](https://user-images.githubusercontent.com/95320903/173230199-164e9343-130c-4613-a81a-8f08a43d1630.png)

https://hub.docker.com/repository/docker/tedfak/nginx

## Задача 2

Посмотрите на сценарий ниже и ответьте на вопрос:
"Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.

--

Сценарий:

- Высоконагруженное монолитное java веб-приложение;
```
Нет, подойдет физический сервер потому, что не обходим доступ к ресурсам.
```
- Nodejs веб-приложение;
```
Для веб-приложений Docker подойдет. Легко разворачивать и можно масштабировать.
```
- Мобильное приложение c версиями для Android и iOS;
```
Docker не подходить, нужен графический интерфейс.
```
- Шина данных на базе Apache Kafka;
```
Подойдет желательно использовать кластер для отаказаустойчивости.
```
- Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;
```
Docker подходит для stack ELK
```
- Мониторинг-стек на базе Prometheus и Grafana;
```
Docker подойдет для использования, не хранят данные и можно масштабироватью
```
- MongoDB, как основное хранилище данных для java-приложения;
```
Использование контейнеров Docker для размещения MongoDB можно легко создавать новые изолированные экземпляры. 
```
- Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.
```
Подходит использование Docker, не требует хранение данных.
```

## Задача 3

```bash
root@server1:/home/vagrant# docker run -v /data:/data -ti centos bash
Unable to find image 'centos:latest' locally
latest: Pulling from library/centos
a1d0c7532777: Pull complete 
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
[root@4a8b3a89c4c4 /]# cd data
[root@4a8b3a89c4c4 data]# echo 'Im here to Centos' > ckeck_centos
[root@4a8b3a89c4c4 data]# ls
ckeck_centos
root@server1:/home/vagrant# docker run -v /data:/data -ti debian bash
Unable to find image 'debian:latest' locally
latest: Pulling from library/debian
e756f3fdd6a3: Pull complete 
Digest: sha256:3f1d6c17773a45c97bd8f158d665c9709d7b29ed7917ac934086ad96f92e4510
Status: Downloaded newer image for debian:latest
root@39fe3995da84:/# cd data
root@39fe3995da84:/data# echo 'Im here to Debian' > check_debian
root@server1:/home/vagrant# cd /data
root@server1:/data# vim check_host
root@server1:/home/vagrant# docker run -v /data:/data -ti debian
root@7d2acf3e288a:/# ls data
check_debian  check_host  ckeck_centos
```

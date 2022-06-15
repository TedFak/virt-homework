# Домашнее задание к занятию "5.5. Оркестрация кластером Docker контейнеров на примере Docker Swarm"


## Задача 1

Дайте письменые ответы на следующие вопросы:

- В чём отличие режимов работы сервисов в Docker Swarm кластере: replication и global?
```
Replication - запускаю одинаковые копии задачи.
Global - запускает задачу на каждой ноде.
```
- Какой алгоритм выбора лидера используется в Docker Swarm кластере?
```
Алгоритм raft. При сбое лидера любая другаю менеджер нода может заменить лидера.
```
- Что такое Overlay Network?
```
Подсеть с помощью которой контейнеры на разных нодах обмениваются информацией.
```

## Задача 2

Создать ваш первый Docker Swarm кластер в Яндекс.Облаке

Для получения зачета, вам необходимо предоставить скриншот из терминала (консоли), с выводом команды:
```
[root@node01 ~]# docker node ls
ID                            HOSTNAME             STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
o5k29ppvmw79h2bkzotav5qzx *   node01.netology.yc   Ready     Active         Leader           20.10.17
re9yqczu2mt53yxs9g1z2ooyc     node02.netology.yc   Ready     Active         Reachable        20.10.17
z9pxbld11zy4u9vdzh19viwjk     node03.netology.yc   Ready     Active         Reachable        20.10.17
bg7ap8878gc1nquxg2jgb3e10     node04.netology.yc   Ready     Active                          20.10.17
ott2274e0vi2lt7jpqlbss5pr     node05.netology.yc   Ready     Active                          20.10.17
7rd0pua9f02ghgt3y0hpikes7     node06.netology.yc   Ready     Active                          20.10.17
```

## Задача 3

Создать ваш первый, готовый к боевой эксплуатации кластер мониторинга, состоящий из стека микросервисов.

Для получения зачета, вам необходимо предоставить скриншот из терминала (консоли), с выводом команды:
```
[root@node01 ~]# docker service ls
ID             NAME                                MODE         REPLICAS   IMAGE                                          PORTS
5tw7hqrufzd9   swarm_monitoring_alertmanager       replicated   1/1        stefanprodan/swarmprom-alertmanager:v0.14.0    
4z74ginrjtdn   swarm_monitoring_caddy              replicated   1/1        stefanprodan/caddy:latest                      *:3000->3000/tcp, *:9090->9090/tcp, *:9093-9094->9093-9094/tcp
6riezoinjx25   swarm_monitoring_cadvisor           global       6/6        google/cadvisor:latest                         
yp1trj1bb7ca   swarm_monitoring_dockerd-exporter   global       6/6        stefanprodan/caddy:latest                      
ygbg0o3454us   swarm_monitoring_grafana            replicated   1/1        stefanprodan/swarmprom-grafana:5.3.4           
jy84bw0iq4h8   swarm_monitoring_node-exporter      global       6/6        stefanprodan/swarmprom-node-exporter:v0.16.0   
9bcicxyb29fd   swarm_monitoring_prometheus         replicated   1/1        stefanprodan/swarmprom-prometheus:v2.5.0       
zq6u0q6jlz3i   swarm_monitoring_unsee              replicated   1/1        cloudflare/unsee:v0.8.0
```


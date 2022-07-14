# Домашнее задание к занятию "6.5. Elasticsearch"

## Задача 1

В ответе приведите:
- текст Dockerfile манифеста
```bash
# Elasticsearch

FROM centos:7

RUN groupadd -g 1000 elasticsearch && useradd elasticsearch -u 1000 -g 1000

RUN yum update -y
RUN yum install java-11-openjdk -y
RUN yum install wget -y
RUN yum install perl-Digest-SHA -y

RUN wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.3.2-linux-x86_64.tar.gz && \
    wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.3.2-linux-x86_64.tar.gz.sha512

RUN shasum -a 512 -c elasticsearch-8.3.2-linux-x86_64.tar.gz.sha512 && \
    tar -xzf elasticsearch-8.3.2-linux-x86_64.tar.gz

WORKDIR /var/lib/elasticsearch

COPY elasticsearch.yml /var/lib/elasticsearch/config/
```
- ссылку на образ в репозитории dockerhub

https://hub.docker.com/repository/docker/tedfak/elastic

- ответ `elasticsearch` на запрос пути `/` в json виде
```bash
[elasticsearch@a22c41bb4726 elasticsearch-8.3.2]$ curl -u elastic --insecure -X GET https://localhost:9200/
Enter host password for user 'elastic':
{
  "name" : "a22c41bb4726",
  "cluster_name" : "netology_test",
  "cluster_uuid" : "K4_3QHx-SAC8MVI74zR_4A",
  "version" : {
    "number" : "8.3.2",
    "build_type" : "tar",
    "build_hash" : "8b0b1f23fbebecc3c88e4464319dea8989f374fd",
    "build_date" : "2022-07-06T15:15:15.901688194Z",
    "build_snapshot" : false,
    "lucene_version" : "9.2.0",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}
```
## Задача 2
Добавьте в `elasticsearch` 3 индекса, в соответствии со таблицей:

| Имя | Количество реплик | Количество шард |
|-----|-------------------|-----------------|
| ind-1| 0 | 1 |
| ind-2 | 1 | 2 |
| ind-3 | 2 | 4 |
```bash
[elasticsearch@d68215b80095 elasticsearch]$ curl -k --insecure -X PUT "http://localhost:9200/ind-1" -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
{"acknowledged":true,"shards_acknowledged":true,"index":"ind-1"}[elasticsearch@d68215b80095 elasticsearch]$ 
[elasticsearch@d68215b80095 elasticsearch]$ curl -k --insecure -X PUT "http://localhost:9200/ind-2" -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 2,  "number_of_replicas": 1 }}'
{"acknowledged":true,"shards_acknowledged":true,"index":"ind-2"}[elasticsearch@d68215b80095 elasticsearch]$ 
[elasticsearch@d68215b80095 elasticsearch]$ curl -k --insecure -X PUT "http://localhost:9200/ind-3" -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 4,  "number_of_replicas": 2 }}'
{"acknowledged":true,"shards_acknowledged":true,"index":"ind-3"}[elasticsearch@d68215b80095 elasticsearch]$ 
```
Получите список индексов и их статусов, используя API и **приведите в ответе** на задание.
```bash
[elasticsearch@d68215b80095 elasticsearch]$ curl -X GET "http://localhost:9200/_cat/indices?v"
health status index uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   ind-1 Hd5zL8hhTe-UoFoQHd5lNg   1   0          0            0       225b           225b
yellow open   ind-2 FFA1lVbVQ3a8V2zv2o8DhA   2   1          0            0       450b           450b
yellow open   ind-3 sSNLXOgLQH-N5yEt9UJjDA   4   2          0            0       900b           900b
```
Получите состояние кластера `elasticsearch`, используя API.
```bash
[elasticsearch@d68215b80095 elasticsearch]$ curl --insecure -X GET http://localhost:9200/_cluster/health/?pretty=true
{
  "cluster_name" : "netology_test",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 8,
  "active_shards" : 8,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 10,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 44.44444444444444
}
```
Как вы думаете, почему часть индексов и кластер находится в состоянии yellow?
```
Потому что нет реплик, только один хост и другие шарды  в состоянии unassigned.
```
Удалите все индексы.
```bash
[elasticsearch@d68215b80095 elasticsearch]$ curl --insecure -X DELETE "http://localhost:9200/ind-1?pretty"
{
  "acknowledged" : true
}
[elasticsearch@d68215b80095 elasticsearch]$ curl --insecure -X DELETE "http://localhost:9200/ind-2?pretty"
{
  "acknowledged" : true
}
[elasticsearch@d68215b80095 elasticsearch]$ curl --insecure -X DELETE "http://localhost:9200/ind-3?pretty"
{
  "acknowledged" : true
}
```
## Задача 3

Создайте директорию `{путь до корневой директории с elasticsearch в образе}/snapshots`.
```
path.repo: /var/lib/elasticsearch/snapshots добавил в elasticsearch.yml
```
Используя API [зарегистрируйте](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-register-repository.html#snapshots-register-repository) 
данную директорию как `snapshot repository` c именем `netology_backup`.

**Приведите в ответе** запрос API и результат вызова API для создания репозитория.
```bash
[elasticsearch@47c9b6d65547 elasticsearch-8.3.2]$ curl --insecure -X PUT http://localhost:9200/_snapshot/netology_backup?pretty -H 'Content-Type: application/json' -d' { "type": "fs", "settings": { "location": "/var/lib/elasticsearch/snapshots"}}'
{
  "acknowledged" : true
}
```
Создайте индекс `test` с 0 реплик и 1 шардом и **приведите в ответе** список индексов.
```bash
[elasticsearch@47c9b6d65547 elasticsearch-8.3.2]$ curl --insecure -X GET http://localhost:9200/_cat/indices?v
health status index uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   test  XU7TTK_fTJ-8qvitDIQauQ   1   0          0            0       225b           225b
```
[Создайте `snapshot`](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-take-snapshot.html) 
состояния кластера `elasticsearch`.

**Приведите в ответе** список файлов в директории со `snapshot`ами.
```bash
[elasticsearch@47c9b6d65547 elasticsearch-8.3.2]$ ll /var/lib/elasticsearch/snapshots/
total 36
-rw-rw-r-- 1 elasticsearch elasticsearch   846 Jul 14 16:25 index-0
-rw-rw-r-- 1 elasticsearch elasticsearch     8 Jul 14 16:25 index.latest
drwxrwxr-x 4 elasticsearch elasticsearch  4096 Jul 14 16:25 indices
-rw-rw-r-- 1 elasticsearch elasticsearch 18459 Jul 14 16:25 meta-7ZEwcDyjQ9e1kqgs1B9kPg.dat
-rw-rw-r-- 1 elasticsearch elasticsearch   359 Jul 14 16:25 snap-7ZEwcDyjQ9e1kqgs1B9kPg.dat
```
Удалите индекс `test` и создайте индекс `test-2`. **Приведите в ответе** список индексов.
```bash
[elasticsearch@47c9b6d65547 elasticsearch-8.3.2]$ curl --insecure -X GET http://localhost:9200/_cat/indices?v
health status index  uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   test-2 k-rB5oqfRNyeRm009Zoq5Q   1   0          0            0       225b           225b
```
[Восстановите](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-restore-snapshot.html) состояние
кластера `elasticsearch` из `snapshot`, созданного ранее. 

**Приведите в ответе** запрос к API восстановления и итоговый список индексов.
```bash
[elasticsearch@47c9b6d65547 elasticsearch-8.3.2]$ curl --insecure -X POST http://localhost:9200/_snapshot/netology_backup/elasticsearch/_restore
{"accepted":true}
```
```bash
[elasticsearch@47c9b6d65547 elasticsearch-8.3.2]$ curl --insecure -X GET http://localhost:9200/_cat/indices?v
health status index  uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   test-2 k-rB5oqfRNyeRm009Zoq5Q   1   0          0            0       225b           225b
green  open   test   g27Im0d8QNSAyfj2mbHWrg   1   0          0            0       225b           225b
```
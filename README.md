# Проектная работа "организация HighLoad стенда"

## Задача

> организация HighLoad стэнда\
> поднимаем вебпроект\
> строим нагрузочное тестирование\
> делаем скрипты для оптимизации инфраструктуры
> 1) sysctl
> 2) кластеризация веба
> 3) оптимизация, проксирование и кластеризация базы

## Наброски архитектуры

сервера:\
точка входа http и веб-прокси (2 штуки) - haproxy/keepalived или nginx\
веб (2 штуки) - приложение zabbix или glaber (pacemaker/corosync + apache или nginx+php-fpm)\
БД прокси (1 или 2 штуки) - pgbouncer\
БД кластер (4 штуки) - postgresql+patroni+etcd\
или postgresql + consul на серверах с pgbouncer

очереди - rabbitmq/NATS/kafka\
NATS:\
[https://nats-io.github.io/docs/nats_streaming/gettingstarted/run.html](https://nats-io.github.io/docs/nats_streaming/gettingstarted/run.html)\
[https://blindwarf.com/post/nats-io/](https://blindwarf.com/post/nats-io/)\
[https://habr.com/ru/post/466263/](https://habr.com/ru/post/466263/)\
[https://github.com/devfacet/natsboard](https://github.com/devfacet/natsboard)

или\
кэш - redis или memcached

При всём при этом, в zabbix (glaber) будет мониторинг всех ресурсов всех хостов + мониторинг БД + мониторинг nginx. Соответственно, при проведении тестов можно будет наглядно увидеть нагрузку на все хосты и на БД.\
Подключить в zabbix haproxy?

## Тесты

можно использовать что-нибудь из этого:\
http: яндекс.танк, [https://locust.io/](https://locust.io/), Siege\
БД: sysbench, pgbench, HammerDB\
всё: [https://jmeter.apache.org/](https://jmeter.apache.org/) (redis)

## Схема проекта

текущая схема проекта
![scheme_current.png](scheme/scheme_current.png)

целевая схема проекта, примерно, такая:
![scheme_target.png](scheme/scheme_target.png)

## Заметки

### ansible

В плейбуках ansible используются переменные, которые описаны в файле [variables](provisioning/HA/variables). Если нужно изменить имя сервера, то кроме файла variables необходимо проверить файл [hosts](provisioning/HA/hosts) или [hosts_vagrant](provisioning/HA/hosts_vagrant) (если используется vagrant) и play-файлы плейбуков на соответствие имён серверов.
В play-файлах плейбуков учтено использование разных имён серверов из разных инвентори (hosts и hosts_vagrant).

При выполнении роли [05_zabbix_createDB](provisioning/HA/05_zabbix_createDB/tasks/main.yml) происходит удаление и повторное создание БД и пользователя zabbix в postgresql, если эти объекты ранее существовали. Если этот функционал не нужен, то можно это закомментировать.

### web (zabbix)

Кластер zabbix, построенный с помощью pacemaker/corosync по-умолчанию находится в режиме active/passive и будет устойчив к сбоям, но не будет распределять нагрузку. Для распределения нагрузки необходим кластер active/active с клонированием ресурсов, которые должны быть запущены одновременно на всех нодах кластера.
Необходимо синхронизировать между соответствующими серверами каталог web-интерфейса zabbix и каталог сессий php. DRBD, glusterfs? Можно написать systemd-юнит rsync/lsyncd для периодической синхронизации.

#### pacemaker/corosync

Web-интерфейс кластера [https://hl-zabbix-vip.otus:2224](https://hl-zabbix-vip.otus:2224) или [https://10.51.21.56:2224](https://10.51.21.56:2224) (или https://имя_или_адрес_любой_ноды:2224)
Кластер работает в режиме active/passive.

Ресурсы кластера:

- cluster_vip - общий виртуальный ip-адрес, мониторится каждые 2 секунды
- zabbix-server - systemd-ресурс на основе zabbix_server.service, мониторится каждые 10 секунд
- nginx - systemd-ресурс на основе nginx.service, мониторится каждые 4 секунды
- php-fpm - systemd-ресурс на основе php-fpm.service, мониторится каждые 4 секунды

Все ресурсы кластера запускаются на одной ноде.
Кластер успешно переживает жёсткое отключение одной из нод без потери пинга.
При убийстве любого из контролируемых сервисов (ресурсов), этот ресурс успешно поднимается на той же самой ноде в течении интервала времени, указанного при создании ресурса.

После настройки кластера web-интерфейс zabbix работает на [http://hl-zabbix-vip.otus:8080/zabbix](http://hl-zabbix-vip.otus:8080/zabbix)
Дефолтные логин-пароль для доступа к web-интерфейсу zabbix ```Admin - zabbix```.

Или же сразу можно входить гостем, что и нужно будет использовать в тестах производительности
[http://hl-zabbix-vip.otus:8080/zabbix/index.php?enter=guest](http://hl-zabbix-vip.otus:8080/zabbix/index.php?enter=guest)

### postgresql/patroni

Кластер postgresql/patroni отказоустойчивый, но ему необходима очередь запросов - pgbouncer?
Подключение к БД postgresql ограничено в pg_hba.conf (patroni) только сетью 10.51.21.0/24.

### разное

ACL для haproxy от ddos

```bash
# block if 5 consecutive requests continue to come faster than 10 sess
# per second, and reset the counter as soon as the traffic slows down.
acl abuse sc0_http_req_rate gt 10
acl kill  sc0_inc_gpc0 gt 5
acl save  sc0_clr_gpc0 ge 0
tcp-request connection accept if !abuse save
tcp-request connection reject if abuse kill
```

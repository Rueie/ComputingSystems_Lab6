# Требования
* docker
* kind

# Структура
1. Nodes: (файл __k8s.yaml__)
     - control-plane (х1)
     - worker (х3)
3. Namespace: __*test*__ (файл __namespace.yaml__)
4. Pods:
     - Базы данных:
          * PostgresQL: __*shopbd*__ (файл __postgres-pod.yaml__)
          * KDB: __*keydb*__ (файл __kdb-pod.yaml__)
    - Брокеры сообщений:
        * RMQ: __*rmqbd*__ (файл __rmq-pod.yaml__)
    - Сервисы на языке GO:
        * Сервис товаров: __*productserv*__ (файл __product-pod.yaml__)
        * Сервис инвентаря: __*inventoryserv*__ (файл __inventory-pod.yaml__)
        * Сервис уведомлений: __*notificationserv*__ (файл __notification-pod.yaml__)
        * Сервис заказов: __*orderserv*__ (файл __order-pod.yaml__)
        * Сервис с GraphQL: __*graphqlserv*__ (файл __graphql-pod.yaml__)
5. Services: <br>
  Структура аналогична pod-ам, только файлы в формате __имя-*service*.yaml__.

Саму структуру и принцип работы сервисов можно посмотреть в [ЛР №4](https://github.com/Rueie/ComputingSystems_Lab4).

# Сборка
Запуск bash скрипта под названием __*scriptinitCluster*__

# Удаление кластера
```bash
sudo kind delete cluster --name test
```

# Откуда берутся images для кластера?
Образы из [ЛР №5](https://github.com/Rueie/ComputingSystems_Lab5) были залиты в docker. А именно в репозиторий [rueies](https://hub.docker.com/repositories/rueies).

# Проверка составляющих
## Проверка узлов
``` bash
sudo kubectl get nodes -n test
```
## Проверка состояния pod-ов
``` bash
sudo kubectl get pods -n test
```
## Проверка сервисов
``` bash
sudo kubectl get svc -n test
```

# Запросы
Аналогичны запросам в репозитории [ЛР №4](https://github.com/Rueie/ComputingSystems_Lab4).
<br>
__Важно__ отметить, что порты для запросов вместо семейства 8000-ых изменены на семейство 30000-ых. Этот выбор обусловлен ограничением выбора внешних портов для кубера.
<br>
Также, __ip хоста__ идентичен хосту одного из worker нодов кластера. Например, в моём случае, это 172.20.0.2-4.
<br> Для того, чтобы узнать ip воркеров используем следующие команды:
``` bash
sudo kubectl get nodes -n test
# список узлов кластера
sudo kubectl describe node <имя нода> -n test
# описание нода
# из раздела Adress забираем его IP
```
Также важно отметить, что __список товаров также был изменён с мебели на книги__, их список можно узнать в сервисе graphQL.

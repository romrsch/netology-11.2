# netology-11.2
## Домашнее задание к занятию "11.02 Микросервисы: принципы"
Вы работаете в крупной компанию, которая строит систему на основе микросервисной архитектуры. Вам как DevOps специалисту необходимо выдвинуть предложение по организации инфраструктуры, для разработки и эксплуатации.

### Задача 1: API Gateway
>Предложите решение для обеспечения реализации API Gateway. Составьте сравнительную таблицу возможностей различных программных решений. На основе таблицы сделайте выбор решения.
>Решение должно соответствовать следующим требованиям:
> * Маршрутизация запросов к нужному сервису на основе конфигурации
> * Возможность проверки аутентификационной информации в запросах
> * Обеспечение терминации HTTPS
> * Обоснуйте свой выбор.
### Ответ:
 Наиболее распространенные шаблоны шлюзов API, с которыми обычно сталкиваемся:
 
 #### Централизованный пограничный шлюз (Centralized, Edge Gateway)
 |            Критерий      	| Централизованный пограничный шлюз|Двухуровневый шлюз	| Микрошлюз| Шлюз для каждого модуля (Per-Pod)  |Sidecar Gateways and Service Mesh |
|------------------|---------|--------|--------|--------|--------|
|SSL/TLS termination								|+| +| +| +| +|
|Authentication										|+| +| +|+ | +|
|Authorization										|+| +| | |+ |
|Request routing (маршрутизация запросов)			|+| +| +| +| |
|Rate limiting (ограничение скорости)				|+| | +| +| |
|Манипулирование запросами/ответами					|+| | | | |
|Логирование 										|-| +| | +| +|
|Отслеживание инъекций 								| | +| | | +|
|Балансировка нагрузки								| |+ |+ | | +|
|							..						| Подходит для публичного предоставления служб приложений из монолитных приложений с централизованным управлением. Однако он плохо подходит для архитектур микросервисов|Hе поддерживает распределенное управление подходит для обеспечения гибкости|Подходит для отдельных команд разработки может управлять трафиком между службами сложно добиться согласованности и контроля|Обычно невелик, его конфигурация является статической. Его не нужно перенастраивать при изменении топологии приложения. |Сложность в управлении |
 
Вывод: Для микросервисной архитектуры можно применить либо микрошлюзы либо шлюз per-pod, оба варианта поддерживают маршрутизацию. Микрошлюзы при использовании микросервисов могут обеспечить автономность для каждой отдельной команды, разрабатывающей свой отдельный сервис. 

### Задача 2: Брокер сообщений
>Составьте таблицу возможностей различных брокеров сообщений. На основе таблицы сделайте обоснованный выбор решения.
>Решение должно соответствовать следующим требованиям:
>* Поддержка кластеризации для обеспечения надежности
>* Хранение сообщений на диске в процессе доставки
>* Высокая скорость работы
>* Поддержка различных форматов сообщений
>* Разделение прав доступа к различным потокам сообщений
>* Протота эксплуатации

>Обоснуйте свой выбор.

### Ответ:

|            Критерий      	| RabbitMQ	|ActiveMQ	| Qpid C++	| SwiftMQ	|Artemis	 |Apollo|
|------------------|---------|--------|--------|--------|--------|--------|
|Кластеризация											|+| +| +| +| +| -|
|Хранение сообщений										|+| +| +| +| +| +|
|Скорость работы										|+| -| -| -| -| -|
|Поддержка различных форматов							|+| -| -| -| -| -|
|Права доступа											|+| +| +| +| +| +|
|Простота эксплуатации									|+| +| -| -| +| +|

Подходящим вариантом будет использование RabbitMQ, он обеспечит надежность и гибкость маршрутизации. Если будем работать с большими нагрузками, и будет важна масштабируемость, доставка сообщений в правильном порядке и возможность просматривать историю сообщений, то подойдет Apache Kafka.

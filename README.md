# Домашнее задание к занятию "`Базы данных, их типы`" - `Заярченко Иван`


### Задание 1 СУБД

Кейс
Крупная строительная компания, которая также занимается проектированием и девелопментом, решила создать правильную архитектуру для работы с данными. Ниже представлены задачи, которые необходимо решить для каждой предметной области.

Какие типы СУБД, на ваш взгляд, лучше всего подойдут для решения этих задач и почему?

1.1. Бюджетирование проектов с дальнейшим формированием финансовых аналитических отчётов и прогнозирования рисков. СУБД должна гарантировать целостность и чёткую структуру данных.
- Лучше: PostgreSQL или Oracle – строгая структура, ACID, аналитика.

1.1.* Хеширование стало занимать длительно время, какое API можно использовать для ускорения работы?
- Можно использовать Redis для кеширования результатов хеширования.

1.2. Под каждый девелоперский проект создаётся отдельный лендинг, и все данные по лидам стекаются в CRM к маркетологам и менеджерам по продажам. Какой тип СУБД лучше использовать для лендингов и для CRM? СУБД должны быть гибкими и быстрыми.
- Лендинги: MongoDB - гибкость). CRM: PostgreSQL - надёжность.

1.2.* Можно ли эту задачу закрыть одной СУБД? И если да, то какой именно СУБД и какой реализацией?
- Можно одной СУБД: PostgreSQL.

1.3. Отдел контроля качества решил создать базу по корпоративным нормам и правилам, обучающему материалу и так далее, сформированную согласно структуре компании. СУБД должна иметь простую и понятную структуру.
- Графовая (Neo4j) — если важны связи между документами.
- Документная (MongoDB) — если структура иерархическая.
- Реляционная (PostgreSQL) — если данные строго формализованы.

1.3.* Можно ли под эту задачу использовать уже существующую СУБД из задач выше и если да, то как лучше это реализовать?
- PostgreSQL (если уже есть) или MongoDB (для иерархичных данных).

1.4. Департамент логистики нуждается в решении задач по быстрому формированию маршрутов доставки материалов по объектам и распределению курьеров по маршрутам с доставкой документов. СУБД должна уметь быстро работать со связями.
- PostgreSQL + PostGIS (маршруты) или Neo4j (связи).

1.4.* Можно ли к этой СУБД подключить отдел закупок или для них лучше сформировать свою СУБД в связке с СУБД логистов?
- Лучше отдельная схема в PostgreSQL.

1.5.* Можно ли все перечисленные выше задачи решить, используя одну СУБД? Если да, то какую именно?
- Да, PostgreSQL – поддержка JSONB.

### Задание 2. Транзакции
2.1. Пользователь пополняет баланс счёта телефона, распишите пошагово, какие действия должны произойти для того, чтобы транзакция завершилась успешно. Ориентируйтесь на шесть действий.

```

1. Аутентификация.
Пользователь проходит аутентификацию в банковском приложении, либо проходит авторизацию на сайте, вводя свой номер и данные платежных систем.
2. Проверка данных.
Система проверяет корректность данных и наличие средств на счёте пользователя.
3. Блокировка суммы на счёте.
Указанная сумма резервируется на банковском счете.
4. Передача информации оператору о транзакции на указанный номер абонента.
Банковская система отправляет данные платежа оператору сотовой связи.
5. Зачисление на телефон.
Оператор связи сверяет информацию о поступившем платеже с действующим номером в своей БД и если данные совпадают, начисляет средства на счет абонента.
6. Завершение транзакции и уведомление пользователя.
В случае успешной транзакции абоненту отправляется уведомление. 
```

2.1.* Какие действия должны произойти, если пополнение счёта телефона происходило бы через автоплатёж?

```
1. Авторизация по расписанию.
2. Автосписание (если хватает денег).
3. Уведомление. При ошибке – повтор или отмена.
```

### Задание 3. SQL vs NoSQL
3.1. Напишите пять преимуществ SQL-систем по отношению к NoSQL.
Чёткая схема данных.
```
1. ACID-гарантии.
2. Сложные запросы (JOIN).
3. Стандартный SQL.
4. Аналитика (оконные функции).
```

3.1.* Какие, на ваш взгляд, преимущества у NewSQL систем перед SQL и NoSQL.
- ACID + масштабируемость. Лучше SQL для кластеров, лучше NoSQL для транзакций.

### Задание 4. Кластеры
Необходимо производить большое количество вычислений при работе с огромным количеством данных, под эту задачу выделено 1000 машин.
На основе какого критерия будете выбирать тип СУБД и какая модель распределённых вычислений здесь справится лучше всего и почему?

Для обработки данных 1000 машин нужно определить необходимые данные и операции. 
Структурированные данные с сложным анализом лучше обрабатывать аналитическими системами, такими как ClickHouse или Druid.  
Неструктурированные данные можно хранить в NoSQL-системах, например, Cassandra.  
Для вычислений оптимален Apache Spark с высокой масштабируемостью и поддержкой SQL и машинного обучения.

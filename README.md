# Домашнее задание к занятию "11-01 Базы данных, их типы" - Zhukov Semen

---

### Задание 1. СУБД

Кейс
Крупная строительная компания, которая также занимается проектированием и девелопментом, решила создать правильную архитектуру для работы с данными. Ниже представлены задачи, которые необходимо решить для каждой предметной области.

Какие типы СУБД, на ваш взгляд, лучше всего подойдут для решения этих задач и почему?

1.1. Бюджетирование проектов с дальнейшим формированием финансовых аналитических отчётов и прогнозирования рисков. СУБД должна гарантировать целостность и чёткую структуру данных.

- Традиционно для финансовых систем используют реляционные СУБД, такие как PostgreSQL или MySQL. Они обеспечивают строгую структуру, целостность данных, сложные запросы и аналитику.

1.1.\* Хеширование стало занимать длительно время, какое API можно использовать для ускорения работы?

- Если речь о хеш-индексах в БД, то возможно стоит перейти на использование in-memory баз данных или кэширования через Redis. Либо использовать API для распределённых вычислений.

1.2. Под каждый девелоперский проект создаётся отдельный лендинг, и все данные по лидам стекаются в CRM к маркетологам и менеджерам по продажам. Какой тип СУБД лучше использовать для лендингов и для CRM? СУБД должны быть гибкими и быстрыми.

-Для лендингов, где данные по лидам должны быть гибкими и быстрыми, возможно, подойдёт NoSQL база, MongoDB позволяет гибко изменять структуру данных. Для CRM тоже подойдёт та же MongoDB или другая документная БД. Документная NoSQL обеспечивает высокую производительность при записи/чтении.

1.2.\* Можно ли эту задачу закрыть одной СУБД? И если да, то какой именно СУБД и какой реализацией?

-Если CRM и лендинги могут работать с документной моделью, то подойдёт MongoDB.

1.3. Отдел контроля качества решил создать базу по корпоративным нормам и правилам, обучающему материалу и так далее, сформированную согласно структуре компании. СУБД должна иметь простую и понятную структуру.

-Тут важна простая структура, возможно подошла бы , иерархическая или документная. Подойдёт так же и реляционная БД, если данные структурированы.

- Реляционная (PostgreSQL) или документная (MongoDB).

1.3.\* Можно ли под эту задачу использовать уже существующую СУБД из задач выше и если да, то как лучше это реализовать?

-Если уже выбрана PostgreSQL для бюджетирования, можно создать отдельную схему или таблицы для хранения корпоративных норм. Либо, если используется MongoDB, создать отдельную коллекцию. Главное что бы была изоляция данных и управление доступом.

1.4. Департамент логистики нуждается в решении задач по быстрому формированию маршрутов доставки материалов по объектам и распределению курьеров по маршрутам с доставкой документов. СУБД должна уметь быстро работать со связями.

-Тут важны связи между объектами, возможно, графовая БД, так как маршруты и связи между точками можно эффективно моделировать графами (маршруты → точки доставки → курьеры). Позволяет быстро находить кратчайшие пути и анализировать зависимости.

1.4.\* Можно ли к этой СУБД подключить отдел закупок или для них лучше сформировать свою СУБД в связке с СУБД логистов?

- Если логистика использует графовую БД, отдел закупок может использовать ту же, если их данные связаны с маршрутами и поставщиками. Либо использовать отдельную БД, но интегрированную через API.

1.5.\* Можно ли все перечисленные выше задачи решить, используя одну СУБД? Если да, то какую именно?

-Скорее всего, нет, так как разные задачи требуют разных моделей данных. Если возможно мириться с компромиссами, то PostgreSQL с разными подходами (реляционные таблицы, JSONB для гибких данных, графовые расширения) может покрыть большинство задач.

---

### Задание 2. Транзакции

2.1. Пользователь пополняет баланс счёта телефона, распишите пошагово, какие действия должны произойти для того, чтобы транзакция завершилась успешно. Ориентируйтесь на шесть действий.

<ins>Шаги успешного пополнения баланса телефона.</ins>

_Инициация транзакции:_ - Пользователь вводит номер телефона, сумму пополнения и подтверждает операцию.

_Валидация данных:_ - Проверка формата номера телефона. - Проверка доступности сервиса пополнения для указанного оператора связи.

_Проверка платёжного средства:_ - Авторизация карты/кошелька. - Подтверждение достаточности средств на счёте пользователя.

_Списание средств:_ - Блокировка суммы на счёте пользователя. - Передача запроса на зачисление платежа оператору связи.

_Зачисление средств:_ - Оператор связи подтверждает пополнение баланса. - Средства разблокируются у пользователя и окончательно списываются.

_Фиксация транзакции:_ - Запись операции в историю платежей пользователя. - Отправка чека/уведомления (SMS, email, push) об успешном пополнении.

2.1.\* Какие действия должны произойти, если пополнение счёта телефона происходило бы через автоплатёж?

- Для автоплатежа процесс похож, но добавляется триггер по расписанию или условию. Пользователь заранее настраивает автоплатеж: указывает сумму, периодичность, источник оплаты.

---

### Задание 3. SQL vs NoSQL

3.1. Напишите пять преимуществ SQL-систем по отношению к NoSQL.

    1. Первое это структурированность данных. SQL базы используют строгую схему, что обеспечивает целостность данных.
    2. Второе — поддержка ACID.
    3. Мощные возможности запросов с помощью SQL.
    4. Зрелость и экосистема. SQL системы существуют давно, много инструментов для мониторинга, резервного копирования, интеграции с BI.
    5. Tранзакции на уровне строк. Это позволяет обрабатывать конкурентные операции без потери данных.

3.1.\* Какие, на ваш взгляд, преимущества у NewSQL систем перед SQL и NoSQL.

    - Первое преимущество NewSQL — горизонтальная масштабируемость без потери ACID. Традиционные SQL плохо масштабируются горизонтально, а NoSQL жертвует ACID. NewSQL решает это.
    - Второе — поддержка распределенных транзакций. В классических SQL это сложно, в NoSQL часто отсутствует. NewSQL обеспечивает согласованность в распределенных системах.
    - Третье — совместимость с SQL.
    - Четвертое — высокая производительность при больших нагрузках.

---

### Задание 4. Кластеры

Необходимо производить большое количество вычислений при работе с огромным количеством данных, под эту задачу выделено 1000 машин.

На основе какого критерия будете выбирать тип СУБД и какая модель распределённых вычислений здесь справится лучше всего и почему?

- Kлючевые критерии — масштабируемость, производительность, отказоустойчивость и эффективность распределения нагрузки. Возможно, больше подойдут NoSQL или NewSQL, так как они лучше масштабируются горизонтально.

Смотря какие вычисления производятся. Если это аналитика, то MPP или Spark. Если онлайн-транзакции с высокой нагрузкой, то подойдёт шардинг в NoSQL или NewSQL.

---

# Мониторинг состояния кластера и хостов

Вы можете отслеживать состояние кластера {{ mms-name }} и отдельных его хостов с помощью:

* инструментов мониторинга в консоли управления на странице кластера;
* в сервисе [{{ monitoring-full-name }}](../../monitoring/).

Они предоставляют диагностическую информацию в виде графиков, обновляющихся каждые {{ graph-update }}.

## Мониторинг состояния кластера {#monitoring-cluster}

Для просмотра детальной информации о состоянии кластера {{ mms-name }}:

1. Перейдите на страницу каталога и выберите сервис **{{ mms-name }}**.
1. Нажмите на имя нужного кластера и выберите вкладку **Мониторинг**.

На странице появятся следующие графики:

* **Is alive [bool]** — доступность кластера в виде суммы состояний его хостов.

    Каждый хост в состоянии **Alive** увеличивает общую доступность на 1.

* **Active Transactions [count]** — количество активных транзакций в кластере.

* **Transactions/sec on primary** — среднее количество транзакций, выполненных на первичной реплике за одну секунду.

* **Disk capacity on primary, [bytes]** — емкость диска на первичной реплике (в байтах):

    * **Used Space** — использованное дисковое пространство.
    * **Available Space** — свободное дисковое пространство.

* **Is Primary [bool]** — какой хост является первичной репликой и как долго.

* **Lazy Writes/sec on primary** — скорость физической записи на первичной реплике. {{ MS }} разделяет запись данных для повышения эффективности работы с хранилищем:

    * _Логическая запись_ — изменение данных в RAM.
    * _Физическая запись_ — запись измененных страниц из RAM в хранилище.

    Подробнее см. в [документации {{ MS }}](https://docs.microsoft.com/en-us/sql/relational-databases/writing-pages).

* **User connections** — количество подключений к кластеру.

    Несколько подключений всегда будут активны. Они используются самим кластером и службами {{ yandex-cloud }}, отвечающими за мониторинг его состояния.

* **Page Life Expectancy [sec]** — время жизни страниц памяти (в секундах) до записи в хранилище.

    Чем оно больше, тем эффективнее используется буфер, и тем реже кластеру приходится обращаться к хранилищу для выборки нужных данных.

* **CPU** — процентное соотношение режимов работы процессора во времени:

    * **precent_user_time** — пользовательский режим.
    * **percent_processor_time** — суммарное время работы процессора под нагрузкой (не в режиме ожидания).
    * **percent_privileged_time** — режим ядра.
    * **precent_interrupt_time** — обработка прерываний.
    * **precent_idle_time** — режим ожидания.

* **Batch Requests/sec** — количество пакетных операций, выполненных на каждом хосте за 1 секунду.

    Подробнее о пакетных операциях см. в [документации {{ MS }}](https://docs.microsoft.com/en-us/sql/odbc/reference/develop-app/batches-of-sql-statements).

* **Memory Grants Pending on primary** — количество запросов, ожидающих выделения RAM.

    Подробнее см. в [документации {{ MS }}](https://docs.microsoft.com/en-us/sql/relational-databases/memory-management-architecture-guide).

## Мониторинг состояния хостов {#monitoring-hosts}

Для просмотра детальной информации о состоянии отдельных хостов {{ mms-name }}:

1. Перейдите на страницу каталога и выберите сервис **{{ mms-name }}**.
1. Нажмите на имя нужного кластера и выберите вкладку **Хосты** → **Мониторинги**.
1. Выберите нужный хост из выпадающего списка.

На этой странице выводятся графики:

* **Active Transactions** — количество активных транзакций для каждой базы данных.

* **SQL Errors [count]** — количество ошибок при обработке SQL-запросов:

    * **User_Errors** — ошибки пользователей.
    * **Kill_Connection_Errors** — серьезные ошибки, которые привели к закрытию соединения.
    * **Info_Errors** — количество информационных событий, связанных с ошибками.
    * **DB_Offline_Errors** — ошибки, связанные с недоступностью баз данных.
    * **Total** — общее количество ошибок на хосте.

* **Page Life Expectancy** — время жизни страниц памяти до сброса в хранилище (в секундах).

    Чем больше значение этой характеристики, тем эффективнее используется буфер, и тем реже кластеру приходится обращаться к хранилищу для чтения или записи нужных данных.

* **Packets send/received** — количество обработанных сетевых пакетов:

    * **packets_received_discarded** — количество отброшенных пакетов.

        Пакеты отбрасываются по следующим причинам:

        * ошибки при передаче;
        * ошибки форматирования;
        * недостаток места в хранилище.

        Хотя отбрасывание некоторых пакетов неизбежно, растущее значение этой характеристики может указывать на проблемы в работе кластера.

    * **packets_outbound_errors** — количество ошибок при отправке пакетов.
    * **packets_received_errors** — количество ошибок при получении пакетов.
    * **packets_received_persec** — среднее количество пакетов, отправленных за одну секунду.
    * **packets_sent_persec** — среднее количество пакетов, принятых за одну секунду.

* **Bytes send/received** — среднее количество байт за одну секунду:

    * **bytes_received_persec** — полученных из сети;
    * **bytes_sent_persec** — отправленных в сеть.

* **User connections** — среднее количество подключений к кластеру.

    Несколько подключений всегда будут активны. Они используются самим кластером и службами {{ yandex-cloud }}, отвечающими за мониторинг его состояния.

* **Disk read/write time** — среднее время записи данных в хранилище или чтения из него (в миллисекундах).

* **Space used/available** — степень использования хранилища хоста (в байтах).

* **Disk bytes** — средний объем данных, записанных в хранилище и прочитанных из него (в байтах).

* **CPU (processor time)** — использование vCPU (в процентах):

    * **Interrupt Time** — обработка прерываний.
    * **User Time** — работа в пользовательском режиме.
    * **Privileged Time** — работа в режиме ядра.

* **Memory Grants Pending** — количество ждущих выделения RAM запросов на хосте.

* **Disk Latency** — задержка при работе с хранилищем (в миллисекундах):

    * **avg._disk_sec/transfer** — среднее время операций ввода-вывода.
    * **avg._disk_sec/write** — среднее время записи.
    * **avg._disk_sec/read** — среднее время чтения.

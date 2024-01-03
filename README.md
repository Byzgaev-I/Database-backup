# Домашнее задание к занятию "`«Резервное копирование баз данных»`" - `Бызгаев Александр`

---

### Задание 1. Резервное копирование

**Кейс**  

**Финансовая компания решила увеличить надёжность работы баз данных и их резервного копирования.**
**Необходимо описать, какие варианты резервного копирования подходят в случаях:**    
 **-1.1. Необходимо восстанавливать данные в полном объёме за предыдущий день.**    
 **-1.2. Необходимо восстанавливать данные за час до предполагаемой поломки.**  
 **-1.3***.**Возможен ли кейс, когда при поломке базы происходило моментальное переключение на работающую или починенную базу данных.**    
**Приведите ответ в свободной форме.**

### Решение:

Для увеличения надежности работы баз данных и их резервного копирования финансовой компании следует организовать систему бэкапов и репликаций, которая позволит восстанавливать данные в соответствии с требованиями именно ее бизнеса. Как было сказано на вебинаре под каждый вид модели бизнеса свои порой схемы резервирования данных.    
Рассмотрим каждый из кейсов:

**1.1. Восстановление данных в полном объеме за предыдущий день.**  
Для такого сценария подойдет ежедневное полное резервное копирование (full backup). Все данные базы данных копируются в конце дня и хранятся на отдельных носителях или в удаленном хранилище.  
В случае сбоя, данные за весь предыдущий день можно восстановить, используя последний полный бэкап.

**1.2. Восстановление данных за час до предполагаемой поломки.**  
Для минимизации потери данных при сбое требуется более частое резервное копирование. Один из подходов — инкрементное резервное копирование (incremental backup).   
После ежедневного полного бэкапа, каждый час происходит копирование только тех данных, которые изменились с момента последнего бэкапа.   
Это уменьшает объем хранимых данных и время на восстановление, но при этом позволяет восстановить базу данных почти до момента сбоя.  

Альтернативой может быть непрерывное резервное копирование (continuous data protection), где каждая транзакция или изменение в базе данных тут же реплицируется на резервный сервер.   
Это обеспечивает возможность восстановления данных почти в реальном времени.

**1.3*** **.Моментальное переключение на работающую или починенную базу данных.**  
Для реализации моментального переключения на работающую базу данных в случае сбоя используются стратегии высокой доступности, такие как:

Репликация в реальном времени: синхронная или асинхронная репликация изменений на вторичный сервер базы данных обеспечивает наличие актуальной копии данных, готовой к переключению в случае сбоя основного сервера.

Кластеризация баз данных: группа серверов работает вместе так, что если один из них выходит из строя, другой автоматически берет на себя его функции. Это может быть реализовано через технологии, такие как Microsoft SQL Server Always On Availability Groups, Oracle Real Application Clusters (RAC) и другие.

Отказоустойчивые системы (fault-tolerant systems): системы, спроектированные так, чтобы при сбое одного компонента, другой компонент мгновенно заменял его без потери работы.

Системы распределённых баз данных: данные распределяются между несколькими узлами, которые могут находиться в разных географических локациях. Это обеспечивает географическое распределение рисков и возможность переключения на работающий узел при сбое другого.

Применение каждого из этих подходов зависит от требований бизнеса к доступности данных, времени на восстановление после сбоя (Recovery Time Objective, RTO) и допустимому объему потери данных (Recovery Point Objective, RPO), а также от бюджета, выделенного на обеспечение надежности баз данных.

Для ответа на 1.3* перелопатил инет и нашел похожие сценарии, которые мне показались похожими на правду. 


### Задание 2. PostgreSQL

**2.1. С помощью официальной документации приведите пример команды резервирования данных и восстановления БД (pgdump/pgrestore).**      
**2.1.*** **Возможно ли автоматизировать этот процесс? Если да, то как?**    
**Приведите ответ в свободной форме.**

### Решение:

**2.1. Резервирование данных и восстановление БД в PostgreSQL.**   

В PostgreSQL для резервного копирования данных используется утилита pg_dump, а для восстановления — pg_restore (для бинарных бэкапов) или psql (для SQL-форматированных бэкапов). Вот примеры команд:

Резервирование данных:

bash
pg_dump -U username -W -F c -f "/path_to_backup/backup_file.dump" dbname
-U username — имя пользователя, под которым будет выполнена команда.
-W — запрашивает пароль перед подключением к базе данных.
-F c — указывает формат бэкапа (в данном случае, custom).
-f — путь и имя файла для сохранения дампа.
dbname — имя базы данных, которую нужно скопировать.
Восстановление БД:

bash

pg_restore -U username -d dbname -W -F c -C "/path_to_backup/backup_file.dump"
-U username — имя пользователя для подключения к базе данных.
-d dbname — имя целевой базы данных, в которую будет восстановлен дамп.
-W — запрашивает пароль пользователя.
-F c — указывает формат бэкапа (custom).
-C — создает базу данных перед восстановлением (если это необходимо).
"/path_to_backup/backup_file.dump" — путь к файлу дампа.

**2.1.*** **Автоматизация процесса резервного копирования и восстановления.**  

Автоматизировать процесс резервного копирования и восстановления данных в PostgreSQL можно несколькими способами, вот некоторые из них:

Cron задания (Linux/Unix): Используя системный планировщик cron, можно настроить периодическое выполнение команды pg_dump для резервного копирования. Например, добавление следующей строки в crontab (crontab -e) позволит выполнять бэкап каждый день в полночь:
bash

0 0 * * * /usr/bin/pg_dump -U username -F c -f "/path_to_backup/backup_$(date +\%Y-\%m-\%d).dump" dbname
Скрипты оболочки: Можно написать скрипт на Bash или другом языке командной строки, который будет выполнять pg_dump и pg_restore, а затем настроить cron для его выполнения.

Использование PostgreSQL Continuous Archiving: PostgreSQL позволяет настроить непрерывное архивирование (Continuous Archiving) и точки восстановления (Point-In-Time Recovery, PITR), что является частью более комплексной стратегии бэкапа и восстановления.

Использование специализированного программного обеспечения: Существуют специализированные решения для бэкапа PostgreSQL, такие как Barman, pgBackRest и другие, которые предоставляют расширенные возможности управления резервными копиями и автоматизации.

Интеграция с облачными сервисами: Если база данных размещена в облаке (например, на AWS, Azure, GCP), то можно использовать инструменты и сервисы облака для автоматизации резервного копирования и восстановления.

При автоматизации важно также предусмотреть мониторинг успешности выполнения задачи резервного копирования, а также регулярно тестировать процесс восстановления, чтобы удостовериться в том, что данные могут быть восстановлены в случае необходимости.





























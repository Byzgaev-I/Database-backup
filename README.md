# Домашнее задание к занятию "`«Резервное копирование баз данных»`" - `Бызгаев Александр`

---

### Задание 1. Резервное копирование

Кейс  

Финансовая компания решила увеличить надёжность работы баз данных и их резервного копирования.    
Необходимо описать, какие варианты резервного копирования подходят в случаях:    
 - 1.1. Необходимо восстанавливать данные в полном объёме за предыдущий день.    
 - 1.2. Необходимо восстанавливать данные за час до предполагаемой поломки.    
 - 1.3.* Возможен ли кейс, когда при поломке базы происходило моментальное переключение на работающую или починенную базу данных.    
Приведите ответ в свободной форме.



Для увеличения надежности работы баз данных и их резервного копирования финансовой компании следует организовать систему бэкапов и репликаций, которая позволит восстанавливать данные в соответствии с требованиями бизнеса. Рассмотрим каждый из кейсов:

1.1. Восстановление данных в полном объеме за предыдущий день
Для такого сценария подойдет ежедневное полное резервное копирование (full backup). Все данные базы данных копируются в конце дня и хранятся на отдельных носителях или в удаленном хранилище. В случае сбоя, данные за весь предыдущий день можно восстановить, используя последний полный бэкап.

1.2. Восстановление данных за час до предполагаемой поломки
Для минимизации потери данных при сбое требуется более частое резервное копирование. Один из подходов — инкрементное резервное копирование (incremental backup). После ежедневного полного бэкапа, каждый час происходит копирование только тех данных, которые изменились с момента последнего бэкапа. Это уменьшает объем хранимых данных и время на восстановление, но при этом позволяет восстановить базу данных почти до момента сбоя.

Альтернативой может быть непрерывное резервное копирование (continuous data protection), где каждая транзакция или изменение в базе данных тут же реплицируется на резервный сервер. Это обеспечивает возможность восстановления данных почти в реальном времени.

1.3. Моментальное переключение на работающую или починенную базу данных
Для реализации моментального переключения на работающую базу данных в случае сбоя используются стратегии высокой доступности, такие как:

Репликация в реальном времени: синхронная или асинхронная репликация изменений на вторичный сервер базы данных обеспечивает наличие актуальной копии данных, готовой к переключению в случае сбоя основного сервера.

Кластеризация баз данных: группа серверов работает вместе так, что если один из них выходит из строя, другой автоматически берет на себя его функции. Это может быть реализовано через технологии, такие как Microsoft SQL Server Always On Availability Groups, Oracle Real Application Clusters (RAC) и другие.

Отказоустойчивые системы (fault-tolerant systems): системы, спроектированные так, чтобы при сбое одного компонента, другой компонент мгновенно заменял его без потери работы.

Системы распределённых баз данных: данные распределяются между несколькими узлами, которые могут находиться в разных географических локациях. Это обеспечивает географическое распределение рисков и возможность переключения на работающий узел при сбое другого.

Применение каждого из этих подходов зависит от требований бизнеса к доступности данных, времени на восстановление после сбоя (Recovery Time Objective, RTO) и допустимому объему потери данных (Recovery Point Objective, RPO), а также от бюджета, выделенного на обеспечение надежности баз данных.

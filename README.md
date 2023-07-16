
# Домашнее задание к занятию "12.6 «Репликация и масштабирование. Часть 1»" - Соловьёв Андрей SYS-18

---

Работа была выполнена на виртуальной машине с Linux Lite 6.4 



## Задание 1


На лекции рассматривались режимы репликации master-slave, master-master, опишите их различия.


- 1. Репликация Master-Slave

Плюсы

Аналитические приложения могут считывать данные с подчиненных устройств, не влияя на главный.

Резервное копирование всей базы данных относительно не влияет на мастер.

Подчиненные устройства могут быть переведены в автономный режим и синхронизированы с ведущим устройством без каких-либо простоев.

Минусы

В случае неудачи подчиненный должен быть повышен до ведущего, чтобы занять его место. Отсутствие автоматического переключения на другой ресурс.

Время простоя и, возможно, потеря данных при сбое master

Все записи также должны быть сделаны ведущему устройству в формате ведущий-ведомый

Каждое дополнительное ведомое устройство добавляет некоторую нагрузку на ведущее устройство, поскольку двоичный журнал должен быть прочитан, а данные скопированы на каждое ведомое устройство

Возможно, потребуется перезапустить приложение

- 2. Репликация Master-Master

Плюсы

Приложения могут считывать данные с обоих основных устройств

Распределяет нагрузку на запись между обоими главными узлами

Простой, автоматический и быстрый переход на другой ресурс

Минусы

Слабо согласовано

Настроить и развернуть не так просто, как master-slave

Для репликации мастер-мастер (двусторонняя репликация) и так же при настройки “Master-Slave” с точностью до наоборот (основной Master настраивается как Slave, а второй (бывший Slave) - как Мастер). Если на втором мастере не будет производиться запись (”пассивный” мастер), то изменение Autoincrement-increment не потребуется. При репликации двух активных master-master следует обратить внимание на Autoincrement. Действительно, если на двух серверах одновременно будет создана запись с одинаковым первичным ключом, то при попытке репликации получим ошибку Dublicate entry.

## Задание 2

Выполните конфигурацию master-slave репликации, примером можно пользоваться из лекции.

Использован образ mysql:8.0.33-debian

![docker.png](https://github.com/Andrewsolo1969/12-6-hw/blob/main//img/docker.png)


Скриншот Master

![master_conf.png](https://github.com/Andrewsolo1969/12-6-hw/blob/main//img/master_conf.png)


![master_status_1.png](https://github.com/Andrewsolo1969/12-6-hw/blob/main//img/master_status_1.png)

Скриншот Slave


![slave_conf.png](https://github.com/Andrewsolo1969/12-6-hw/blob/main//img/slave_conf.png)

![slave_status.png](https://github.com/Andrewsolo1969/12-6-hw/blob/main//img/slave_status.png)



Master-Slave status

![master_slave_status.png](https://github.com/Andrewsolo1969/12-6-hw/blob/main//img/master_slave_status.png)


Создание базы andrew на Master и работа репликации - появление базы andreww на Slave

![create_andrew.png](https://github.com/Andrewsolo1969/12-6-hw/blob/main//img/create_andrew.png)


Drop базы andrew на Master и работа репликации - отсутствие базы andreww на Slave

![drop_andrew.png](https://github.com/Andrewsolo1969/12-6-hw/blob/main//img/drop_andrew.png)


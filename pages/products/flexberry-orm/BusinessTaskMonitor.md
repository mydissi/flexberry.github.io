---
title: Монитор задач
sidebar: product--sidebar
keywords: Flexberry ORM, Public
toc: true
permalink: ru/business-task-monitor.html
folder: product--folder
lang: ru
---

<div style="margin:5px; padding-left:28px; float:right; width:40%; outline:1px solid white;">
<br>
<table border="0" width="100%" bgcolor="#6495ED">
<tbody><tr><td bgcolor="#FFFFFF">
* '''Продукт''': [Flexberry ORM](flexberry-o-r-m.html)
* '''Предназначение:''' Отслеживание задач (вызовов некоторых методов), наблюдение за процессом работы где-либо (в отдельном окне, в лог-файле). Отслеживание SQL-запросов [сервиса данных](data-service.html).
* '''Программная библиотека:''' ICSSoft.STORMNET.Business.dll
</td>
</tr></tbody></table></a>
</div>

# Описание
Бывает удобным подключить к приложению некоторый монитор задач, когда можно отслеживать задачи (вызовы некоторых методов), наблюдая весь процесс работы где-либо (в отдельном окне, в лог-файле). Примером может служить мониторинг SQL-запросов, которые выполняют сервисы данных в хранилище.


В связи с мониторингом существует два вопроса, которые прикладной разработчик должен решить:

# Какой монитор задач должен предоставляться (выбор стандартного монитора задач или создание какого-то собственного);
# Подключение монитора задач непосредственно к системе.

# Подключение монитора задач к приложению
Для того, чтобы подключить любой монитор задач, необходимо выполнить одно из следующего:
# В коде проинициализировать статическое свойство `ICSSoft.STORMNET.Business.BusinessTaskMonitor.TaskMonitor` экземпляром класса — монитора задач.
# Указать в настройках `config`-файла приложения настройку `BusinessTaskMonitorType` — тип класса, который собственно и является монитором задач. Это основной способ подключения монитора.

Пример: подключение `EventTaskMon` из вышеприведённого примера к приложению через настройку `config`-файла:
Содержимое конфигурационного файла:
```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <appSettings>
    <add key="BusinessTaskMonitorType" value="CustomTaskMon.EventTaskMon, CustomTaskMon, Version=1.0.0.1, Culture=neutral, PublicKeyToken=null"/>
  </appSettings>
</configuration>
```

<msg type='Important' head='Внимание!'>Сборка с монитором задач должна обязательно находиться в том же каталоге, что и приложение.</msg>

Компоненты платформы Flexberry активно используют монитор задач. Например, [сервис данных](data-service.html) пишет SQL-запросы, которые выполняются в СУБД. При запуске примера с `EventTaskMon`, в `Windows Application Log` появятся записи, соответствующие выполненным SQL-запросам.

# Как писать в монитор задач свои действия
Существует класс `ICSSoft.STORMNET.Business.BusinessTaskMonitor`, с набором статических методов, идентичных описанным в `ICSSoft.STORMNET.Business.IBusinessTaskMonitor`. Этими методами и нужно пользоваться, если вы хотите писать в монитор задач какие-либо свои действия.


Например, для начала действия вызовите метод `BeginTask`.

# Перечень стандартных мониторов задач
[BusinessTaskMonitorsInOrm|Перечень стандатных мониторов задач.]

# Как создать свой монитор задач
[Создание и подключение монитора задач](creating-and-connection--business-task-monitor.html)
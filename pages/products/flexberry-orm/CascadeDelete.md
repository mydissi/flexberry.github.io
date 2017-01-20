---
title: Каскадное удаление объектов
sidebar: product--sidebar
keywords: DataObject (объекты данных), Flexberry ORM, Public, БД, Бизнес-серверы, Ограничения
toc: true
permalink: ru/cascade-delete.html
folder: product--folder
lang: ru
---

# Удаление мастеров
Пусть дана следующая [диаграмма](class-diagram.html):

![](/images/pages/img/Filters/KreditDiagramm.png)

'''Вопрос:'''

Что произойдет при попытке удаления объекта типа `Адрес`, если в базе данных есть объекты типа `Клиент`, ссылающиеся на него?

'''Ответ:'''

![](/images/pages/img/Filters/DeleteError.png)

База данных не даст удалить такой объект.

# Варианты решения проблемы
Вариантов может быть очень много, в данной статье будут приведено только несколько. Технология предоставляет механизмы для решения проблемы, варианты ограничиваются лишь фантазией разработчика. Этот механизм: '''[бизнес-сервера](business--servers--wrapper--business--facade.html)'''.

# Удалять все объекты, ссылающиеся на данный, а также рекурсивно удалять все объекты, ссылающиеся на удаленный.
# Создавать новый объект и "перебрасывать" на него все ссылки.
# Вводить поле-флаг, отвечающее за "удаление" объекта. Объект с таким флагом будет считаться удаленным, но по факту, будет оставаться в базе.
# В версии Flexberry после 15.12.2014 для каскадного удаления объектов были добавлены интерфейсы [IReferencesCascadeDelete](i-references-cascade-delete.html) и [IReferencesNullDelete](i-references-null-delete.html).

## Рекурсивное удаление
Самый простой вариант, но самый недружелюбный к пользователю. Удалив 1 объект можно удалить целую кучу информации.

Алгоритм выполнения простой:

# В бизнес-сервере мастера (в нашем случае - `Адрес`) вычитываем все объекты, ссылающиеся на удаляемый.
# Проставляем всем объектам статус `[ObjectStatus.Deleted](object-status-and-loading-state.html)`.
# Отправляем на удаление все объекты.
# Проделать с пункта 1 рекурсивно для всех объектов.

Пример (с удалением объектов, ссылающихся на данный, но без рекурсии): [События WebObjectListView](w-o-l-v-events.html)


## Фиктивный объект
Такой вариант позволяет сохранить все данные, кроме того объекта, который реально необходимо было удалить, однако в базе может возникнуть каша из-за объектов, ссылающихся непонятно куда. Вариантов опять же несколько, либо создавать фиктивный объект при каждом удалении, либо создать по 1 фиктивному объекту для каждого класса и "вешать" все ссылки на него. В данной статье будет рассмотрен последний вариант.

Алгоритм:

# (один раз) создаем объект и пишем его в базу. Запоминаем его `[PrimaryKey](primary-keys-objects.html)`, например в файле конфигурации или в файле с константами.
# В бизнес-сервере мастера (в нашем случае - `Адрес`) вычитываем все объекты, ссылающиеся на удаляемый.
# Проставляем всем объектам ссылку на фиктивный объект.
# Отправляем на обновление все объекты.

Стоит отметить, что данный способ требует дополнительной обработки данных при выводе пользователю. Объекты, ссылающиеся на фиктивные, необходимо фильтровать или обрабатывать особым образом.

## Фиктивное удаление
При фиктивном удалении данные на самом деле не удаляются из базы, а всего лишь помечаются как удаленные. Во все объекты добавляется какое-нибудь поле типа `bool` (на примере проекта СЭДиЖ: `Актуально:bool`). При удалении объекта в бизнес-сервере перехватывается объект, у него меняется статус с `Deleted` на `Altered` и изменяется поле `Актуально = false;`.

После чего объект уходит на обновление в базу и остается в ней, но считается удаленным. Разумеется, необходимо реализовывать логику, которая будет "считать" такие объекты удаленными: при выводе информации пользователю необходимо накладывать ограничения на выводимые данные.

(((<msg type=note>Такой способ позволяет восстанавливать удаленные объекты.</msg>)))

### Пример
Необходимо доработать [диаграмму классов](class-diagram.html) таким образом, чтобы она поддерживала данный способ: добавляем поле `Актуально:bool`.

![](/images/pages/img/Filters/KreditDiagrammAktualno.png)

Добавляем логику в бизнес-сервера объектов (на примере `Адреса`):

```
if (UpdatedObject.GetStatus() == ObjectStatus.Deleted)
            {
                // Не дадим объекту удалиться, но выставим флаг Актуальности
                UpdatedObject.SetStatus(ObjectStatus.Altered);
                UpdatedObject.Актуально = false;

                // Найдем все объекты, ссылающиеся на "удаляемый" и удалим их.
                var ds = (SQLDataService) DataServiceProvider.DataService;
                var klients =
                    ds.Query<Клиент>(Клиент.Views.КлиентE)
                      .Where(k => k.Прописка.__PrimaryKey == UpdatedObject.__PrimaryKey);
                foreach (var k in klients)
                {
                    k.SetStatus(ObjectStatus.Deleted);
                }

                return klients.ToArray();
            }
```

(((<msg type=Note>Обратите внимание, что мы посылаем ссылающиеся объекты на удаление, однако, они точно также перехватятся в своем бизнес-сервере и не удалятся.</msg>)))

Далее, чтобы пользователю не выводились "удаленные" данные при просмотре списка объектов.

Для этого, наложим ограничение на [WebObjectListView](web-object-list-view.html) (опять же на примере `Адреса`):

```
var ds = (MSSQLDataService) DataServiceProvider.DataService;
IQueryable<Клиент> limit1 = ds.Query<Адрес>(Адрес.Views.АдресL).Where(Address => Address.Актуально);
Function onlyActual = LinqToLcs.GetLcs(limit1.Expression, Адрес.Views.АдресL).LimitFunction;
webObjectListView1.LimitFunction = onlyActual;
```

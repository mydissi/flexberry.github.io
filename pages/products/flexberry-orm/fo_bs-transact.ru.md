---
title: "Бизнес-сервер: обновление нескольких объектов одной транзакцией"
sidebar: flexberry-orm_sidebar
keywords: Flexberry ORM, Public, Бизнес-серверы
toc: true
permalink: ru/fo_bs-transact.html
---

## Обновление нескольких объектов одной транзакцией
Если возникает необходимость обновить несколько объектов одной транзакцией (к примеру, чтобы поддержать ссылочную целостность), можно воспользоваться стандартным механизмом [бизнес-сервера](fo_business--servers--wrapper--business--facade.html).

## Использование
Для формирования транзакции используются объекты, возвращаемые методом [`OnUpdate`](fo_bs-example.html) любого бизнес-сервера:

* Создать \ вычитать объекты для транзакции.
* Внести необходимые изменения во все объекты для транзакции (к примеру, проставить ссылки или изменить поля).
* Добавить все объекты транзакции в массив возвращаемых методом элементов.

{% include important.html content="Для всех объектов транзакции отработает бизнес-сервер, отвечающий за их обновление (кроме того объекта, из которого была инициирована транзакция)." %}

## Пример

![](/images/pages/products/flexberry-orm/BSExample.PNG)

Предположим, у нас есть два класса, ссылающиеся друг на друга обязательной связью. Стоит задача при создании объекта класса A автоматически создавать объект класса В и проставлять ссылки друг на друга.

## Для начала, необходимо создать бизнес-сервер для класса A и привязть его к обновлению объектов этого класса.
## В методе OnUpdate класса A необходимо написать следующий код:

```cs
var a = UpdatedObject;
var b = new B();
a.Копия = b;
b.Копия = a;

return new DataObject[2] { a, b }; 
```

Теперь, при создании объекта типа А автоматически будет создаваться его копия - объект типа В.

{% include important.html content="Обратите внимание на изменившийся `return` метода OnUpdate: именно через него формируется список элементов транзкции." %}

---
title: UI Message
sidebar: ember-flexberry_sidebar
keywords: Flexberry Ember
toc: true
permalink: ru/ef_ui-message.html
folder: products/ember-flexberry/controls/
lang: ru
summary: Представлено описание контрола для отображения сообщений о состоянии контента
---

## Описание

Основное предназначение __контрола UI Message__ - отображение сообщений о состоянии контрола. Например, отображение об успешности/неуспешности сохранения формы, предупреждения, информация и т.п.

Свойства сообщений задаются в разметке страницы приложения.

### Список свойств, описанных в компоненте UI Message

Название свойства |Краткое описание
:-----------------|:----------------------------------------------------------------------
`visible`| Отображение (видимость) сообщения, по умолчанию `true`.
`floating`| Эффект "плавающего" сообщения, по умолчанию `false`.
`compact`| Отображение в компактном (сжатом) виде, по умолчанию `false`.
`attached`| Примыкает ли сообщение к другому сообщению или контенту, по умолчанию `false`.
`closeable`| Возможность скрыть сообщение, по умолчанию `false`.
`type`| Тип сообщения (ошибка, информация и т.д.), по умолчанию не задано (`null`).
`color`| Цвет сообщения, по умолчанию не задано (`null`).
`size`| Размер сообщения, по умолчанию не задано (`null`).
`icon`| Иконка для сообщения, по умолчанию не задано (`null`). Виды иконок доступны [на сайте semantic-ui.com](http://semantic-ui.com/elements/icon.html).
`caption`| Заголовок сообщения, по умолчанию не задано (`null`).
`message`| Содержание (текст) сообщения, по умолчанию не задано (`null`).

Подробно доступные свойства описаны [на сайте semantic-ui.com](http://semantic-ui.com/collections/message.html).

## Пример отображения сообщения

Например, необходимо добавить сообщение об ошибке при сохранении страницы. Разметка будет выглядеть следующим образом:

```html
<div class="field">
  {{#ui-message
    caption='Результат проверки'
    type='error'
    message='Операция не выполнена'
    closeable=true
  }}
</div>
```
Сообщению задан заголовок (`caption`), тип (`error`), содержание сообщения (`message`) и возможность скрывать сообщение (`closable`).
В итоге сообщение будет выглядеть следующим образом:

![](/images/pages/products/ember-flexberry/controls/example-for-ui-message.png)

Свойства сообщений можно компоновать необходимым для решения поставленной задачи образом (менять цвет, размер, добавлять иконки и т.д.). Однако есть некоторые исключения в использовании свойств.

## Прмечания

Свойство `compact` не следует применять со свойством `icon`: сообщение будет отображено в стандартном, а не компактном виде.  
Свойство `floating` не следует использовать со свойством `type`: не будет эффекта всплывающего окна (тени). Для придания необходимого внешнего вида сообщения, присущего тому или иному типу, можно использовать свойство `color`.  
Свойство `attached` применяется только в том случае, если указано не менее чем для двух сообщений.
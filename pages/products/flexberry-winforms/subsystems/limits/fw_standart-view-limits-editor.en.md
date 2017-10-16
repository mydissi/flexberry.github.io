---
title: Стандартный вид редактора ограничений. Формат ограничений
sidebar: flexberry-winforms_sidebar
keywords: Ограничения
toc: true
permalink: en/fw_standart-view-limits-editor.html
folder: products/flexberry-winforms/
lang: en
---

В этой статье будет описаны возможный формат ограничений, который соответствует формату редактируемых ограничений в стандартном виде.

## Логические атрибуты и операции
Для логических атрибутов возможны следующие операции:
* " "(Пробел) - логический атрибут вставится без применения операции. Например, если нужно написать "Актуально И Последняя"
* "=", "<>" - Можно сравнивать атрибуты только с параметрами типа Boolean. Например, "Актуально = @param". В стандартном виде редактора сравнить два поля между собой или сравнить поле с истиной невозможно. Для первого случая нужно пользоваться расширенным редактором, для второго можно просто вставить атрибут без применения операций.
* "Не пусто", "Не заполнено", "Не" - Обернуть атрибут в соответствующую функцию, принимают только 1 параметр
* "И", "ИЛИ"

Если вы создаете ограничение в расширенном редакторе ограничений с логическими операциями, то оно должно удовлетворять следующим критериям:
* Должны использоваться только перечисленные выше операции
* Если используются операции "=", "<>", то первый операнд должен быть атрибутом из представления, а второй - параметр(@param). Например, "Актуально = @param";

## Функции для работы с датами
Работа с датами в стандартном виде описана в статье [Функции для работы с датами в 'стандартном виде'](fw_date-limits-standart-view.html)

Если вы создаете ограничение в расширенном редакторе ограничений с функциями для дат, то оно должно удовлетворять следующим критериям:
* Если в операции сравнения "=", "<>", ">", "<" и пр. в качестве операнда используется функция над атрибутом, которая возвращает значение типа DateTime, то вторым операндом должна быть та же самая функция. Например, "Только дата(ДатаРожения) = Только дата(Сегодня())". Функция вида "Только дата(ДатаРождения) = Сегодня()" отображаться не будет;

## Задание SQL выражений
Задание SQL выражений доступно только в качестве логического операнда. Само выражение можно написать в текстовом поле самого редактора, либо открыть форму для задания SQL выражений. Для этого необходимо кликнуть на кнопку "..." в строке задания значений для SQL функции.

В тексте SQL выражения могут встречаться переносы строк. Результатом выполнения запроса должно являться логическое значение.

Подробно о программном использовании SQL выражений в ограничениях описано в статье [funcSQL](fo_func-sql.html).
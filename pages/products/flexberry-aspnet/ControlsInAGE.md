---
title: Настройка контролов внутри AGE
sidebar: product--sidebar
keywords: Flexberry ASP-NET, Web UI (Контролы)
toc: true
permalink: ru/controls-in-a-g-e.html
folder: product--folder
lang: ru
---



# Описание настройки
Иногда возникает задача дополнительной настройки свойств контролов, генерируемых для редактирования и отображения значений свойств детейлов в AGE. Для этого можно использовать делегаты, задаваемые статически для класса `AjaxGroupEdit`. Делегат задается для конкретного имени представления и имеет тип `TuneControlDelegate`, подробнее про который можно почитать [здесь](tune-control-delegate-method.html). Добавить делегат можно добавить с помощью метода `SetControlTuner`:
```cs
/// <summary>
/// Установить метод для настройки контролов, отображающихся в AGE.
/// </summary>
/// <param name="viewName">Имя представления (детейла, отображаемого в AGE), для которого будет вызываться передаваемый метод.</param>
/// <param name="method">Метод, выполняющий настройку контролов.</param>
public static void SetControlTuner(string viewName, TuneControlDelegate method)
```
Задавать делегаты лучше в `Global.asax.cs`, в методе `Application_Start`.
Пример использования:
```cs
AjaxGroupEdit.SetControlTuner(
    Подзадача.Views.ПодзадачаE.Name,
        (control, data) =>
           { 
              if (control.GetType() #  typeof(TextBox) && data.PropertyName  Information.ExtractPropertyName<Подзадача>(x => x.Описание)) 
                 ((TextBox)control).TextMode = TextBoxMode.MultiLine; 
           });
```

Подробно об AjaxGroupEdit написано в этой [статье](ajax-group-edit.html).
 


 
---
title: IMasterPageService
sidebar: product--sidebar
keywords: 
toc: true
permalink: ru/i-master-page-service.html
folder: product--folder
lang: ru
---

<div style="margin:5px; padding-left:28px; float:right; width:40%; outline:1px solid white;">
<br>
<table border="0" width="100%" bgcolor="#6495ED">
<tbody><tr><td bgcolor="#FFFFFF">
* '''Продукт''': [Flexberry ASP.NET](flexberry-a-s-p-n-e-t.html)
* '''Программная библиотека''': ICSSoft.STORMNET.Web.AjaxControls.dll
* '''Предназначение''': Интерфейс сервиса для работы с Master-страницами.
</td>
</tr></tbody></table></a>
</div>



# IMasterPageService
IMasterPageService - интерфейс сервиса для работы с Master-страницами.
Сервис используется для настройки Master-страниц в технологических страницах, хранящихся в сборках, в связи с тем, что на этапе компиляции не известна структура и конфигурация конкретного прикладного проекта. Сервис позволяет указывать как используемую Master-страницу, так и контенйер, в котором будет отображаться содержимое страницы.

## Создание пользователя в Web через группу
Для этого нужно:<br>
1. Добавить класс в проект.<br>
Свойство '''MasterPageFile'''.
```cs
    public class StaticMasterPageService : IMasterPageService
    {
        /// <summary>
        /// Серверный путь до текущей Master-страницы.
        /// </summary>
        private readonly string _masterPageFile;

        /// <summary>
        /// Идентификатор контейнера для содержимого страницы.
        /// </summary>
        private readonly string _placeHolderId;

        /// <summary>
        /// Конструктор класса.
        /// Явно устанавливает параметры для работы с Master-страницей.
        /// </summary>
        /// <param name="masterPageFile">Серверный путь до текущей Master-страницы.</param>
        /// <param name="placeHolderId">Идентификатор контейнера для содержимого страницы.</param>
        public StaticMasterPageService(string masterPageFile, string placeHolderId)
        {
            _masterPageFile = masterPageFile;
            _placeHolderId = placeHolderId;
        }

        /// <summary>
        /// Серверный путь до текущего Master-файла в WebAppTemplate.
        /// </summary>
        public string MasterPageFile
        {
            get
            {
                switch (ThemeService.Current.Theme)
                {
                    case "Boost":
                        return "~/Boost.Master";
                    case "Smart":
                        return "~/Site1.Master";
                }

                return _masterPageFile; 
            }
        }

        /// <summary>
        /// Метод для получения контрола-контейнера для содержимого страницы.
        /// Возвращает основной ContentPlaceholder из Master-страницы WebAppTemplate.
        /// </summary>
        /// <param name="page">Страница, у которой нужно определить контейнер для основного контента.</param>
        /// <returns>Контрол-контейнер, в который будут помещены все контролы страницы.</returns>
        public Control GetContentPlaceholder(Page page)
        {
            return page.FindControlsByID(_placeHolderId).FirstOrDefault();
        }

    }
```
2. Изменить Web.config.<br>
Параметр '''mapTo'''.
```cs
<!-- Конфигурация сервиса Master-страниц. -->
<register type="NewPlatform.Flexberry.Web.MasterPage.IMasterPageService, ICSSoft.STORMNET.Web.AjaxControls" 
	mapTo="IIS.Бюджет.StaticMasterPageService, Бюджет(ASP.NET Application)">
	<lifetime type="singleton" />
	<constructor>
		<param name="masterPageFile" type="System.String" value="~/Site1.Master" />
		<param name="placeHolderId" type="System.String" value="ContentPlaceholder1" />
	</constructor>
</register>
```




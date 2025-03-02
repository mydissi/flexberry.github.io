--- 
title: Locking in web systems 
sidebar: flexberry-aspnet_sidebar 
keywords: Flexberry ASP-NET 
toc: true 
permalink: en/fa_working-locks-web.html 
folder: products/flexberry-aspnet/ 
lang: en 
autotranslated: true 
hash: 3f0a156f0f9966f8e009cd305af860bdaa73dc4e34e82239ebb9bd2fe34c7d2c 
--- 

Logic lock: 

* When loading the edit page of the object the lock is always fitted (if the object is not locked by another user), ie if the database already has a record with an expired lock on the object, then it is updated. If no record, then it is created. The actual lock within 30 seconds, if during this time it is not renewed, then the object is not locked. 
* Next, every 30 seconds, the edit page sends ajax requests to extend the lock. Using ajax request you can only renew a lock (i.e. to change the existing database record, but not create a new one). If the records in the database do not appear, the user will see a warning and is prompted to open a read-only object or go to the previous page 
* Before you create or extend the lock in both of the above-described points, it is first checked that at the moment the object is not locked by another user. If you find that the object is locked, the user will see a warning and will be able to return to the previous page or to open an object for viewing. 
* In the folder `Security` attached form `Lock\LockL` with a list of the currently existing locks. Remove lock from list removes the corresponding entry from the database.


{% include callout.html content="Переведено сервисом «Яндекс.Переводчик» <http://translate.yandex.ru>" type="info" %}
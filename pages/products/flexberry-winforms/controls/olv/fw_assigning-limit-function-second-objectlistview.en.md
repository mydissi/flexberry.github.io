---
title: Присвоение LimitFunction для второго ObjectListView
sidebar: flexberry-winforms_sidebar
keywords: Windows UI (Контролы), Ограничения
toc: true
permalink: en/fw_assigning-limit-function-second-objectlistview.html
folder: products/flexberry-winforms/
lang: en
---

Для второго [ObjectListView](fw_objectlistview.html) ограничение надо присваивать до вызова базового метода Edit (в переопределённом на зависимой списковой форме). Иначе будут гонки между поднятием формы и обновлением списка и присвоением ограничения.
---
title: 'FIX: Запуск только одного экземпляра Outlook'
date: 2025-01-14 09:00:00 +0300
description: Описание решения по запуску только одного экземпляра Outlook на Windows
categories: [Windows, Решение]
tags: [Windows, Outlook, fix]
img_path: /assets/screenshots/outlook
image:
  path: outlook.jpg
  width: 100%
  height: 100%
---

## Проблема

При запущенном ранее экземпляре Outlook под Windows, можно случайно запустить второй, третий и т.д. экземпляр приложения. После чего в трее Windows, если нажать на иконку Outlook можно увидеть несколько запущенных экземпляров приложений.

![Desktop View](outlook-any-instance.jpg){: w="700" h="400" }
_количество запущенных экземпларов Outlook_

## Решение

Чтобы этого избежать, необходимо в ярлыке запуска приложения Outlook добавить параметр **-recycle**, который предотвратит запуск нового экземпляра приложения и активирует уже ранее запущенный.

![Desktop View](outlook-shortcut.jpg){: w="350" h="200" }
_параметр -recycle в ярлыке запуска Outlook_
---
title: 'FIX: Windows уходит в сон после отключения по RDP'
date: 2024-01-28 22:30:00 +0300
description: Решение проблемы с автоматическим уходом в сон Windows 11
categories: [Windows, Решение]
tags: [Windows 11, fix]
img_path: /assets/screenshots/windows-goes-to-sleep
image:
  path: Regedit.jpg
  width: 100%
  height: 100%
---

Решение проблемы с автоматическим уходом в сон Windows 11.<br>

<hr>

_Спящий режим — одна из функций экономии электроэнергии в операционной системе Windows. В зависимости от настроек устройство может переходить в режим сна после определенного периода бездействия._


## Проблема

Если завершить подключение к удаленному рабочему столу, закрыв приложение Microsoft Remote Desktop или отключится от текущей сессии, то удаленный компьютер под управлением Windows 11 через пару минут уходил в сон.

После пробуждения компьютер через Wake on LAN и при подключении к удаленному столу, в журнале Windows в разделе System обнаруживалась запись: 


**Kernel-Power** <br>
**The system is entering sleep.** <br>
**Sleep Reason: System Idle**


![Desktop View](Kernel-Power.jpg){: w="700" h="400" }
_События Kernel-Power во время ухода компьютера в сон_

При этом на удаленном компьютере всегда работают виртуальные машины под Hyper-V и в созданном плане электроэнергии (Power Option) режим сна и гибернации были отключены.

![Desktop View](Power-Option-1.jpg){: w="350" h="200" }
_Настроки Power Option_


## Решение

Необходимо в реестре Windows внести изменнеия, чтобы в плане электроэнергии появился дополнительный пункт **System unattended sleep timeout**

Для этого:

1. Нажимаем **Win + R**, вводим **regedit** и запускаем редактор реестра
2. Открываем ветку реестра **'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\PowerSettings\238C9FA8-0AAD-41ED-83F4-97BE242C8F20\7bc4a2f9-d8fc-4469-b07b-33eb785aaca0'**
3. И вносим изменение в параметр **Attributes**, выставив значение **2**
4. Закрываем редактор реестра и перезагружаем компьютер, чтобы применились настройки
5. Открываем свой план электроэнергии (Power Option) и находим в разделе Sleep новый параметр **System unattended sleep timeout**, который необходимо установить в **0**


![Desktop View](Power-Option-2.jpg){: w="350" h="200" }
_Настроки Power Option_

После этого компьютер уже не будет уходить в сон если отключится от сессии подключения удаленного стола по RDP.
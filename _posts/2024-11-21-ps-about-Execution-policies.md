---
title: 'FIX: Политика выполнения PowerShell ограничивает выполнения скрипта'
date: 2024-11-21 09:00:00 +0300
description: Управление политикой PowerShell для запуска скрипта на локальном компьютере
categories: [PowerShell, Решение]
tags: [PowerShell, Windows, fix]
img_path: /assets/screenshots/ps-about-Execution-policies
image:
  path: powershell.jpg
  width: 100%
  height: 100%
---

## Проблема

При запуске скрипта на PowerShell появляется сообщение об ошибке

![Desktop View](Get-ExecutionPolicy.jpg){: w="700" h="400" }
_Системная ошибка "cannot be loaded because running scripts is disabled on this system"_

```console
File %anypath%\any_script.ps1 cannot be loaded because running scripts is disabled on this system. For more information, see about_Execution_Policies at 
https:/go.microsoft.com/fwlink/?LinkID=135170. 
```

## Причина

Это связано с тем, что политика выполнения PowerShell установлена для предотвращения влияния ненадежных скриптов в среде Windows. Политики выполнения - это настройки безопасности, которые определяют уровень доверия для скриптов, запускаемых в PowerShell. Политика выполнения по умолчанию является «строгой», что предотвращает запуск команд и скриптов PowerShell.


## Решение для "... cannot be loaded because running scripts is disabled on this system..."

Для запуска скриптов на PowerShell и устранения системной ошибке необходимо установить политику выполнения с помощью командлета [Set-ExecutionPolicy](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.4), чтобы скрипт PowerShell работал на конкретном устройстве под управлением Windows. Необходмо выполнитеь следующие шаги:


### Шаг 1. Проверить текущую политику выполнения

Откройте консоль PowerShell, выбрав «Выполнить от имени администратора» (или щелкните правой кнопкой мыши меню «Пуск» и выберите «Windows PowerShell (администратор)» из контекстного меню) и получите политику выполнения с помощью командлета [Get-ExecutionPolicy](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.security/get-executionpolicy?view=powershell-7.4):

```console
PS C:\Windows\system32> Get-ExecutionPolicy
Restricted 
```
Получим результат текущей политики используемой в Windows на конечном устройстве. [С видами политик можно ознакомиться на сайте вендора}(https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.4).


### Шаг 2: Установить политику выполнения

Установить политику выполнения с помощью следующей команды:

```powershell
Set-ExecutionPolicy RemoteSigned
```

![Desktop View](Set-ExecutionPolicy-RemoteSigned.jpg){: w="700" h="400" }
_Установка политики запуска сриптов PowerShell_

Появится предупреждение о риске безопасности. Необходимо ввести «Y» или «A» при появлении запроса на продолжение. После чего проблема с запуском скрипта на PowerShell будет решена.


## Дополнительно

Можно использовать

```powershell
Set-ExecutionPolicy Unrestricted
```

, чтобы снять все ограничения политики безопасности. После того, как будет изменена политика выполнения, можно запускать скрипты без ошибки «running scripts is disabled on this system».

Но лучше рассмотреть политику **RemoteSigned** - требует от доверенного издателя подписывать скрипты и файлы конфигурации из Интернета. Любые загруженные неподписанные скрипты будут заблокированы, но выполнение скриптов, созданных локально, разрешено без какой-либо цифровой подписи.

При установке той или иной политики устанавливается ключ **ExecutionPolicy** в ветке реестре: **HKLM\Software\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell**. Также можно изменить политику через редактор реестра.

![Desktop View](regedit-ExecutionPolicy.jpg){: w="700" h="400" }
_Политика запуска скриптов PowerShell в редакторе реестра_

Параметр политики принимает следующие значения:

**Restricted** - Выполнения скриптов не допускаются
**Unrestricted** - Можно запустить любой скрипт, подпись не требуется.
**RemoteSigned** - Хорошо подходит для тестовых и сред разработки. Нужно подписать только файлы из Интернета. В противном случае будет отображено, что файл .ps1 не имееет цифровой подписью. Это настройка политики по умолчанию на серверах. Более безопасный вариант по сравнению с неограниченным.
**AllSigned** - локальный или удаленный скрипт. Доверенный издатель должен подписать его. Разрешены только цифровые скрипты PowerShell.


## Что делать, если нет возможности установить политику выполнения, запустив консоль PowerShell с правами администратора?

Чтобы установить политику выполнения для текущей области пользователя, можно воспользоваться следующим:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Область по умолчанию - "LocalMachine", которая устанавливает политику для всех пользователей текущего устройства. Можно получить политику выполнения для всех областей, используя:

```console
PS C:\Windows\system32> Get-ExecutionPolicy -List

        Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process       Undefined
  CurrentUser       Undefined
 LocalMachine    RemoteSigned 
```

## Временный обход выполнения для сеанса

Можно обойти политику выполнения PowerShell только для одноразового сеанса, в командной строке необходимо ввести:

```powershell
PowerShell -ExecutionPolicy Bypass
```

После закрытия окна PowerShell текущий сеанс PowerShell завершается, и обход также закрыт. Это позволяет временно запускать файл скриптов PowerShell, сохраняя при этом настройки ExecutionPolicy для всех других сеансов PowerShell.


## Использовать объект групповой политики для установки политики выполнения для нескольких компьютеров

Если необходимо изменить политику выполнения на нескольких компьютерах, необходимо использовать групповую политику на контроллере домена.

1. Открыть редактор групповых политик.
2. В разделе «Local Computer Policy» перейти в «Computer Configuration» >> Administrative Templates >> Windows Components >> Windows PowerShell
3. Включить политику «Turn on Script Execution», затем необходимо выбрать желаемую политику выполнения из выпадающего списка, например «Allow local scripts and remote signed scripts», что эквивалентно свойству «RemoteSigned», которое устанавливается с помощью командлета Set-ExecutionPolicy.


## В завершении

Ошибка «cannot be loaded because running scripts is disabled on this system» в PowerShell возникает при первом использовании PowerShell после установки Windows, но это легко исправить, изменив политику выполнения PowerShell. Понимая принцип работы политики выполнения PowerShell и следуя данной инструкциям, можно включить выполнение скриптов PowerShell и воспользоваться всеми преимуществами автоматизации с помощью PowerShell.
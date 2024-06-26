# Инструкция по созданию поста

## Структура

Внутрення структура сайта состоит из следующих каталогов, которые необходимы для создания новой публикации.

```shell
.
├── _posts
|  └── YYYY-MM-DD-name_post.md
├── assets
|  └── screenshots
|      └── name_post
|          └── image.jpg
└── index.html
```

Новый пост создается в файле **YYYY-MM-DD-name_post.md**, где <br>
**YYYY** - год, <br>
**MM** - месяц, <br>
**DD** - день. <br>
**name_post** - название на латинице с расширением **.md**

Все графические файлы поста располагаются в каталоге с названием поста (name_post), расположенные: <br>
./assets/screenshots/...

Изображения в формате **.jpg** с разрешением **720p**. Рекомендуемый размер одного файла до **100KB**.

## Заголовок .md файла

Пример заголовка нового .md файла для поста

```
---
title: 'История развития версий Configuration Manager'
date: 2024-01-28 22:00:00 +0300
description: Краткая история развитий версий Configuration Manager
categories: [Configuration Manager, Статья]
tags: [ConfigMgr]
img_path: /assets/screenshots/configmgr-history
image:
  path: configmgr-roadmap.jpg
  width: 100%
  height: 100%
---
```

## Оформление картинок

**Средний размер**

```
![Desktop View](sms1.jpg){: w="700" h="400" }
_Консоль администрирования SMS 1.x. Скриншот взят в сети Интернет_
```

**Маленький размер**

```
![Desktop View](HyperVSettings.png){: w="350" h="200" }
_настройки месторасположения хранения виртуальных машин_
```

## Оформления блока текста

**Блок совета**

```
> Рекомендуется по возможности использовать самую последнюю версию обновления.
>
> <https://docs.microsoft.com/ru-ru/sccm/core/servers/manage/updates>
{: .prompt-tip }
```

**Информационный блок**

```
> Чтобы узнать, что нового появляется в новых версиях, необходимо ознакомится со статьей в официальной документации.
> 
> <https://docs.microsoft.com/ru-ru/mem/configmgr/core/plan-design/changes/whats-new-incremental-versions>
{: .prompt-info }
```
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
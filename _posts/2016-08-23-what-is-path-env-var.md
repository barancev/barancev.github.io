---
layout: article
title: ...что такое переменная среды PATH?
date: 2016-08-23T13:39:54+03:00
excerpt: В этой переменной перечисляются директории, в которых операционная система ищет исполняемые файлы.
tags: [howto]
image:
  feature: 2016-08-23-what-is-path-env-var/labyrinth.jpg
  credit: 3DECOR&nbsp;DESIGN
  creditlink: http://www.3decor.eu/interior-wallpapers/labyrinth/1/
  teaser: 2016-08-23-what-is-path-env-var/path.png
  thumb:
comments: true
toc: true
ads: true
---
## Что такое вообще переменная среды?

Когда операционная система запускает какую-нибудь программу, она стартует новый процесс и каким-то образом передаёт ему информацию о настройках среды, или окружения (в английском языке используется термин environment). Эта информация состоит из набора переменных, содержащих некоторые значения. Процесс может получить эти значения, обратившись к нужной переменной по имени. Например, чтобы узнать, где находится директория, которую операционная система рекомендует использовать для хранения временных файлов, необходимо получить значение переменной среды `TEMP`.

## Как посмотреть значения переменных среды?

В консоли Windows можно посмотреть значение этой переменной, выполнив команду `echo %TEMP%`, в консоли PowerShell необходимо для этого выполнить команду `echo $Env:TEMP`, а в консоли Linux или MacOS -- команду `echo $TEMP`.

Если вы пишете программу на языке программирования Python, значение этой переменной можно получить так:

{% highlight python %}
import os
temp = os.environ["TEMP"]
{% endhighlight %}

В языке Java это можно сделать следующим образом:

{% highlight java %}
String temp = System.getenv().get("TEMP");
{% endhighlight %}

В языке C# аналогичное действие выглядит следующим образом:

{% highlight csharp %}
string temp = System.Environment.GetEnvironmentVariable("TEMP");
{% endhighlight %}

## На что влияет переменная окружения `PATH`?

При помощи переменных окружения можно передавать информацию не только запускаемым процессам, но и самой операционной системе. Она тоже читает и использует значения переменных окружения, поэтому можно управлять некоторыми аспектами поведения операционной системы, изменяя переменные окружения.

Переменная `PATH` содержит список директорий, в которых операционная система пытается искать исполняемые файлы, если пользователь при запуске не указал явно путь к нужному исполняемому файлу.

Давайте представим себе, что на компьютере с операционной системой Windows установлено две разных версии интерпретатора языка программирования Python. Это можно сделать, если установить их в разные директории, например, `C:\Python27` и `C:\Python34`. Исполняемый файл для обоих версий называется `python.exe`.

Для того, чтобы запустить исполняемый файл нужной версии, можно указать полный путь к нему, например, `C:\Python34\python.exe`:

![](/images/2016-08-23-what-is-path-env-var/console1.png "Запуск программы с указанием полного пути")

Но каждый раз указывать полный путь лень, да ещё и помнить его надо.

Альтернатива -- добавить в переменную окружения PATH путь к директории, где находится этот исполняемый файл, и тогда его можно будет запускать, указывая только имя. А чтобы узнать, где он (по мнению операционной системы) находится, можно использовать команду `where` в операционной системе Windows либо команду `which` в операционной системе Linux или MacOS.

![](/images/2016-08-23-what-is-path-env-var/console2.png "Запуск программы по имени в Windows")

Эта переменная содержит список директорий, в которых операционная система должна искать исполняемые файлы. В качестве разделителя используется точка с запятой (;) в операционной системе Windows и двоеточие (:) в операционных системах Linux и MacOS.

Обратите внимание, что в переменную PATH нужно добавлять не пути к исполняемым файлам, а пути к директориям, где они находятся!

## Как изменять значения переменных среды?

[Инструкция для разных версий операционной системы Windows](http://www.computerhope.com/issues/ch000549.htm)

Пользователям других операционных систем предлагаю погуглить :)

## Переменную поменял, но эффекта нет. Почему?

Когда вы меняете значение некоторой переменной окружения, об этом узнаёт только операционная система. При запуске новых программ она сообщит им новые значения переменных. Но ранее запущенные программы будут продолжать использовать те значения переменных окружения, которые были актуальны на момент запуска программы.

Поэтому после изменения переменных окружения придётся перезапустить те программы, которым необходимо сообщить новые значения переменных.
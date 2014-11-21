---
layout: article
title: ...как установить Java на Windows?
modified:
categories: 
excerpt: Скачать инсталлятор, запустить, а когда установится - настроить переменные окружения и удалить лишние исполняемые файлы.
tags: [howto java windows]
image:
  feature: 2014-11-20-how-to-install-java-on-windows/feature.png
  credit: arkabana
  creditlink: http://arkabana.deviantart.com/art/Windows-8-1-Screensaver-413871909
  teaser: 2014-11-20-how-to-install-java-on-windows/teaser.jpg
  thumb:
comments: true
toc: true
date: 2014-11-20T17:14:02+03:00
---
Во многих моих тренингах так или иначе используется Java, либо как язык программирования для разработки автотестов, либо как виртуальная машина для запуска приложений, написанных на Java — инструментов тестирования, сред разработки, и даже клиент системы видеоконференций GotoWebinar требует наличия Java.

Поэтому я решил описать процедуру установки Java для операционной системы Windows и последующей настройки системы, потому что, к сожалению, недостаточно просто "запустить инсталлятор и всегда нажимать кнопку Next".

## 1. Где взять Java?

[На официальном сайте Oracle Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

## 2. Какую версию выбрать?

Разумеется, последнюю доступную (на момент написания инструкции это Java 8).

Выбирая из 32-битной и 64-битной версий, берите 64-битную, если ваша операционная система это позволяет, потому что у неё менее строгие ограничения по памяти, доступной исполняемым Java-приложениям.

## 3. Что устанавливать, JRE или JDK?

_Java Runtime Environment_, или JRE — это виртуальная машина, позволяющая запускать приложения, написанные на языке программирования Java.

_Java Development Kit_, или JDK — это набор инструментов, для разработки программ на языке программирования Java (компилятор, архиватор, генератор документации и прочие). JRE разумеется тоже входит в дистрибутив JDK.

Правило очень простое: если вы собираетесь что-нибудь писать на языке программирования Java, значит вам потребуется JDK. А если только запускать готовые программы — тогда достаточно JRE.

## 4. Установка Java

Вот тут, действительно, всё просто — нужно запустить инсталлятор и следовать указаниям визарда. Можно просто всё время нажимать кнопку Next.

## 5. Настройка переменных окружения

К сожалению, инсталлятор Java не выполняет настройку переменных окружения, поэтому придётся сделать это вручную после установки.

Во-первых, необходимо установить переменную `JAVA_HOME`, которая должна указывать на директорию, в которую установлена Java. Многие программы используют эту переменную, чтобы определить, где находится Java.

Во-вторых, надо в переменную `PATH` добавить путь к директории `%JAVA_HOME%\bin`. Эта переменная указывает операционной системе список директорий, в которых нужно искать исполняемые файлы, и чтобы можно было запускать Java из консоли, переменная `PATH` должна быть правильно настроена.

Для установки переменных окружения сначала нужно открыть свойства компьютера, либо использовав сочетание клавиш Win-Pause, либо через меню "Пуск":

![](/images/2014-11-20-how-to-install-java-on-windows/properties.png)

Затем нужно выбрать "Дополнительные параметры системы", в открывшемся диалоге перейти на вкладку "Дополнительно" и нажать кнопку "Переменные среды", после чего появится диалог настройки переменных окружения.

![](/images/2014-11-20-how-to-install-java-on-windows/environment.png)

Если у вас уже есть переменная окружения `JAVA_HOME` — надо её отредактировать, если нет — создать новую. В качестве значения нужно указать путь к директории, куда установлена Java, то есть, например `c:\Program Files\Java\jdk1.8.0_25\`, если вы установили JDK, либо `c:\Program Files\Java\jre1.8.0_25\`, если вы установили только JRE.

После того, как вы установили значение переменной `JAVA_HOME`, необходимо отредактировать значение переменной PATH, добавив туда путь к директории, где находятся исполняемые файлы Java, то есть `%JAVA_HOME%\bin`

![](/images/2014-11-20-how-to-install-java-on-windows/path.png)

И сохранить всё это, закрыв все открытые диалоги в обратном порядке кнопками OK.

Обратите внимание, что если вы устанавливаете JDK, то в названии директории указывается номер версии, поэтому впоследствии, когда вы решите установить более новую версию, не забудьте поменять значение переменной окружения `JAVA_HOME`.

После того, как вы изменили переменные окружения, новые значения будут действительны только для новых запускаемых программ, уже запущенные программы не узнают о том, что переменные окружения поменялись. Поэтому если вы, например, пытались запустить Java из консоли и у вас не получилось из-за неправильных настроек переменной `PATH`, вам придётся перезапустить консоль после того, как вы поменяли значение переменной.

## 6. Удаление лишних файлов

Запустите консоль (`cmd`) и выполните в ней команду `where java`.

В результате вы должны увидеть путь к исполняемому файлу `java.exe`, который операционная система должна успешно обнаружить в том месте, куда вы установили Java. Если файл не нашёлся — значит неправильно настроена переменная `PATH` и нужно вернуться к предыдущему пункту.

Однако иногда бывает и наоборот, находятся «лишние» исполняемые файлы:

![](/images/2014-11-20-how-to-install-java-on-windows/terminal.png)

Происходит это из-за того, что инсталлятор Java вместо того, чтобы правильно настроить переменные окружения, пытается положить исполняемые файлы в директорию `C:\Windows\system32`

Это не очень хорошо — засоряется системная директория, может возникнуть рассогласование версий Java (в разных директориях разные версии). Поэтому надо удалить из каталога `C:\Windows\system32` исполняемые файлы `java.exe`, `javaw.exe` и `javaws.exe`, если они там обнаружатся.

Вот и всё, теперь можно пользоваться Java. Только не забывайте о том, что после установки новой версии надо будет обновить переменную окружения `JAVA_HOME`!
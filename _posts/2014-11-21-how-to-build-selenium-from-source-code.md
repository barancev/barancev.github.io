---
layout: article
title: ...как самостоятельно собрать Selenium из исходников?
date: 2014-11-21T18:09:23+03:00
excerpt: Клонировать репозиторий с исходниками, выполнить в консоли команду go release, немного подождать - и свежий Selenium готов.
tags: [howto selenium]
image:
  feature: 2014-11-21-how-to-build-selenium-from-source-code/feature.jpg
  credit: ZOOB
  creditlink: http://poof-slinky.com/product/zoob-35/
  teaser: 2014-11-21-how-to-build-selenium-from-source-code/teaser.jpg
comments: true
toc: true
---
Краткая инструкция по сборке Selenium:

* [установить Java SDK](/how-to-install-java-on-windows/)
* [установить клиент Git](http://git-scm.com/)
* [клонировать репозиторий Selenium](https://code.google.com/p/selenium/source/checkout)
* запустить консоль в директории, куда клонировали репозиторий, и выполнить там команду `go release`
* через какое-то время (не очень скоро) забрать готовый файл `build\dist\selenium-server-standalone-2.44.0.jar` (номер версии, понятно, может отличаться)

Всё, можно пользоваться.

А теперь всё то же самое, но более подробно.

## 0. Зачем?

Вот именно, зачем? Зачем вообще надо собирать Selenium из исходников? Ведь можно просто взять готовый дистрибутив и использовать его.

Да, большинству пользователей в большинстве случаев именно так и надо делать -- использовать готовый дистрибутив. Но иногда хочется, знаете, чего-то такого...

Например, вот прямо сейчас, в тот момент, когда я пишу эту заметку, "релизная" версия Selenium ещё не позволяет запускать тесты в новых версиях браузера Opera, которые построены на движке Blink, а если собрать Selenium из исходников -- [это уже можно делать](https://github.com/SeleniumHQ/selenium/commit/d69a533af1a1a1dc20cb380beabafd6093a6bbad):

Или, скажем, вышло предупреждение, что Apache HttpClient версий до 4.3.6 содержит потенциальную уязвимость, а вы очень беспокоитесь по поводу защищенности тестового стенда -- [это уже исправлено](https://github.com/SeleniumHQ/selenium/commit/8f771f2df2e7c3f2dee532ddd751d8687011a5a4), но новый релиз будет ещё не скоро.

Ну или вы набрались решимости самостоятельно исправить какой-нибудь баг в Selenium, который вам ужасно мешает, а ленивые разработчики Selenium почему-то его игнорируют (видимо, им не мешает).

В общем, хочется иногда. Эта заметка именно для тех, кому хочется.

## 1. Установить Java SDK

Это подробно описано в [инструкции по установке Java](/how-to-install-java-on-windows/), только обратите внимание, что для сборки Selenium потребуется Java SDK, а не JRE.

## 2. Установить клиент Git

Клиентов Git много, на любой вкус и цвет, но лично я предпочитаю либо консольный, либо GUI-клиент [Git Extensions](https://code.google.com/p/gitextensions/), именно он дальше будет показан на скриншотах.

## 3. Клонировать репозиторий Selenium

Существует два репозитория, в которых хранится код Selenium, они синхронизируются раз в несколько минут, и можно клонировать любой из них:

* основной - [https://code.google.com/p/selenium/](https://code.google.com/p/selenium/)
* зеркало - [https://github.com/SeleniumHQ/selenium/](https://github.com/SeleniumHQ/selenium/)

Запускаем клиент Git Extensions и жмём там кнопку-ссылку `Clone repository`:

![](/images/2014-11-21-how-to-build-selenium-from-source-code/git_extensions.png)

Указываем адрес репозитория, локальную директорию, в которую его нужно клонировать, и жмём кнопку `Clone`:

![](/images/2014-11-21-how-to-build-selenium-from-source-code/clone.png)

Репозиторий достаточно объёмный, поэтому тут придётся немного подождать, можно успеть выпить кофе перед следующим весьма ответственным шагом.

## 4. Выполнить сборку Selenium

Запускаем консоль (`cmd`), переходим в ту директорию, в которую был клонирован репозиторий командой `cd c:\temp\selenium` и там выполняем команду `go release`:

![](/images/2014-11-21-how-to-build-selenium-from-source-code/go_release.png)

Тут придётся подождать ещё дольше (хотя, конечно, не так долго, как если бы вы собирали Firefox или Chromium из исходников)...

Во время сборки из интернета будут загружаться четыре версии Gecko SDK. Они нужны для того, чтобы скомпилировать библиотеки для поддержки нативных событий в браузере Firefoх (что это такое - я расскажу как-нибудь потом). На самом деле, если у вас не установлен на машине компилятор для языка C, эти библиотеки скомпилироваться не смогут. Вместо них будут использованы предварительно собранные версии библиотек, которые уже есть в репозитории. Но четыре версии Gecko SDK всё равно скачаются. Когда-нибудь мы это оптимизируем. Может быть.

В общем, если подождать достаточно долго, то этот процесс завершится, и в подкаталоге `buid\dist` вы найдёте собранный файл `selenium-server-standalone-2.44.0.jar` (на момент написания версии номер версии именно такой, в будущем, понятно, он будет отличаться).

Это и есть готовый дистрибутив, который содержит всё необходимое для разработки тестов на Java с использованием Selenium, а также Selenium Server.

Вот и всё.

Ну, или почти всё, потому что ещё есть библиотеки для C#, Ruby и Python, которые собираются немного иначе. Но это уже совсем другая история.

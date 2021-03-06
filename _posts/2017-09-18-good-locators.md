---
layout: article
title: ...как строить хорошие локаторы?
date: 2017-09-18T09:49:05+03:00
excerpt: Самое главное -- понять, чем хорошие локаторы отличаются от плохих
tags: [selenium]
image:
  feature: 2017-09-18-good-locators/dom.png
  teaser: 2017-09-18-good-locators/green-duck.png
  thumb:
comments: true
ads: true
---
На самом деле конкретно про то, как строить локаторы, я буду рассказывать на [онлайн-конференции КоТэ](http://koteconf.ru/). А здесь и сейчас -- небольшое предисловие о том, чем хорошие локаторы отличаются от плохих, а также небольшое упражнение.

Инструмент автоматизации веб-приложений, который работает через браузер (Selenium или любой другой) должен иметь механизм поиска элементов на странице приложения. Прежде чем выполнить действие с элементом -- его нужно сначала найти. Механизмы поиска элементов как раз и называются "локаторами".

Большинство инструментов автоматизации веб-приложений (а может быть даже все) ограничены теми средствами поиска элементов, которые предоставляют браузеры. Это ровно те же самые функции языка JavaScript, которые используют программисты, разработчики фронтэнда. Например, функция [getElementById](https://developer.mozilla.org/ru/docs/Web/API/Document/getElementById) позволяет найти элемент по идентификатору (значению атрибута id), функция [getElementsByName](https://developer.mozilla.org/ru/docs/Web/API/Document/getElementsByName) используется для поиска элементов по имени, а функция [querySelector](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector) выполняет поиск при помощи языка запросов [CSS Selectors](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Selectors).

В общем, с одной стороны, разных механизмов поиска достаточно много, с другой стороны, сложные языки запросов (CSS Selectors и XPath) дают большую свободу в построении локаторов. Поэтому для каждого элемента можно построить локатор, который его находит, многими разными способами.

Как понять, какой из возможных локаторов самый лучший?

"Какой-нибудь" локатор построить легко. Например, можно записать рекордером действие с элементом, и рекордер предложит некоторый локатор, по которому элемент находится.

Но как известно, основные проблемы и расходы в автотестировании связаны не с созданием тестов, а с их поддержкой и сопровождением -- анализ сбоев, доработка после изменений тестируемого приложения, улучшение самих тестов.

Поэтому самыми важными являются именно те свойства локаторов, которые облегчают сопровождение тестов.

Исходя из этого требования, я сформулирую три критерия "хорошести" локаторов.

**1. Хороший локатор должен быть устойчив к изменениям.**

Это, пожалуй, самое главное. Если после небольшого изменения тестируемого приложения все тесты приходится переделывать -- это просто катастрофа.

Есть изменения, которые изначально заложены в приложение и могут определяться его настройками -- локализация, расположение блоков на странице (например, включить/отключить рекламные блоки), "шкурки" (skins) и прочее. Локаторы нужно сразу проектировать так, чтобы они работали с разными вариантами настроек.

Есть изменения, которые случатся в будущем. Тут заранее подготовиться на 100% нельзя, но можно хотя бы постараться предугадать, с чем эти изменения будут связаны. Например, если сейчас пока приложение одноязычное, но есть планы выхода на новые рынки -- вероятно, добавятся разные локализации, поэтому поиск элементов по тексту перестанет работать. Если дизайнер скажет, что кнопки нужно сделать не прямоугольными, а закруглёнными -- поменяются стили и вёрстка кнопок, поэтому полагаться при поиске на конкретный стиль рискованно, лучше выбрать какие-то более стабильные признаки.

Конечно, при массированном изменении тестируемого приложения противостоять изменениям в тестах не удастся, но это вполне ожидаемо. Но незначительные изменения не должны приводить к разрушению тестов.

**2. Хороший локатор должен быть понятным.**

Когда вы разрабатываете тесты, вы помните структуру страницы и можете легко представить, что найдётся по тому или иному локатору. Но через несколько месяцев память будет уже не так свежа, и глядя на запутанный локатор будет уже не так легко догадаться, что он ищет или хотя бы что он должен искать.

Конечно, можно добавить комментарии и пояснения. Но лучше всё таки стремиться к выразительности и понятности самих локаторов. Они должны быть простыми и осмысленными. Тогда читать и понимать старые тесты будет легко и приятно.

**3. Хороший локатор должен быть.**

Увы, иногда хороший локатор вообще не удаётся построить. Вёрстка страницы плывёт, свойства элементов постоянно меняются. Если пытаться написать простой локатор -- он оказывается неустойчивым, уже на следующий он не работает. В попытках повысить устойчивость повышаем сложность локаторов -- но уже не через месяц, а через неделю становится непонятно, что они ищут, потому что прочитать локатор длиной в две строчки вообще невозможно.

И тогда у тестировщика опускаются руки, он говорит "приложение невозможно автоматизировать" и уходит.

Это неправильно. Нужно бороться за свои права.

Идите к разработчикам и договаривайтесь о повышении тестопригодности приложения. Вообще-то они хорошие люди, просто они думали о своих проблемах и не думали о ваших. Если вы не расскажете им о том, что вам нужно от приложения -- как они узнают?

**Ну а теперь обещанные упражнения.**

При подготовке мастер-класса по построению локаторов, который я буду проводить на [онлайн-конференции КоТэ](http://koteconf.ru/), я подготовил серию упражнений для участников.

Но даже если вы не являетесь участником конференции -- всё равно можете попробовать

**[выполнить эти упражнения](https://docs.google.com/forms/d/e/1FAIpQLSeH2tck0nP80ThQOaojA3hyVnMgmiz7sdav2wYWYsLfCOhpTw/viewform)**

"Правильные" ответы я опубликую после конференции, то есть примерно через две недели, и тогда вы сможете сравнить свой вариант решения с моим. А если хотите не только решения, но и подробный разбор -- [приходите на конференцию](http://koteconf.ru/). На мастер-классе я проведу разбор типовых ошибок, покажу те варианты, которые я считаю "хорошими", и расскажу о конкретных приемах и правилах, которыми я руководствуюсь при построении локаторов, чтобы они получались хорошими.
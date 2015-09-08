---
layout: ru
title: Riot ЧАВО
description: Часто задаваемые вопросы о Riot
---

# ЧАсто задаваемые ВОпросы

## Почему проект называется Riot (англ. "бунт")?
Riot выступает против нынешнего стремления к "швейцарским ножам" и невостребованному усложнению. Мы считаем, что компактность, чёткий API и чистый синтаксис гораздо важнее для фронтенд-библиотеки.

## Riot бесплатен?
Да, Riot бесплатен, открыт и предоставляется по лицензии MIT. У нас нет никаких [патентных условий](https://github.com/facebook/react/blob/master/PATENTS).


## Могу я использовать Riot в продакшене?
Конечно! Он полностью готов к продакшену и уже [широко используется](https://twitter.com/search?q=riotjs).

## Почему не поддерживается IE8?
Потому что это безумие - тратить время разработчиков на умирающий браузер. Согласно [статистике W3](http://www.w3counter.com/trends), только 1.5% используют IE8:

![](/img/ie8-trend.png)

Statcounter считает, что доля IE8 [2.5%](http://gs.statcounter.com/#browser_version_partially_combined-ww-monthly-201408-201507).

Этот нелепый браузер может быть благоплучно забыт. Riot 2.0 был запущен при поддержке IE8, но с тех пор доля этого браузера снизилась более чем на 50%.


## Нужно ли использовать дефиси в названиях пользовательских тегов?
W3C спецификация требует, чтобы использвания дефисов в названиях тегов. Вместо `<person>` вы должны использовать `<my-person>`. Соблюдайте это правило, если вы заботитесь о W3C. Это не влияет на работоспособность Riot.


## Почему в исходном коде не используется точка с запятой?
Без точек с запятой код легче читать. Это соответствует нашему минималистичному подходу. Мы используем одинарные кавычки по той же причине. Если Вы участвуете в разработке Riot, пожалуйста, не ставьте точки с запятой и двойные кавычки.

## Почему испольуется злосчастный `==` оператор?
Оператор нестрого равенства удобен в использовании, если им умело пользоваться. Вот пример:

`node.nodeValue = value == null ? '' : value`


## Могу я использовать тег `style` в файлах с расширением .tag?
Да. Вы можете использовать стили как обычно. Стандартный веб-компонент также имеет механизм инкапсуляции из CSS. Тем не менее, маловероятно, что это, в целом, облегчит управление вашим CSS.


## Какова роль jQuery?
Riot снижает потребность в jQuery. Вам больше не нужны селекторы, перебоы, события и прочии манипуляции. Но некоторые приёмы, вроде делегации событий могут быть полезны. jQuery-плагины можно без проблем использовать с Riot.


## Разве `onclick` не зло?
Нет, просто это выглядит старомодно. Соединение JS и HTML в одном компоненте важнее эстетеки. Минималистичный синтаксис Riot для обработки событий выглядит вполне прилично.

## Есть какие-нибудь планы на будущее?

Конечно. Мы в основном сфокусированы на [стабильности и производительности](https://github.com/riot/riot/issues) и хотим предоставить больше примеров того, [как можно использовать Riot](https://github.com/riot/examples).

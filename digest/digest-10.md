## Дайджест новостей по БЭМ. Выпуск десятый.

Коротко о том, как мы завершили прошлый год и с чем вошли в новый. 

## Новости документации
* Полностью обновили [документ про сборку БЭМ-проектов](https://ru.bem.info/methodology/build/). Свежий, красивый и понятный читайте на [bem.info](https://ru.bem.info/).
* Опубликовали новый документ про то, [как описывать зависимости в БЭМ](https://ru.bem.info/platform/deps/).
* Написали [спецификацию для DEPS](https://ru.bem.info/platform/deps-spec/) в БЭМ.
* Внесли много мелких правок в [методологической части](https://ru.bem.info/methodology/) документации. 

## Новости сайта 
Выкатили раздел [БЭМ-библиотек](https://ru.bem.info/platform/libs/) в новом дизайне:
* [bem-components](https://ru.bem.info/platform/libs/bem-components/5.0.0/)
* [bem-core](https://ru.bem.info/platform/libs/bem-core/4.1.1/)

## Новости библиотек

### bem-core
* Выпустили bem-core [4.1.0](https://ru.bem.info/platform/libs/bem-core/4.1.0/) и [4.1.1](https://ru.bem.info/platform/libs/bem-core/4.1.1/). Все изменения, вошедшие в оба релиза, описаны в [CHANGELOG](https://ru.bem.info/platform/libs/bem-core/4.1.1/changelog/#411). 

### bem-components
* Выпустили обещанную в прошлом дайджесте [bem-components v4.0.0](https://ru.bem.info/platform/libs/bem-components/4.0.0/) с небольшим обновлением дизайна контролов и переходом со Stylus на postCSS.
* Выпустили долгожданную версию [bem-components 5.0.0](https://github.com/bem/bem-components/releases/tag/v5.0.0), которая под капотом использует [bem-core 4.1.1](https://ru.bem.info/platform/libs/bem-core/4.1.1/). В версию 5.0.0 вошли сразу два набора стилей: исходники на postCSS и скомпилированный CSS на случай, если вы предпочитаете использовать препроцессор.

### bem-history
* Пока не выпустили в релиз, но призываем вас уже посмотреть и пощупать библиотеку в отдельной [ветке v4](https://github.com/bem/bem-history/tree/v4). Эта версия совместима с `bem-core v4`.  
Главное изменение — переименование блока `uri` в элемент `uri__querystring`, который расширяет базовую реализацию одноименного модуля из `bem-core` классом `Uri`. Методы класса сохранились без изменений.

Если вы давно этого ждали — самое время попробовать и рассказать о возможных проблемах до того, как мы выпустим версию.

### bem-react-core
* Работаем над [BEM React Core](https://github.com/bem/bem-react-core) — библиотекой для описания React-компонентов в виде деклараций БЭМ-сущностей.
* Разработали полный пакет документации: [README](https://github.com/bem/bem-react-core/blob/master/README.ru.md), [REFERENCE](https://github.com/bem/bem-react-core/blob/master/REFERENCE.ru.md) и [CONTRIBUTION GUIDE](https://github.com/bem/bem-react-core/blob/master/CONTRIBUTING.ru.md). 

## Новости технологий

### bem-express
Выпустили очередную партию мажорных обновлений:
* Обновили bem-core до версии [4.1.1](https://ru.bem.info/platform/libs/bem-core/4.1.1/) и bem-components до [5.0.0](https://github.com/bem/bem-components/releases/tag/v5.0.0).
* Перешли со Stylus к PostCSS. Из коробки поставляется тот же набор плагинов, что и в bem-components.
* Внедрили опциональный `livereload`. Подробнее смотри в [документации](https://github.com/bem/bem-express/blob/master/development.blocks/livereload/livereload.md) и в [README](https://github.com/bem/bem-express/blob/master/README.md) проекта.
* Добились ускорения сборки за счет прогрева `npm`-модулей, необходимых для сборки.
* Отказались от `bower` для поставки библиотек. Теперь все зависимости ставятся через `npm` в папку `node_modules`.

### bem-xjst

* **[v8.3.1](https://github.com/bem/bem-xjst/releases/tag/v8.3.1) (v7.4.1)**  
    * Исправлен баг в режиме `extend()`. Теперь режим работает как ожидается.
    * Дополнена документация: описание `this.extend(o1, o2)`.

* **[v8.4.0](https://github.com/bem/bem-xjst/releases/tag/v8.4.0) (v7.6.0)**  
    * Новая опция `unquotedAttrs` позволяет не выводить двойные кавычки у тех HTML-атрибутов, значения которых позволяют это сделать.

* **[v8.4.1](https://github.com/bem/bem-xjst/releases/tag/v8.4.1) (v7.6.1)**  
    * Колбек режима `extend(function(ctx, json) { … })` теперь принимает такие же аргументы, как и остальные колбеки в других режимах. Первый — ссылка на контекст исполнения шаблона `(this)`, второй — ссылка на узел BEMJSON, на который сматчился шаблон.

* **[v8.4.2](https://github.com/bem/bem-xjst/releases/tag/v8.4.2)**  
    * Функции экранирования теперь возвращают пустую строку, если аргументом был `undefined`, `null` или `NaN`. Раньше вы получали результат приведения к строке, что было исправлено.

* **[v8.5.0](https://github.com/bem/bem-xjst/releases/tag/v8.5.0)**  
    * В BEMTREE добавлены режимы, имеющие отношение к данным: `js()`, `addJs()`, `mix()`, `addMix()`, `mods()`, `addElemMods()`, `elemMods()`. Остальные режимы, которые имеют отношение только к HTML, доступны в BEMHTML.

* **[v8.5.1](https://github.com/bem/bem-xjst/releases/tag/v8.5.1)**  
    * Исправлен баг с расчетом `this.position` во время использования режима `replace()`.

* **[v8.5.2](https://github.com/bem/bem-xjst/releases/tag/v8.5.2) (v7.6.4)**  
    * Исправлен баг в BEMTREE, связанный с рендерингом специального значения поля `content` `{ html: '<unescaped value>' }`.  
    * Обновлена [bem-xjst onlinе demo](http://bem.github.io/bem-xjst/):    
	    * Добавлен переключатель движков BEMHTML/BEMTREE.  
	    * Добавлена заглушка для `BEM.I18N()`, которая возвращает свой второй аргумент. Это удобно для копирования кода из продакшена в песочницу.    
    * Обновлен [README](https://github.com/bem/bem-xjst/blob/master/README.ru.md): описали основные отличия шаблонизатора, фичи и т.д.  
    * Силами Михаила Степанова обновлена песочница. Вы тоже можете помочь: у нас есть задачи, помеченные как [help wanted](https://github.com/bem/bem-xjst/issues?q=is%3Aissue+is%3Aopen+label%3A"help+wanted").

### enb-bemxjst

* Выпустили новую версию [enb-bemxjst v8.5.2](https://github.com/enb/enb-bemxjst/tree/v8.5.2) с зависимостью `"dependencies": { "bem-xjst": "8.5.2"`. Однако продолжаем активно поддерживать две ветки: 7.x и 8.x.

Обо всех изменениях читайте в примечаниях к релизу [v8.5.2](https://github.com/bem/bem-xjst/releases/tag/v8.5.2) и [v7.6.4](https://github.com/bem/bem-xjst/releases/tag/v7.6.4). Полный список изменений описан в [CHANGELOG](https://github.com/enb/enb-bemxjst/blob/v8.5.2/CHANGELOG.md).

## Новости инструментов

### bem-tools

Выпустили [bem-tools 2.0.0](https://github.com/bem-tools/bem-tools), где обновили [bem-tools-create](https://github.com/bem-tools/bem-tools-create). Подробности читайте в [документации](https://github.com/bem-tools/bem-tools-create/blob/master/README.ru.md).

### bem-walk

Написали полный и понятный [README](https://github.com/bem-sdk/bem-walk/blob/master/README.ru.md).

### project-stub

* Внедрили новую версию [bem-components v4.0.0](https://ru.bem.info/platform/libs/bem-components/4.0.0/) с учётом перехода на postCSS и новую версию [bem-tools 2.0.0](https://github.com/bem-tools/bem-tools).
* В качестве эксперимента включили [gulp-bem](https://github.com/gulp-bem) в project-stub. 

## Новости мероприятий и сообщества
* Выступили на [DevCon School: Технологии будущего](https://events.techdays.ru/Future-Technologies/2016-11/) с докладом «Разрабатываем ASP.NET MVC приложение с БЭМ-фронтендом».
* Рассказали на форуме об [опыте внедрения gulp-bem](https://ru.bem.info/forum/1186/).
* Антон Виноградов [ответил на вопросы](https://ru.bem.info/forum/1212/) про bem-xjst и React.
* Занялись портированием Animate.CSS на БЭМ. [Инструкция по подключению](https://github.com/bem-contrib/bem-animations).
* Работаем над [библиотекой блоков](https://github.com/bem-hackaton-12-16/typography) для типографики.
* Провели [BEMup для новичков](https://events.yandex.ru/events/bemup/09-12-2016/), где рассказали про независимые блоки с самого начала — очень понятно и без BEMJSON/BEMHTML/bem-tools. Поговорили про сценарии, когда один проект требуется распараллелить на несколько разработчиков. Для тех, кто пропустил, опубликовали скринкаст с BEMup'а для начинающих.  
https://youtu.be/Ai-yt0b8iKE

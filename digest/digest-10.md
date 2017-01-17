## Дайджест новостей по БЭМ. Выпуск десятый.

Коротко о том, как мы завершили прошлый год и с чем вошли в новый: 

### Новости документации
* Полностью обновили документ про сборку БЭМ-проектов. Свежий, красивый и понятный читайте [тут](https://ru.bem.info/methodology/build/).
* Написали новый документ про то, как описывать зависимости в БЭМ — [Технология для описания зависимостей](https://github.com/bem-archive/bem-tools/blob/dev/docs/depsjs/depsjs.ru.md)
* Создали [спецификацию](https://github.com/bem-archive/bem-tools/blob/dev/docs/depsjs/specification.ru.md) для DEPS в БЭМ.
* Обновили [README](https://github.com/bem/bem-xjst/blob/master/README.ru.md) в репозитории [bem-xjst (BEMHTML)](https://github.com/bem/bem-xjst): описали основные отличия шаблонизатора, фичи и т.д.
* Написали полный и понятный [README](https://github.com/bem-sdk/bem-walk/blob/master/README.ru.md) в репозитории [bem-walk](https://github.com/bem-sdk/bem-walk).
* Внесли много мелких правок в методологической части документации. 

### Новости сайта 
Выкатили раздел [БЭМ-библиотек](https://ru.bem.info/platform/libs/) в новом дизайне:
* [bem-components](https://ru.bem.info/platform/libs/bem-components/5.0.0/)
* [bem-core](https://ru.bem.info/platform/libs/bem-core/4.1.1/)

### Новости библиотек

#### bem-core
Выпустили bem-core [4.1.0](https://ru.bem.info/platform/libs/bem-core/4.1.0/) и [4.1.1](https://ru.bem.info/platform/libs/bem-core/4.1.1/). Все изменения, вошедшие в оба релиза описаны в [Changelog](https://ru.bem.info/platform/libs/bem-core/4.1.1/changelog/#411). 

#### bem-components
* Выпустили обещанную в прошлом дайджесте [bem-components v4.0.0](https://ru.bem.info/platform/libs/bem-components/4.0.0/) с небольшим обновлением дизайна контролов и переходом со Stylus на postCSS.
* Выпустили долгожданную версию [bem-components 5.0.0](https://github.com/bem/bem-components/releases/tag/v5.0.0), которая под капотом использует [bem-core 4.1.1](https://ru.bem.info/platform/libs/bem-core/4.1.1/). Как и в bem-components 4.0.0, в версию 5.0.0 вошли сразу два набора стилей: исходники на postCSS и скомпилированный CSS на случай, если вы предпочитаете использовать препроцессор.

#### bem-react-core
Работаем над [BEM React Core](https://github.com/bem/bem-react-core) — библиотекой для описания React-компонентов в виде деклараций БЭМ-сущностей.

### Новости технологий

#### bem-express
Выпустили очередную партию мажорных обновлений:
* Обновили bem-core до версии [4.1.1](https://ru.bem.info/platform/libs/bem-core/4.1.1/) и bem-components до [5.0.0](https://github.com/bem/bem-components/releases/tag/v5.0.0).
* Перешли со Stylus к PostCSS. Из коробки поставляется тот же набор плагинов, что и в bem-components.
* Внедрили опциональный 'livereload'. Подробнее смотри в [документации](https://github.com/bem/bem-express/blob/master/development.blocks/livereload/livereload.md) и в [README](https://github.com/bem/bem-express/blob/master/README.md) проекта.
* Добились ускорения сборки за счет прогрева 'npm'-модулей, необходимых для сборки.
* Отказались от 'bower' для поставки библиотек. Теперь все зависимости ставятся через 'npm; в папку 'node_modules'.

#### bem-xjst

##### [v8.3.1](https://github.com/bem/bem-xjst/releases/tag/v8.3.1) (v7.4.1)
Исправлен баг в режиме `extend()`. Теперь он работает как ожидается.
Дополнена документация: описание `this.extend(o1, o2)`.

##### [v8.4.0](https://github.com/bem/bem-xjst/releases/tag/v8.4.0) (v7.6.0)
Новая опция `unquotedAttrs` позволяет не выводить двойные кавычки у тех HTML-атрибутов, значения которых позволяют это сделать.

##### [v8.4.1](https://github.com/bem/bem-xjst/releases/tag/v8.4.1) (v7.6.1)
Колбек режима `extend(function(ctx, json) { … })` теперь принимает такие же аргументы как и остальные колбеки в других режимах. Первый — ссылка на контекст исполнения шаблона (this), второй — ссылка на узел BEMJSON, на который сматчился шаблон.


##### [v8.4.2](https://github.com/bem/bem-xjst/releases/tag/v8.4.2)
Функциии экранирования теперь возвращают пустую строку если аргументом был `undefined` или `null` или `NaN`. Раньше вы получали результат приведения к строке, что было исправлено.

##### [v8.5.0](https://github.com/bem/bem-xjst/releases/tag/v8.5.0)
В BEMTREE добавлены режимы имеющие отношение к данным: `js()`, `addJs()`, `mix()`, `addMix()`, `mods()`, `addElemMods()`, `elemMods()`. Остальные режимы, которые имеют отношение только к HTML доступны только в BEMHTML.

##### [v8.5.1](https://github.com/bem/bem-xjst/releases/tag/v8.5.1)
Исправлен баг с расчетом `this.position` во время использования режима `replace()`.

##### [v8.5.2](https://github.com/bem/bem-xjst/releases/tag/v8.5.2) (v7.6.4)
Исправлен баг в BEMTREE связанный с рендерингом специального значения поля `content` `{ html: '<unescaped value>' }`.

Обновлена [bem-xjst onlinе demo](http://bem.github.io/bem-xjst/):
— добавлен переключатель движков BEMHTML/BEMTREE
— добавлена заглушка для BEM.I18N(), которая возвращает свой второй аргумент. Это удобно для комипования кода из продакшена в песочницу.

Обновление песочницы сделано силами Михаила @mikhailrojo Степанова. Вы тоже можете помочь: у нас есть задачи помеченные как [help wanted](https://github.com/bem/bem-xjst/issues?q=is%3Aissue+is%3Aopen+label%3A"help+wanted").


#### enb-bemxjst

Выпустили новую версию [enb-bemxjst v8.5.2](https://github.com/enb/enb-bemxjst/tree/v8.5.2) с зависимостью `"dependencies": { "bem-xjst": "^7.5.0"`.
Обо всех изменениях читайте в примечании к релизу (https://github.com/bem/bem-xjst/releases/tag/v8.5.2). Полный список изменений описан в [Changelog](https://github.com/enb/enb-bemxjst/blob/v8.5.2/CHANGELOG.md).

### Новости инструментов

#### bem-tools
Выпустили [bem-tools 2.0.0](https://github.com/bem-tools/bem-tools), где обновили [bem-tools-create](https://github.com/bem-tools/bem-tools-create). Подробности читайте в [документации](https://github.com/bem-tools/bem-tools-create/blob/master/README.ru.md).

#### project-stub
Внедрили новую версию [bem-components v4.0.0](https://ru.bem.info/platform/libs/bem-components/4.0.0/) с учётом перехода на postCSS и новую версию [bem-tools 2.0.0](https://github.com/bem-tools/bem-tools).

### Новости мероприятий и сообщества

* Провели BEMup для новичков, где рассказали про независимые блоки с самого начала — очень понятно и без всяких BEMJSON/BEMHTML/bem-tools. Поговорили про флоу, когда один проект требуется распараллелить на несколько разработчиков. [Скринкаст с BEMup'а для начинающих](https://www.youtube.com/watch?v=Ai-yt0b8iKE&feature=youtu.be).
* Выступили на на [DevCon School: Технологии будущего](https://events.techdays.ru/Future-Technologies/2016-11/) с докладом «Разрабатываем ASP.NET MVC приложение с БЭМ-фронтендом».
* Рассказали на форуме об [опыте внедрения gulp-bem](https://ru.bem.info/forum/1186/).
* Антон Виноградов [отвечает на вопросы](https://ru.bem.info/forum/1212/) про bem-xjst и React.
* Занялись портированием Animate.CSS на БЭМ. [Инструкция по подключению](https://github.com/bem-contrib/bem-animations).
* Работаем над [библиотекой блоков](https://github.com/bem-hackaton-12-16/typography) для типографики.

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
Выпустили новую версию [v8.5.2](https://github.com/bem/bem-xjst/tree/v8.5.2):
* Изменено поведение модуля: [Подробности](https://github.com/bem/bem-xjst/blob/v8.5.2/CHANGELOG.md#changes-in-modes-behaviour):
	** Теперь режимы 'mix()', 'js()', 'attrs()' заменят значения, указанные в BEMJSON.
	** 'applyNext()' используется для расширения значений BEMJSON.
	** Во всех режимах 'applyNext()' теперь возвращает значение BEMJSON, если это возможно.
	** Для добавления 'mix', 'js' или 'attrs' можно использовать режимы 'addMix()', 'addJs()' или 'addAttrs()', соответственно.
* Появились новые режимы: 'mods()', 'elemMods()', 'addMods()', 'addElemMods()'. Подробности читай в [документации](https://github.com/bem/bem-xjst/blob/v8.5.2/CHANGELOG.md#2016-10-06-v810-miripiruni).
* Появились новые опции: 'omitOptionalEndTags' и 'production'.
* Пофикшен баг со специальным значением `content: { html: '…' }` и BEMTREE.

Обновили [песочницу](http://bem.github.io/bem-xjst/). Теперь можно проверять любые новые примеры bem-xjst.

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
* Антон Виноградов [отвечает на вопросы](https://ru.bem.info/forum/1212/) про BEM-XJST и React.
* Занялись портированием Animate.CSS на БЭМ. [Инструкция по подключению](https://github.com/bem-contrib/bem-animations).
* Работаем над [библиотекой блоков](https://github.com/bem-hackaton-12-16/typography) для типографики.
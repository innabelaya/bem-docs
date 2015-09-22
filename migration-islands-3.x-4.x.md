# Миграция islands 3.x → 4.x

- [Настройка сборки](#Настройка-сборки)
- [Удаление .borschik](#Удаление-borschik)
- [Переход на bemhtml.js](#Переход-на-bemhtmljs)
- [Прекращение поддержки BEM.HTML](#Прекращение-поддержки-bemhtml)
- [Отказ от !important](#Отказ-от-important)
- [Удаление опциональных зависимостей](#Удаление-опциональных-зависимостей)
- [Перечень удаленных блоков, элементов, модификаторов и публичных методов](#Удаленное)
- [Pointer Events](#pointer-events)

## Настройка сборки

В версии 4.0 произошло множество изменений, для поддержки которых необходимо исправить конфигурацию сборки на уровне
сервиса.

Что изменилось:

1. Библиотеки `islands-*` и мета-библиотека `islands`, которая подключает другие библиотеки по зависимостям, объединены в одну — `islands`.
2. Изменился синтаксис BEMHTML-шаблонов:
   на смену старому «сахарному» синтаксису пришел новый JS-синтаксис.
3. Пересмотрен состав зависимостей всех блоков в `deps.js`-файлах.
4. Принято решение использовать Stylus для написания стилей.

Для поддержки этих изменений вам необходимо в конфигурации сборки:

1. Использовать функцию `getPlatformLevels(platform, options)`, чтобы
   указывать список уровней переопределения для сборки (см. ниже).
2. Обновить `enb` до версии 0.16+.
3. Перейти на пакет `enb-bem-techs` или обновить его до версии 2.0+.
4. Перейти на пакет `enb-bemxjst` для сборки BEMHTML-шаблонов.
5. Перейти на пакет `enb-stylus` версии 2.0+ для сборки CSS-файлов.
6. Обновить остальные пакеты `enb-*` до свежих версий (желательно).

Подробнее:
- [Уровни переопределения](#Уровни-переопределения)
- [Обновление ENB](#Обновление-enb)
- [Обновление enb-bem-techs](#Обновление-enb-bem-techs)
- [Переход на enb-bemxjst](#Переход-на-enb-bemxjst)
- [Переход на enb-stylus](#Переход-на-enb-stylus)

[Пример][stub-270] с изменениями в сборке [generator-project-stub].

[stub-270]: https://github.yandex-team.ru/project-stub/generator-project-stub/pull/270
[generator-project-stub]: https://github.yandex-team.ru/project-stub/generator-project-stub

### Уровни переопределения

Чтобы увеличить степень свободы в организации кода внутри библиотеки `islands`, разработан публичный API для получения списка уровней переопределения, предоставляемых
библиотекой — функция `getPlatformLevels(platform, options)`.

Начиная с релиза 4.0, функция `getPlatformLevels(platform, options)` — рекомендованный способ получения списка уровней.

Пример использования:

```js
var islands = require('../libs/islands'),
    levelsTech = require('enb-bem-techs/techs/levels');

module.exports = function(config) {
    config.nodes('desktop.bundles/*', function(nodeConfig) {
        nodeConfig.addTech([levelsTech, {levels: getDesktopLevels()}]);
    });
};

function getDesktopLevels() {
    // формируем список уровней сервиса
    var projectLevels = [
            {path: 'common.blocks', check: true}
            {path: 'desktop.blocks', check: true}
        ]
        .map(function(l) {
            // резолвим пути от корня сервиса
            return config.resolvePath(l);
        });

    // дописываем к уровням библиотеки islands уровни сервиса
    return islands.getPlatformLevels('desktop', {check: true})
        .concat(projectLevels);
}
```

### Обновление ENB

В `enb`, начиная с версии 0.16.0, появился [пул дочерних процессов][enb-257],
который можно использовать из технологий. Общий пул помогает балансировать
задачи сборки и сокращает время сборки на __15-30%__.

Обновить `enb` необходимо также для возможности использования последних версий пакетов `enb-bemxjst` и `enb-borschik`.

[enb-257]: https://github.com/enb-make/enb/pull/257

### Обновление enb-bem-techs

В пакете [enb-bem-techs], начиная с версии 2.0.0, в технологии `files` поменялся
порядок вычисления списка файлов по суффиксам.
__Было:__

```
level1/block1/block1.css
level1/block1/block1_type_simple.css
level1/block1/block1.ie.css
level2/block1/block1.ie.css
```

__Стало:__

```
level1/block1/block1.css
level1/block1/block1.ie.css
level2/block1/block1.ie.css
level1/block1/block1_type_simple.css
```

Новый порядок следования файлов позволяет при написании, например, «костылей» для IE 8 ориентироваться только
на стили текущей БЭМ-сущности, а не на стили всего бандла.

CSS-код блоков в `islands` был модифицирован, чтобы поддержать это изменение.
В блоках сервиса необходимо так же поддержать эти изменения.

В технологиях `deps` и `deps-old` исправлены критичные ошибки, которые влияли на порядок файлов при сборке.

Настоятельно рекомендуем перейти на последнюю версию пакета [enb-bem-techs].

[enb-bem-techs]: https://github.com/enb-bem/enb-bem-techs

### Переход на enb-bemxjst

Необходимо перейти на новый пакет для сборки сервиса — [enb-bemxjst], так как все шаблоны в `islands` переписаны на новый JS-синтаксис.

Технология [bemhtml][bemhtml-tech] из этого пакета по умолчанию работет с новым JS-синтаксисом, но позволяет собирать шаблоны и в старом синтаксисе
(при установке опции `compat: true`). Возможность собирать шаблоны в старом «сахарном» синтаксисе реализована для тех, кто для сборки шаблонов использовал пакет [enb-bemhtml] или [enb-xjst].

Опция `compat` позволяет использовать шаблоны из `islands` в JS-синтаксисе вместе
с шаблонами собственных блоков в старом синтаксисе.

[bemhtml-tech]: https://github.com/enb-bem/enb-bemxjst#bemhtml

### Переход на enb-stylus

Начиная с версии 4.0, мы переходим на [Stylus] для разработки стилей.
Это значит, что в последующих версиях блоков начнут появляться файлы с расширением `.styl`.
При этом перед выпуском версии планируется статически компилировать `.styl` файлы в `.css`,
и выпускать версию как со `.styl`, так и с `.css` файлами.

Если вы используете препроцессор Stylus, рекомендуем для
сборки CSS-файлов на сервисе применять пакет [enb-stylus] версии 2.0+.
Он позволяет собирать CSS-файлы бандлов, как из `.styl`-, так и из `.css`-файлов блоков.

В пакете `enb-stylus` решены все текущие задачи по сборке `.css`, в том числе
сборка source map и добавление вендорных префиксов при помощи [autoprefixer].

Пример использования технологии [stylus][stylus-tech] из пакета `enb-stylus`:

```js
var stylus = require('enb-stylus/techs/stylus');

module.exports = function(config) {
    config.nodes('desktop.bundles/*', function(nodeConfig) {
        // ...
        nodeConfig.addTech([stylus, {
            sourcemap: true,
            autoprefixer: {
                browsers: [
                    'chrome last 2 versions',
                    'firefox last 2 versions',
                    'opera 12.1',
                    'ie 8'
                ]
            }
        }]);
    });
};
```

[enb-bemhtml]: https://www.npmjs.com/package/enb-bemhtml
[enb-xjst]: https://github.com/enb-bem/enb-xjst
[enb-bemxjst]: https://github.com/enb-bem/enb-bemxjst
[Stylus]: https://learnboost.github.io/stylus/
[enb-stylus]: https://github.com/enb-make/enb-stylus
[autoprefixer]: https://github.com/postcss/autoprefixer
[stylus-tech]: https://github.com/enb-make/enb-stylus#stylus


## Удаление .borschik

Из библиотеки `islands` удален конфиг `.borschik`.
Теперь библиотека `islands` не содержит настроек по обработке ссылок на изображения в CSS-файлах.
По умолчанию все ресурсы загружаются отдельными запросами.

Если вам необходимо вернуть функциональность обработки ссылок на изображения (как было реализовано в версиях `islands 3.x`), то в проектный конфиг `.borschik`
нужно добавить следующие инструкции:

```js
{
    "freeze_paths": {
        "libs/islands/*.blocks/**/*.svg": ":encodeURIComponent:",
        "libs/islands/common.blocks/attach/**/*.png": ":base64:",
        "libs/islands/{common,desktop}.blocks/popup/**/*.png": ":base64:",
        "libs/islands/touch-phone.blocks/{input,select,checkbox,arr,page-layout}/**/*.png" : ":base64:"
    }
}
```


## Переход на bemhtml.js

В `islands`, начиная с 4.0, появились BEMHTML-шаблоны в новом JS-синтаксисе.

Чтобы продолжать разрабатывать шаблоны своего сервиса в старом синтаксисе,
которые будут совместимы с новым синтаксисом шаблонов из `islands`,
необходимо использовать пакет `enb-bemxjst` для сборки шаблонов.

### Конвертация шаблонов в новый JS-синтаксис

Вы можете преобразовать BEMHTML-шаблоны на своем сервисе в новый синтаксис.
Для этого используйте пакет [bemhtml-syntax].

1. Установите пакет командой:

    ```sh
    npm install -g bemhtml-syntax
    ```

2. Сконвертируйте нужные шаблоны в JS-синтаксис командой:

    ```sh
    bemhtml-syntax --format bemhtml -Qr -i path/to/block.bemhtml -o path/to/block.bemhtml.js
    ```

3. Чтобы сконвертировать все шаблоны сразу и отформатировать код,
   согласно своему стилю кодирования (при помощи `jscs`), воспользуйтесь командой:

    ```sh
    find *.blocks -name '*.bemhtml' -type f | while read f; do \
        bemhtml-syntax --format bemhtml -Qr -i "${f}" -o "${f}.js"; jscs --fix "${f}.js";\
    done
    ```

4. После проверки работоспособности новых шаблонов (`.bemhtml.js`),
   старые шаблоны (`.bemhtml`) можно удалить командой:

    ```sh
    find *.blocks -name '*.bemhtml' -type f -delete
    ```

[bemhtml-syntax]: https://github.com/vkz/bemhtml-syntax

### Доопределение контекста шаблонов

В отличие от шаблонов в старом «сахарном» синтаксисе, в `bemhtml.js`-шаблонах
появилась возможность доопределять прототип `BEMContext` в шаблонах блоков.
Объект этого класса доступен в шаблонах через `this` в блоках.

Для переопределения необходимо использовать функцию `oninit(cb(exports))`.

```js
oninit(function(exports) {
    exports.BEMContext.prototype.myFunc = function() {
        return 'it works!';
    };
});
```

Способ использования шаблона:

```js
block('my-block').content(function() {
    return this.myFunc();
});
```

Результат применения шаблона:

```html
<div class="my-block">it works!</div>
```

### Поддержка bem-xjst 4.0+

В `islands 4.0` появилась поддержка `bem-xjst 4.0+`.
Это потребовало внести [обратно несовместимые изменения][ISL-1722] в BEMHTML-шаблоны.

[ISL-1722]: https://st.yandex-team.ru/ISL-1722

#### Базовые шаблоны BEMHTML (i-bem__html.bemhtml.js)

Для получения результата выполнения шаблонов в виде строки, вместо хака с `this._buf` появился `this.reapply()`.
Это обычный `BEMHTML.apply()`, доступный из шаблонов.
Важное отличие от использования `this._buf` — с помощью `this.reapply()` нельзя передать флаги и поля в `this`, только в `this.ctx` ([ISL-1737]).

[ISL-1737]: https://st.yandex-team.ru/ISL-1737

#### i-global

В BEMHTML-шаблонах блока произошли значительные изменения в API ([ISL-1738]).

Теперь значения параметров выставляются в `oninit(function() { ... })` в виде:

```js
oninit(function(exports) {
    exports.BEMContext.prototype['i-global'] = {
        key: 'value'
    };
});
```

Появились методы:
- `isPublic(param)` — проверяет доступность значений параметра(ов)  `param` в браузере;
- `makePublic(param, flag)` — позволяет сделать доступными в браузере переданный параметр `param`
  или набор параметров, переданных в виде `{param: true}`.
  Метод используется вместо mode `public-params` в предыдущей реализации блока.

Как и прежде, значения параметров доступны через `this['i-global'].key`.

[ISL-1738]: https://st.yandex-team.ru/ISL-1738

#### i-services

Изменился API выставления и переопределения значений ([ISL-1741]).
Теперь эти действия выполняются  в `oninit(function() { ... })` в следующем виде:

```js
oninit(function(exports) {
    exports.BEMContext.prototype['i-services']._data['service-name'] = {
        ru: 'http://...',
        tr: 'http://...',
        '': 'http://...'
    };
});
```

Значения параметров доступны как `this['i-services'].serviceName(id)` и `this['i-services'].serviceUrl(id, region)`, как и раньше.

[ISL-1741]: https://st.yandex-team.ru/ISL-1741


## Прекращение поддержки BEM.HTML

В новой версии `islands` мы [отказались от поддержки][ISL-1485] шаблонизатора `BEM.HTML`.
Реализация шаблонизатора находилась на desktop-уровне в `i-bem__html.js`.
При этом на common-уровне для той же сущности, в `i-bem__html.bemhtml.js` располагалось ядро `BEMHTML`.
Это неоправданно увеличивало размер клиентского JavaScript-кода в проектах, которые `BEM.HTML` не использовали.

Код шаблонизатора перенесен в блок `i-bem-html` и подключен по зависимостям в
блоки, которым `BEM.HTML` все еще нужен.

Список блоков, в которых не произведен рефакторинг и которые не удалены в версии 4.0 библиотеки `islands`:
* `b-autocomplete-item`
* `b-form-input`
* `i-popup`
* `b-popup`
* `b-form-checkbox`

Блоки, в которых `BEM.HTML` заменен на `BEMHTML`:
* `b-icon`
* `b-link`
* `b-form-button`
* `b-form-checkbox`
* `b-dropdown`

В этих блоках удалены декларации `BEM.HTML.decl(desc, props)`.
Если вы использовали шаблоны этих блоков, замените вызовы `BEM.HTML.build(bemjson)`
на `BEMHTML.apply(bemjson)` или `BH.apply(bemjson)`,
либо скопируйте `BEM.HTML` декларации блоков на свой уровень из `islands-romochka`.
Код можно взять по ссылкам:

- __b-icon__
  - https://github.yandex-team.ru/lego/islands-romochka/blob/support/3.x/desktop.blocks/b-icon/b-icon.js
- __b-link__
  - https://github.yandex-team.ru/lego/islands-romochka/blob/support/3.x/desktop.blocks/b-link/b-link.js
  - https://github.yandex-team.ru/lego/islands-romochka/blob/support/3.x/desktop.blocks/b-link/_is-bem/b-link_is-bem_yes.js
  - https://github.yandex-team.ru/lego/islands-romochka/blob/support/3.x/desktop.blocks/b-link/_pseudo/b-link_pseudo_yes.js

- __b-form-button__
  - https://github.yandex-team.ru/lego/islands-romochka/blob/support/3.x/desktop.blocks/b-form-button/b-form-button.js#L179-L236
  - https://github.yandex-team.ru/lego/islands-romochka/blob/support/3.x/desktop.blocks/b-form-button/_type/b-form-button_type_simple.js

- __b-form-checkbox__
  - https://github.yandex-team.ru/lego/islands-romochka/blob/support/3.x/desktop.blocks/b-form-checkbox/b-form-checkbox.js#L112-L167

- __b-dropdowna__
  - https://github.yandex-team.ru/lego/islands-romochka/blob/support/3.x/desktop.blocks/b-dropdowna/b-dropdowna.js#L68-L74
  - https://github.yandex-team.ru/lego/islands-romochka/blob/support/3.x/desktop.blocks/b-dropdowna/__switcher/b-dropdowna__switcher.js

Если у вас в проекте есть блоки, использующие `BEM.HTML`, необходимо:
* Добавить в зависимости этих блоков блок `i-bem-html`.
* Удалить зависимость от `i-bem__html`, если в блоке одновременно с `BEM.HTML` не
используется `BEMHTML`.

Мы рекомендуем постепенно портировать блоки на `BEMHTML` или `BH`, так как
в следующей мажорной версий `islands` блок `i-bem-html` будет удален.

[ISL-1485]: https://st.yandex-team.ru/ISL-1485


## Отказ от !important

В рамках задачи [ISL-880] мы убрали из CSS-файлов использование `!important`.

Главным образом применение `!important` решало две проблемы:
- Переопределение глобальных стилей ([ISL-1259]).
- Переопределение блоком своих собственных базовых стилей (модификатор `theme`).

Теперь вы можете удалить все `!important`, перебивающие библиотечные стили на
ваших уровнях, и, при необходимости, использовать менее инвазивные средства.
Например, усиление селектора.

[ISL-880]: https://st.yandex-team.ru/ISL-880
[ISL-1259]: https://st.yandex-team.ru/ISL-1259


## Удаление опциональных зависимостей

В рамках задачи [ISL-469] мы удалили избыточные зависимости у блоков.
Теперь в сборку будет попадать гораздо меньше кода, но для правильной работы  //«гораздо меньше кода» ни о чем конкретном не говорит. Тут надо написать, что будут попадать только инимальный набор зависимостей или только неоходимые зависимости блока (не знаю, какой вариант правильный)
некоторых блоков потребуется добавить их опциональные зависимости на уровне проекта.

Обратите внимание, что некоторые элементы и модификаторы стали опциональными.
Если вы используете один из перечисленных ниже блоков или элементов, убедитесь, что зависимости от опциональных элементов и модификаторов указаны явно.
Для этого добавьте необходимые зависимости в `bemjson.js`, из которого будет собираться бандл,
в декларацию бандла (`bemdecl.js`) или в зависимости к какому-нибудь блоку.

[ISL-469]: https://st.yandex-team.ru/ISL-469

Список блоков и элементов, в которых произошли изменения.

* [i-jquery__debounce](#i-jquery__debounce)
* [auth](#auth)
* [checkbox](#checkbox)
* [header](#header)
* [logo](#logo)
* [service-table](#service-table)
* [service](#service)
* [select](#select)
* [user](#user)

### [i-jquery__debounce](https://st.yandex-team.ru/ISL-1347)

В элементе `i-jquery__debounce` находится реализация методов `debounce` и `throttle`
в виде расширения jQuery. Элемент больше не подключается вместе с блоком `i-bem`,
поэтому добавьте его в зависимости к тем блокам, которые используют `$.debounce` или `$.throttle`.

### [auth](https://st.yandex-team.ru/ISL-1326)

Если вы не используете модификатор `content_auto` у блока `auth`, то вам
необходимо добавить в зависимости элементы `button`, `error`, `haunter`,
`password`, `register`, `remember` и `username`.

### [checkbox](https://st.yandex-team.ru/ISL-1334)

Модификатор `theme_pseudo` стал опциональным.

### [header](https://st.yandex-team.ru/ISL-1344)

Элементы `main`, `logo`, `search`, `nav` и `project-name` стали опциональными.
Также вам нужно подключить блоки `logo`, `user` и `arrow_type_search`. //«подключить» — добавить в зависимости?

### [logo](https://st.yandex-team.ru/ISL-1370)

Модификаторы `lang_ru` и `lang_en` стали опциональными. // здесь тоже нужно упомянуть о добавлении в зависимости нужных значений

### [service-table](https://st.yandex-team.ru/ISL-1397)

Модификатор `type`, в котором реализованы различные наборы «Табло Сервисов»,
стал опциональным. Используемый набор значений модификатора нужно подключить в зависимости самостоятельно.

### [service](https://st.yandex-team.ru/ISL-1395)

Элементы `url`, `name`, `icon` и  модификатор `hoverable_yes` стали опциональными.
Также вам нужно добавить все используемые иконки — элементы блока `service-icon`.

### [select](https://st.yandex-team.ru/ISL-1394)

Модификатор `autosize_yes` стал опциональным.

### [user](https://st.yandex-team.ru/ISL-1413)

Элементы `icon`, `name`, `enter` и `enter-icon` стали опциональными.


## ~~Удаленное~~

В `islands 4.0` удалено множество устаревших блоков, элементов, модификаторов и публичных методов.
В основном это наследие из [bem-bl] и [islands-romochka], но некоторые сущности были удалены и из `islands-*`.
Обратите внимание на список удаленных сущностей:

- [i-fastclick](#i-fastclick)
- [i-pointer-events](#i-pointer-events)
- [i-ecma](#i-ecma)
- [search](#search)
- [navigation](#navigation)
- [b-logo](#b-logo)
- [b-head-logo](#b-head-logo)
- [b-mooa, b-mooa-panel](#b-mooa-b-mooa-panel)
- [b-pager](#b-pager)
- [b-country-flag](#b-country-flag)
- [notice](#notice)
- [notification-panel, notification-panel-notice](#notification-panel-notification-panel-notice)
- [i-https](#i-https)
- [i-ctr](#i-ctr)
- [i-error-reporter](#i-error-reporter)
- [b-geolocation](#b-geolocation)
- [b-related-list](#b-related-list)
- [i-services-request-controller](#i-services-request-controller)
- [b-search](#b-search)
- [b-search-filter](#b-search-filter)
- [images, serp, serp-ru, video](#images-serp-serp-ru-video)
- [i-oframebust](#i-oframebust)
- [l-page](#l-page)
- [l-head](#l-head)
- [b-layout-table](#b-layout-table)
- [b-text](#b-text)
- [b-static-text](#b-static-text)
- [b-text-list](#b-text-list)
- [i-jquery_dummy_yes](#i-jquery_dummy_yes)
- [i-jquery__is-empty-object](#i-jquery__is-empty-object)
- [b-page_theme_normal](#b-page_theme_normal)
- [header2__tableau](#header2__tableau)
- [i-jquery__core](#i-jquery__core)
- [i-bem__dom](#i-bem__dom)
- [i-loader](#i-loader)
- [i-request_type_ajax](#i-request_type_ajax)
- [i-ua_interaction_yes](#i-ua_interaction_yes)
- [logo__text](#logo__text)
- [popup](#popup)
- [radio-button__radio_next-for-pressed](#radio-button__radio_next-for-pressed)
- [services-table](#services-table)
- [i-global](#i-global)
- [b-link_type_global](#b-link_type_global)
- [i-jquery__touch](#i-jquery__touch)
- [i-geodata](#i-geodata)
- [Условные комментарии для IE6-7](#Условные-комментарии-для-ie6-7)
- [Неиспользуемые переводы](#Неиспользуемые-переводы)

[bem-bl]: https://github.com/bem/bem-bl/
[islands-romochka]: https://github.yandex-team.ru/lego/islands-romochka

### [i-fastclick](https://st.yandex-team.ru/ISL-1215)

__Было:__
В блоке `i-fastclick` находился немного доработанный код библиотеки [Fastclick.js](https://github.com/ftlabs/fastclick).
Он располагался на touch-уровне и подключался в любую touch-сборку через зависимость блока `i-bem`.

С помощью Fastclick.js мы решали проблему отзывчивости блоков в мобильных устройствах.
Смысл был в том, чтобы снять задержку между touch-событиями и синтетическим событием `click`.  // может « чтобы уменьшить время отклика ... или ускорить нажатие кнопок ... в мобильных устройствах»

Мы поняли, что это решение не подходит для общей библиотеки блоков и гораздо // в этом месте добавить пару слов причину
почему не устроила Fastcklick и чем оптимальнее  Pointer Events вместо фразы «перспективнее использовать»
перспективнее использовать [Pointer Events] со своими расширениями, которые точечно решают проблемы с отзывчивостью.

__Стало:__

В `islands` 4.0 мы удалили блок `i-fastclick` и перешли на использование специального события `pointerclick`.
Это унифицированное событие для всех устройств, которое работает без задержек на touch-устройствах, а на desktop является простым алиасом к событию `click`.

Событие становится доступным, если добавить в mustDeps-зависимости `pointerevents__click`:

```js
mustDeps: [
    {block: 'pointerevents', elem: 'click'}
]
```

Про переход на Pointer Events читайте подробнее в [отдельном разделе](#pointer-events).

### [i-pointer-events](https://st.yandex-team.ru/ISL-1291)

__Было:__

В блоке `i-pointer-events` находился код библиотеки [handjs](https://github.com/Deltakosh/handjs).
Это полифилл [Pointer Events] от Microsoft. Он был расположен на touch-уровне и подключался в любую touch-сборку через
зависимость у блока `i-bem`.

__Стало:__

В `islands 4.0` мы решили перейти на использование pointer-событий во всех блоках на всех платформах, но за основу взяли
 [PEP](https://github.com/jquery/PEP), который поддерживается jQuery Foundation.

Реализация событий теперь находится на common-уровне в блоке `pointerevents`.
Для поддержки IE8 мы перешли на использование событийного механизма jQuery, так что подписаться теперь можно только
через jQuery.

Для переезда вам достаточно исправить название блока в зависимостях.

Блоки несовместимы, одновременно их использовать невозможно.

Про переход на Pointer Events читайте подробнее в [отдельном разделе](#pointer-events).

### [i-ecma](https://st.yandex-team.ru/ISL-1277)

__Было:__

В блоке `i-ecma` хранились полифиллы для `JSON.parse/stringify` и широко используемых методов массивов, объектов, функций и строк.
Этот блок при необходимости позволял точечно подключить тот или иной полифилл.
В процессе работы выяснилось, что из поддерживаемых браузеров эти полифиллы нужны только в IE8.
Поэтому блок `i-ecma` был удален.

__Стало:__

Загрузка полифиллов отдельным файлом в IE8 выполняется с помощью conditional comment.
Для этого мы добавили специальный элемент-хелпер `b-page__cc`.

Загрузка должна происходить в самом начале, до jQuery. Использовать можно [es5-shims].

Пример:
```js
{
    block: 'b-page',
    head: [
        {elem: 'css', url: '_page.css', ie: false},
        {elem: 'css', url: '_page', ie: true}
    ],
    content: [
        '...',
        {
            elem: 'cc',
            condition: 'IE 8',
            content: {elem: 'js', url: 'https://yastatic.net/es5-shims/0.0.1/es5-shims.min.js'}
        },
        {block: 'i-jquery', mods: {version: 'default'}},
        {elem: 'js', url: '_page.js'}
    ]
}
```

[es5-shims]: https://yastatic.net/es5-shims/0.0.1/es5-shims.min.js

### [search](https://st.yandex-team.ru/ISLPAGE-594)

На замену блоку `search` разработан блок `search2`. При тех же возможностях, в блоке `search2` произошли следующие улучшения:
в нем практически нет стилей, не используется табличная верстка, упрощен BEMJSON API.

Документация, инструкция по миграции и примеры на сайте Лего:
- https://lego.yandex-team.ru/libs/islands/v4.x/desktop/search2/

Если вам нужен блок `search`, скопируйте его на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-page/tree/support/3.x/common.blocks/search
- https://github.yandex-team.ru/lego/islands-page/tree/support/3.x/desktop.blocks/search
- https://github.yandex-team.ru/lego/islands-page/tree/support/3.x/touch.blocks/search
- https://github.yandex-team.ru/lego/islands-page/tree/support/3.x/touch-phone.blocks/search
- https://github.yandex-team.ru/lego/islands-page/tree/support/3.x/touch-pad.blocks/search

### [navigation](https://st.yandex-team.ru/ISLPAGE-591)

Блок `navigation` реализовывал старую вертикальную навигацию с иконками сервисов.
В современном дизайне такая навигация нигде не используется, поэтому блок был удален.

Если вам нужен блок `navigation`, скопируйте его на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-page/tree/support/3.x/common.blocks/navigation
- https://github.yandex-team.ru/lego/islands-page/tree/support/3.x/desktop.blocks/navigation
- https://github.yandex-team.ru/lego/islands-page/tree/support/3.x/touch.blocks/navigation
- https://github.yandex-team.ru/lego/islands-page/tree/support/3.x/touch-pad.blocks/navigation
- https://github.yandex-team.ru/lego/islands-page/tree/support/3.x/touch-phone.blocks/navigation

### [b-logo](https://st.yandex-team.ru/ISLROMOCHKA-199)

__Было:__

Блок `b-logo` находился в библиотеке `bem-bl` и давно не используется сервисами.

__Стало:__

Блок `b-logo` удален, как дубликат `logo`, в котором реализованы аналогичные возможности.

Документация и примеры для блока `logo`:
- https://lego.yandex-team.ru/libs/islands/v4.x/desktop/logo/

Если вам нужен блок `b-logo`, скопируйте его на проектный уровень переопределения:
- https://github.com/bem/bem-bl/tree/support/2.x/blocks-desktop/b-logo

### [b-head-logo](https://st.yandex-team.ru/ISLROMOCHKA-195)

В блоке `b-head-logo` дублируется функциональность блока `logo`, мы удалили его как дубликат.

Документация и примеры для блока `logo` на сайте Лего:
- https://lego.yandex-team.ru/libs/islands/v4.x/desktop/logo/

Если вам нужен блок `b-head-logo`, скопируйте его на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/desktop.blocks/b-head-logo

### [b-mooa](https://st.yandex-team.ru/ISLROMOCHKA-201), [b-mooa-panel](https://st.yandex-team.ru/ISLROMOCHKA-203)

Блок `b-mooa` реализовывал «жучка». Однако блок не работал с версии 2.0.
Текущая реализация блока не поддерживается, поэтому  `b-mooa` и вспомогательный `b-mooa-panel` были удалены.
В ближайших версиях библиотеки разработка замены блока `b-mooa` не планируется.

Если вам нужен блок `b-mooa`, скопируйте его на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/desktop.blocks/b-mooa
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/desktop.blocks/b-mooa-panel

### [b-pager](https://st.yandex-team.ru/ISLROMOCHKA-205)

__Было:__

Блок `b-pager` реализовывал пагинацию в старом дизайне.

__Стало:__

В современном дизайне пагинация реализуется с помощью `radio-button` и отдельного блока не требует.

Если вам нужен `b-pager`, скопируйте код на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/desktop.blocks/b-pager

### [b-country-flag](https://st.yandex-team.ru/ISLROMOCHKA-193)

__Было:__

Блок `b-country-flag` позволял получить иконку с флагом ограниченного количества стран.
Иконки хранились в единственном размере 16x11px, только в формате PNG.

__Стало:__

Вместо блока `b-country-flag` используйте `country-flag`, в котором есть иконки всех стран (в трех размерах, в форматах SVG и PNG).

Документация блока `country-flag`:
- https://lego.yandex-team.ru/libs/islands/v4.x/desktop/country-flag/

Если вам нужен блок `b-country-flag`, скопируйте код на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/desktop.blocks/b-country-flag

### [notice](https://st.yandex-team.ru/ISLUSER-382)

Вместо блока `notice`  используйте блок `ticker`, который предоставляет те же возможности.

Документация блока `ticker`:
- https://lego.yandex-team.ru/libs/islands/v4.x/desktop/ticker/docs/

Если вам нужен блок `notice`, скопируйте код на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-user/tree/support/3.x/common.blocks/notice
- https://github.yandex-team.ru/lego/islands-user/tree/support/3.x/desktop.blocks/notice

### [notification-panel, notification-panel-notice](https://st.yandex-team.ru/ISLUSER-382)

Мы удалили блоки `notification-panel` и `notification-panel-notice` в связи с прекращением развития
«Нотификационной Панели» на портале.

### [i-https](https://st.yandex-team.ru/ISLROMOCHKA-191)

Утилитарный блок `i-https` предназначался для проверки поддержки https в браузере и использовался
в блоке `b-head-userinfo` для выставления ссылки на Паспорт.
В настоящий момент Паспорт использует только https,
а в схеме работы блока `i-https` были обнаружены [проблемы](https://ml.yandex-team.ru/thread/2370000000185323425/).

Блок `i-https` удален за ненадобностью.

Если вам нужен блок `i-https`, скопируйте его к себе на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/common.blocks/i-https

Обратите внимание, что теперь для использования блока на проектном уровне,
нужно исправить путь до файла `check-https.js`.

### [i-ctr](https://st.yandex-team.ru/ISLROMOCHKA-211)

Блок `i-ctr` предоставлял реализацию счетчика на основе `i-counter` и применялся для сбора статистики просмотров и кликов по Лего-блокам.
Блок устарел и не используется сервисами.

Блок `i-ctr` был удален.

### [i-error-reporter](https://st.yandex-team.ru/ISLROMOCHKA-213)

Блок использовался для отправки в Statface-данных о произошедших на странице JS-ошибках.
Не была реализована настройка блока. Блок по факту присутствия на странице переопределял `window.onerror` и отправлял данные с `dtype=jserror`.

В текущем виде блок использоваться не может.

Блок `i-error-reporter` был удален.

Если вам нужен блок `i-error-reporter`, скопируйте его на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/desktop.blocks/i-error-reporter

### [b-geolocation](https://st.yandex-team.ru/ISLROMOCHKA-221)

Блок в виде кнопки, который показывал регион пользователя, определенный с помощью `navigator.geolocation`.
Был реализован в старом дизайне, на текущий момент устарел и никем не используется.

Блок `b-geolocation` был удален.

Если вам нужен блок `b-geolocation`, скопируйте код из версии 3.x:
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/touch.blocks/b-geolocation

### [b-related-list](https://st.yandex-team.ru/ISLROMOCHKA-223)

Блок реализовывал «Связанные запросы» для touch-версий поисковых сервисов.

Блок `b-related-list` устарел и был удален.

### [i-services-request-controller](https://st.yandex-team.ru/ISLROMOCHKA-225)

Блок использовался в горизонтальном меню в старой реализации блока `b-header` для добавления текста запроса к ссылкам.
Теперь такой вид меню нигде не используется.

Блок `i-services-request-controller` устарел и был удален.

### [b-search](https://st.yandex-team.ru/ISLROMOCHKA-231)

Старая реализация поисковой формы из библиотеки `bem-bl`.
В данный момент нигде на портале не используется.

### [b-search-filter](https://st.yandex-team.ru/ISLROMOCHKA-232)

Реализация старых «Поисковых фильтров».

Блок `b-search-filter` устарел и был удален.

### [images, serp, serp-ru, video](https://st.yandex-team.ru/ISLICONS-105)

В блоках хранились фавиконки соответствующих сервисов, поэтому у каждого блока
был только один пользователь. Вносить такие блоки в общую библиотеку нецелесообразно.

Если вам нужны эти блоки, скопируйте их на проектный уровень:
- https://github.yandex-team.ru/lego/islands-icons/tree/support/3.x/common.blocks/serp
- https://github.yandex-team.ru/lego/islands-icons/tree/support/3.x/common.blocks/serp-ru
- https://github.yandex-team.ru/lego/islands-icons/tree/support/3.x/common.blocks/video
- https://github.yandex-team.ru/lego/islands-icons/tree/support/3.x/common.blocks/images

### [i-oframebust](https://st.yandex-team.ru/ISLROMOCHKA-215)

__Было:__
Блок `i-oframebust` был предназначен для защиты страниц от показа в `<iframe>`.

__Стало:__
Блок `i-oframebust` заменен на `framebuster` с измененным API и обновленными значениями по умолчанию.

Рекомендуем вам перейти на новый блок.

Документация блока `framebuster`:
- https://lego.yandex-team.ru/libs/islands/v4.x/desktop/framebuster/

Если вам нужен блок `i-oframebust`, скопируйте его на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/desktop.blocks/i-oframebust

### [l-page](https://st.yandex-team.ru/ISLROMOCHKA-219)

В блоке `l-page` находилась реализация типовой табличной раскладки для страницы в старом дизайне.
Табличная верстка больше не используется.

Блок `l-page` устарел и был удален.

Если вам нужен блок `l-page`, скопируйте его на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/desktop.blocks/l-page

### [l-head](https://st.yandex-team.ru/ISLROMOCHKA-217)

В блоке `l-head` находилась реализация табличной раскладки для старой версии блока `b-header`.
Блок `b-header` больше не применяется. Вместо него используйте `header2`.

Блок `l-head` устарел и был удален.

Если вам нужен блок `l-head`, скопируйте его на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/desktop.blocks/l-page

### [b-layout-table](https://st.yandex-team.ru/ISLROMOCHKA-197)

Блок `b-layout-table` использовался как вспомогательный для создания таблиц.
Он не содержал специальной логики и стилей и представлял собой простую таблицу с ячейками.

Блок `b-layout-table` устарел и был удален.

Если вам нужен блок `b-layout-table`, скопируйте его на проектный уровень переопределения:
- https://github.com/bem/bem-bl/tree/support/2.x/blocks-desktop/b-layout-table
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/desktop.blocks/b-layout-table

### [b-text](https://st.yandex-team.ru/ISLROMOCHKA-207)

Блок `b-text` был предназначен для оформления заголовков, параграфов и списков.
Блок `b-text` был удален из-за узкой сферы применения.

Если вам нужен блок `b-text`, скопируйте его на проектный уровень переопределения:
- https://github.com/bem/bem-bl/tree/support/2.x/blocks-desktop/b-text

### [b-static-text](https://st.yandex-team.ru/ISLROMOCHKA-187)

Блок `b-static-text` также был предназначен для оформления заголовков, параграфов, списков и т.д.
Блок `b-static-text` был удален из-за узкой сферы применения.

Если вам нужен блок `b-static-text`, скопируйте его на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/common.blocks/b-static-text

### [b-text-list](https://st.yandex-team.ru/ISLROMOCHKA-209)

Блок `b-text-list` предназначался для реализации  простых списков.
Блок `b-text-list` был удален.

Если вам нужен блок `b-text-list`, скопируйте его на проектный уровень переопределения:
- https://github.com/bem/bem-bl/tree/support/2.x/blocks-desktop/b-text-list

### [i-jquery_dummy_yes](https://st.yandex-team.ru/ISL-1710)

Модификатор `i-jquery_dummy_yes` был добавлен, чтобы использовать `i-bem` на сервере.
Содержит необходимый минимум из jQuery.

Для библиотеки `islands` этот код устарел и больше не используется.

Если вам нужен модификатор `i-jquery_dummy_yes`, скопируйте его на проектный уровень переопределения:
- https://github.com/bem/bem-bl/tree/support/2.x/blocks-common/i-jquery/_dummy

### [i-jquery__is-empty-object](https://st.yandex-team.ru/ISL-1713)

Элемент `i-jquery__is-empty-object` содержал расширение jQuery, в котором был  реализован метод `isEmptyObject`. Аналогичный метод
есть в jQuery [из коробки][jQuery.isEmptyObject], поэтому  необхидомсть в `i-jquery__is-empty-object` отпала.

Проверьте, что у вас на проекте в `deps.js` файлах не указана зависимость от этого элемента.
Если есть такие зависимости – удалите.

[jQuery.isEmptyObject]: https://api.jquery.com/jQuery.isEmptyObject/

### [b-page_theme_normal](https://st.yandex-team.ru/ISL-1281)

Модификатор `b-page_theme_normal` задавал серый фон для страницы.

Модификатор был удален.

Если вам нужен модификатор `b-page_theme_normal`, скопируйте его на проектный уровень:
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/common.blocks/b-page/_theme/b-page_theme_normal.css
- https://github.yandex-team.ru/lego/islands-romochka/tree/support/3.x/touch-phone.blocks/b-page/_theme/b-page_theme_normal.css

### [header2__tableau](https://st.yandex-team.ru/ISL-1723)

__Было:__

Элемент `header2__tableau` реализовывал старое «Табло сервисов», выезжающее сверху вниз.

__Стало:__

Теперь вместо него используется «Табло сервисов» в попапе, которое показывается по наведению на логотип.

Чтобы перейти на новое «Табло сервисов», добавлен модификатор `header2_tableau_yes`.

Пример  перехода на новое «Табло сервисов» :
- https://github.yandex-team.ru/lego/islands/blob/63b43a9fd1/desktop.blocks/header2/header2.examples/60-tableau.bemjson.js#L21

### [i-jquery__core](https://st.yandex-team.ru/ISL-1261)

Элемент `core` был реализован для вставки `<script>` на страницу, который загружал версию jQuery 1.8.3.
На desktop-уровне элемента находился модификатор `version`, для получения  JS-кода jQuery 1.8.3 и 1.6.2.

Элемент  `i-jquery__core` устарел. Вместо него используйте модификатор `i-jquery_version_default`.

### [i-bem__dom](https://st.yandex-team.ru/ISL-1570)

Из [i-bem__dom.js] удалены методы `liveInitOnBlockInit` и `liveInitOnBlockInsideInit`.
Вместо них используйте `liveInitOnBlockEvent` и `liveInitOnBlockInsideEvent`:

```js
liveInitOnBlockInit(blockName, callback) → liveInitOnBlockEvent('init', blockName, callback)
liveInitOnBlockInsideInit(blockName, callback) → liveInitOnBlockInsideEvent('init', blockName, callback)
```

[i-bem__dom.js]: https://github.yandex-team.ru/lego/islands/blob/b494a4d5c4/common.blocks/i-bem/__dom/i-bem__dom.js

### [i-loader](https://st.yandex-team.ru/ISL-1246)

Блок `i-loader` предназначался для загрузки бандлов (JS+CSS+BEMHTML одним файлом).
Блок устарел и был удален, так как использовался в библиотеке  `islands` только в блоке `b-mooa`, который также был удален.

Убедитесь, что блок  не используется у вас в проекте.

Если вам нужен блок `i-loader`, скопируйте его на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-romochka/blob/support/3.x/desktop.blocks/i-loader/__bem/techs/bembundle.js

### [i-request_type_ajax](https://st.yandex-team.ru/ISL-1246)

Из [i-request_type_ajax.js] удален метод `preventCallbacks`, используйте вместо него метод `abort`.

[i-request_type_ajax.js]: https://github.yandex-team.ru/lego/islands/blob/b494a4d5c4/common.blocks/i-request/_type/i-request_type_ajax.js

### [i-ua_interaction_yes](https://st.yandex-team.ru/ISL-1270)

Модификатор `i-ua_interaction_yes` заменен на отдельный блок `pointerfocus`.
Если вы использовали модификатор `i-ua_interaction_yes`, вам необходимо:
* Удалить его из зависимостей и добавить новую — для `pointerfocus`.
* В стилях произвести следующую замену:

```css
.i-ua_interaction_yes[data-interaction='pointer'] → html.pointerfocus
.i-ua_interaction_yes[data-interaction='keyboard'] → html.utilityfocus
```

### [logo__text](https://st.yandex-team.ru/ISL-1246)

Из блока `logo` удален элемент `text`, так как перестал соответствовать портальному дизайну.
Если элемент используется на вашем проекте, удалите его.

### [popup](https://st.yandex-team.ru/ISL-1246)

В блоке `popup` удален метод `repaintShadowIfNeeded`, который использовался только для обратной совместимости.
Если вы вызываете этот метод в вашем проекте, удалите его.

### [radio-button__radio_next-for-pressed](https://st.yandex-team.ru/ISL-1246)

В блоке `radio-button` удалена логика про выставление модификатора `next-for-pressed` у элемента `radio`.
Теперь для тех же целей используется [adjacent sibling combinator].

```css
/* Было */
.radio-button__radio_next-for-pressed_yes { }
/* Стало */
.radio-button__radio_pressed_yes + .radio-button__radio { }
```

[adjacent sibling combinator]: http://www.w3.org/TR/css3-selectors/#adjacent-sibling-combinators

### [services-table](https://st.yandex-team.ru/ISL-1246)

В блоке удалена ширина по умолчанию, равная 800px.
Чтобы задать нужную ширину, используйте модификатор `size`.
Теперь ширину в 800px можно получить через модификатор `size_l`.

### [i-global](https://st.yandex-team.ru/ISL-1259)

У блока [i-global] удалены элементы `body`, `hover`, `link`, `quote`, `reset` и `wbr`, которые
находились на уровне desktop и содержали различные CSS Resets для сброса стилей у тегов.
Перечисленные элементы нарушали инкапсуляцию других блоков, что затрудняло использование блока `i-global`.
Основные проблемы были связаны с использованием элемента `hover`, который определелял цвет ссылок с использованием `!important`.

Убедитесь, что вы не используете эти элементы в вашем проекте.
Если вам нужны какие-то глобальные стили, скопируйте их в каждый блок.

### [b-link_type_global](https://st.yandex-team.ru/ISL-1279)

Модификатор использовался для изменения стилей элемента `i-global__hover`, который подключался по зависимостям в большинстве случаев.
Элемент `i-global__hover` был удален, и модификатор `b-link_type_global` потерял смысл.

### [i-jquery__touch](https://st.yandex-team.ru/ISL-1639)

Элемент `i-jquery__touch` содержал JS-код jQuery Mobile, который не используется в библиотеке `islands`. Версия не обновлялась с 2011 года.
Если библиотека jQuery Mobile нужна вам в проекте, подключите ее другим удобным вам способом.

### [i-geodata](https://st.yandex-team.ru/ISLROMOCHKA-189)

Шаблоны блока содержали группу наиболее частотных регионов в виде статических данных.
Ранее, в XSLT-шаблонах блоков можно было определять регион через XScript. Например, это использовалось в блоке  `lang-switcher`. После перехода на BEMHTML такая возможность исчезла. Для удобства пользователей было решено сохранить группу наиболее частотных регионов в виде статических данных в шаблоны отдельного блока `i-geodata`.

Идея использования подобных шаблонов не оправдала себя. Поскольку данные, такие как язык пользователя, должны передаваться явно в параметрах блока, а
логика приведения региона к языку должна находиться за пределами блока, на проектном уровне или еще выше.

Поэтому  блок `i-geodata` был удален. Но его логика временно продублирована в блоке `lang-switcher`
для обратной совместимости.

Если вам нужен  блок `i-geodata, скопируйте его на проектный уровень переопределения:
- https://github.yandex-team.ru/lego/islands-romochka/tree/990ace8d70/common.blocks/i-geodata

### [Условные комментарии для IE6-7](https://st.yandex-team.ru/ISL-1258)

При использовании флага `ie: true` в `b-page__css` формируются условные комментарии для загрузки
своего CSS-файла для каждой версии IE. Раньше условные комментарии создавались для 4-х  версий IE6-9.
Так как IE6-7 уже не поддерживается, теперь условные комментарии формируются только для IE8-9.


Если вам нужно загружать CSS-файлы для IE6-7, воспользуйтесь элементом `сс` блока `b-page`:

```js
{
    block: 'b-page',
    head: [
        {elem: 'css', url: '_page.css', ie: false}, // IE10+ и остальные браузеры
        {elem: 'css', url: '_page', ie: true}, // IE8-9
        {elem: 'cc', condition: 'IE 6', content: {elem: 'css', url: '_page.ie6.css'}}, // IE6
        {elem: 'cc', condition: 'IE 7', content: {elem: 'css', url: '_page.ie7.css'}} // IE7
    ],
    content: ['...']
}
```

### [Неиспользуемые переводы](https://st.yandex-team.ru/ISL-1247)

Неиспользуемые переводы были удалены из блоков.
Это не должно повлиять на работу сервисов, однако, если вы используете локализационные ключи из библиотеки, необходимо проверить переводы.

diff: https://github.yandex-team.ru/lego/islands/commit/e384e289d612ee1240116aa9d0be53c5fa28c57b


## Pointer Events

mouse-события зачастую работают с задержкой на touch-устройствах.
Раньше мы решали эту проблему с помощью библиотеки FastClick и дополнительной подписки на touch-события.
Но desktop-версии блоков на touch-устройствах продолжали работать не стабильно, в библиотеке FastClick
регулярно появлялись новые ошибки, а код блоков приходилось разпределять по уровням переопределения.

В islands 4.0 мы решили эти проблемы переходом на унифицированные события [Pointer Events].
Рекомендуем вам использовать [Pointer Events] у себя на сервисе.

### Особенности реализации

Реализация pointer-событий находится в блоке `pointerevents` на common-уровне.
За основу был взят полифилл [PEP], в который были внесены существенные изменения для
поддержки корректной работы в IE8-9.

Из-за невозможности создания в IE8 неизвестных браузеру событий, их генерация работает через
событийный механизм jQuery. Это означает, что подписаться на pointer-события можно только с помощью jQuery.

Кроме того, была удалена поддержка CSS-свойства `touch-action`, которая в PEP реализована через
соответствующий атрибут. Отслеживание изменений атрибута и добавления новых элементов с этим
атрибутом реализуется через [MutationObserver], чего мы, к сожалению, позволить себе не можем. // ? что не может бытьреализовано в библиотеке islands?
Другие способы отслеживания изменений атрибута `touch-action` ненадежны или непроизводительны.
Поэтому мы отказались от поддержки атрибута `touch-action`.

Если вам нужно запретить вертикальное или горизонтальное прокручивание или отменить масштабирование по двойному касанию (double tap to zoom),
 необходимо перейти на более низкий уровень и работать с
touch-событиями.

В PEP атрибут `touch-action` служит еще и маркером того, что на элементе нужно генерировать pointer-события.
Такая схема обратно несовместима с механизмом подписки на pointer-события через блок `i-pointer-events` (предыдущая
реализация Pointer Events в `islands`).
Используя `i-pointer-events`, можно было подписаться на pointer-события обычным образом, не добавляя специальных атрибутов на DOM-элемент.
Мы решили поддерживать стандартную схему.

В целом pointer-события в `islands` работают так же, как в [bem-core][bem-core-pointerevents].

### Расширения

Поверх pointer-событий мы реализовали специальные события: `pointerpress/pointerrelease` и `pointerclick`.
Они также находятся на common-уровне в элементах `pressrelease` и `click` блока `pointerevents`.

#### События `pointerpress/pointerrelease`

Событие `pointerpress` — это алиас к `pointerdown`.
Событие `pointerrelease` — это алиас к `pointercancel` и `pointerup`.

Эти события полезны в блоках, у которых есть состояние `pressed` (кнопки и производные).
Если источником события была мышь, то в `pointerdown/pointerup` проверяется, что нажатие совершено левой кнопкой.

#### Событие `pointerclick`

Если источником события была мышь, то `pointerclick` — это `click` левой кнопкой мыши.
На touch-устройствах — это последовательность `pointerdown → pointerup`, между которыми не произошло `pointerleave/pointercancel`.
Также предусмотрена специальная логика по фильтрации синтетических mouse-событий, которые происходят с задержкой.

### События `tap`, `leftclick` и библиотека FastClick

В библиотеке `islands` событие `pointerclick` используется вместо [FastClick].
Блок `i-fastclick` удален из библиотеки. Если вы хотите продолжить использование, скопируйте его на проектный уровень.

События `tap` и `leftclick` больше не используются. Поэтому мы объявили элементы `tap` и `leftclick`
блока `i-jquery` устаревшими. В следующей мажорной версии элементы `tap` и `leftclick` будут удалены.

### Миграция

Переход на [Pointer Events] в библиотеке `islands` не должен вызвать конфликтов с проектным кодом.
Однако, мы рекомендуем переключиться на pointer-события и в проектном коде.
Это улучшит работу сервиса на touch-устройствах и упростит реализацию блоков.

Для перехода нужно добавить в зависимости блок `pointerevents`:

```js
// Базовые события
mustDeps: [
    {block: 'pointerevents'}
]

// Расширения `pointerclick` и `pointerpress/pointerrelease`
mustDeps: [
    {block: 'pointerevents', elems: ['click', 'pressrelease']}
]
```

Дальше все, как правило, сводится к обычному «search&replace»:

```
mouseover → pointerover
mouseenter → pointerenter
mousedown, touchstart → pointerdown
mousemove, touchmove → pointermove
mouseup, touchend → pointerup
touchcancel → pointercancel
mouseout → pointerout
mouseleave → pointerleave
click, leftclick, tap → pointerclick
```

[PEP]: https://github.com/jquery/PEP
[Pointer Events]: http://www.w3.org/TR/pointerevents/
[FastClick]: https://github.com/ftlabs/fastclick
[bem-core-pointerevents]: https://ru.bem.info/libs/bem-core/v2.7.0/desktop/jquery/#Элемент-event-блока-jquery
[MutationObserver]: https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver

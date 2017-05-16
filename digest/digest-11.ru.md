## Новости документации
* Полностью обновили [FAQ](https://ru.bem.info/methodology/faq/). 
* Внесли много мелких правок в [методологической части](https://ru.bem.info/methodology/) документации. 

Чего стоит ждать в ближайшем будущем:
* Новый большой и полезный туториал по всему БЭМ-стеку.
* Новые документы в разделе [Методология](https://ru.bem.info/method/).

## Новости библиотек

### bem-core
Выпустили bem-core [4.2.0](https://ru.bem.info/platform/libs/bem-core/4.2.0/). Версия полностью обратносовместимая, так что обновление должно быть совершенно «бесплатным».  
Главным изменением является совместимость с bem-xjst 8.x.  
Все изменения, вошедшие в релиз, описаны в [CHANGELOG](https://github.com/bem/bem-core/blob/v4/CHANGELOG.ru.md#420). 

bem-core 4.2.0 уже внедрена в project-stub.

### bem-components
Выпустили две версии `v5.1.0` и `v6.0.0`.

#### v5.1.0 
Версия v5.1.0 обратносовместимая. Обновление не должно потребовать дополнительных усилий.

Основные изменения:
* обновлена зависимость от bem-core до 4.2.0;
* добавлено визуальное оформление для `link_disabled`; 
* исправлены некоторые ошибки.

Подробности в [CHANGELOG](https://github.com/bem/bem-components/blob/v5.1.0/CHANGELOG.ru.md#510).

#### v6.0.0 
Bерсия v6.0.0 обязательно требует обновления до bem-xjst v8.x, где появились новые режимы и исправлена работа режима `extend`.
Необходимые пакеты для сборки на ENB (enb-bemxjst 8.6.7) или gulp (gulp-bem-xjst 3.0.0) уже доступны для установки.

При переходе вам может пригодиться [автоматический мигратор шаблонов](https://github.com/bem/bem-xjst/tree/master/migration#migration-tool-for-templates).

Кроме новых шаблонов версия ничем не отличается от 5.1.0.

bem-components 6.0.0 внедрена в project-stub.

Подробности в [CHANGELOG](https://github.com/bem/bem-components/blob/v6.0.0/CHANGELOG.ru.md#600).

### bem-history

Выпустили новую версию [bem-history v4.0.0](https://github.com/bem/bem-history), анонсированную в прошлом выпуске дайджеста, Версия v4.0.0 полностью совместима с bem-core v4.
Главное изменение — переименование блока `uri` в элемент `uri__querystring`, который расширяет базовую реализацию одноименного модуля из `bem-core` классом `Uri`. Методы класса сохранились без изменений.

Подробное описание изменений в [CHANGELOG](https://github.com/bem/bem-history/releases/tag/v4.0.0).

### bem-calendar
Опубликовали мини-библиотеку [bem-calendar](https://github.com/bem/bem-calendar/) на основе bem-components.

### bem-textarea-editor
Опубликовали библиотеку [bem-textarea-editor](https://github.com/tadatuta/bem-textarea-editor) с блоком `editor`, позволяющим писать текст на маркдауне с удобной панелью инструментов (примерно как на Github) и получать превью до отправки поста на сервер.

Посмотреть на работу блока в действии можно [тут](https://tadatuta.github.io/bem-textarea-editor/).

### bem-font-awesome

Опубликовали библиотеку [bem-font-awesome](https://github.com/tadatuta/bem-font-awesome), которая позволяет использовать [Font Awesome](http://fontawesome.io/) с использованием БЭМ-нотации и не тянуть лишние стили в проект.

Как установить и успользовать библиотеку, читайте в [README](https://github.com/tadatuta/bem-font-awesome/blob/master/README.md) проекта или в [посте на форуме](https://ru.bem.info/forum/1272/).

### bem-font-awesome-icons

Опубликовали альтернативный вариант библиотеки `bem-font-awesome` — [bem-font-awesome-icons](https://github.com/tadatuta/bem-font-awesome-icons), где распилили шрифт на отдельные SVG-иконки, так что теперь на клиент приедет только то, что реально используется. 

Подробности в [документации](https://github.com/tadatuta/bem-font-awesome-icons/blob/master/README.md) и [на форуме](https://ru.bem.info/forum/1274/).

## Новости БЭМ из мира React

### https://github.com/bem/create-bem-react-app

как реактовый проджект стаб


### bem-react-core
Выпустили bem-react-core v0.2.0.

Основные изменения:
- Рендер без CSS-класса (bem:false).
- Поддержка пропса `cls`.
- Доопределение статических полей `defaultProps` и `propTypes`.
- Поддержка пропса `mix`.
- Сокращенный синтаксис декларации модификаторов.
- Новый `this.__base(...arguments)`.
- Поддержка [HOC](https://facebook.github.io/react/docs/higher-order-components.html)(redux, flux и других оберток).

Помимо того, что написали подробную документацию – [REFERENCE](https://github.com/bem/bem-react-core/blob/master/REFERENCE.ru.md), мы много и интересно рассказывали про нашу новую библиотеку:
* Провели вебинар [Немного БЭМ в вашем React](https://www.youtube.com/watch?v=UeqPPkGXmbE).
* Рассказали на [митапе по БЭМ](https://events.yandex.ru/events/bemup/24-march-2017/). Видео доклада [Новости БЭМ из мира React](https://ru.bem.info/forum/1320/) уже доступно на канале [bem.info](https://www.youtube.com/channel/UCsHVzqjMO31I8qKHhWHsobg).
* Рассказали на [React Moscow Meetup #2](https://events.yandex.ru/events/yagosti/15-march-2017/).

### bem-react-components


### webpack-bem-loader

* Добавили генератор i18n, который обеспечивает возможность локализации компонентов.
Ability to use bem-config per level (#25)

появилсь возм колнфигурировать каждый уровень сборки отдельно с помошью bem-config


## Новости технологий

### bem-express
Обновили bem-core до версии [4.2.0](https://ru.bem.info/platform/libs/bem-core/4.2.0/) и bem-components до [6.0.0](https://github.com/bem/bem-components/releases/tag/v6.0.0).

### project-stub

Обновили версии библиотек [bem-components v6.0.0](https://ru.bem.info/platform/libs/bem-components/6.0.0/) и [bem-core v4.2.0](https://ru.bem.info/platform/libs/bem-core/4.2.0/) и другие зависимости.

### bem-xjst

Выпустили следующие релизы: 

* **[v8.6.11](https://github.com/bem/bem-xjst/releases/tag/v8.6.11)**  
    * Исправлена ошибка: переданные `oninit()` во время второй и последующих вызовов `compile()` не вызывались. Теперь это исправлено.

* **[v8.6.10](https://github.com/bem/bem-xjst/releases/tag/v8.6.10)**  
    * Исправлена ошибка, приводящая к утечке памяти.
    
* **[v8.6.9](https://github.com/bem/bem-xjst/releases/tag/v8.6.9)**  
    * Исправлена ошибка про некорректную работу `this.reapply()` и `depth`.
    * Исправлена ошибка с отсутствием `i-bem` при миксе элемента с `js`.
    * Исправлена ошибка с `ApplyCtx`.

* **[v8.6.8](https://github.com/bem/bem-xjst/releases/tag/v8.6.8)**  
    * BEMTREE и BEMHTML: добавлена возможность подключения сторонних библиотек как глобально, так и для разных модульных систем с помощью опции `requires`.

* **[v8.6.7](https://github.com/bem/bem-xjst/releases/tag/v8.6.7)**  
    * Повторное использование функции-колбека в `recompileInput`.

* **[v8.6.6](https://github.com/bem/bem-xjst/releases/tag/v8.6.6)**  
    * Исправлена ошибка с `oninit`, которая возникла в результате минификации бандлов.

* **[v8.6.5](https://github.com/bem/bem-xjst/releases/tag/v8.6.5)**  
    * Исправлена ошибка: теперь опция `unquotedAttrs: true` в BEMHTML should escape attributes

* **[v8.6.4](https://github.com/bem/bem-xjst/releases/tag/v8.6.4)**  
    * Компилятор `.generate()` больше не требуется в рантайме.

* **[v8.6.3](https://github.com/bem/bem-xjst/releases/tag/v8.6.3)**  
    * Исправлена ошибка в теле шаблона функции и `appendContent()/prependContent()`. 

* **[v8.6.2](https://github.com/bem/bem-xjst/releases/tag/v8.6.2)**  
    * Исправлена ошибка при использовании `match()` без аргументов.

* **[v8.6.1](https://github.com/bem/bem-xjst/releases/tag/v8.6.1)**  
    * Размер бандлов BEMHTML и BEMTREE уменьшен (–6%).
    * В теле функции-колбека `match` вы можете использовать `apply()` для вызова любого режима, относящегося к данному узлу.

* **[v8.6.0](https://github.com/bem/bem-xjst/releases/tag/v8.6.0)**  
    * Создан [автомигратор](https://github.com/bem/bem-xjst/blob/master/migration/README.md#migration-tool-for-templates), который умеет править код проектных шаблонов так, чтобы он начал соответствовать указанной мажорной версии.
    * Реализовали [статический линтер](https://github.com/bem/bem-xjst/blob/master/migration/README.md#static-linter-for-templates), который обеспечивает запуск статической проверки для ваших шаблонов и включает их (наравне с runtime-проверками) в ваш процесс разработки.
    * Подробности в [CHANGELOG](https://github.com/bem/bem-xjst/blob/v8.6.0/CHANGELOG.md).

Важный [пост по миграции проектных шаблонов](https://github.com/bem-site/bem-forum-content-ru/issues/1239), на случай, если вы пропустили. 

### deps

Gjzdbkcz gfrtn пферукштп вузы,позволяющий собрать новые зависимости: [https://www.npmjs.com/package/gather-reverse-deps](https://www.npmjs.com/package/gather-reverse-deps).

## Новости инструментов

### bem-naming
    
Выпустили пакеты [2.0.0-5](https://github.com/bem-sdk/bem-naming/tree/v2.0.0-5) и [2.0.0-6](https://github.com/bem-sdk/bem-naming/tree/v2.0.0-6)

* Теперь, если не указан разделитель значения модификатора, он не наследуется от разделителя имени модификатора и возвращается к значению по умолчанию `bemNaming.modValDelim`.
* Добавлено поле `delims` вместо` elemDelim`, `modDelim` и` modValDelim` для соответствия функции `bemNaming`.

### borschik

Выпустили новую версию [borschik v1.7.0](https://github.com/borschik/borschik/tree/v1.7.0), где хеш-функция, используемая при фризе статики, была вынесена в отдельный пакет [borschik-hash](https://github.com/borschik/borschik-hash).
Прекращена поддержка node 0.8.0
Подробности в [CHANGELOG](https://github.com/borschik/borschik/blob/v1.7.0/CHANGELOG.ru.md).

Обновили [документацию](https://github.com/bem/webpack-bem-loader/blob/v0.4.0/README.md).

### bem-tools-create

починили проблемы
добавили поддержку опций
пользуйтесь


## Новости мероприятий и сообщества

* Провели целую серию BEMup'ов:  
  * Продолжили проводить [BEMup'ы для новичков](https://events.yandex.ru/events/bemup/27-january-2017/) — на этот раз встреча была для тех, кто уже имеет представление о базовых понятиях методологии. Опубликовали [скринкаст](https://www.youtube.com/watch?v=cDLhO3Psygc&feature=youtu.be). [Тут](https://youtu.be/Ai-yt0b8iKE) лежит видео с первого BEMup'а для новичков, для тех, кто пропустил начало.  
  * Рассказали про сборку БЭМ-проектов с enb и про все новости БЭМ из мира React на [BEMup'е в Москве](https://events.yandex.ru/events/bemup/24-march-2017/). Конечно, опубликовали [видео](https://ru.bem.info/forum/1320/).  
  * Провели [Bemup в Екатеринбурге](https://events.yandex.ru/events/bemup/13-april-2017/) для разработчиков, использующих БЭМ в своих проектах и желающих больше узнать про БЭМ-технологии.  
  * Провели [мастер-класс](https://events.yandex.ru/events/bemup/21-april-2017/), на котором написали проект на основе `project-stub`. На живых примерах показали, для чего нужны технологии BEMJSON, BEMTREE, BEMHTML, DEPS, и как использовать их вместе. Опубликовали [скринкаст](https://www.youtube.com/watch?v=z6BqV4r6JkA).  
* Антон Виноградов провел вебинар [Немного БЭМ в вашем React](https://www.youtube.com/watch?v=UeqPPkGXmbE), где рассказал, как начать использовать bem-react-core — декларативно описывать React-компоненты, гибко их доопределять и использовать уровни переопределения.
* Сергей Бережной рассказал, [что нового в bem-react-core](https://events.yandex.ru/lib/talks/4484/) на [React Moscow Meetup #2](https://events.yandex.ru/events/yagosti/15-march-2017/).
* Владимир Гриненко выступил на United Dev Conf в Минске с докладом [Dependencies in component web done right](http://unitedconf.com/dokladchiki/dependencies-in-component-web-done-right/). [Слайды](https://yadi.sk/d/uaRNnF_v3Gim9K) к докладу в keynote.
* http://dump-conf.ru/section/27/

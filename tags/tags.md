Существующие теги (47)

## Задачи

* Сократить количество тегов.
* Сделать единый список тегов для форума и сайта.
* Сделать многоуровневую систему тегирования. 

## Реализация

Верхний уровень тегов (6)
	* methodology
	* toolbox
	* platform
	* community
	* news
	* tutorials
	* tests ?

Можно выбрать несколько тегов верхнего уровня для поста на форуме. Например: news + platform или  methodology + tutotials.

После выбора тега верхнего уровня пользователю становятся доступны теги следующего уровня.

* methdology (3)
	* css
	* html
	* js
	
* toolbox (5)
	* bem-sdk (5)
		* bem-naming
		* bem-config
		* bem-fs-scheme
		* bem-deps (!!! ВАЖНО: может возникнуть путаница из-за разных тегов deps и bem-deps)
		* bem-walk
	* bem-tools
	* bemhint 
	* bemmet
	* enb 
	
* platform (8)
	* bemjson
	* bemtree
	* bemhtml
	* i-bem
	* deps
	* project-stub
	* bem-xjst
	* libraries (3)
		bem-components
		bem-core
		bem-history

* community (4)
	* events
	* video
	* webinar
	* jobs


## Что делаем?
	* ide
	* i18n
	* bh - закопали?
	* borschik - закопали?
	* ymodules - закопали?

Нам нужны теги про тестирование? На форуме их используют. Предлагаю оставить один — tests. Вынести его на верхний уровень.

## Убираем  

enb — весь зоопарк тегов про enb. Тег дублирует название.
bem.info
	bemhint-fs-naming — Тег дублирует название.
	bemhint-deps-specification — Тег дублирует название.
bugs
bytheteam
deploy
deps.js
design
documentation
gemini
generator-bem-stub
hackaton
optimizers
testing
tools

## Статус существующих тегов

api — убираем
asktheteam — убираем
bem-bl — убираем
bem-components
bem-core
bem-forum — убираем
bem-mvc — убираем
bem-naming — 2ой уровень
bem-tools — 2ой уровень
bem.info — убираем
bemhtml — 2ой уровень
bemjson — 2ой уровень
bemtree — 2ой уровень
bh — убираем ? 
borschik — убираем ?
bugs — убираем
bytheteam — убираем
community — 1й уровень
css — 2ой уровень
deploy — убираем
deps.js — меняем на deps 
design — убираем
documentation — убираем
enb — 2ой уровень
events — 2ой уровень
gemini — убираем
generator-bem-stub — убираем
hackaton — убираем
html — 2ой уровень
i-bem — 2ой уровень
i18n — ?
ide — ?
jobs — 1й уровень
js — 2ой уровень
libraries — 2ой уровень
methodology — 1й уровень 
news — 1й уровень
optimizers — убираем
project-stub — 2ой уровень
talks — убираем 
technologies — убираем
testing — меняем на tests
tools — убираем
video — 2ой уровень
webinar — 2ой уровень
xjst - убираем
ymodules — убираем ?  
 
## Сокращенный вариант

В итоге получилось около 35 тегов. Сократить до 15 не получается. Даже если поубирать всё

Верхний уровень тегов (6)
	* methodology
	* toolbox
	* platform
	* community
	* news
	* tutorials

* methdology (0)
	
* toolbox (5)
	* bem-sdk (5)
		* bem-naming
		* bem-config
		* bem-fs-scheme
		* bem-deps
		* bem-walk
	* bem-tools
	* bemhint 
	* bemmet
	* enb 
	
* platform (8)
	* bemjson
	* bemtree
	* bemhtml
	* i-bem
	* deps
	* project-stub
	* bem-xjst
	* libraries (0)
		
* community (3)
	* events
	* video
	* jobs


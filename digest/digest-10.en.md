Brief news about the BEM world since the beginning of this year:

## Documentation news
* Updated document [BEM project building methodology](https://en.bem.info/methodology/build/). 
* Published a new document about deps.js — [a technology to declare dependencies](https://en.bem.info/platform/deps/).
*  Made a lot of minor changes in [the methodological part](https://en.bem.info/methodology/) of documentation.

## Site news 
Rolled out the [BEM libraries](https://en.bem.info/platform/libs/) section in a new design on [bem.info](https://en.bem.info/):
* [bem-components](https://en.bem.info/platform/libs/bem-components/5.0.0/)
* [bem-core](https://en.bem.info/platform/libs/bem-core/4.1.1/)

## Libraries news

### bem-core
* Released bem-core [4.1.0](https://en.bem.info/platform/libs/bem-core/4.1.0/) and [4.1.1](https://en.bem.info/platform/libs/bem-core/4.1.1/). All changes of the both releases are described in a [CHANGELOG](https://en.bem.info/platform/libs/bem-core/4.1.1/changelog/#411). 

### bem-components
* Released [bem-components v4.0.0](https://en.bem.info/platform/libs/bem-components/4.0.0/) with update of controls design and with the transition from the Stylus to postCSS.
* Released [bem-components 5.0.0](https://github.com/bem/bem-components/releases/tag/v5.0.0) that used [bem-core 4.1.1](https://en.bem.info/platform/libs/bem-core/4.1.1/). There are two style sets in v5.0.0: source files with postCSS and compiled CSS in case you prefer to use some preprocessor.

### bem-history
* Not yet released but is already used in a separate [branch v4](https://github.com/bem/bem-history/tree/v4). This version is compatible with `bem-core v4`. Feel free to try, revise and send us a feedback before we release a new version.
The major change is rename of `uri` block to `uri__querystring` element, which extends the basic implementation of the same name module in `bem-core` with `Uri` class. Class methods remained unchanged.

### bem-react-core
* Working on [BEM React Core](https://github.com/bem/bem-react-core) — a core library for develop with React and BEM methodology.
* Released a complete documentation package: [README](https://github.com/bem/bem-react-core/blob/master/README.md), [REFERENCE](https://github.com/bem/bem-react-core/blob/master/REFERENCE.md) и [CONTRIBUTION GUIDE](https://github.com/bem/bem-react-core/blob/master/CONTRIBUTING.md). 

## Technologies news

### bem-express
Released the following major updates:
* Updated bem-core to [4.1.1](https://en.bem.info/platform/libs/bem-core/4.1.1/) and bem-components to [5.0.0](https://github.com/bem/bem-components/releases/tag/v5.0.0).
* Began to use PostCSS instead of Stylus. We provide the same set of plugins that bem-components has.
* Implemented an optional `livereload`. For details read the [documentation](https://github.com/bem/bem-express/blob/master/development.blocks/livereload/livereload.md) and [README](https://github.com/bem /bem-express/blob/master/README.md) of the project.
* Achieved the speed acceleration of the build procedure by updating `npm`-modules required for assembly.
* Abandoned `bower` to supply libraries. Now all dependencies are set through `npm` in `node_modules` directory. 

### bem-xjst

* **[v8.3.1](https://github.com/bem/bem-xjst/releases/tag/v8.3.1) (v7.4.1)**  
    * `extend()` mode fixed. Now it works as expected.
    * Documentation updated: `this.extend(o1, o2)` description added.

* **[v8.4.0](https://github.com/bem/bem-xjst/releases/tag/v8.4.0) (v7.6.0)**  
    * New `unquotedAttrs` option allows us to ommit unnececary quotes of HTML attributes in some cases.

* **[v8.4.1](https://github.com/bem/bem-xjst/releases/tag/v8.4.1) (v7.6.1)**  
    * `extend(function(ctx, json) { … })` mode callback now have two arguments, the same as have the callbacks of other modes. The first argument is a link to context of `(this)` template execution. The second — link to a BEMJSON node.

* **[v8.4.2](https://github.com/bem/bem-xjst/releases/tag/v8.4.2)**  
    * Escaping functions fixed: now argumengs `undefined` or `null` and `NaN` will give you empty string as a result. In previous versions you get stringified results (`'undefined'`, `'null'`, `'NaN'`).

* **[v8.5.0](https://github.com/bem/bem-xjst/releases/tag/v8.5.0)**  
    * BEMTREE: added modes related to data: `js()`, `addJs()`, `mix()`, `addMix()`, `mods()`, `addElemMods()`, `elemMods()`. The rest of the modes that are relevant only to-HTML are available in BEMHTML.

* **[v8.5.1](https://github.com/bem/bem-xjst/releases/tag/v8.5.1)**  
    * Fixed bug: calculate position if block/elem was replaced via `replace()`.

* **[v8.5.2](https://github.com/bem/bem-xjst/releases/tag/v8.5.2) (v7.6.4)**  
    * Fixed bug in BEMTREE related to the rendering of special value field `content` `{ html: '<unescaped value>' }`.  
    * [bem-xjst onlinе demo](http://bem.github.io/bem-xjst/) updated:    
	    * Added a switch of BEMHTML/BEMTREE engines.  
	    * Added a plug for `BEM.I18N ()`, which returns its second argument. This is useful to copy the code from production to a sandbox.  
    * [README](https://github.com/bem/bem-xjst/blob/master/README.md) updated.    
    * A sandbox updated. You could help our projest to fix issues with [help wanted](https://github.com/bem/bem-xjst/issues?q=is%3Aissue+is%3Aopen+label%3A"help+wanted") lable.


### enb-bemxjst

* Released a new version of [enb-bemxjst v8.5.2](https://github.com/enb/enb-bemxjst/tree/v8.5.2)  with `"dependencies": { "bem-xjst": "8.5.2" }`. However, we continue to actively support the both branches: 7.x and 8.x.

All changes are described in releas notes [v8.5.2](https://github.com/bem/bem-xjst/releases/tag/v8.5.2) and [v7.6.4](https://github.com/bem/bem-xjst/releases/tag/v7.6.4) and in a [CHANGELOG](https://github.com/enb/enb-bemxjst/blob/v8.5.2/CHANGELOG.md).

## BEM toolbox news

### bem-tools

Released [bem-tools 2.0.0](https://github.com/bem-tools/bem-tools) with updated [bem-tools-create](https://github.com/bem-tools/bem-tools-create). Detailed description is in [README](https://github.com/bem-tools/bem-tools-create/blob/master/README.md).

### bem-walk

Wrote a full and clear [README](https://github.com/bem-sdk/bem-walk/blob/master/README.md).

### project-stub

* Implemented a new version of [bem-components v5.0.0](https://ru.bem.info/platform/libs/bem-components/5.0.0/) taking into account the transition to postCSS and a new version of [bem-tools 2.0.0](https://github.com/bem-tools/bem-tools).
* As an experiment, included [gulp-bem](https://github.com/gulp-bem) in a project-stub.

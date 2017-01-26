The Yandex-developed BEM methodology has been the topic of many articles around the Web. We have decided it's time to offer a more structured look into its origin and what made BEM what it is today. We believe it will be interesting not just for existing BEM practitioners but also for those who consider this methodology to be unsuitable for their particular projects. They may find that we were addressing issues similar to their own and discover something of benefit to themselves.

<img src="https://habrastorage.org/getpro/habr/post_images/09d/d3b/d6d/09dd3bd6d684e767c5f4d5b9564607d8.png" alt="image"/>

Naturally, it was Yandex’s own needs that kicked it all off. As the company grew, so did the number of staff involved in front-end development. Eventually the team reached the size when it became obvious that it would be difficult to work without some common standards. Besides, we are based in different Yandex offices in different cities. An idea of a common methodology emerged, one that would help set up processes in a big team working on different projects. Crucially, our goals were not limited to streamlining and speeding up the development process — we also wanted to shorten the learning curve for newcomers.

## What the BEM methodology is for
These are the requirements that we identified:

* Developers should always understand their own code (even a year on after they wrote it) as well as the code of any programmer in their BEM project team.
* Any code block can be re-used: we must create a common knowledge base and use ready-made solutions where possible instead of starting from scratch every time.
* When working in the same team, programmers, managers, designers, and front-end developers must all stick to the same terminology. In other words, they must use the same lingo.
* Teams can exchange engineers for implementing some specific functionality.
* The learning curve for anyone joining a new project must be shortened by using a standard organizational structure across all of the BEM projects and standard naming rules for all entities.

We were aiming for a model whereby having more developers in a team results in better quality of the product. That means that the developers must keep abreast of each other’s work and not reinvent something that’s already been implemented. We wanted to create a single team that works on different projects.


<a name="present"> </a>
## The basics of BEM methodology
Technologies (HTML, CSS, JavaScript) that we used varied from project to project, but the BEM principles had to be universal.

We formulated the main rules to govern the progress and development of our projects, rules that would be technology- and tool-agnostic.

To speed up the development process, we needed to make the maintenance of HTML and CSS for individual page components easier, to reduce code coupling. To achieve that, we split the page into separate parts. That’s how a new concept was born — a [block](https://en.bem.info/method/key-concepts/#block). A block could consist of different [elements](https://en.bem.info/method/key-concepts/#element), which were not used outside of the block itself. The state and behavior of a block and an element could be defined using [modifiers](https://en.bem.info/method/key-concepts/#modifier).


### Block
A logically and functionally independent page component. A block is fully self-contained: it can have its own behavior, templates, styles, documentation, and more. Blocks can be placed on a page wherever you need them, the same block can be used more than once, even in different projects.

Blocks can be nested inside each other, grouped together, used for creating compound blocks.

<img src="https://github.com/innabelaya/bem-docs/blob/master/SM/block.png" alt="image"/>


### Element
A constituent part of a block that can’t be used outside of it and makes sense only in the context of its parent block. Elements can be mandatory or optional.

An important tip to bear in mind when dealing with elements: it is not recommended to create elements of elements. Embedding one element into another makes it impossible to change the internal structure of the block: elements cannot be swapped around, removed or added without modifying the existing code.

<img src="https://github.com/innabelaya/bem-docs/blob/master/SM/elem.png" alt="image"/>


### Modifier
A property of a block or an element that modifies their appearance, state or behavior.
A modifier has a name and it can also have a value. The use of modifiers is optional. One block or element can have several different modifiers.

For example, a modifier can be used to change not only the color of the sword but also its functionality (as is shown in the example with the red sword):

<img src="https://habrastorage.org/getpro/habr/post_images/81c/325/0b2/81c3250b2a8b7cbb09420e82aed2c34c.png" alt="image"/>


### Naming rules for BEM blocks, elements and modifiers
All of the BEM principles were developed and adopted in stages. We began by defining strict rules for naming [BEM entities](https://en.bem.info/methodology/key-concepts/#bem-entity).

* Individual words within names are separated by a hyphen (`-`).
* An element name is delimited by a double underscore (`__`). A modifier name is delimited by a single underscore (`_`).
* Names of BEM entities are written using numbers and lowercase Latin characters.

A block became an important defining entity in the naming of BEM entities:

* The full name of an element/modifier is formed in such a way as to indicate what block this element/modifier belongs to.
* The name of an element modifier must give a clear indication as to what specific element of what specific block this modifier belongs to.

**Example**

* Block name — `header`.
* Block element name — `header__search-form` — the element `search-form` of the block `header`.
* Block modifier name — `header_theme_green-forest` — the modifier `theme` witht the value `green-forest` of the block `header` .
* Element modifier name — `header__search-form_disabled` — the boolean modifier `disabled` of the element `search-form` of the block `header`.

There are also some [alternative naming schemes](https://en.bem.info/method/naming-convention/#alternative-naming-schemes). You are free to choose whichever one you like.

However, we recommend using the scheme above as it is the one that the [BEM platform](https://en.bem.info/platform/) tools are specifically geared towards.


## BEM in CSS
In the BEM methodology, blocks are not unique and can always be re-used, so we abandoned the use of tag and ID selectors in describing CSS rules. The styles of BEM blocks and elements are defined by class selectors only.

Class selectors allow you to select a particular HTML element on a page, independent of the tag. The class selector is accessed via the class attribute that is required for every HTML element.

The name of the class selector should fully and accurately describe the entity represented by the selector.

As an example, let's look at these four lines of CSS code:

```css
.button {}
.button__icon {}
.button__text {}
.button_theme_islands {}
```

We can be fairly sure that we are dealing with a single block, and its HTML implementation looks like this:

```html
<button class="button button_theme_islands">
    <span class="button__icon"></span>

    <span class="button__text">...</span>
</button>
```

It's harder to make the same assumption with a group of selectors like this:

```css
.button {}
.icon {}
.text {}
.theme_islands {}
```

The names `icon`, `text`, and `theme_islands` aren't as informative.

The general rules for naming blocks, elements, and modifiers allow you to:

* Make the names of CSS selectors as informative and clear as possible.
* Solve the problem of name collisions.
* Independently define styles for blocks and their optional elements.

**Example**

HTML

```html
<!-- `logo` block -->
<div class="logo logo_theme_islands">
    <img src="URL" alt="logo" class="logo__img">
</div>

<!-- `user` block -->
<div class="user user_theme_islands">
    <img src="URL" alt="user-logo" class="user__img">
    ...
</div>
```

Naming CSS classes:

```css
.logo {}                  /* CSS class for the `logo` block */

.logo__img {}             /* CSS class for the `logo__img` element */

.logo_theme_islands {}    /* CSS class for the `logo_theme_islands` modifier */

.user {}                  /* CSS class for the `user` block */

.user__img {}             /* CSS class for the `user__img` element */

.user_theme_islands {}    /* CSS class for the `user_theme_islands` modifier */
```

The value of the class attribute can be a space-separated list of words. This allows you to use several BEM entities on a single DOM node.

**Example**

HTML

```html
<header class="header">
    <!--
    `header__button` — element of the `header` block;
    `button` — block;
    `button_theme_islands` — modifier.
    -->
    <button class="header__button button button_theme_islands">...</button>
</header>
```

A block must neither be dependent on nor affect other blocks around it, which is why in CSS we recommend abandoning the use of:

* [Nested selectors](#nested-selectors)
* [Combining a tag and a class in a selector](#combining-a-tag-and-a-class-in-a-selector)

### Nested selectors
The BEM methodology does allow using selectors like this, but we recommend keeping them to a minimum. Nested selectors increase code coupling and make reuse impossible.

For example, nesting is appropriate if you need to change the styles of elements relative to the state of the block or the theme set.

**Example**

CSS

```css
.button_hovered .button__text
{
    text-decoration: underline;
}

.button_theme_islands .button__text
{
    line-height: 1.5;
}
```

### Combining a tag and a class in a selector
The BEM methodology not recommended to combine a tag and a class in a selector. Combining the tag and the class (for example, button.button) makes the CSS rules more specific, which makes it more difficult to override them. This starts priority battles, in which stylesheets are loaded by overly complicated selectors.

**Example**

HTML

```html
 <button class="button">...</button>
```

CSS rules are defined in the `button.button` selector.


Let's say we added the active modifier to the block and set the value to true:

```html
<button class="button button_active">...</button>
```

`.button_active` selector doesn't redefine the block properties written as `button.button`, because `button.button` has a weight higher than `.button_active`. For successful redefinition, the selector for the block modifier also must be combined with the `button.button_active` tag.

As the project develops, it's possible that blocks could be added with the selectors `input.button`, `span.button` and `a.button`. In this case, all the modifiers of the button block and nested elements would require four different declarations for each case.

Try to use only class selectors:

```css
.button {}
.button_active {}
```

## BEM in HTML
We wanted to structure HTML and eventually found ourselves no longer writing HTML manually. For details, see the section describing the [BEM toolbox](#tools).

In HTML, every BEM entity is defined by its class name.

```html
<div class="block-name">
    <div class="block-name__elem"> </div>
    ...
</div>
```

In the simplest case, there is a one-to-one correspondence between a block and a DOM node. However, a DOM node does not always equal a block. A single DOM node can host several BEM entities. This is called a [mix](https://en.bem.info/method/key-concepts/#mix).

Mixes allow you to

* Combine the behaviors and styles of several BEM entities while avoiding code duplication.
* Create semantically new interface components on the basis of existing blocks, elements, and modifiers.
* Define the position of a nested block inside its parent without creating any additional modifiers.

Let's describe a few examples to show how mixes work:

* [External geometry and positioning](#position)
* [Styling groups of blocks](#style)

<a name="position"> </a>
### External geometry and positioning
In CSS with BEM, styles that are responsible for the external geometry and positioning are set via the parent block.

**Example**

HTML

```html
<!-- `header` block -->
<header class="header">
    <button class="button">...</button>
</header>

<!-- `form` block -->
<form class="form" >
</form>
CSS implementation of a button:
```

CSS 

```css
.button {
    font-family: Arial, sans-serif;
    text-align: center;
    border: 1px solid black;
    margin: 30px;               /* Margin */
}
```

**Task**

Use the button from `header` block in the `form` block.

The `button` block includes a padding of 30px, which may prevent its reuse.

The solution looks like this:

**Example**

HTML

```html
<!-- `header` block -->
<header class="header">
    <!-- Added the `header__button` class to the `button` block -->
    <button class="button header__button">...</button>
</header>

<!-- `form` block -->
<form class="form" >
    <button class="button">...</button>
</form>
```

CSS implementation of a button:

```css
.button {
    font-family: Arial, sans-serif;
    border: 1px solid black;
}
```

CSS implementation of the `button` element in the `header` block:

```css
.header__button {
    margin: 30px;
    position: relative;
}
```

In this example, the external geometry and positioning of the `button` block are set via the `header__button` element. Now the `button` block is independent and universal, because it doesn't specify any padding.

<a name="style"> </a>
### Styling groups of blocks
Sometimes you need to apply the same formatting to multiple different HTML elements on a page at once. Group selectors are often used for this purpose.

**Example**

HTML

```html
<article class="article"></article>

<footer class="footer">
    <div class="copyright"></div>
</footer>
```

CSS

```css
article, .footer div {
    font-family: Arial, sans-serif;
    font-size: 14px;
    color: #000;
}
```

In this example the text inside the `article` and `copyright` blocks has the same color and font.

Although group selectors do allow you to quickly change the design of the page, this approach tightens code coupling.

This is why BEM uses mixes to uniformly format an entire set of HTML elements.

**Example**

HTML

```html
<article class="article text"></article>

<footer class="footer">
    <div class="copyright text"></div>
</footer>
```

CSS

```css
.text {
    font-family: Arial, sans-serif;
    font-size: 14px;
    color: #000;
}
```

## File system organization
We weren’t quite happy with the original project file structure: it was difficult to navigate and locating needed entity technologies within it wasn't too easy, either.

What we were looking to achieve with the new structure:

* A unified file system for all BEM projects.
* A universal extensible repository structure. For any new technology that is added to a project, associated files are stored in a pre-determined location.
* A quick search through the file system of a project.
* Code re-use.
* Unlimited moving of the code of an entire block between projects.

### Blocks before technologies
In a bid to create the desired project structure and fulfill our goals, we shifted the focus from technologies to blocks.

A block in a file system is absolutely independent: all the technologies required for its implementation are kept in the block directory.

We invented a new term — **implementation technology**.

Blocks can have different functions on a page. The implementation of a block may vary depending on its purpose. In BEM, implementation means behavior, appearance, templates, documentation for a block, all types of tests, images, etc.

Blocks can be implemented in one or more technologies, for example:

* behavior — JavaScript, CoffeeScript
* appearance — CSS, Stylus, Sass
* templates — Jade, Handlebars, XSL, BEMHTML, BH
* documentation — Markdown, Wiki, XML.

You are not limited in your choice of implementation technologies, except maybe by your project requirements.

The BEM file structure is organized in such a way that each implementation technology is represented by an individual file with the corresponding extension. All implementation files for a block are stored in that block directory.

### Principles of file system organization for BEM projects

* A block is represented by a separate directory in the file system. The block directory has the same name as the block.
* A block implementation is divided into separate files.
* Files related to a block are always stored in the block directory.
* Optional elements and modifiers are stored in separate files.
* A project is divided into [redefinition levels](https://en.bem.info/method/key-concepts/#redefinition-level).


**Example**

```files
blocks/
    input/                  # input block directory
        _theme/             # theme optional modifier directory
            input_theme_forest.css # Implementation of theme modifier with the value forest in CSS technology
        __clear/             # clear optional modifier directory
            input__clear.css # Implementation of clear optional modifier in CSS technology
            input__clear.png # Implementation of clear element in PNG technology
        input.css            # input block in CSS technology
        input.js             # input block in JavaScript technology
    button/                  # button block directory
        button.css
        button.js
        button.png
```

### What we achieved by dividing code into parts
**Faster development**

* Blocks can be re-used.
* Block implementations can be modified on a new redefinition level without affecting the basic functionality and styles.
* A block is an independent page component; everything that is needed to ensure the proper functioning of a block is contained within the block directory. This makes it easy to move blocks from project to project: you only need to copy the block directory.

**Faster refactoring**

* Developers work with small blocks of code.
* The implementation technologies of one block are separated from those of any other block.
* The uniform repository structure allows you to navigate the project and locate necessary files easily.

**Universal extensible system**

* [Redefinition levels](#level) appeared.
* The number of technologies is unlimited. Any new implementation technologies is contained in a file for a specific block. For instance, we weren't expecting to write unit tests in JavaScript at the time of creating the new file structure. But when later we found ourselves needing to do just that, we knew where in the project to put the files.

<a name="level"> </a>
### Redefinition level

Redefinition level is the term we use to refer to directories containing block implementations. The introduction of levels enabled us to change the implementation of a block by adding new properties (extending) or modifying old ones (overriding) on a different level. The final implementation of a block is assembled from all the levels in consecutive order.

Redefinition levels allow us to
* Link libraries and update them without changing the code.
* Store common parts of block implementations on one level and specific instances (e.g., specific implementation for particular services) on another.
* Divide a project into platforms. Store implementation common to all platforms on one level and store platform-specific implementation on another.
* Avoid code duplication and the creation of new entities if existing functionality needs to be changed. If we compare levels to layers, then the basic layer is the original implementation of the block, and each next layer is added on top and complements (inherits) or modifies the basic implementation.

<img src="https://en.bem.info/kqvCO2ZXeivuLHCbn2to5chFZrM.png" alt="image"/>

Let's describe some examples how redefinition levels work to be more clear:

* [How to update a library linked to your project](#link-lib)
* [How to test some changes in your project](#test-project)

<a name="link-lib"> </a>
#### How to update a library linked to your project

There is a third-party library linked to a project on a separate level. The library contains ready-made block implementations. The project-specific blocks are stored on a different redefinition level.

```files
project/            # Project level
library-blocks/     # Library level
    input/          # Basic implementation of input block
    button/         # Basic implementation of button block
    popup/          # Basic implementation of popup block
```

Let's say we need to modify the appearance of one of the library blocks. That doesn't require changing the CSS rules of the block in the library source code or copying the code at the project level. We only need to create additional CSS rules for that block at the project level. 

```files
project/            # Project level
    input/          # Modified implementation of input block
    button/
    header/
library-blocks/     # Library level
    input/          # Basic implementation of input block
    button/
    popup/
```

During the build process, the resulting implementation will incorporate both the original rules from the library level and the new styles from the project level. So, you didn't change any string of code in the linked library. You could update it whenever you need. All changes made for your project would be stored on `project` level and would be available after the library update.

<a name="test-project"> </a>
#### How to test some changes in your project
Another practical case of redefinition level usage is execute A/B testing in your project, without changing the code of the project itself.

**Example**

You have to make some requested changes in your project. For example, you have to change the header on all pages. You need to test all cases and choose the best one. To test the result and not to ruin your project or not to write long story about `if` statements, you just create new redefinition levels for tests on a project.

You could create as much levels as you want to test new functionality. 

```files
project/            # Project level that works good
    header/         # Current header block of the project
    ...
test-1/             # Level for test 1
    header/         # header block with some changes 
test-2/             # Level for test 2
    header/         # header block with some changes 
test-n/             # Level for test 3
    header/         # header block with some changes 
```

You could build your project including every test layer to view the results (e.g. `project` + changes from `test-1` or `project` + changes from `test-n` or `project` + changes from `test-1` and `test-2`). If some tests are not passed, you could just remove unacceptable layer with these tests from the project assembly. Nothing will crash the current project and affect other tests.


## BEM and object-oriented programming
In the BEM methodology, the basic principles of object-oriented programming (OOP) are applied to blocks development.

* [Single responsibility principle](#single-responsibility-principle)
* [Open/closed principle](#open-closed-principle)
* [DRY](#dry)
* [Composition instead of inheritance](#composition-instead-of-inheritance)


<a name="single-responsibility-principle"> </a>
### Single responsibility principle
Just as in object-oriented programming, the single responsibility principle in the BEM approach to CSS means that every CSS implementation must have a single responsibility.

**Example**

HTML

```html
<header class="header">
    <button class="button header__button">...</button>
</header>
```

CSS

```css
.button {
    font-family: Arial, sans-serif;
    border: 1px solid black;
    background: #fff;
}
```

Responsibility: external geometry and positioning (let's set the external geometry and positioning for the `button` block via the `header__button` element).

Correct:

```css
.header__button {
    margin: 30px;
    position: relative;
}
```

Incorrect:

```css
.header__button {
    font-family: Arial, sans-serif;
    position: relative;
    border: 1px solid black;
    margin: 30px;
}
```

Single responsibility selectors give the code more flexibility.


<a name="open-closed-principle"> </a>
### Open/closed principle
Any HTML element on a page should be open for extension by modifiers, but closed for changes. You should develop new CSS implementations without needing to change existing ones.

**Example**

HTML

```html
<button class="button">...</button>
<button class="button">...</button>
```

CSS

```css
.button {
    font-family: Arial, sans-serif;
    text-align: center;
    font-size: 11px;
    line-height: 20px;
}
```

Let's say we need to change the size of a button. According to the open/closed principle, we extend the button.

HTML

```html
<button class="button">...</button>
<button class="button button_size_s">...</button>
```

CSS

```css
.button {
    font-family: Arial, sans-serif;
    text-align: center;
    font-size: 11px;
    line-height: 20px;
}

.button_size_s {
    font-size: 13px;
    line-height: 24px;
}
```

The existing button functionality is extended using the `button_size_s` class (the `font-size` and `line-height` properties are redefined). Now the page has two buttons of different sizes.

HTML

```html
<button class="button">...</button>
<button class="button button_size_s">...</button>
```

#### Violating the open/closed principle

* Changing an existing CSS implementation.
    ```css
    .button {
        font-family: Arial, sans-serif;
        text-align: center;
        font-size: 13px;
        line-height: 24px;
    }
    ```  
    The current CSS implementation of the button should be closed for changes. Changes will apply to all the `button` blocks.

* Modification by context.  
    ```css
    .button {
        font-family: Arial, sans-serif;
        text-align: center;
        font-size: 11px;
        line-height: 20px;
    }

    .content .button {
        font-size: 13px;
        line-height: 24px;
    }
    ```
    The button design now depends on its location. Changes will apply to all `button` blocks inside the `content` block.

<a name="dry"> </a>
### DRY
DRY ("don't repeat yourself") is a software development principle aimed at reducing repetitions in code.

In relation to the BEM methodology, the essence of this principle is that each BEM entity must have a single, unambiguous representation within the system.

**Example**

HTML

```html
<button class="button">...</button>
<button class="btn">...</button>
```

CSS

```css
.button {
    font-family: Arial, sans-serif;
    text-align: center;
    color: #000;
    background: #fff;
}

.btn {
    font-family: Arial, sans-serif;
    text-align: center;
    color: #000;
    background: rgba(255, 0, 0, 0.4);
}
```

As shown in the example, the `btn` selector repeats the existing implementation of the `button` block.

Let's rewrite this example using the DRY principle:

HTML

```html
<button class="button button_theme_islands">...</button>
<button class="button button_theme_simple">...</button>
```

CSS

```css
.button {
    font-family: Arial, sans-serif;
    text-align: center;
}

.button_theme_islands {
    color: #000;
    background: #fff;
}

.button_theme_simple {
    color: #000;
    background: rgba(255, 0, 0, 0.4);
}
```

With the addition of modifiers, we got rid of the `btn` block.

Important The DRY principle only applies to functionally similar components of a page, such as buttons.

**Example**

<img src="https://en.bem.info/MtWaHCPL7u0kVtAV31-5yNw4jEs.png" alt="image"/>

As the example shows, there are some small external differences between buttons. The DRY principle addresses entities that are functionally similar but have different formatting.

There isn't any reason to combine different types of blocks just because they have, for instance, the same color or size.

**Example**

<img src="https://en.bem.info/VC8Rh5_HgcpHmJdiIX3bXIYYbb4.png" alt="image"/>

<a name="composition-instead-of-inheritance"> </a>
### Composition instead of inheritance
Inheritance is a mechanism for defining a new CSS class based on an existing one (a parent or base class). The derived class can add its own properties, as well as use the parent properties.

New CSS implementations are formed in BEM by combining existing ones. This keeps the code uncoupled and flexible.

**Example**

Let's say we have three existing implementations:

* button — the button block
* menu — the menu block
* popup window — the popup block

**Task**

Implement a drop-down list (the `select` block).

Developing a drop-down list with a custom appearance is not an easy task. However, if you have ready-made components (a button, a popup window, and a menu), you just need to correctly describe how they interact.

**Example**

HTML

```html
<div class="select">
    <button class="button select__button">
        <span class="button__text">Block</span>
    </button>
</div>

<div class="popup">
    <div class="menu">
        <div class="menu__item">Block</div>
        <div class="menu__item">Element</div>
        <div class="menu__item">Modifier</div>
    </div>
</div>
```

## How to get started with BEM
As you may have noticed, our team implemented BEM in stages. The flexibility of BEM allows you to adapt this methodology to your current processes.

There is no “one-size-fits-all” way to start using the methodology in your project. Each particular team integrates it into their development process and uses it in a way that best suits them.

Say, you have a project where you would like to use BEM only for the layout. You use CSS and HTML in the project, so you can start with naming rules for CSS selectors. This is the most common way of applying the BEM methodology. As such, it is the starting point for a lot of teams. This is also how we got started.

As more new rules get introduced, the need for your own tools and technologies makes itself felt.


### BEM and its technologies
In web development, a final product is a mix of different technologies (e.g., HTML, CSS, JavaScript). The main principle of the BEM methodology is to use unified terms and implementation approaches in all the technologies that you use.


#### JavaScript
To operate in BEM terms and write declarative JavaScript that could be separated into redefinition levels, we introduced our own framework — [i-bem](https://en.bem.info/technology/i-bem/).


#### BEM tree
Our typical web development process basically consisted in writing HTML and then copying the result into templates. If the design changed, we’d have to manually edit the HTML and the templates.
To eliminate the manual work, we added a new abstraction level — a **BEM tree**, which allowed us to manipulate a web page structure in terms of blocks, elements, and modifiers. A BEM tree is an abstraction over a DOM tree.

A BEM tree describes all the BEM entities that are used on a page, their states, their order, and nesting. It can be presented in any format that supports a tree-like structure, such as XML or JSON.

**Example**

Let's say we have the following DOM tree:

```html
<header class="header">
    <img class="logo">
    <form class="search-form">
        <input type="input">
        <button type="button"></button>
    </form>
    <div class="lang-switcher"></div>
</header>
```

The corresponding BEM tree looks like this:

```files
header
    logo
    search-form
        input
        button
    lang-switcher
```

It can be compared to the [Jade](http://jade-lang.com/) templating engine, but the difference is that we use abstractions instead of HTML.

This is what the same BEM tree will look like in XML and BEMJSON formats:

XML

```xml
<block:header>
    <block:logo/>
    <block:search-form>
        <block:input/>
        <block:button/>
    </block:search-form>
    <block:lang-switcher/>
</block:header>
```

[BEMJSON](https://en.bem.info/technology/bemjson/) is a JavaScript format that lets you operate in BEM terms. BEMJSON allows you to describe a page in terms of blocks, elements, and modifiers, abstracting away HTML markup.

```javascript
{
    block: 'header',
    content : [
        { block : 'logo' },
        {
            block : 'search-form',
            content : [
                { block : 'input' },
                { block : 'button' }
            ]
        },
        { block : 'lang-switcher' }
    ]
}
```

We describe a page that we want to see in a browser as a BEM tree instead of writing HTML code manually: the [BEMHTML](https://en.bem.info/technology/bemhtml/v2/rationale/) template engine then processes the BEMJSON and generates the HTML.


<a name="tools"> </a>
### BEM toolbox
To make it easier for developers to work with all the different technologies, we decided on a structure whereby a project is divided into many separate files. That gave us the advantages described above. However, [code building](https://en.bem.info/method/build/) and optimization were also needed to make the code work in a browser.

Assembling files manually was impractical, so we set about automating a lot of routine processes. This resulted in the creation of [bem-tools](https://en.bem.info/toolbox/bem-tools/) — a toolkit for working with BEM files — that was later superseded by [ENB](https://en.bem.info/tools/bem/enb-bem/).

To be able to assemble unrelated files spread throughout the system, we introduced a technology called [DEPS](https://en.bem.info/technology/deps/), which specifies the dependencies of one block on another one or a set of blocks.

The BEM tools are intended to give the developer leeway as far as coding is concerned, leaving it for robots to take care of optimization and linking relevant files to the project in a correct order.


### BEM and its libraries
A lot of BEM libraries are available as open-source software. The following are the the main ones:

* [bem-core](https://en.bem.info/libs/bem-core/) — the core block library containing the JavaScript framework `i-bem` as well as 20 helper blocks for web development in BEM terms.
* [bem-components](https://en.bem.info/libs/bem-components/) — a universal library of ready-made visual components (blocks). It contains form controls and other basic components for creating interfaces.

The bem-components library can be linked Bootstrap-fashion: add the pre-built library files and insert them into the HTML pages using the `link` and `script` elements.

```html
<link rel="stylesheet" href="https://yastatic.net/bem-components/latest/desktop/bem-components.css">
<script src="https://yastatic.net/bem-components/latest/desktop/bem-components.js+bemhtml.js"> </script>
```

This distribution method is known as [Dist](https://en.bem.info/platform/libs/bem-components/current/#integrating-the-pre-assembled-library-files-dist) and includes pre-built CSS and JavaScript code and templates. You won't need build tools or template engines with it, as all the blocks are already built and working.

To learn about linking files via CDN or locally, using bower or building library files from source code, read the [library description](https://en.bem.info/libs/bem-components/current/#usage).


### Project stub
The quickest way to start with your own BEM project is by using [project-stub](https://en.bem.info/tutorials/project-stub/) — a project with pre-configured technologies and tools. You can familiarize yourself with it using [Quick start with BEM](https://en.bem.info/platform/tutorials/quick-start-static/).

The extended examples of using project-stub is described in the [Tutorials](https://en.bem.info/platform/tutorials/) section on [bem.info](https://en.bem.info). 


### bem-react-core
If you use ReactJS, we have developed a new alternative world of BEM platform — [bem-react-core](https://github.com/bem/bem-react-core) — core library for develop with React and BEM methodology. 


## In conclusion
The BEM methodology is not for CSS only. It is a set of rules and guidelines on how to organize your work on a project.

Somewhere along the line we separated the methodology from its practical implementation — the [platform](https://en.bem.info/platform/).
The BEM platform is an instance of the implementation of BEM's general principles. Since all the technologies were designed to meet the requirements of our projects and kept evolving over time, the BEM platform encompasses all the possibilities offered by the BEM methodology. You can read about it in detail [here](https://en.bem.info/method/).

All parts of the BEM platform are integrated to work together but can be also used individually. Each part serves a specific purpose and can be adapted to your process or substituted for some other part.

Choose whatever suits your project best, feel free to experiment. You can even ask your questions about [BEM methodology](https://en.bem.info/methodology/), [BEM toolbox](https://en.bem.info/toolbox/) or [BEM platform](https://en.bem.info/platform/) on our [forum](https://en.bem.info/forum/). Follow us on [Twitter](https://twitter.com/bem_en) to get all BEM news or feel free to join our discussins on [Telegram](https://telegram.me/bem_en) channel. We appreciate your feedback.

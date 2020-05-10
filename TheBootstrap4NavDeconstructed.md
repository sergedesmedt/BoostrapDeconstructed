# The Bootstrap 4 Nav (layout) Deconstructed

## Content

## Introduction

What would a website be without a navigation bar? Nothing! No means to navigate to other sections, etc...

In my investifation of Bootstrap 4 and what it has to offer I will dissect the Bootstrap 4 Nav responsive navigation bar. The Bootstap Nav bar allows you to define a navigation bar which is responsive: it can behave differently on various screensizes. I will take a similar approach to my Grid article: start with simple constructs, explain what makes them work and then build up to more complex navigation bars and what makes those work.

I will not explain the styling classes, which allow you to choose the color scheme used by your navigation bar. For someone with a basic knowledge of CSS they should not contain anything surprising. And I also don't want to make the scope of this article too broad.

Again, a basic knowledge of HTML and CSS is assumed.

Let's get started.

## Constructing the basics: what can we do

### The Nav bar: it's all about bars and items

The basic structure of a navigation bar is the bar itself and navigation items inside.

A very basic markup is:

```
<h1><a name="basic"></a>A basic navigation bar</h1>
<nav class="navbar navbar-light bg-light">
    <div class="navbar-nav">
        <a class="nav-item nav-link active" href="#">Item1 Active</span></a>
        <a class="nav-item nav-link" href="#">Item2</a>
        <a class="nav-item nav-link disabled" href="#">Item3 Disabled</a>
    </div>
</nav>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/Sample01.SimpleNavBar.Bootstrap4.html#basic)

There may be a suprise when viewing this: everything is shown vertical. The items in the navigation bar are shown as vertical rows instead of horizontally next to each other. On the other hand, this should not be that surprising: Bootstrap is all about *mobile first* and you probably don't want to show all the items in your navigation bar next to each other on a mobile device with a limited screenwidth.

We can of course put multiple `navbar-nav`s in a single `navbar`:

```
<h1><a name="basicmulti"></a>A basic navigation bar (multiple groups)</h1>
<nav class="navbar navbar-light bg-light">
    <div class="navbar-nav">
        <a class="nav-item nav-link active" href="#">Item1.1 Active</span></a>
        <a class="nav-item nav-link" href="#">Item1.2</a>
        <a class="nav-item nav-link disabled" href="#">Item1.3 Disabled</a>
    </div>
    <div class="navbar-nav">
        <a class="nav-item nav-link" href="#">Item2.1</span></a>
    </div>
    <div class="navbar-nav">
        <a class="nav-item nav-link" href="#">Item3.1</span></a>
        <a class="nav-item nav-link" href="#">Item3.2</a>
    </div>
</nav>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/Sample01.SimpleNavBar.Bootstrap4.html#basicmulti)

Notice how the multiple `navbar-nav`s get spread evenly across the width of the parent `navbar`.

### Get Responsive: defining Responsive Breakpoints

If you want the items inside your navigation bar to flow horizontally on larger screens you can use following code:

```
<h1><a name="responsive"></a>A responsive navigation bar</h1>
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="navbar-nav">
        <a class="nav-item nav-link active" href="#">Item1 Active</span></a>
        <a class="nav-item nav-link" href="#">Item2</a>
        <a class="nav-item nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Item3 Disabled</a>
    </div>
</nav>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/Sample01.SimpleNavBar.Bootstrap4.html#responsive)

You will need to shrink or expand the width of your browserwindow to see the effect: the items inside the navigation bar transition from a horizontal layout on wider screenwidth to vertically stacked on less wider sceenwidth

Notice the addition of the `navbar-expand-lg` class. The "lg" part defines the responsive breakpoint: it defines at what width the navbar will expand from vertical rows into a horizontal layout. Values you can use are:

- sm: larger than or equal to 576px
- md: larger than or equal to 768px
- lg: larger than or equal to 992px
- xl: larger than or equal to 1200px

So, in the above we transition from rows to a horizontal layout when the browserwindow exceeds 992px.

Of course, if we do this on a mobile device, the first thing the user will see is the vertically stacked rows of the navigation bar, and not the content of your website. Although navigation is important, your content is what your visitors are coming to see. What we want is a means to make the navigation items appear and disappear. Enter the `navbar-toggler`

```
<h1><a name="responsivebtn"></a>A responsive navigation bar with toggle button</h1>
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent_resptb" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarSupportedContent_resptb">
    <div class="navbar-nav">
        <a class="nav-item nav-link active" href="#">Item1 Active</span></a>
        <a class="nav-item nav-link" href="#">Item2</a>
        <a class="nav-item nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Item3 Disabled</a>
    </div>
    </div>
</nav>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/Sample01.SimpleNavBar.Bootstrap4.html#responsivebtn)

First things first: the `navbar-toggler` uses javascript under the hood. So you'll need to reference the Bootstrap javascript library on your page. You can see in the example pages how this is done.
Second, apart from the added `button` itself, what is important here are the `data-toggle` and the `data-target` attributes on the button, and the added `id` attribute and `collapse` class on the `div`. The value of the `id` attribute must be the same as the value of the `data-target` attribute on the button. The `data-target` attribute defines what element to hide.

What happens is actually simple: through javascript the button click is intercepted and the `display` property of the `div` is toggled between `none` for hiding and the standard `block` for showing.


## Deconstructing the basics: what makes it work?

If you read the article about the [Bootstrap 4 Grid](https://www.codeproject.com/Articles/5265672/The-Bootstrap-4-Grid-Deconstructed) you have an edge: the navigation bar is also based on the CSS3 flexbox model. If you haven't, read on. If you have: also read on because I'll be explaining other things too.

There are a few constructs which make the navigation bar work:

1. The CSS flexbox system
2. CSS selectors: select a hierarchy of HTML elements
3. Responsive breakpoints
4. A little javascript magic to modify the classes on a html element

### Deconstructing a basic navigation bar

Let us start by looking at the CSS for the basic navigation bar (I will just show the properties which are important for the discussion):

```
.navbar {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
}

.navbar-nav {
  display: flex;
  flex-direction: column;
}
```

Ok, so we define a flexbox using `display: flex` with a `flex-direction` of type `column`. What does this do?

### The CSS flexbox system: managing the flow inside a flexbox

Let me start by saying that the CSS flexbox system is a huge subject. As such, I will only explain stuff directly important for the navigation bar. There will be some overlap with the explanation of the flexbox system in the [Bootstrap 4 Grid](https://www.codeproject.com/Articles/5265672/The-Bootstrap-4-Grid-Deconstructed) article.

So, let's start with a basic flexbox setup:
```
.simple-flexbox {
    display:flex;
}

<h1><a name="flowdir"></a>Managing the flow direction</h1>
<div class="simple-flexbox parent-container">
    <div>Default Flexbox subitem 1</div>
    <div>Default Flexbox subitem 2</div>
    <div>Default Flexbox subitem 3</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/CSSFlexbox.Deconstructed.html#flowdir)

The `display:flex` makes the `div` on which it is applied layout it's children differently then what we are used to. Normally, elements inside a `div` are stacked vertically. But the flexbox changes this default to horizontally. Also, where in a default layout the child `div`s take-up the whole width of the screen, within a flexbox `div` they only take the space necessary.

The above is what happens with a default flexbox layout. However, there is a whole bunch of related properties allowing you to change this behaviour. Let's experiment with some of them.

First, the direction of the layout inside a flex container. The default is horizontally, but we can change this direction to anything we like:

```
.vertical-flexbox {
    display:flex;
    flex-direction: column;
}

<h1><a name="flowdir"></a>Managing the flow direction</h1>
<div class="vertical-flexbox parent-container">
    <div class="bg-red">Vertical Flexbox subitem 1</div>
    <div class="bg-green">Vertical Flexbox subitem 2</div>
    <div class="bg-blue">Vertical Flexbox subitem 3</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/CSSFlexbox.Deconstructed.html#flowdir)

Et voila, vertically stacked `div`s which take up the whole width of the window.
Just like we have in the basic navigation bar.

You can also manage the alignment of the children. There are two directions in which you can manage the alignment. A first direction is the direction of the flow of the flexbox. A second direction is perpendicular to the flow: the so-called <em>cross direction</em>.

To manage the alignment in the flow direction we have the `justify-content` property.

```
.flexbox-justify-end {
    justify-content: flex-end;
}

.flexbox-justify-between {
    justify-content: space-between;
}

<h1>Managing the layout of flex children: in the direction of the flow for horizontal flow</h1>
<div class="simple-flexbox parent-container flexbox-justify-end">
    <div class=" bg-red">Flexbox subitem 1</div>
    <div class=" bg-green">Flexbox subitem 2</div>
    <div class=" bg-blue">Flexbox subitem 3</div>
</div>
<div class="simple-flexbox parent-container flexbox-justify-between">
    <div class=" bg-red">Flexbox subitem 1</div>
    <div class=" bg-green">Flexbox subitem 2</div>
    <div class=" bg-blue">Flexbox subitem 3</div>
</div>

<h1>Managing the layout of flex children: in the direction of the flow for vertical flow</h1>
<div class="vertical-flexbox height-100 parent-container flexbox-justify-end">
    <div class=" bg-red">Flexbox subitem 1</div>
    <div class=" bg-green">Flexbox subitem 2</div>
    <div class=" bg-blue">Flexbox subitem 3</div>
</div>
<div class="vertical-flexbox height-100 parent-container flexbox-justify-between">
    <div class=" bg-red">Flexbox subitem 1</div>
    <div class=" bg-green">Flexbox subitem 2</div>
    <div class=" bg-blue">Flexbox subitem 3</div>
</div>

```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/CSSFlexbox.Deconstructed.html#flowdirlayout)


I think it is obvious what this does. There are some other possible values for this property, you can check the references section for places on the internet where you can get a full description of the flexbox system. The standard alignment of the `navbar` class is `justify-content: space-between` which means the excess space is evenly distributed between the children of the flexbox. That is why in the basic navigation bar with groupings the different groups are layed-out across the total width of the screen.

Do notice however that for the column flexbox direction we explicitely had to set the height of the flexbox container to be able to demonstrate the settings!

And you can also manage the alignment in the cross direction of the items in a flexbox by using the `align-items` property:


```
.width-33percent {
    width: 33%;
}

.flexbox-align-top {
    align-items: flex-start;
}

.flexbox-align-center {
    align-items: center;
}

<h1>Managing the layout of flex children: in the cross direction for horizontal flow</h1>
<div class="simple-flexbox parent-container flexbox-align-top">
    <div class="width-33percent bg-red">Flexbox subitem 1 with extra text ... to trigger overflow</div>
    <div class="width-33percent bg-green">Flexbox subitem 2</div>
    <div class="width-33percent bg-blue">Flexbox subitem 3</div>
</div>
<div class="simple-flexbox parent-container flexbox-align-center">
    <div class="width-33percent bg-red">Flexbox subitem 1 with extra text ... to trigger overflow</div>
    <div class="width-33percent bg-green">Flexbox subitem 2</div>
    <div class="width-33percent bg-blue">Flexbox subitem 3</div>
</div>

<h1>Managing the layout of flex children: in the cross direction for vertical flow</h1>
<div class="vertical-flexbox parent-container flexbox-align-top">
    <div class="width-33percent bg-red">Flexbox subitem 1 with extra text ... to trigger overflow</div>
    <div class="width-33percent bg-green">Flexbox subitem 2</div>
    <div class="width-33percent bg-blue">Flexbox subitem 3</div>
</div>
<div class="vertical-flexbox parent-container flexbox-align-center">
    <div class="width-33percent bg-red">Flexbox subitem 1 with extra text ... to trigger overflow</div>
    <div class="width-33percent bg-green">Flexbox subitem 2</div>
    <div class="width-33percent bg-blue">Flexbox subitem 3</div>
</div>
<div class="vertical-flexbox parent-container flexbox-align-center">
    <div class="bg-red">Flexbox subitem 1</div>
    <div class="bg-green">Flexbox subitem 2 but made a bit larger</div>
    <div class="bg-blue">Flexbox subitem 3 also larger</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/CSSFlexbox.Deconstructed.html#crossdirlayout)

Most of this should be obvious. I'd like to pull your attention to two things however: 
1. Notice how in the first two examples I've made the text inside the first sub`div` extra long to make the sibling `div`s take the position as requested by the `align-items` property
2. Notice how in the examples for cross direction with vertical flow, the last two examples clearly show how the alignment is applied to the children themselves and NOT the content inside the children.

### Deconstructing a responsive navigation bar

Next is the responsive navigation bar.

Apart form the above CSS classes, we have following additional classes:

```
@media (min-width: 992px) {
  .navbar-expand-lg {
    flex-flow: row nowrap;
    justify-content: flex-start;
  }
  .navbar-expand-lg .navbar-nav {
    flex-direction: row;
  }
}
```

First, lets review the second definition. This may look like a strange class definition: it has two class names. What is going on here?

### CSS selectors: select a hierarchy of HTML elements

We've been defining our responsive navigation bar as follows:

```
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="navbar-nav">
        <a class="nav-item nav-link active" href="#">Item1 Active</span></a>
        <a class="nav-item nav-link" href="#">Item2</a>
        <a class="nav-item nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Item3 Disabled</a>
    </div>
</nav>
```

What we added with respect to the none responsive version is the `navbar-expand-lg` class.
This changes the CSS of the `div` with the class `navbar-nav` so that the `flex-direction` property changes from `column` to `row`. How does it do that? By using CSS classes which are applied on a specific hierarchy of HTML elements.

We're all familiar with the traditional CSS classes:

```
.parentsection {
    border: solid;
    border-color: red;
}

.mainsection {
    border: solid;
    border-color: blue;
}

<div>
    <div class="mainsection">
        <div class="mainsection">A first subsection</div>
        <div>A second subsection</div>
    </div>
    <div class="mainsection">A second main section</div>
</div>
<div class="parentsection">
    <div class="mainsection">
        <div class="mainsection">A first subsection</div>
        <div>A second subsection</div>
    </div>
    <div class="mainsection">A second main section</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/CSSClasses.Regular.Deconstructed.html)

No surprises here: the `div`'s with the `mainsection` CSS class applied have a border with a blue color.

However, what if we wanted `div`'s with a `mainsection` inside `parentsection` to behave differently then those who are not. Enter hierarchical selectors:

```
.parentsection {
    border: solid;
    border-color: red;
}

.mainsection {
    border: solid;
    border-color: blue;
}

.parentsection .mainsection {
    border: solid;
    border-color: green;
}

.something-inbetween{
    border: solid;
    border-color: purple;
}

<h1><a name="simple"></a>Simple nesting</h1>
<div class="parentsection">
    <div class="mainsection">A mainsection inside a parentsection</div>
    <div>
        <div class="mainsection">A mainsection two levels deep</div>
        <div>A second subsection</div>
    </div>
    <div class="mainsection">
        <div class="mainsection">A mainsection within another mainsection</div>
        <div>A second subsection</div>
    </div>
    <div class="something-inbetween">
        <div class="mainsection">A mainsection with something inbetween</div>
        <div>A second subsection</div>
    </div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/CSSClasses.Nested.Deconstructed.html#simple)

There are a few things to notice here:
1. There is a space between the class names when declaring the hiërarchy
2. The hiërarchy is defined for each `mainsection` which is child of a `parentsection`. It doesn't matter if other html elements are inbetween.
3. The hiërarchy is defined for each `mainsection` which is a child of a `parentsection`. It doesn't matter if there are any other same-named or different-named elements inbetween.

Of course, the last remark is only valid if no specific hiërarchy is defined for the classes inbetween:

```
.othersection {
    border: dashed;
    border-color: blue;
}

.parentsection .othersection {
    border: dashed;
    border-color: green;
}

.parentsection .othersection .othersection {
    border: dashed;
    border-color: maroon;
}

<h1><a name="complex"></a>Complex nesting</h1>
<div class="parentsection">
    <div class="othersection">A othersection inside parentsection</div>
    <div class="othersection">
        <div class="something-inbetween">
            <div class="othersection">A othersection with something-inbetween</div>
        </div>
    </div>
    <div class="something-inbetween">
        <div>A second subsection</div>
        <div class="othersection">A othersection with something-inbetween</div>
        <div class="othersection">
            <div class="othersection">Something-inbetween a othersection within another othersection</div>
        </div>
    </div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/CSSClasses.Nested.Deconstructed.html#complex)

Because we defined an even more specific nesting of `.parentsection .othersection .othersection`, the second `div` (with the text *A othersection with something-inbetween*) and the last `div` (with the text *Something-inbetween a othersection within another othersection*) have a maroon border.

In the definition of the nested classes the space between the class names is important. When you leave off the space the meaning is something different: then all classes must be applied on a single element (but with a space between them):

```
.se_first_part {
    border: dashed;
    border-color: mediumvioletred;
}

.se_second_part {
    border: dashed;
    border-color: olive;
}

.se_first_part.se_second_part {
    border: double;
    border-color:turquoise;
}

<h1><a name="singleelem"></a>Single element target</h1>
<div>
    <div class="se_first_part se_second_part">A first_part and a second_section together</div>
    <div class="se_first_part">
        <div class="se_second_part">A second part_nested within a first_part</div>
        <div>A subsection</div>
    </div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/CSSClasses.Nested.Deconstructed.html#singleelem)

Let me repeat a part of the bootstrap CSS here:

```
@media (min-width: 992px) {
  .navbar-expand-lg .navbar-nav {
    flex-direction: row;
  }
}
```

What we are saying here is that for all `navbar-nav`s inside a `navbar-expand-lg` the `flex-direction` property should be set to `row`, thus rendering the items horizontally.

But there is more going on.

### The CSS flexbox system: managing the wrapping inside a flexbox

Let's repeat some of the CSS:

```
.navbar {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
}

@media (min-width: 992px) {
  .navbar-expand-lg {
    flex-flow: row nowrap;
    justify-content: flex-start;
  }
}
```

We know the `justify-content` and `align-items` properties by now: it manages the horizontal flow of our child items of the flexbox. 

But what is this `flex-wrap` property?

```
.flexbox-wrapping {
    flex-wrap: wrap;
}

.flexbox-nowrapping {
    flex-wrap: nowrap;
}

<h1>Managing content wrapping</h1>
<div class="simple-flexbox flexbox-wrapping">
    <div class=" bg-red">Flexbox subitem 1</div>
    <!-- a lot more divs here in the sample code -->
    <div class=" bg-blue">Flexbox subitem 3</div>
</div>
<div class="simple-flexbox flexbox-nowrapping">
    <div class=" bg-red">Flexbox subitem 1</div>
    <!-- a lot more divs here in the sample code -->
    <div class=" bg-blue">Flexbox subitem 3</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/CSSFlexbox.Deconstructed.html#wrapping)

Now, stretch and shrink your browserwindow in the horizontal direction.

This should clear things up: `wrap` allows the content of the flexbox to be wrapped at the end if it is to exceed the total width of the flexbox. `nowrap` prevents this and instead wraps the content of the child `div`s.

So applying the `navbar-expand-lg` class prevents wrapping of the items indide the flexbox to which it got applied. The `flex-flow` property is actually a shortcut for the combination of the `flex-direction` and `flex-wrap` property.

###  Responsive breakpoints

And what is this strange `@media` thing inside which the `navbar-expand-lg` is defined? That is the responsive breakpoint which makes sure the CSS class is only known for screenwidths larger than 992px.

I'll make it easy on myself here and forward you to my article on the [Boostrap 3 Grid](https://www.codeproject.com/Articles/1109210/The-Bootstrap-Grid-Deconstructed#toc132) where I explain responsive breakpoints in detail.


### Deconstructing a responsive navigation bar with toggle button

Finally a responsive navigation bar with a toggle button.

```
.collapse:not(.show) {
  display: none;
}

.navbar-collapse {
  flex-basis: 100%;
  flex-grow: 1;
  align-items: center;
}

@media (min-width: 992px) {
  .navbar-expand-lg .navbar-collapse {
    display: flex !important;
    flex-basis: auto;
  }
  .navbar-expand-lg .navbar-toggler {
    display: none;
  }
}

<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
</button>

<div class="collapse navbar-collapse" id="navbarSupportedContent">
<div class="navbar-nav">
    <a class="nav-item nav-link active" href="#">Item1 Active</span></a>
    <a class="nav-item nav-link" href="#">Item2</a>
    <a class="nav-item nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Item3 Disabled</a>
</div>
</div>
```

Everything should be clear here, except for following:
1. What is this `:not(.show)` thingy
2. What are those `data` attributes on the `button` element?

(I know: what does this `flex-basis` and `flex-grow` mean? I'll skip them for now because for the simple navigation bars we are constructing now it is unimportant. But I will get back to it when discussing more complex layouts)

### CSS selectors: the not() selector

We've already seen how to select a hierarchy of html elements. But there is a whole syntax and along with it operators which can be used to select elements on a page for applying the CSS.

One such an operator is the `:not()` operator. It allows you to exclude specific elements from a selected group of elements. An example will make eveything clear:

```
.bordered:not(.noborder) {
    border: solid;
    border-color: blue;
}

<div class="bordered">
    A bordered section
</div>
<div class="bordered noborder">
    A not bordered section
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/CSSClasses.NotOperator.Deconstructed.html)

It should be clear what is going on here: by applying the `:not()` selector we can, by adding another CSS class to an element, exclude that element from being affected by the CSS class. Notice also that the CSS class specified in the `:not()` operator does not even have to be defined!

Ok, so what then are those `data` attributes?

### A little javascript magic to modify the classes on a html element

First: the `data-*` style attributes are a standardized way of adding additional data to an element. See also the reference section for a more detailed discussion.

The Boostrap javascript library makes use of these attributes to find the buttons and have them act on certain HTML elements.

It basically boils down to following steps:
1. Find all elements on the page with a certain reference attribute which has a certain value (named `data-search` with value `searchForMe` in the example below)
2. For these, get the value of another attribute (named `data-refid` in the example below)
3. Attach a click handler to those found elements which adds or removes a CSS class which controls the desired properties (named `el-frame-violet` in the example below) to the element with an id equal to the value of the `data-refid` attribute.

(I deliberately used different names here for the attributes from the ones used by Bootstrap to show it doesn't matter what those names are. In fact, they don't even need to start with the string `data`! That string is just a uniform way of adding specific attributes to a HTML element which are foreign to the HTML specification of that element.)

```
.el-frame-violet {
    border: solid;
    border-color: violet;
}

<button data-search="searchForMe" data-refid="actOnMe1" >Button 1</button>
<button data-search="searchForMe" data-refid="actOnMe2" >Button 2</button>
<div>
    <div id="actOnMe1" class="bg-red">Target of button 1</div>
    <div id="actOnMe2" class="bg-green">Target of button 2</div>
    <div class="bg-blue">No target</div>
</div>

<script>
    console.log("Running the code...");

    // Find all elements on the page with a certain reference attribute which has a certain value
    let allAffectedButtons = document.querySelectorAll('[data-search="searchForMe"]');


    allAffectedButtons.forEach(function(button) {
        console.log("Button: " + button.innerHTML);

        // For these, get the value of the another attribute
        let btnTarget = button.getAttribute("data-refid");

        // Attach a click handler to those found elements
        button.addEventListener("click", function(){
            console.log("Button target (from the click handler): " + btnTarget);

            // which adds or removes a CSS class which controls the desired properties 
            // to the element with an id equal to the value of the `data-refid` attribute
            document.getElementById(btnTarget).classList.toggle('el-frame-violet');
        });
    })
</script>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/JS.AttributeManipulation.html)

There isn't a lot to explain on the above code. Perhaps two things:
1. The syntax for selecting the elements is a kind of mini-language which allows you to select elements based on if they have a certain attribute, a certain attribute with a certain value, etc... In the above example we search all elements with an attribute data-search which has a value "searchForMe".
2. The javascript is defined at the end of the document: because the code in the `script` tags gets excecuted as the page gets processed, if we define it before the document body, the buttons will not be found because they do not yet exist at that time.

### Why navbar-collaps instead of navbar-nav?

Let us try to create a collapsable navigation bar using `navbar-nav`:

```
<h1><a name="responsivebtnwrong"></a>A responsive navigation bar with toggle button NOT USING navbar-collapse ???</h1>
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent_btn_wrong" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-nav" id="navbarSupportedContent_btn_wrong">
    <div class="navbar-nav">
        <a class="nav-item nav-link active" href="#">Item1 Active</span></a>
        <a class="nav-item nav-link" href="#">Item2</a>
        <a class="nav-item nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Item3 Disabled</a>
    </div>
    </div>
</nav>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/Sample01.SimpleNavBar.Bootstrap4.html#responsivebtnwrong)

Above the responsive breakpoint we see nothing!!! What is happening?

Well, we have following CSS in the order it is applied (see the references section to know how this order is determined):

```
.navbar-nav {
  display: flex;
  flex-direction: column;
}

.collapse:not(.show) {
  display: none;
}

@media (min-width: 992px) {
  .navbar-expand-lg .navbar-nav {
    flex-direction: row;
  }
}
```

As you can see the `collapse` class sets the `display` property to `none` effectively hiding the content. Because the `collapse` class is applied after the `navbar-nav` class the `display` property of this last class is not used. But if we shrink the width of hte page below the responsive breakpoint, then the button appears. When we click it the `show` class is added on the element with the `collapse` class, effectively removing this class of that element and then the `display` property of the `navbar-nav` class does take effect. The element is displayed as a flexbox container.

However, by using the `navbar-collapse` class we have in order of application:

```
.navbar-collapse {
    /* does not contain any display properties */
}

.collapse:not(.show) {
  display: none;
}

@media (min-width: 992px) {
  .navbar-expand-lg .navbar-collapse {
    display: flex !important;
  }
}
```

As you can see, above the responsive breakpoint the `display: flex` values is applied for a `navbar-collapse` used inside a `navbar-expand-xx` responsive class. This masks the `display: none` value of the `collapse` class. Under the responsive brealpoint, the hierarchical definition of `.navbar-expand-lg .navbar-collapse` is not known and the `collapse` classes `display: none` value is applied.

## Constructing complex navigation bars

But of course, there is much more we can do with the navigation bar.

### More alignment

The toplevel `navbar` item has a default direction of row, thus putting all its children on a row. However, you can force a direction of column:

``` 
<h1><a name="colcent"></a>A navigation bar with direction column center aligned (with branding)</h1>
<nav class="navbar flex-column navbar-light bg-light">
    <a class="navbar-brand">My Brand</a>
    <div class="navbar-nav flex-row">
        <a class="nav-item nav-link" href="#">Centered1</a>
        <a class="nav-item nav-link" href="#">Centered2</a>
        <a class="nav-item nav-link" href="#">Centered3</a>
    </div>
</nav>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/Sample02.ComplexNavBar.Bootstrap4.html#colcent)

There are a few things to notice here:
- the `flex-column` class forces the direction of the flexbox on which it is applied to be column, thus putting all its children a a column.
- The items inside the `navbar` are horizontally centered.

We can of course change the alignment:

```
<h1><a name="colend"></a>A navigation bar with direction column end aligned (with branding)</h1>
<nav class="navbar flex-column align-items-end navbar-light bg-light">
    <a class="navbar-brand">My Brand</a>
    <div class="navbar-nav flex-row">
        <a class="nav-item nav-link" href="#">Item1</a>
        <a class="nav-item nav-link" href="#">Item2</a>
        <a class="nav-item nav-link" href="#">Item3</a>
    </div>
</nav>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/Sample02.ComplexNavBar.Bootstrap4.html#colend)

By applying the `align-items-end` class, the children of the flexbox are aligned at the end of the flexbox. Notice also how this type of alignment works: it is perpendicular to the direction of the flexbox! If we where to remove the `flex-column` class the `align-items-end` would have no effect in this particular layout.

### More spacing

A layout which is popular is to have some navigation items to the left and some to the right. How can this be done? There are some possibilities.

A first one is to use the `justify-content-between` class:
```
<h1><a name="multbetween"></a>A second navigation bar (with content between)</h1>
<nav class="navbar navbar-expand-lg justify-content-between navbar-light bg-light">
    <div class="navbar-nav">
        <a class="nav-item nav-link" href="#">Left1</a>
        <a class="nav-item nav-link" href="#">Left2</a>
        <a class="nav-item nav-link" href="#">Left3</a>
    </div>
    <div class="navbar-nav">
        <a class="nav-item nav-link" href="#">Right1</a>
        <a class="nav-item nav-link" href="#">Right2</a>
    </div>
</nav>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/Sample02.ComplexNavBar.Bootstrap4.html#multbetween)

What this does is distribute the excess space evenly between the children of the element on which this class was applied. 

However, when viewing this below the responsive breakpoint width, this probably does not what you expect: the two child `navbar-nav` items have their children in rows, but because the `navbar` itself defaults to the row style direction, the `navbar-nav`s themselfves are each rendered at the extremes of the parent container: all excess space is put between them because of the `justify-content-between` on their parent.

The solution is simple: put the two children inside their own parent `navbar-nav`.

```
<h1><a name="multcolbetween"></a>A second navigation bar with collapsing (with content between)</h1>
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="navbar-nav flex-fill justify-content-between">
        <div class="navbar-nav">
            <a class="nav-item nav-link" href="#">Left1</a>
            <a class="nav-item nav-link" href="#">Left2</a>
            <a class="nav-item nav-link" href="#">Left3</a>
        </div>
        <div class="navbar-nav">
            <a class="nav-item nav-link" href="#">Right1</a>
            <a class="nav-item nav-link" href="#">Right2</a>
        </div>
    </div>
</nav>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/Sample02.ComplexNavBar.Bootstrap4.html#multcolbetween)

Notice the use of the `flex-fill` class. This is important in this case because it tells the element on which it is applied to take up the whole space inside its parent flexbox (in this case the toplevel `nav` element). Because of the way a flexbox functions, if the direction is set to rows, then children of the flexbox container always just take the space they need and not the full width.

A second one is to use the `mr-auto` or `ml-auto` class:

```
<h1><a name="multcolauto"></a>A second navigation bar with collapsing (with mr-auto)</h1>
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="navbar-nav flex-fill">
        <div class="navbar-nav mr-auto">
            <a class="nav-item nav-link" href="#">Left1</a>
            <a class="nav-item nav-link" href="#">Left2</a>
            <a class="nav-item nav-link" href="#">Left3</a>
        </div>
        <div class="navbar-nav">
            <a class="nav-item nav-link" href="#">Right1</a>
            <a class="nav-item nav-link" href="#">Right2</a>
        </div>
    </div>
</nav>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/Sample02.ComplexNavBar.Bootstrap4.html#multcolauto)

What these do is to have either the right (for `mr-auto`) or left (for `ml-auto`) margin take-up the remaining space. In the above I immediately put everything inside a parent `navbar-nav` because otherwise we suffer the same problem here as with the `justify-content-between` examples.

Another thing to take into account for the layout below the responsive breakpoint is the choice between `mr-auto` and `ml-auto`:

```
<h1><a name="multmlauto"></a>A second navigation bar with collapsing (with ml-auto, but probably not what you want below the responsive breakpoint)</h1>
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="navbar-nav flex-fill">
        <div class="navbar-nav">
            <a class="nav-item nav-link" href="#">Left1</a>
            <a class="nav-item nav-link" href="#">Left2</a>
            <a class="nav-item nav-link" href="#">Left3</a>
        </div>
        <div class="navbar-nav ml-auto">
            <a class="nav-item nav-link" href="#">Right1</a>
            <a class="nav-item nav-link" href="#">Right2</a>
        </div>
    </div>
</nav>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/Sample02.ComplexNavBar.Bootstrap4.html#multmlauto)

This puts the margin on the left side of the element on which it is applied. Although above the responsive breakpoint the behaviour is the same, below the breakpoint the second `navbar-nav` is pushed to the right side of the screen, while the first `navbar-nav` is on the left side of the screen.

For the above layout the use of `justify-content-between` and `mr-auto` result in the same behaviour, but there is a fundamental difference between the two which becomes apparent when working with more than two child `navbar-nav`s:

```
<h1><a name="mult3between"></a>A second navigation bar with 3 subsections and collapsing (with content between)</h1>
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="navbar-nav flex-fill justify-content-between">
        <div class="navbar-nav">
            <a class="nav-item nav-link" href="#">Left1</a>
            <a class="nav-item nav-link" href="#">Left2</a>
            <a class="nav-item nav-link" href="#">Left3</a>
        </div>
        <div class="navbar-nav">
            <a class="nav-item nav-link" href="#">Right1.1</a>
            <a class="nav-item nav-link" href="#">Right1.2</a>
        </div>
        <div class="navbar-nav">
            <a class="nav-item nav-link" href="#">Right2.1</a>
            <a class="nav-item nav-link" href="#">Right2.2</a>
        </div>
    </div>
</nav>


<h1><a name="mult3auto"></a>A second navigation bar with 3 subsections and collapsing (with mr-auto)</h1>
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="navbar-nav flex-fill">
        <div class="navbar-nav mr-auto">
            <a class="nav-item nav-link" href="#">Left1</a>
            <a class="nav-item nav-link" href="#">Left2</a>
            <a class="nav-item nav-link" href="#">Left3</a>
        </div>
        <div class="navbar-nav">
            <a class="nav-item nav-link" href="#">Right1.1</a>
            <a class="nav-item nav-link" href="#">Right1.2</a>
        </div>
        <div class="navbar-nav">
            <a class="nav-item nav-link" href="#">Right2.1</a>
            <a class="nav-item nav-link" href="#">Right2.2</a>
        </div>
    </div>
</nav>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/Sample02.ComplexNavBar.Bootstrap4.html#mult3between)

As you can see by using the `justify-content-between` class, when we have more then two children, the space is evenly distibuted between the children. By using `mr-auto` all the excess space is taken by the margin of the first child which pushes the second and third child all the way to the right.

Can we do the same with a button? Yes we can:

```
<h1><a name="multbetweenbtn"></a>A third navigation bar (with a button)</h1>
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent_btn" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse justify-content-between" id="navbarSupportedContent_btn">
        <div class="navbar-nav">
            <a class="nav-item nav-link" href="#">Left1</a>
            <a class="nav-item nav-link" href="#">Left2</a>
            <a class="nav-item nav-link" href="#">Left3</a>
        </div>
        <div class="navbar-nav">
            <a class="nav-item nav-link" href="#">Right1</a>
            <a class="nav-item nav-link" href="#">Right2</a>
        </div>
    </div>
</nav>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/Sample02.ComplexNavBar.Bootstrap4.html#multbetweenbtn)

Notice however we do NOT need the `flex-fill` class here. You'll find out why in the next section.

## Deconstructing complex navigation bars

### Deconstructing more alignment

Remember from the simple examples how the `navbar` class was defined:

```
.navbar {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
}
```

The fact that, when we specify an explicit direction of type `column`, the items end up centered is of course because of the `align-items: center` property. Remember that `align-items` works in the cross direction which in case of a flow direction column is horizontal.

### Deconstructing more spacing

We have already seen how to do alignment in the direction of the flow in the simple navigation bar section. However, when we try alignment in the flex direction for nested flexboxes, maybe you will not get what you expect:

```
<h1>Managing the layout of flex children: in the direction of the flow for nested horizontal flow flexboxes</h1>
<div class="simple-flexbox parent-container">
    <div class="simple-flexbox parent-container flexbox-justify-end">
        <div class=" bg-red">Flexbox subitem 1</div>
        <div class=" bg-green">Flexbox subitem 2</div>
        <div class=" bg-blue">Flexbox subitem 3</div>
    </div>
</div>
<div class="simple-flexbox parent-container">
    <div class="simple-flexbox parent-container flexbox-justify-between">
        <div class=" bg-red">Flexbox subitem 1</div>
        <div class=" bg-green">Flexbox subitem 2</div>
        <div class=" bg-blue">Flexbox subitem 3</div>
    </div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/Sample02.ComplexNavBar.Bootstrap4.html#mult3between)

Allthough we basically applied the same content alignment as in the simple navigation bar deconstruction, there is no difference for these flexboxes: they both start at the beginning. What is happening?

Inside a flexbox, the children just take the space they need. In the above, the child flexbox just needs space for it's three children. Which is why the two constructs result in the same result.

We can change this behaviour by explicitely setting the width of the inner flexbox. In the navbar examples above we did this through the `flex-fill` class.

### Deconstructing flexbox child sizing

Below is the definition of the `flex-fill` class:

```
.flex-fill {
  flex: 1 1 auto !important;
}
```

This is actually a shortcut notation for three other flexbox properties:

```
.flex-fill {
  flex-basis: auto;
  flex-grow: 1;
  flex-shrink: 1;
}
```

I assume you read the section on responsive breakpoints, so you know what the `important!` construct does.

Ok, we'll start with `flex-basis`.

This property allows you to define how the dimension in the flow direction is defined. It can be a number with a unit or just `auto`. In the case you use a number with a unit it overrides any other dimension settings on the element to which it is aplied. If you set it to `auto`, the element takes either the dimension as specified on the element or sizes automatically based on its content.


```
.width-100 {
    width: 100px;
}

.flexitem-auto {
    flex-basis: auto;
}
Rem
.flexitem-dim {
    flex-basis: 150px;
}

.flexitem-dimpercentage {
    flex-basis: 33%;
}

.flexitem-dimpercentage-large {
    flex-basis: 60%;
}

<h1><a name="sizebasis"></a>Managing the size of flex children</h1>
<div class="simple-flexbox">
    <div class="width-100 bg-red">Flexbox subitem 1</div>
    <div class="width-100 bg-green">Flexbox subitem 2</div>
    <div class="width-100 bg-blue">Flexbox subitem 3</div>
</div>
<div class="simple-flexbox">
    <div class="width-100 bg-red">Flexbox subitem 1</div>
    <div class="flexitem-auto bg-red">Flexbox subitem 2</div>
    <div class="flexitem-dim width-100 bg-green">Flexbox subitem 3</div>
    <div class="flexitem-dimpercentage width-100 bg-blue">Flexbox subitem 4</div>
</div>
<div class="vertical-flexbox">
    <div class="flexitem-dim bg-green">Flexbox subitem 2</div>
    <div class="flexitem-dimpercentage bg-blue">Flexbox subitem 3</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/CSSFlexbox.Deconstructed.html#sizebasis)

So, what is going on here?

The first `div` just defines the width in a regular way: nothing special here.

In the above description I deliberately avoided talking about the width and height of an element but instead used the more abstract *dimension*. The `flex-basis` property works in the direction of the flow of the flexbox. This means that when the flow is horizontal it controls the width and if the flow is vertical it controls the height.

The second `div` uses the flex-basis property to define and override the standard width property. As you can see the width defined in this property supersedes the normal width. It can also have a textual value of `auto` which means: take the value of the width property as normally calculated and which is based on the content inside the element on which it is applied or the value of the width property if it is explicitely defined.

The third `div` does a similar thing, but inside a flex `div` with a vertical flow. Notice how the item with the flex-basis defined as a pixel value now gets this applied to its height!

Next are the `flex-grow` and `flex-shrink` properties.

```
.flexitem-expand-grow1 {
    flex-basis: auto;
    flex-grow: 1;
}

.flexitem-expand-grow2 {
    flex-basis: auto;
    flex-grow: 2;
}

.flexitem-collapse-noshrink {
    flex-shrink: 0;
}

.flexitem-collapse-shrink1 {
    flex-basis: 60%;
    flex-shrink: 1;
}

.flexitem-collapse-shrink2 {
    flex-basis: 60%;
    flex-shrink: 2;
}

<h1>Managing the size of flex children: excess or shortage of space</h1>
<div class="simple-flexbox">
    <div class="flexitem-expand-grow1 bg-red">Flexbox subitem 1</div>
    <div class="flexitem-expand-grow2 bg-green">Flexbox subitem 2</div>
    <div class="bg-blue">Flexbox subitem 3</div>
</div>
<div class="simple-flexbox">
    <div class="flexitem-collapse-noshrink bg-red">Flexbox subitem 1</div>
    <div class="flexitem-dimpercentage-large flexitem-collapse-noshrink bg-green">Flexbox subitem 2</div>
    <div class="flexitem-dimpercentage-large flexitem-collapse-noshrink bg-blue">Flexbox subitem 3</div>
</div>
<div class="simple-flexbox">
    <div class="flexitem-collapse-noshrink bg-red">Flexbox subitem 1</div>
    <div class="flexitem-dimpercentage-large flexitem-collapse-shrink1 bg-green">Flexbox subitem 2</div>
    <div class="flexitem-dimpercentage-large flexitem-collapse-shrink2 bg-blue">Flexbox subitem 3</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/CSSFlexbox.Deconstructed.html#sizespace)

The `flex-grow` property manages the distribution of exess space over the items inside a flexbox. As you can see from the first example, although we specify a `flex-basis` of auto and would expect the children to only take the space they need, because of the `flex-grow` property on the first two items the excess space is distributed across these items. What is more: the space is distributed proportionely with the numbers assigned to these properties.

In contrast, the `flex-shrink` property regulates shrinking of children of the flexbox when there is insufficient space for all the children. Again, the amount the items shrink is set by the value of the `flex-shrink` property.

A detailed analysis of the calculation along with a demonstration can be found in the [Bootstrap 4 Grid: The CSS Flex Layout System](https://www.codeproject.com/Articles/5265672/The-Bootstrap-4-Grid-Deconstructed) article.

### Deconstructing more spacing continued

So, back to spacing and nested flexboxes.

In the complex navigation bar samples we wrapped the nested `navbar-nav`s in a parent `navbar-nav` on which we set the `flex-fill` class. As seen above the `flex-fill` class looks like this:

```
.flex-fill {
  flex: 1 1 auto !important;
}
```

And now we can understand how this works: because by using this class we set the `flex-grow` property of the nested flexbox to 1 it takes the excess space available in its parent, thus taking the full width of the window. And because of this the children of the nested flexbox can now apply the `justify-content` property with a visible result.

And what about the `mr-auto` and `ml-auto` classes?

```
.mr-auto {
  margin-right: auto !important;
}

.ml-auto {
  margin-left: auto !important;
}
```

These set the margin of the element on which they are applied to take the remaining space available in the parent. There is however a caveat:

```
.width-100 {
    width: 100px;
}

.marginleft-auto {
    margin-left: auto;
}

Let us first find out what this does in a non flexbox element:

<h1><a name="automargin_noflexbox"></a>Applying auto margins outside a flexbox</h1>
<div>Some text and then suddenly a <span class="marginleft-auto bg-red">span with left margin set to auto</span> and then some more text: nothing is happening!</div>
<div>
    <div class="marginleft-auto bg-red">Some nested div inside a parent div</div>
    <div class="width-100 marginleft-auto bg-blue">Some nested div inside a parent div set to a fixed width</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Nav%20bar/CSSFlexbox.Deconstructed.html#sizespace)

Notice how:
- For inline content (like the `span` element) setting the margin to `auto` resolves to a value of 0: no margins are applied.
- For block content (like the `div` element) setting the margin to `auto` also has no effect because these typically take the full width of their parent element.
- For block content with a specified width, the margin setting does take effect: the margin takes all left over space.


Now, for a flexbox element:

```
<h1><a name="automargin_flexbox"></a>Applying auto margins inside a flexbox</h1>
<div class="simple-flexbox">
    <div class="bg-red">Flexbox subitem 1</div>
    <div class="marginleft-auto bg-blue">Flexbox subitem 2</div>
</div>
<div class="simple-flexbox">
    <div class="bg-red">Flexbox subitem 1</div>
    <div class="marginleft-auto bg-blue">Flexbox subitem 2</div>
    <div class="marginleft-auto bg-green">Flexbox subitem 3</div>
</div>
<div class="vertical-flexbox">
    <div class="bg-red">Flexbox subitem 1</div>
    <div class="marginleft-auto bg-blue">Flexbox subitem 2</div>
</div>
```

Notice how:
- For a flow of `row`, if multiple children have the `auto` setting for their margin, the excess space is evenly distributed over these children.
- For a flow of `column`, if we use the `auto` setting on the `margin-left` or `margin-right` property that is actually in the cross direction. But the margin is still applied.

### Deconstructing responsive navigation bars with a toggle button: what I didn't tell you earlier...

Remember when I explained responsive navigation bars with a button I ignored some properties.

Notice how in the navigation bar with a button we defined two child `navbar-nav`s, set `justify-content-between` et voila, we had seperated subgroups. No need for `flex-fill` as you might expect. What is going on here?

The definition of the CSS classes will make eveything clear immediately:

```
.collapse:not(.show) {
  display: none;
}

.navbar-collapse {
  flex-basis: 100%;
  flex-grow: 1;
  align-items: center;
}

@media (min-width: 992px) {
  .navbar-expand-lg .navbar-collapse {
    display: flex !important;
    flex-basis: auto;
  }
}
```

Notice the `flex-grow` property on the `navbar-collapse` class: there is no need for `flex-fill` because the setting is already made in the `navbar-collapse` class itself!

Also notice the `flex-basis: 100%` setting on the `navbar-collapse` class: this makes sure the items inside it get neatly pushed below the button. Above the responsive breakpoint, the `flex-basis: auto` setting from `.navbar-expand-lg .navbar-collapse` kicks in.

## Conclusion

There is a lot more to say about the navigation bar in Bootstrap 4. However, the most important things to take with you from this article, and which should get you a long way are:

1. `navbar` is a flexbox with a default direction of `row`
2. `navbar-nav` is also a flexbox but with a default direction of `column`
3. Because `navbar-nav` is normally used inside a `navbar` it does not take the full width of its parent but only the necessary width to display its content.
4. `navbar-expand-xx` modifies the direction of `navbar-nav` to `row` starting at the responsive breakpoint.
5. `navbar-collapse` is used with the `collapse` class to make the element a flexbox container and set its width to 100%, thus pushing its content below any predecessors.
6. `navbar-expand-xx` modifies `navbar-collapse` to have a width of `auto` thus only taking the space needed for its children and forces a `display: flex` to make it visible beyond the responsive breakpoint.


## References

A very thourough article on the flexbox system: [understanding flexbox everything you need to know](https://www.freecodecamp.org/news/understanding-flexbox-everything-you-need-to-know-b4013d4dc9af/)
Another guide to the flexbox system: [A guide to flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
More about the `data` attribute: [HTML data-* Attribute](https://www.w3schools.com/tags/att_data-.asp)
The order in which CSS classes are applied is called "the specificity". You can read more about it here: [specifics on css specificity](https://css-tricks.com/specifics-on-css-specificity/)

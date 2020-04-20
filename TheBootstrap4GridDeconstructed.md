# The Bootstrap 4 Grid Deconstructed

An update article on what you can do with the flex based Bootstrap 4 Grid system and what makes it different from the Bootstrap 3  implementation.

## Content

## Introduction

Few years back I wrote an article [The Bootstrap Grid Deconstructed](https://www.codeproject.com/Articles/1109210/The-Bootstrap-Grid-Deconstructed) in which I investigated what actually made that grid system work.

In the meanwhile however, my attention got drawn to other subjets, but Bootstrap also got a complete rewrite of its grid system. The new grid system is based on the new flex layout CSS system. 

While upgrading my skillset and implementing an Angular based website I again liked at the Bootstrap Grid system and decided to deep-dive into it and see what makes it work.

I'll be using my original article as a kind of template for the structure of this article and will sometimes reference it for things explained there. I will also assume a basic knowledge of HTML and CSS. That you know what a `<div>`, `<span>`, etc are..., that you know about CSS inheritance rules, ... I also assume you have read the article about the Bootstrap 3 grid system so you are familiar with responsive breakpoints and the like.

*DISCLAIMER: this article is about the Bootstrap Grid and not about the CSS flexbox system. This means I will explain the flexbox system as used by the Bootstrap Grid and thus is by no means a complete investigation of the flexbox system. I will introduce the necessary concepts to understand what makes the Bootstrap Grid work and why certain values of CSS properties here choose.* 

So, let's get started.

## Constructing the basics: what can we do

### The Grid: it's still all about rows and colums

Nothing changed here: we still need to define a container with rows which in turn contains columns. However, where in the Bootstrap 3 grid you had to always specify the width of your columns and make them add up to a total of 12, this is no longer true for the Bootstrap 4 grid.

The Bootstrap 4 grid defines a simple "col" class which allows you to evenly spread your columns over the width of your page while taking up as much space as necessary for the content to match the column.

Of course, you still have the responsive versions of the col CSS class with the "col-xx-yy" structure:

- xx: specifies the breakpoint. That is: from which screensize does the corresponding span take effect.
- yy: specifies the span of the column. That is: how much space does it take in a row.

Some valid markup is:
```
<div  class="container">
	<div  class="row el-frame bg-gray">
		<div  class="col bg-red">First</div>
		<div  class="col bg-green">Second</div>
		<div  class="col bg-blue">Third</div>
	</div>
</div>

<div  class="container">
	<div  class="row el-frame bg-gray">
		<div  class="col-md-4 bg-red">First</div>
		<div  class="col-md-4 bg-green">Second</div>
		<div  class="col-md-4 bg-blue">Third</div>
	</div>
</div>

<div  class="container">
	<div  class="row el-frame bg-gray">
		<div  class="col-md-8 bg-red">The first column</div>
		<div  class="col-md-4 bg-green">The second column</div>
	</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/Sample01.SimpleSpanSum.Bootstrap.html)


Nothing much has changed here: the sum of the spans still add to 12, and the xx portion of the name still defines from what screen width the columns transition to rows.

### Get Responsive: defining Responsive Breakpoints

Of course, the grid is all about being responsive: we want to re-arrange the layout of some portions of our webpage to different positions depending of the screen size of the device we use to view the page.

If we have a large screen, we may want to show some portions next to each other. If we have however a narrow screen, we may consider showing them underneath each other and be able to specify which portion should be above/below which other portion.

This is where the "xx"-portion of the CSS class names comes into play.

Let's try following:

```
<div  class="container">
	<div  class="row el-frame bg-gray">
		<div  class="col-sm-8 bg-red">The first column</div>
		<div  class="col-sm-4 bg-green">The second column</div>
	</div>
</div>
 
<div  class="container">
	<div  class="row el-frame bg-gray">
		<div  class="col-md-8 bg-orange">The first column</div>
		<div  class="col-md-4 bg-blue">The second column</div>
	</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/Sample02.SinglularBreakpoint.Bootstrap4.html)

 If you now stretch and shrink your browser window, you will notice below a certain width the columns will transition into rows. For the "sm" columns this is at a different width than for the "md" columns. The Bootstrap 4 breakpoints define an extra breakpoint and as a result the width at which the breakpoints are applied is also different:

- *none*: if you specify nothing for the breakpoint, then you will always have columns (smaller than 576px)
- sm: larger than or equal to 576px
- md: larger than or equal to 768px
- lg: larger than or equal to 992px
- xl: larger than or equal to 1200px

So, in the above when we used a class with "sm" as the breakpoint, then we meant:: if your browser window is wider then or equal to 576px use columns, otherwise use rows.

Of course, we can use classes with different breakpoints on a single element:
```
<div  class="container">

	<div  class="row">
		<div  class="col-sm-2 col-lg-5">1 First</div>
		<div  class="col-sm-8 col-lg-2">1 Second</div>
		<div  class="col-sm-2 col-lg-5">1 Third</div>
	</div>

	<div  class="row">
		<div  class="col-4 col-md-5">2 First</div>
		<div  class="col-4 col-md-2">2 Second</div>
		<div  class="col-4 col-md-5">2 Third</div>
	</div>

	<div  class="row">
		<div  class="col-4">3 First</div>
		<div  class="col-4">3 Second</div>
		<div  class="col-4">3 Third</div>
	</div>

</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/Sample03.ColumnsTwoBreakpoints.Bootstrap4.html)

In the first row we are telling bootstrap to:
- use column widths proportional to 5, 2, 5 if the screenwidth is larger than 992px
- use column widths proportional to 2, 8, 2 if the screenwidth is between 576px and 992px
- use all rows for screenwidth below 576px

For the last two rows we always have columns. They never transition into rows because we used the *none* breakpoint which is always valid.

## Deconstructing the basics: what makes it work?

So, while we didn't really gain anything in Bootstrap 4, the way things work is completely different from Bootstrap 3. Let's find out what makes it work.

### Deconstructing the grid

#### How is it defined in CSS?

The basic structure of the grid is still the same:
- We have a div which acts as a container
- In this container we have rows
- And finally we fill the rows with columns

The definition of the container doesn't differ much from Bootstrap 3:
```
.container {
  width: 100%;
  padding-right: 15px;
  padding-left: 15px;
  margin-right: auto;
  margin-left: auto;
}
```

Our row class however now looks like this:
```
row {
  display: flex;
  flex-wrap: wrap;
  margin-right: -15px;
  margin-left: -15px;
}
```

The properties to notice here are the display: flex and the flew-wrap: wrap properties. We will get back to these soon.

Next are the col classes. They exist of two parts:
- A *constant* part used when no responsive breakpoints are applied
- A variable part defined by a responsive breakpoint
Let's look at the constant part
```
.col-md-4 {
   position: relative;
   width: 100%;
   padding-right: 15px;
   padding-left: 15px;
 }
```

Although I defined this above for the .col-md-4 specifier, the same is used for all others like .col, .col-1, ..., .col-lg-1, etc...

There is already a difference her: we explicitly set the column width to 100%

Next is the variational part, driven by the responsive breakpoints:

```
.col-md-4 {
   flex: 0 0 33.333333%;
   max-width: 33.333333%;
 }
```

Whereas in Bootstrap 3 we simply specified the width here, in Bootstrap 4 we have this flex specification and an additional max-width property.

So, what is this all about? Well, you may have read about the Bootstrap 4 Grid using the CS flex system. Well, this is it. Let's investigate this further.

#### The CSS Flex layout system

Let's start out simple and investigate the `display: flex`CSS property:

```
<style>
.my-flex {
	display: flex;
	flex-wrap: wrap;
}
</style>

<div>
	<div>My first item in a regular DIV</div>
	<div>My second item a regular DIV</div>
	<div>My third item a regular DIV</div>
</div>

<div  class="my-flex">
	<div>My first item in the flexbox</div>
	<div>My second item in the flexbox</div>
	<div>My third item in the flexbox</div>
</div>
```
[See it in action: Regular vs Flexbox DIV](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/CSSFlexbox.Deconstructed.html#regvsflexbox)

If you look at how this is displayed, you will notice that in contrast with the regular `div`, the `div`with `display: flex`applied has its items layed-out horizontally. Also, when you stretch and shrink your browser window, then the child `div`s are wrapped vertically. This is because of the `flex-wrap: wrap` property. If you leave it of, the items shrink to fit the `div`. Also notice how on the example page, all children take on the same height.

The CSS Flexbox model allows you to easily specify how to layout the children of a container on which the CSS `display: flex` property is applied. The default is a horizontal layout with no wrapping of the content. You can alter the direction of the layout by using the [flex-direction](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-direction) property which can have value `row`, `row-reverse`, `column`and `column-reverse`. The default is `row`which is why in our example the children are layed-out horizontally.

To demonstrate the `flex-wrap`property, I have explicitly set the width of the child `div`s but if you leave it off they take just the available space necessary. In the Bootstrap 4 grid however, columns fill the entire row. This is where the `flex`and `max-width`properties of the col definition come in.

First, the syntax of the `flex`property:
```
.col-md-4 {
   flex: 0 0 33.333333%;
 }
 ```

The syntax is actually a shortcut syntax for 3 other properties:

1. The first number is for the `flex-grow` property
2. The second is for the `flex-shrink` property
3. The third is for the `flex-basis` property

Let's start with the third: the `flex-basis` property. It specifies the initial width of the element it is applied to. In the above it specifies the element should take 33.333333% of the width of the parent flex container, which in our case is the div element with the `row` class. But wait !!! We also specify the `width` of the element as being 100% ! Well, the `flex-basis` property takes precedence over the `width` property.

The default value is `auto` which makes the element use its `size` property.

Some examples:
```
<!-- flexbox DIV with width specification for flex items: use flex-basis percentage-->
<div  class="my-flex el-frame">
	<div  style="flex-basis:20%; background-color:red;">My first item with flex width</div>
	<div  style="flex-basis:30%; background-color:green;">My second item with flex width</div>
	<div  style="flex-basis:50%; background-color:blue;">My third item with flex width</div>
</div>

<!-- flexbox DIV with width specification for flex items: also specify a size-->
<div  class="my-flex el-frame">
	<div  style="flex-basis:auto; width:30%; background-color:red;">My first item with flex width and size</div>
	<div  style="flex-basis:50%; width:25%; background-color:blue;">My second item with flex width and size</div>
</div>
```
[See it in action: Flexbox DIV with width specification for flex items](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/CSSFlexbox.Deconstructed.html#flexboxwidth)

The first example specifies three children for which the width adds-up to exactly 100%. We use percentages here but could also have used pixel width or any other unit.
The second example also specifies a size: the `width`property. Notice how:
- for the `flex-basis: auto` the width of the `width` property is used.
- for the `flex-basis: 50%` the width of this `flex-basis` is used and not the width of the `width` property

OK, then what about `flex-grow` and `flex-shrink` ? They both control what to do with respectively the remaining space or the shortage of space and how to distribute it over the children.

First `flex-grow`. If the sum of the widths we specified for the child elements is less then total width of the flex container, then the `flex-grow`property specifies how much of the available excess space to use to extend/grow the size of the element it is applied to. If applied on multiple child elements, the ratio of these numbers tells how to distribute the excess space.

An example (there are more in the accompanying code):
```
<!-- flexbox DIV with width specification for flex items: also specify a size-->
<div  class="my-flex el-frame">
	<div  style="flex-basis:30%; background-color:red;">My first item with flex width and size</div>
	<div  style="flex-basis:50%; background-color:blue;">My second item with flex width and size</div>
</div>
<!-- flexbox DIV with width specification for flex items: also specify growth-->
<div  class="my-flex el-frame">
	<div  style="flex-basis:30%; flex-grow:1; background-color:red;">My first item with growth</div>
	<div  style="flex-basis:50%; flex-grow:2; background-color:blue;">My second item with double growth</div>
</div>
<!-- flexbox DIV with width specification for flex items: growth calculation-->
<div  class="my-flex el-frame">
	<div  style="width:30%; background-color:red;">My first item original width</div>
	<div  style="width:6.666666%; background-color:orange;">growth</div>
	<div  style="width:50%; background-color:blue;">My second item original width</div>
	<div  style="width:13.333337%; background-color:orange;">growth</div>
</div>
```
[See it in action: Flexbox DIV with growth specification for flex items](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/CSSFlexbox.Deconstructed.html#flexboxgrowth)

- The first `div` shows what we get when *not* specifying any growth: the children take-up the space they are assigned.
- The second `div` shows what happens when we specify a growth and distribute it unevenly over the children: the first child can grow twice as much as the second child.
- The third `div` shows how the excess space of the second example is distributed over the children with a `flex-growth` property. 

The calculation for the distribution of the excess space is simple: take the excess space and distribute it propertionally according the `flex-grow` factor over the children. Thus:

The excess space: 100 - (30+50) = 20
Sum of all growth factors: 1+2 = 3
For a growth factor of 1: 1 x (20/3) = 6.666667
For a growth factor of 2: 2 x (20/3) = 13.333333


Next `flex-shrink`. If the sum of the widths specified for the child element exceeds the total width of the flex container, then the `flex-shrink`property comes into play. It controls by what amount the shortage of space must be distributed over the child elements.

An example:
```
<!-- flexbox DIV with width specification for flex items: specify size-->
<div  style="display: flex; flex-wrap: nowrap;">
	<div  style="flex-basis:60%; flex-shrink:0; background-color:red;">First no shrink</div>
	<div  style="flex-basis:50%; flex-shrink:0; background-color:blue;">Second no shrink</div>
	<div  style="flex-basis:20%; flex-shrink:0; background-color:green;">Third no shrink</div>
</div>

<!-- flexbox DIV with width specification for flex items: also specify shrink-->
<div  style="display: flex; flex-wrap: nowrap;">
	<div  style="flex-basis:60%; flex-shrink:2; background-color:red;">First with double shrink</div>
	<div  style="flex-basis:50%; flex-shrink:1; background-color:blue;">Second with shrink</div>
	<div  style="flex-basis:20%; flex-shrink:1; background-color:green;">Third with shrink</div>
</div>

<!-- flexbox DIV with width specification for flex items: shrink calculation-->
<div  class="my-flex el-frame">
	<div  style="width:41.052632%; background-color:red;">My first item after double shrink</div>
	<div  style="width:18.947368%; background-color:orange;">shrink</div>
</div>
```
[See it in action: Flexbox DIV with shrink specification for flex items](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/CSSFlexbox.Deconstructed.html#flexboxshrink)

- The first `div`shows what we get when we have no shrinking. Notice that to have no shrinking as a reference, we need to explicitly set `flex-shrink: 0`. Also, to have the children layer-out one after another we must explicitly set the `flex-wrap: nowrap` property on the parent flexbox. Otherwise wrapping will be applied.
- The second `div`shows what happens when shrinking is unevenly distributed over the children: the first child can shrink twice as much as the other children.
- The third `div`shows how the shortage in space is subtracted from the children.

The calculation for the distribution of the space shortage is as follows:

The space shortage is: (60 + 50 + 20) - 100 = 30
The total weighted space is (2 x 60) + (1 x 50) + (1 x 20) = 190
For the weighted space of the first child `div` (2 x 60), the shrink is: 30 x ((2 x 60) / 190) = 18.947368
The remaining space of the first child `div` is 60 - 18.947368 = 41.052632 

Ok, so we now have a feel for what the `flex-basis`, `flex-grow` and `flex-shrink` properties mean. As we have seen, the Bootstrap definition of the column uses the shortcut `flex` property:
```
.col-md-4 {
   flex: 0 0 33.333333%;
 }
 ```

So, Bootstrap just prevents any growing or shrinking and fixes the width of the element to 33.333333% of the parent flex container.

So, I have always written above that the width of the element is a percentage of the parent flex container. This is really the immediate parent! So, if you apply a `flex` property on an element two levels deep inside a parent flexbox instead of a direct child, then this property will NOT be applied.
```
<!-- flexbox div with flex children two levels deep -->
<div  class="my-flex el-frame">
	<div>
		<div  style="flex-basis:20%; background-color:red;">My first item 2 deep</div>
	</div>
	<div  style="flex-basis:30%; background-color:green;">My second item with flex width</div>
	<div  style="flex-basis:50%; background-color:blue;">My third item with flex width</div>
</div>
<div  class="my-flex el-frame">
	<div>
		<div  style="flex-basis:200px; background-color:red;">My first item 2 deep</div>
	</div>
	<div  style="flex-basis:30%; background-color:green;">My second item with flex width</div>
	<div  style="flex-basis:50%; background-color:blue;">My third item with flex width</div>
</div>
```
[See it in action: Flexbox DIV with flex items two levels deep](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/CSSFlexbox.Deconstructed.html#flexboxlevel)

### Deconstructing Responsive Breakpoints

I'll refer you to my article on the Bootstrap 3 Grid system for this one: [Deconstructing Responsive Breakpoints](https://www.codeproject.com/Articles/1109210/The-Bootstrap-Grid-Deconstructed#toc132)

## Constructing complex Grids

### Nested rows

You can also nest rows inside columns:
```
<div  class="container-fluid">
	<div  class="row">
		<div  class="col-md-6">
			<!-- nested rows inside -->
			<div  class="row">
				<div  class="col-6">RowAColARow1Col1</div>
				<div  class="col-6">RowAColARow1Col2</div>
			</div>
			<div  class="row">
				<div  class="col-sm-6">RowAColARow2Col1</div>
				<div  class="col-sm-6">RowAColARow2Col2</div>
			</div>
		</div>
		<!-- the above rows are nested inside a row which is the sibling of the following row
		When we collapse the rows, the following row will float underneath the nested rows -->
		<div  class="col-md-6">RowAColB with a very long text with lots of nonsense and much repeating, with lots of nonsense and much repeating, with lots of nonsense and much repeating, with lots of nonsense and much repeating</div>
	</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/Sample05.ComplexCase1.Bootstrap4.html)

There are a few things to notice here:
- The columns in the nested rows also add-up to a total width of 12. That is because of the way columns are defined in Bootstrap: the width is defined as a percentage of the parent container. With nested rows the parent row is inside a column which, in the above examples, has a width of 6 units. But the row is still taken as 100% for its child columns.
- When the Responsive Breakpoint is reached, the RowAColB column will appear under the nested columns. This is to be expected as it is a sibling of the parent row of the nested rows.
- Where in the Bootstrap 3 framework the height of columns in the same row is defined by there content, in the Bootstrap 4 framework they all have equal height. You can see this if you open the  [Bootstrap 3 demo page for nested rows](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/Sample05.ComplexCase1.Bootstrap.html) and shrink the width so that the long text is higher than the height of the nested rows. The background of the column with the nested rows is completely gray. The background of the text is gray from the nested rows. However, the free area under these rows is also gray: the background of the toplevel row. If you try the same in the above example, you'll notice that the free area is orange: the background of the column which contains the nested rows.

### Reordering columns

Of course, the above explained way of reordering when responsive breakpoints are hit has a drawback: if columns transition into rows, the second column always becomes the second row, the third column the third row, etc..

*In fact, this is not really true: it depends on the reading direction of your browser. But I think you get the picture.*

But that may not be what you want. You may want the left most column to end up as a row under the column on the right of it. In Bootstrap 3 there where pull and push CSS classes for this, but these are no longer present in Bootstrap 4. Bootstrap 4 instead has CSS classes for explicite ordering of the columns and offsetting.

This also means you have to start thinking about the default layout you want and the layout you want when your page is viewed on larger screens. Let's say you have two `div`s which on a mobile screen you want to show as rows. However, on larger screens you want to show the first `div` on the left.

You would write something like following:
```
<div  class="row">
	<div  class="col-md-6 order-md-2">RowAColA_FirstAndOrder2</div>
	<div  class="col-md-6 order-md-1">RowAColB_SecondAndOrder1</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/Sample06.ComplexCase2Reordering.Bootstrap4.html)

Notice how:
- Although we used the CSS classes `order-md-2` and `order-md-1` , the ordering got applied correctly. There was no need to specify an ordering of `order-md-6` This is because of the way the ordering of children in a flexbox world. We will look at it in the deconstruction section.

Where with Bootstrap 3 it was possible to push or pull columns out of view (see the previous article), this is no longer possible with Bootstrap 4.

### Offset columns

Another thing you may want to do (which I didn't handle in the Bootstrap 3 article) is offsetting columns.

You can write something like the following:
```
<div  class="row">
	<div  class="col-md-3">RowAColA_FirstAndOrder2</div>
	<div  class="col-md-3 offset-md-3">RowAColB_Offset3</div>
</div>
<div  class="row">
	<div  class="col-md-3">RowBColA_First</div>
	<div  class="col-md-3 offset-md-9">RowBColB_Offset9</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/Sample08.ComplexCase2Offseting.Bootstrap4.html)

Notice how:
- For the first row, the second column gets pushed 3 units after the first column
- For the second row, we defined an offset of 9 units. You might think that the second column will end up off the screen, but that is not the case. In fact: it gets pushed to the end of the next row! You'll understand why once I explain how it works.

### Visibility of columns

Another thing you may want to do is hide some columns.

You can write the following:
```
<div  class="row">
	<div  class="col-md-6">RowAColA</div>
	<div  class="col-md-6">RowAColB</div>
</div>
<div  class="row">
	<div  class="col-md-6 d-none d-md-block">RowBColA</div>
	<div  class="col-md-6 ">RowBColB</div>
</div>
<div  class="row">
	<div  class="col-md-6 d-md-none">RowCColA</div>
	<div  class="col-md-6 ">RowCColB</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/Sample07.ComplexCase3Visibility.Bootstrap4.html)

There are quite a few changes on the behavior of the display classes with respect to Bootstrap 3.

First, visibility is no longer managed through the `hidden` and `visible` css classes. Instead we need to use the new `d-none`class for hiding and a series of css classes managing the `display` property of an element.

Secondly, where as the Bootstrap 3 the `hidden` and `visible` css classes are managed through a maximum and minimum media query and as such making them valid only between two consecutive breakpoints, the Bootstrap 4 equivalent classes take the usual approach: a class defined for a breakpoint is valid from that breakpoint upward.

## Deconstructing complex Grids: what makes it work?

### Deconstructing Nested rows

This basically works the same way it did for Bootstrap 3, however using the CSS flexbox system. Because we define a row inside the column to contain the nested columns, we actually define a new flexbox container which then serves as the parent for the child columns.

```
<style>
.my-flex {
	display: flex;
	flex-wrap: wrap;
}
</style>

<div  class="my-flex el-frame">
	<div  style="flex-basis:75%;">
		<div  class="my-flex el-frame">
			<div  style="flex-basis:10%; background-color:red;">My first item with flex width</div>
			<div  style="flex-basis:30%; background-color:green;">My second item with flex width</div>
		</div>
	</div>
	<div  style="flex-basis:25%; background-color:blue;">My third item with flex width</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/NestedColFlexboxCSS.Deconstructed.html)

There is actually not much to say about this: we have a toplevel `div` with a `display` value of `flex` and nested inside as direct children items with the `flex-basis` set which as we know by now act as columns. Inside one of this items we again put a flexbox element in which we but direct children with a `flex-basis`. These child elements take their `flex-basis` with respect to the nested flexbox element which is their direct parent.

### Deconstructing Reordering columns

Reordering of columns also got a lot simpler: the flexbox model provides direct support for ordering by making available an `order` property. (To be honest it is actually the other way around: there is an *general* `order` property which is supported, amongst others, by the flexbox model)

```
<div  class="my-flex el-frame">
	<div  style="flex-basis:20%; background-color:red;">My first item followed by repeating text, followed by repeating text, followed by repeating text</div>
	<div  style="flex-basis:30%; background-color:green;">My second item</div>
	<div  style="flex-basis:50%; background-color:blue;">My third item</div>
</div>

<div  class="my-flex el-frame">
	<div  style="flex-basis:20%; order:1000; background-color:red;">My first item with largest order followed by repeating text, followed by repeating text, followed by repeating text</div>
	<div  style="flex-basis:30%; order:-5; background-color:green;">My second item with negative order</div>
	<div  style="flex-basis:50%; background-color:blue;">My third item with no order</div>
</div>

<div  class="my-flex el-frame">
	<div  style="flex-basis:20%; order:1000; background-color:red;">My first item with largest order followed by repeating text, followed by repeating text, followed by repeating text</div>
	<div  style="flex-basis:15%; order:-5; background-color:green;">My second item with negative order -5</div>
	<div  style="flex-basis:15%; order:-5; background-color:yellow;">My third item with negative order -5</div>
	<div  style="flex-basis:50%; background-color:blue;">My fourth item with no order</div>
</div>
```
[See it in action](http://htmlpreview.github.io/?https://github.com/sergedesmedt/BootstrapDeconstructed/blob/master/Responsive%20Grid/ReorderingColsFlexboxCSS.Deconstructed.html)

It is simple how this works: all child elements are sorted by their `order` property and then rendered.

Notice how:
- Specifying no order acts as an order value of 0
- When specifying the same order value twice, the elements are rendered in the order they are defined

### Deconstructing Offset columns

Offsetting columns is done through adding margins to the `div`s representing the columns.

```
<div  class="my-flex el-frame">
<div  style="flex-basis:33%; background-color:red;">My first item followed by repeating text, followed by repeating text, followed by repeating text</div>
<div  style="flex-basis:33%; background-color:green;">My second item</div>
<div  style="flex-basis:34%; background-color:blue;">My third item</div>
</div>

<div  class="my-flex el-frame">
<div  style="flex-basis:33%; background-color:red;">My first item with largest order followed by repeating text, followed by repeating text, followed by repeating text</div>
<div  style="flex-basis:34%; margin-left:33%; background-color:blue;">My third item with no order</div>
</div>

<div  class="my-flex el-frame">
<div  style="flex-basis:33%; background-color:red;">My first item with largest order followed by repeating text, followed by repeating text, followed by repeating text</div>
<div  style="flex-basis:34%; margin-left:45%; ; background-color:blue;">My fourth item with no order</div>
</div>
```

By adding margins to the left, we are pushing the column to the right. Of course, if the margins get to large, the `div` is pushed to the next row. You might expect it then to start at the beginning, but because the margin still exists it doesn't.

### Deconstructing Visibility of columns

For a basic introduction to visibility and the difference between the CSS properties `visibility:hidden` and `display:none` I'll forward you to my previous article on the Bootstrap 3 grid: [Deconstructing Visibility of columns](https://www.codeproject.com/Articles/1109210/The-Bootstrap-Grid-Deconstructed#toc153).

Apart from the naming of the classes and the way responsive breakpoints are handled, there isn't really much new here.

```
<div  class="my-flex el-frame">
	<div  style="display:none;">none</div>
	<div  style="visibility:hidden;">hidden</div>
</div>

<div  class="el-frame">
	<div  style="background:green;width:25%;display:block">Fourth width, display block</div>
	<div  style="background:blue;display:inline;width:50%">Half width, display inline</div>
	<div  style="background:green;width:25%;display:flex">Fourth width, display flex</div>
</div>

<div  class="my-flex el-frame">
	<div  style="background:green;width:25%;display:block">Fourth width, display block</div>
	<div  style="background:blue;display:inline;width:50%">Half width, display inline</div>
	<div  style="background:green;width:25%;display:flex">Fourth width, display flex</div>
</div>

<div  class="el-frame">
	<div  class="showit-always">Show it always</div>
	<div  class="showit hideit">Show it always, unless I hide it</div>
	<div  class="showit hideit-later">Show it always, unless I hide it later</div>
	<div  class="showit-always hideit">Show it always, even if I hide it</div>
</div>
```

Hiding is still done by setting the `display` property to `none`. However, where in Bootstrap 3 there were `visible`and `hidden` CSS classes, in Bootstrap 4 we only have a bunch of classes named after the possible values for the `display` property. 

For making things invisible, the choice is easy: we want to set the `display` property to `none`. 

However, for making things visible, which of all the possible values do you choose? Most of the time one of the `d-xx-block` CSS classes will suffice. They set the `display` property to a value of `block` . The `md-xx-inline` don't make sense as children of a `flex` element, as you can see in the examples above.

As discussed in the previous article, the `inline` value makes elements which normally behave like `blocks`, behave like `inline` elements (like for example `<span>`). Inline elements continue on the same line and take just the space they need, where `block`elements take the full width. This is what you can see  in the second parent `div` in the examples above: the inline blue `div` , although having it's width specified to take 25% of the parent, only takes the necessary space. However, a `flex` parent like the third `div` makes it's children behave like `block`elements, which is why in that case the blue element does take the width specified.

## References:

On how flexible lengths are resolved[W3. org resolve flexible lengths](https://www.w3.org/TR/css-flexbox-1/#resolve-flexible-lengths)
On the sense and nonsense of the new display classes: [missing visible and hidden in bootstrap v4](https://stackoverflow.com/questions/35351353/missing-visible-and-hidden-in-bootstrap-v4)

## History

- Version 1.0: Initial version
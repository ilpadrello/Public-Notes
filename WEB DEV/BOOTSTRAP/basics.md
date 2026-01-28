---
title: "BOOTSTRAP 5 - LAYOUT"
date: "2022-07-31"
---

Some Core concepts to remember:

## Breakpoints

Bootstrap is a mobile-first framework, and to help you create a different kind of look for different devices (or just page size), they created breakpoints that are defined sizes. When the page is larger than the breakpoint, it will behave as expected, so for example, if you set up two columns of 6 and 6 (remember that bootstrap uses a 12-column grid strategy so you have to add up to 12) it will behave normally until the size of the page is below the breakpoint you have specified! Then the two columns will be one under the other!

There are six breakpoints: **xs, s, m, l, xl, xxl** ! And they can be personalized. More info here **[https://getbootstrap.com/docs/5.2/layout/breakpoints](https://getbootstrap.com/docs/5.2/layout/breakpoints)/**.

## Containers

Containers are the fundamental building block when you create a layout with bootstrap. It allows you to contain, pad, and align your content and is required when you want to use the bootstrap grid system. You can nest them, but most layouts do not require you to.

Bootstrap comes with three different containers:

- `.container`, which sets a `max-width` at each responsive breakpoint
- `.container-{breakpoint}`, which is `width: 100%` until the specified breakpoint
- `.container-fluid`, which is `width: 100%` at all breakpoints

If you use just the class container, the container will be "fixed" width, but this fixed value will change following the breaking point, for example under a specific page width the container will be full width with no border, and when the page is more than that specific size, will no longer be 100% width but a specific size! This means that you will have some "borders" to the container (a `max-width` is used in this case)

You can also specify starting from witch breakpoint the width will no longer be 100%. If you use `.container-xxl` the container width will be `100%` until the screen reach the `xxl` breakpoint and then will have a fixed size.

```
<div class="container-sm">100% wide until small breakpoint</div>

<div class="container-md">100% wide until medium breakpoint</div>

<div class="container-lg">100% wide until large breakpoint</div>

<div class="container-xl">100% wide until extra large breakpoint</div>

<div class="container-xxl">100% wide until extra extra large breakpoint</div>
```

If you want your container to be always `100%` you can use `.container-fluid` this will make the container always `100%`

## GRID SYSTEM

Bootstrap has a grid system mobile-oriented, made with flex-box.  
The grid is made out of a 12 column, and this is out how you lay out a basic grid :

```
 <div class="container text-center">

  <div class="row">

    <div class="col">

      Column

    </div>

    <div class="col">

      Column

    </div>

    <div class="col">

      Column

    </div>

  </div>

</div>
```

Inside a container, you put a .`row` class for each row and then a `.col` class for each column

What if you want to have two columns that take both half of the size of the container?

```
<div class="container"> 

        <div class="row border"> <!-- border is just for visualization -->

            <div class="col-6 border">Primo</div>

            <div class="col-6 border">Secondo</div>

        </div>
</div>
```

![](image.png)

As you can see we have `.col-6` and `.col-6` columns, that is because at the end we have to allocate 12 columns.  
So you can do something likle `.col-3` and `.col-9`

![](image-1.png)

So we have 3/12 (1/4) occupied with the first column and then 9/12 (3/4) columns occupied with the second column

What if you don't use all the 12 spaces you dispose of?

![](public/WEB%20DEV/CSS/images/image-2.png)

If you remove the second column, only 1/4 of the space will be used and the rest will be empty! The spaces that you don't use will be empty.

If you don't want a particular division in your row, and you just need an equal separation of the columns you don't need to specify the columns, you can just use `.col`, the number of div with that class will determine how many columns you will have.

## VARIABLE WIDTH CONTENT

Use `col-{breakpoint}-auto` classes to size columns based on the natural width of their content.

## GRID SYSTEM AND MOBILE (Advanced)

We said before that the grid system is mobile-first oriented. Why is that, because you can choose how the grid will look in the different breakpoints.  
For example, you can specify that for `xxl` size you want 4 columns, for `lg` you want 2 columns (one above the other) and for `sm` you may want just one column. How do you do that ? Like this :

```
<div class="row border">

            <div class="col-xxl-3 col-lg-6 col-xs-12 border">Primo</div>

            <div class="col-xxl-3 col-lg-6 col-xs-12 border">Secondo</div>

            <div class="col-xxl-3 col-lg-6 col-xs-12 border">Terzo</div>

            <div class="col-xxl-3 col-lg-6 col-xs-12 border">Quarto</div>

</div>
```

There are a lot more information that you can find on the subject here: [https://getbootstrap.com/docs/5.2/layout/grid/](https://getbootstrap.com/docs/5.2/layout/grid/)

## MORE ABOUT COLUMNS

There is still more to learn about columns in Bootstrap...

The columns in bootstrap are made with flexbox, and that means that we can use some very useful flexbox architectural design.  
Flexbox let you easily align vertically, and also reorder stuff. More on the subject here : [https://getbootstrap.com/docs/5.2/layout/columns/](https://getbootstrap.com/docs/5.2/layout/columns/)

In order to align what is inside a row you can add `align-items-start`, `align-items-center` or `align-items-end`

![](images/image-3.png)

Or you can use it on the column to self align stuff inside the column using `align-self-start`, `align-self-center` or `align-self-end` :

![](images/image-4.png)

You can also use justify-content on the rows :

```
<div class="container text-center">

  <div class="row justify-content-start">

    <div class="col-4">

      One of two columns

    </div>

    <div class="col-4">

      One of two columns

    </div>

  </div>

  <div class="row justify-content-center">

    <div class="col-4">

      One of two columns

    </div>

    <div class="col-4">

      One of two columns

    </div>

  </div>

  <div class="row justify-content-end">

    <div class="col-4">

      One of two columns

    </div>

    <div class="col-4">

      One of two columns

    </div>

  </div>

  <div class="row justify-content-around">

    <div class="col-4">

      One of two columns

    </div>

    <div class="col-4">

      One of two columns

    </div>

  </div>

  <div class="row justify-content-between">

    <div class="col-4">

      One of two columns

    </div>

    <div class="col-4">

      One of two columns

    </div>

  </div>

  <div class="row justify-content-evenly">

    <div class="col-4">

      One of two columns

    </div>

    <div class="col-4">

      One of two columns

    </div>

  </div>

</div>
```

![](images/image-5.png)

You can learn about ordering columns, offsetting columns, margins utility (how to easily apply margins, in different breakpoints), floating and more at [https://getbootstrap.com/docs/5.2/layout/columns/](https://getbootstrap.com/docs/5.2/layout/columns/)

## ROW COLUMNS

Row columns are an easier way to specify how many evenly width columns you want in your design

## GUTTERS

Gutters are padding (with negative margin) within your columns and rows, you use them to space things around.

This is an example on how you use it: `gx-lg-6`

The number specify how much the gaps you want (I think is in `rem`)  
`gx` is for the horizontal gaps  
`gy` is for the vertical gaps  
`g` you can specify vertical and horizontal gaps at the same time  
`g[x|y]-{breakpoint}` is used to specify a gap for a specific breakpoint  
`g-0` to remove the gutter (gap) (it will remove also the negative margin)  
You can use gutter with row columns

## UTILITIES FOR LAYOUTING

There are some utilities that are used to help you layout your page, like the display of containers that changes with the size of the page, or some flexbox options that you can use or how to use margin and padding or toggle visibility...  
Those are topic that i will not discuss here, but I will provide a link to the pages.

For now you can check more here : [https://getbootstrap.com/docs/5.2/layout/utilities/](https://getbootstrap.com/docs/5.2/layout/utilities/)

## Z-INDEX

Several Bootstrap components utilize `z-index`, the CSS property that helps control layout by providing a third axis to arrange content. We utilize a default z-index scale in Bootstrap that’s been designed to properly layer navigation, tooltips and popovers, modals, and more.

These higher values start at an arbitrary number, high and specific enough to ideally avoid conflicts. We need a standard set of these across our layered components—tooltips, popovers, navbars, dropdowns, modals—so we can be reasonably consistent in the behaviors. There’s no reason we couldn’t have used `100`\+ or `500`+.

We don’t encourage customization of these individual values; should you change one, you likely need to change them all.

```
$zindex-dropdown:                   1000;
$zindex-sticky:                     1020;
$zindex-fixed:                      1030;
$zindex-offcanvas-backdrop:         1040;
$zindex-offcanvas:                  1045;
$zindex-modal-backdrop:             1050;
$zindex-modal:                      1055;
$zindex-popover:                    1070;
$zindex-tooltip:                    1080;
$zindex-toast:                      1090;
```

## CSS-GRID

You can also use the alternative method to layout things, the CSS-GRID : [https://getbootstrap.com/docs/5.2/layout/css-grid/](https://getbootstrap.com/docs/5.2/layout/css-grid/)

---
title: "CSS FlexBox"
date: "2022-05-20"
---

Great Tutorial Here (A lot is copied from there) : [https://www.freecodecamp.org/news/understanding-flexbox-everything-you-need-to-know-b4013d4dc9af/](https://www.freecodecamp.org/news/understanding-flexbox-everything-you-need-to-know-b4013d4dc9af/)

## SpeedTips:

- Wrap make the elements go back inline
- Main Axis is horizontal Cross Axis is vertical only when `flex-direction` is `row` but they are inverted when is `column`. That means that `justify-content` and `align-item` switch their effect on the `flex-items` and they are rotated by 90°
- Justify Content and Align-content are not linked to the flex-direction
- You can use `flex-basis` mostly like is the `width` property, but if you put it to 0 it behave differently.
- You can use the short hand for Grow Shrink and Basis like this: `flex: 1 1 0` and also:

```
flex: auto /* For 1 1 auto */ 
flex: 1 /* For 1 1 0;*/
```

If you’re the not the type of person who writes CSS in their dreams, you may want to watch this github [repository](https://github.com/philipwalton/flexbugs).

Some people who are smarter than I am curate a list of Flexbox bugs and their workarounds there.

## Tutorial:

Flexbox is a powerful tool when come to making layouts in CSS.

To start using the Flexbox model, all you need to do is first define a _flex-container._

In regular HTML, laying out a simple list of items takes this form:

```
<ul> <!--parent element-->  
  <li></li> <!--first child element-->  
  <li></li> <!--second child element-->  
  <li></li> <!--third child element-->
</ul>
```

To use the Flexbox model, you must make a parent element a flex container (AKA _flexible container_).

You do this by setting `display: flex` or `display: inline-flex` for the inline variation. It's that simple, and from there you're all set to use the Flexbox model

```
//Style.css
ul{
    display: flex;
    list-style-type:none;
}

li {  
    width: 100px;  
    height: 100px;  
    background-color: #8cacea;  
    margin: 8px;
}
```

And this is what you will see:

![](image.png)

Did you notice that all the `<li>` element are displayed as inline elements? It is like we have used float.

This is because we have `display:flex` to the container (parent) and by doing so, the normal behavior of the elements inside changes.

## Flex Container VS Flex Items

As you can see there is a big difference between the Flex Container and the Flex Items:

**Flex Container:** The environment where flex rules are applied and specified (the parent of all flex items)  
**Flex Items:** Each element that will be positioned within the rules of the Flex Container

## Flex Container Properties

You can set a bunch of very useful properties to the container to change the behavior of the items inside the container:

### Flex-direction

This can be:

- column
- column-reverse
- row
- row-reverse

If we change this value of the container in column, we will see all the elements of the flexbox domain ordered in a column (vertically) and if we use the reverse, the items inside the list will be reversed (in order).

Row is the default value.

### Flex-wrap

- wrap
- nowrap
- wrap-reverse

You can decide if you want the item in the page to be shortened to fit the container using `nowrap` or if you want flex to create a new line if there is no space in the container you use `wrap`. Also if you want the first line to be the last you can use `wrap-reverse`.  
`nowrap` is the default

**Little Note**: Justify content, Align-items, Align-content are not related to flex-direction.

### Justify-content

Defines how flex items are laid out in the horizontal axis (main axis), **this will not work on the vertical axis (cross axis)**

- flex-start
- flex-end
- center
- space-between
- space-around

`Flex-start` (**default**) groups all flex-items to the start of the main axis (horizontally when row)  
`Flex-end` all the items start at the end of the container along main axis  
`Center` all items in the middle of the container  
`Space-between` all items are distributed along the horizontal space  
`Space-around` keeps the same spacing around flex items

`Space-between` and `Space-around` are not easy to understand, so there is an image:

![JXtVnsdtk3UVaaZ-5Pymi3UHJKjeFKBKbkAr](https://cdn-media-1.freecodecamp.org/images/JXtVnsdtk3UVaaZ-5Pymi3UHJKjeFKBKbkAr)

![XHuT7la356SYp4X4Dz6slTIs3vMp7rnQRNSV](https://cdn-media-1.freecodecamp.org/images/XHuT7la356SYp4X4Dz6slTIs3vMp7rnQRNSV)

### Align-Items

Like Justify-content (just above) but for Cross axis (vertical)

- `Flex-start` move everything from the top
- `Flex-end` move everything on the bottom
- `Center` everything is vertically centered
- `Stretch` will stretch the flex-item height to the bottom of the container (if any height is not specified).
- `Baseline` aligns flex-items along their "baseline"

The last one is a little tricky, but this image should help

![8-BPKQHsT3SLuvm--djHqBOl9TWeJap1Hv5l](https://cdn-media-1.freecodecamp.org/images/8-BPKQHsT3SLuvm--djHqBOl9TWeJap1Hv5l)

Every item has a baseline, flex just aligns by that.

### Align-content

Align-conent will set properties for the wrapped items. It takes the same values as `align-items` apart from `baseline`

By definition, it controls how the flex-items are aligned in a multi-line flex container.

Just like `align-items`, the default value is also `stretch`

## Flex-Item Properties

We talked about the `Flex-container` properties, but you can set some properties also for the `Flex-item`.

### Order

You can reorder items inside a container using the `order` property:

```
/*select first li element within the ul */    
li:nth-child(1) {
        order: 1;
/*give it a value higher than 0*/
}
```

This will produce the first item to be rendered at the end of the line. Why? Cause by default all the items have an order of 0. 
This way you could group some items easily and order them by group.

### Flex-grox and Flex-shrink

The flex-grow and flex-shrink properties control how much a flex-item should “grow” (extend) if there are extra spaces, or “shrink” if there are no extra spaces.  
They may take up any values ranging from 0 to any positive number (but it is used like a boolean 0 | 1)

By default, the `flex-grow` property is set to `0`. By implication, the flex-item does NOT grow to fit the entire space available.

The value `0` is like a “turn-off” switch. The `flex-grow` switch is turned off. However, if you changed the `flex-grow` value to `1` the items inside the flex-container will try to "grow" to fill every space possible:

<figure>

![cFuJct8qfJ593WTjnShz3yJtFE0yDP0gORiQ](https://cdn-media-1.freecodecamp.org/images/cFuJct8qfJ593WTjnShz3yJtFE0yDP0gORiQ)

<figcaption>

One `Flex-Item` inside a `Flex-Container` with `Flex-grow` activated

</figcaption>

</figure>

This will override the width value if there is one

If you tried resizing your browser, the flex-item would also “shrink” to accommodate the new screen width.

Why? By default, the `shrink` property is set to 1. Which means the `flex-shrink` switch is also turned on!

### Flex-basis

The `flex-basis` property specifies the initial size of a flex-item. Before the `flex-grow` or `flex-shrink` properties adjust it's size to fit the container or not. The default value is `flex-basis: auto`

TIP: When there are two or more flex-items with zero based `flex-basis` values, they share the spacing available based on the `flex-grow` values.

## The Flex Shorthand

The `flex` shorthand allows you to set the `flex-grow`, `flex-shrink` and `flex-basis` properties all at once

![J1qjG9Ri-3pO4ZDuhAkYaFhyhajSXTdCsGLI](https://cdn-media-1.freecodecamp.org/images/J1qjG9Ri-3pO4ZDuhAkYaFhyhajSXTdCsGLI)

```
li {  flex: 0 1 auto;}
```

Think the acronym GSB to remember it.

Also, you can use something like this:

```
flex: auto /* For 1 1 auto */
flex: 1 /* For 1 1 0;*/
```

### Align-self

This property make everything way more flexible! And that because it allows you to modify the position of a single flex-item!  
It may take one of those values:

- `auto`
- `flex-start`
- `flex-end`
- `center`
- `baseline`
- `stretch`

We already know all the differences between those, so no need to explain them further.

## Beware of `margin: auto` alignment on flex items

It is important to understand how the margin auto behaves when used on the flex items, and it is quite simple the logic.  
The margin: auto will take all the space available inside the row (or column):

<figure>

![hgk7KKNXN8JjEVM2XUugGZ2bznXq-kAcAOnE](https://cdn-media-1.freecodecamp.org/images/hgk7KKNXN8JjEVM2XUugGZ2bznXq-kAcAOnE)

<figcaption>

Mergin-right: auto on the branding div

</figcaption>

</figure>

<figure>

![swRJd9celASXulipDVS77ltOFIZ-dF4Rq4-2](https://cdn-media-1.freecodecamp.org/images/swRJd9celASXulipDVS77ltOFIZ-dF4Rq4-2)

<figcaption>

Margin auto on both sides. The space available is divided left and right

</figcaption>

</figure>

  
And if you do that with more than one element, the space is divided equally.

When you use the auto-margin alignment on a flex-item, the `justify-content` property no longer works.

## Let's Rotate The World

Do you rememeber that you have to choose if you want to have a `flex-direction` of `row` or `column`? Do you remember that `justify-items` let you distribute items on the horizontal axis? That is true only if you use `row` as `flex-direction`! If you want to use `column`, every property that work on the main axis (normally horizontal) will now have effect in the vertical axis, why? Cause when you flip row to column the main and cross axis flips as well:

![qaRmzHMsHKDBYIxBSIfRt4yg9uIBUHVuWaMD](https://cdn-media-1.freecodecamp.org/images/qaRmzHMsHKDBYIxBSIfRt4yg9uIBUHVuWaMD)

So you have to learn to think in 90° :D

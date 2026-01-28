---
title: "Learn CSS Positioning"
date: "2021-09-23"
---

CSS is not the easiest technology out there, and can be easily frustrating.  
That is why you need to experiment A LOT if you want to be good at CSS. Let's start easy with some basic: Positioning.

Positioning specifies the type of position that the element must have on the page, and usually they also change the behavior of their child.

**ATTENTION**: Positioning is not the same thing as `display`. Those are 2 different properties that act and do different things.

Elements are then positioned using the top, bottom, left, and right properties. However, these properties will not work unless the `position` property is set first. They also work differently depending on the position value

There are 5 basic positioning values :

- `static`
- `relative`
- `fixed`
- `absolute`
- `sticky`

Let's start simple with position STATIC :

## STATIC

This kind of positioning, just simply does not affect the position of an element in a special way, it is positioned according to the normal flow of the page.

```
#CSS

.parent {
  background-color:blue;
  padding:10px;
}

.child-one {
  background-color:lime;
}

.child-two {
  background-color:red;
}

.child-three {
.parent {
  position:static;
  background-color:blue;
  padding:10px;
}

.child-one {
  background-color:lime;
  position:static;
}

.child-two {
  background-color:red;
  position:static;
}

.child-three {
  background-color:magenta;
  position:static;
}


<!-- HTML -->
<div class ="parent">
  Parent
  <div class="child-one">One</div>
  <div class="child-two">Two</div>
  <div class="child-three">Three</div>
</div>


```

![](image.png)

As you can see, the DIVS follow the normal flow of the page. The parent contains the three children, that are showed one after the others.

All the HTML elements are positioned STATIC by default

`Static` positioned elements are not affected by the top, bottom, left, and right properties.

## RELATIVE

Position Relative makes the element position relative to its normal position, that's clear, don't you think?  
Well, no! So let's try to understand better what is going on... Let's change all positioning to `relative`.

```
.parent {
  position:relative;
  background-color:blue;
  padding:10px;
}

.child-one {
  background-color:lime;
  position:relative;
}

.child-two {
  background-color:red;
  position:relative;
}

.child-three {
  background-color:magenta;
  position:relative;
}
```

![](image-1.png)

Absolutely nothing has changed... Wait, what? How ? Why ?  
Well first of all, the difference with static is that you can change TOP LEFT RIGHT and BOTTOM properties (while setting them on a static position will not have any effect on the target). So let's try this :

```
.parent {

  padding:10px;
  top:30px;
}

.child-one {

  left: 20px;
}

.child-two {
  bottom:10px;
}

.child-three {
  right:10px;
}
```

![](public/WEB%20DEV/CSS/images/image-2.png)

As you can see the parent is 40 pixels away `relative` to the top of the page  
The first child is 20px away `relative` to the left of his original position (not page border, because before that there is the padding of this father)  
The second child is 10px away `relative` to the bottom of his original position  
The third child is 10px away `relative` to the right of his original position (counting also the padding of his father), as you can see since the padding is 10px, and it moved away from 10px it overlaps the padding of the parent.

You can start to understand why this is called `relative`.

As you can see, also, the element can overflow their neighbors. This can be something that you want or not...  
Also, if you set position `static` to the child two, and you overflow the first one, it will appear on top.  
While even if you overflow the first one on top of the second one, but this last one has a positioning of `relative`, the second one will be on top, because they have the same positioning.

But there is a catch... you don't usually use TOP RIGHT LEFT BOTTOM with the `relative` position...  
Why is that ? And also why use relative then?

Let's imagine that you want a text to go higher relative what are you currently writing :

```
.higher{
  position:relative;
  top:-5px;
}

If you want your text <span class="higher"> higher </span>then the other text.
```

  

![](images/image-3.png)

This is the result! And this is pretty and sweet.

Also, relative is very good to help the childer understand what they have to do... This will be clear in a moment when we will talk about the position :

## ABSOLUTE

Position Absolute will remove the element from the flow of the page, let's have a look:

```
.parent {
  background-color:blue;
  padding:10px;
}

.child-one {
  background-color:lime;
  position:absolute;
  top: 0px;
}

.child-two {
  background-color:red;
}

.child-three {
  background-color:magenta;
}
```

![](images/image-4.png)

The first child is not considered looking at the flow of the page...  
And that is because we said that it must be away 0px from the top, but what is the top? Is the top of its parent? No it's the top of the page! And that is because the parent (if you look a the css) is not set as `relative`, or `absolut`e, it is just `static`.

If we change the position to relative:

![](images/image-5.png)

This time, the first child is 0px away from the top of its parent.. Because it has `position: relative`.  
This is why position relative is useful for its children.  
And this is recursive, the browser will look for the first element that has no static position, and that will be the staring point, as show here:

```
<!-- Now those are incapusulated and no more siblings -->
<div class ="parent">
  Parent
  <div class="child-one">One
  <div class="child-two">Two
  <div class="child-three">Three</div>
  </div>
  </div>
</div>

.parent {
  position:relative;
  background-color:blue;
  padding:10px;
}

.child-one {
  background-color:lime;
  position:relative;
}

.child-two {
  background-color:red;
  position:relative
}

.child-three {
  background-color:magenta;
  position: absolute;
  top:10px;
}
```

We have encapsulated the child div so that child three is the son of the second and the second son of the first one etc  
Since the second child has position relative this will be the result :

![](images/image-6.png)

  
If we set the child-two position as `static`, it will find the first one that has `relative`:

![](images/image-7.png)

And if we change that also to static, it will look for the parent, and if even the parent have no no-static position, the page is limit.  
This is why positioning is important.

Now let's take a look at the position :

## FIXED

Position fixed allows you to fix elements on the page... you can scroll the page, move-it, it will be anchored to the page, and will not move.  
Think of it as the lower bottom of your Facebook page, where usually all the discussions are.  
Even if you go down while scrolling, the bar is at the bottom of the page, and will not scroll.  
Or like a header that will stay on top while you're scrolling the page.

Now, where the element will go initially? Let's look at different scenarios :

```
<!-- this will be our HTML (so only one parent) -->
<div class="parent">
  Parent
  <div class="child-one">One</div>
  <div class="child-two">Two</div>
  <div class="child-three">Three</div>
</div>
```

Now, if we set everything but the third-one as `position:static` (the third one is fixed obviously) this is what will happen:

![](images/image-8.png) with scroll -> ![](images/image-9.png)

The third child will found its natural place and stay there even when you scroll. The dimension of the div is reduced because it is separated from the rest of the flow, but still, the position is the one that it will have using position static.

Now what will happen when we set top:0px ? let's guess...

![](images/image-10.png)

The div will go on top of the page and stay there even when we scroll. Remember that no parent has the position `relative`.  
Will this change if we set the position of parent as `relative`?  
**NOPE**  
The div is still where it was! So this means that using `position: fixed` using TOP, BOTTOM RIGHT AND LEFT, this will always see for the page border...  
The CSS specification requires that `position:fixed` be anchored to the viewport, not the containing positioned element.

## STICKY

The sticky position is a relative new one, it allows the element to be set as it would be with the normal flow of the page, but when you scroll, the element will stop scrolling when it reaches the distance from the border that you choose (using top, left, etc...).

```
This is a test <span class="staythere">or an example<span>

.staythere{
   position:sticky;
   top:0px;
 }

```

This is with no scroll : ![](images/image-11.png) And this is when you scroll : ![](images/image-12.png)

It is very important to understand that the sticky position will work only inside its scrollable container, aka the parent container.  
This means that if we encapsulate that HTML inside a div, the sticky position will no longer work, because the div will have the same height as the span!

This means that you can easily create some sticky things that goes away when you reach the end of the parent DIV.

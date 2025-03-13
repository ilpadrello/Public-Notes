---
title: "CSS GRID"
date: "2022-05-30"
---

Grids are an evolution of `flexbox` (even if they are not the same thing), because it allows doing things that `flexbox` can't do.  
Usually `flexbox` it is considered a 1D layout tool (only on vertical or horizontal) while `GRID` allows you to layout in 2D (so vertical and horizontal at the same time).

The first thing to do is to define a container that will have a display of `grid`:

```
.wrapper{

  display:grid;

}



.wrapper > div {

  background-color:#eee;

}



.wrapper > div:nth-child(odd){

  background:#ddd;

}

<div class="wrapper">
  <div>First Div</div>
  <div>Second Div</div>
</div>
```

![](images/image.png)

For now, the two divs are just one above the other, as they would normally do. And they take 100% width.  
This is not yet a grid because we don't know yet how many columns or rows are there, so we need to specify that:

```
.wrapper{
  display:grid;
  grid-template-columns: 2fr 1fr;
}
```

![](images/image-1.png)

What is happening here is that we said to CSS that we want our grid to be: the first "cell" to be 2 fractions

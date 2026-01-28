---
title: "Reponsive WebSite"
date: "2020-05-09"
---

There is no need to point out why at this moment you NEED to know how to create a responsive website.  
**_One suggestion when making a website is that you should start making it for the smaller screen to the large screen, this way to work should help you out when programming the page_.**

## THE METATAG

Adding a metatag help this out like this:

```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<!–– The web page should have the same size as the device and there should be no scale -->  
```

The name tells the browser what kind of metadata it is, in this case: `viewport`. The browser's [viewport](https://developer.mozilla.org/en-US/docs/Glossary/viewport) is the area of the window in which web content can be seen, This is often not the same size as the rendered page, in which case the browser provides scroll bars for the user to scroll around and access all the content.  

The `width` property controls the size of the viewport. It can be set to a specific number of pixels like `width=600` or to the special value `device-width`, which is the width of the screen in CSS pixels at a scale of 100%. (There are corresponding `height` and `device-height` values, which may be useful for pages with elements that change size or position based on the viewport height.)

The `initial-scale` property controls the zoom level when the page is first loaded. The `maximum-scale`, `minimum-scale`, and `user-scalable` properties control how users are allowed to zoom the page in or out.

### A pixel is not a pixel

In recent years, screen resolutions have risen to the size that individual pixels are hard to distinguish with the human eye. For example, recent smartphones generally have a 5-inch screens with resolutions upwards of 1920—1080 pixels (~400 dpi). Because of this, many browsers can display their pages in a smaller physical size by translating multiple hardware pixels for each CSS "pixel". Initially this caused usability and readability problems on many touch-optimized web sites. Peter-Paul Koch wrote about this problem in [A pixel is not a pixel](http://www.quirksmode.org/blog/archives/2010/04/a_pixel_is_not.html).

On high dpi screens, pages with `initial-scale=1` will effectively be zoomed by browsers. Their text will be smooth and crisp, but their bitmap images will probably not take advantage of the full screen resolution. To get sharper images on these screens, web developers may want to design images – or whole layouts – at a higher scale than their final size and then scale them down using CSS or viewport properties.

### Viewport width and screen width

Sites can set their viewport to a specific size. For example, the definition `"width=320, initial-scale=1"` can be used to fit precisely onto a small phone display in portrait mode. This can cause [problems](http://starkravingfinkle.org/blog/2010/01/perils-of-the-viewport-meta-tag/) when the browser doesn't render a page at a larger size. To fix this, browsers will expand the viewport width if necessary to fill the screen at the requested scale. This is especially useful on large-screen devices like the iPad. (Allen Pike's [Choosing a viewport for iPad sites](http://www.antipode.ca/2010/choosing-a-viewport-for-ipad-sites/) has a good explanation for web developers.)

For pages that set an initial or maximum scale, this means the `width` property actually translates into a _minimum_ viewport width. For example, if your layout needs at least 500 pixels of width then you can use the following markup. When the screen is more than 500 pixels wide, the browser will expand the viewport (rather than zoom in) to fit the screen:

```
<meta name="viewport" content="width=500, initial-scale=1">
```

Other [attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta#Attributes) that are available are `minimum-scale`, `maximum-scale`, and `user-scalable`. These properties affect the initial scale and width, as well as limiting changes in zoom level.

Not all mobile browsers handle orientation changes in the same way. For example, Mobile Safari often just zooms the page when changing from portrait to landscape, instead of laying out the page as it would if originally loaded in landscape. If web developers want their scale settings to remain consistent when switching orientations on the iPhone, they must add a `maximum-scale` value to prevent this zooming, which has the sometimes-unwanted side effect of preventing users from zooming in:

```
<meta name="viewport" content="initial-scale=1, maximum-scale=1">
```

Suppress the small zoom applied by many smartphones by setting the initial scale and minimum-scale values to 0.86. The result is horizontal scroll is suppressed in any orientation and the user can zoom in if they want to.

```
<meta name="viewport" content="width=device-width, initial-scale=0.86, maximum-scale=3.0, minimum-scale=0.86">
```

## USING THE CHROME TOOL

You can use the "inspect" chrome tool to check different size version of your website.

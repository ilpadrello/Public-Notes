---
title: "Cookies For Javascript"
date: "2022-08-06"
---

[https://blog.logrocket.com/javascript-developer-guide-browser-cookies/](https://blog.logrocket.com/javascript-developer-guide-browser-cookies/)

https://blog.logrocket.com/javascript-developer-guide-browser-cookies/

https://blog.logrocket.com/javascript-developer-guide-browser-cookies/

Cookies are a small pieces of data stored on a browser that's created either by the client-side JavaScript or a server during an HTTP request.  
They are generally used for session management, personalization (such as themes or similar settings), and tracking user behavior across websites. Browsers also typically set limits for cookie size and the number of cookies allowed for a particular domain (typically 4kb and 20 cookies per domain).

 **All domain cookies are sent with every request to the server on that domain**!!!

With the modern web, we got the new [Web Storage APIs](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API) (`localStorage` and `sessionStorage`) for client-side storage, which allows browsers to store client-side data in the form of key-value pairs.

So, if you want to persist data only on the client-side, it’s better to use the APIs because they are more intuitive and easier to use than cookies and can store more data (usually up to 5MB).

## SETTING AND ACCESSING COOKIES ON CLIENT

The JavaScript that downloads and executes on a browser whenever you visit a website is generally called the client-side JavaScript. It can access cookies via the `Document` property `cookie`.

This means you can read all the cookies that are accessible at the current location with `document.cookie`. It gives you a string containing a semicolon-separated list of cookies in `key=value` format:

```
const allCookies = document.cookie;
// The value of allCookies would be something like
// "cookie1=value1; cookie2=value2"
```

Similarly, to set a cookie, we must set the value of `document.cookie`. Setting the cookie is also done with a string in `key=value` format with the attributes separated by a semicolon:

```
document.cookie = "hello=world; domain=example.com; Secure";
// Sets a cookie with key as hello and value as world, with
// two attributes SameSite and Secure (We will be discussing these
// attributes in the next section)
```

Just so you’re not confused, the above statement does not override any existing cookies; it just creates a new one or updates the value of an existing one if a cookie with the same name already exists.

Now, I know this is not the cleanest API you have ever seen. That’s why I recommend using a wrapper or a library like [js-cookie](https://github.com/js-cookie/js-cookie) to handle client cookies:

```
Cookies.set('hello', 'world', { domain: 'example.com', secure: true });
Cookies.get('hello'); // -> world
```

Not only does it provide a clean API for CRUD operations on cookies, [it also supports TypeScript](https://blog.logrocket.com/a-simple-guide-for-migrating-from-javascript-to-typescript/), helping you avoid any spelling mistakes with the attributes.

## SERVER (using Express)

The server can access and modify cookies via an HTTP request’s response and request headers. Whenever the browser sends an HTTP request to the server, it attaches all the relevant cookies to that site with the `cookie` header

Accessing and modifying cookies gets much easier with [Express by adding middleware](https://blog.logrocket.com/express-middleware-a-complete-guide/). For reading, add `[cookie-parser](https://www.npmjs.com/package/cookie-parser)` to get all the cookies in the form of a JavaScript object with [`req.cookies`](https://expressjs.com/en/4x/api.html#req.cookies). You can also use the built-in `res.cookie()` method that comes with Express for setting cookies:

```
var express = require('express')
var cookieParser = require('cookie-parser')

var app = express()
app.use(cookieParser())

app.get('/', function (req, res) {
  console.log('Cookies: ', req.cookies)
  // Cookies: { cookie1: 'value1', cookie2: 'value2' }

  res.cookie('name', 'tobi', { domain: 'example.com', secure: true })
})

app.listen(8080)
```

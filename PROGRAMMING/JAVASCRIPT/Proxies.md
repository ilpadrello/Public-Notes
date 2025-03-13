---
title: "Javascript Proxies"
date: "2021-12-27"
---

I think that JS Proxy is one of the most underrated feature that JS can offer us!

What is a proxy? Is an object wrapper for a JS object (target), and that intercepts important fundamental operations of the target object (like property lookup, assignment, enumeration, and function invocations, etc)

## Creating a proxy

```
let proxy = new Proxy(target, handler);
```

- `target` – is an object to wrap.
- `handler` – is an object that contains methods to control the behaviors of the `target`. The methods inside the `handler` object are called **traps**.

## A simple proxy example

First, define an object called `user`:

```
const user = {
    firstName: 'John',
    lastName: 'Doe',
    email: 'john.doe@example.com',
}
```

Second, define a `handler` object:

```
const handler = {
    get(target, property) {
        console.log(`Property ${property} has been read.`);
        return target[property];
    }
}
```

Third, create a `proxy` object:

```
const proxyUser = new Proxy(user, handler);
```

The `proxyUser` object uses the `user` object to store data. The `proxyUser` can access all properties of the `user` object.

we can access the `firstName` and `lastName` properties of the `user` object via the `proxyUser` object:

```
console.log(proxyUser.firstName);
console.log(proxyUser.lastName);
```

Output:

```
Property firstName has been read.
John
Property lastName has been read.
Doe
```

When you access a property of the `user` object via the `proxyUser` object, the `get()` method in the `handler` object is called.

Fifth, if you modify the original object `user`, the change is reflected in the `proxyUser`:

```
user.firstName = 'Jane';
console.log(proxyUser.firstName);
```

Output:

```
Property firstName has been read.
Jane
```

Similarly, a change in the `proxyUser` object will be reflected in the original object (`user`):

```
proxyUser.lastName = 'William';
console.log(user.lastName);
```

Output:

```
William
```

## TRAPS

As we said, the methods inside the handler object are called traps !  
Those methods are defined by JavaScript to be called when certain events happens on the object, we have already see an example with the **`get()`** **method / trap** .

Why are they called traps ? Because they "trap" your event and execute a specific code (inside the trap).

## The get() trap

As we have seen, the `get()` trap is executed when you try to "get" the information from the proxy object.

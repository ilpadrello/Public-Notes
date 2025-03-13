---
title: "Building Microservices"
date: "2021-03-06"
tags: 
  - "build"
  - "microservice"
  - "node-js"
---

Those are my notes taken while reading "Building Microservice" By sam Newaman, the MUST HAVE book for microservice.

### What Are Microservices ?

Microservices are **small**, **autonomous** services that work together, each microservice is focused on doing one thing well.  
Services usually talk with each other via API.  
But how small is small enough? There is no real answer to this question but a good rule o thumb could be :

- When a service no longer "feels" too big
- When your codebase is too large for a single team
- When you understand and master microservices benefits and downsides, and you can make a good decision.
- You don't need more than 2 weeks to rewrite your microservice

What is an **autonomous** service? It's a service that you can deploy by itself independently, without requiring the consumer to change.  
**_This means that you will have to think well about your API before creating your service!_** (This is a very important part of the microservice System development)

### Why Using Microservices ? (Key benefits)

#### Stop Endeless Growth

Using microservice make sure that you will not have a code base that don't stop growing, because you forgot that you already have created a library for that job, and now you have 2 lib that do the same thing, and when you update one lib, the other one will not be updated as well.

#### Code easy to find

Since each microservice as a job, it is easier to find the code that need to change, projects are way smaller.

#### Tech Heterogeneity

Since each microservice is independent you can use the language that you think will fit better for the job. If you know the performance of a specific tech you can use it to improve your microservice.

This means that you can use different databases for different jobs. So if for your microservice you will need to use a graph-based db you can do it.

This also means that you can test out new technologies without fear! Since your microservice is small enough to be rewritten in 2 weeks you can test new techs and see if the fit the project.

#### Resilience

If something go wrong in one of your microservices, the system will still run (unless is an essential one).

For Monitoring all the system the book suggests to use **_Graphite_** for the Metrics and **_Nagios_** for the Health of your server. But what ever you use you should standardize the monitoring for all services.  
The same should be done for logging : All in one place.

#### Scaling

You can scale your microservice independently! And only your microservice will be scaled, the rest of your code will not. Improvement in performance / cost ratio.

#### Optimizing for Replaceability

You are making sure that there will not be a code too large and too risky to touch so that no one wants to make changes to that code.

If a new technology is showing up, or you think that your code is too old, you can replace it easily. No more FORTRAN code 30 years old that nobody wants to touch.

### Downsides

Microservice have also downsides :

#### Difficult to debug

Debugging a microservice is not exactly an easy task, sometimes it's not easy to understand where the bug is located for example.

#### Tasks to Learn

If you want to really get all the performance improvement we talked about, you need to learn and improve your skill in handle deployment, testing and monitoring,

---
title: Kubernetes Yaml File
---
## Labels and Selectors
This is a fundamental concept that connects everything in kubernetes: Labels and Selectors.

In Docker Compose, services are linked by explicit names. In Kubernetes, everything is **loosely coupled** using key-value pairs called **Labels**.

```yaml
# 1. The Pod is given a label:
metadata:
  labels:
    app: payment-api
    env: production

# 2. The Deployment or Service uses a Selector to find those Pods:
spec:
  selector:
    app: payment-api
```

A Service doesn't care _which_ Pods it routes traffic to, and a Deployment doesn't care _where_ its Pods live. They both simply search the cluster for any Pod matching `app: payment-api`.

This tag-based routing is what allows Kubernetes to seamlessly add, remove, or replace containers without breaking network traffic.

how many kind are there ?
We didn't talk about Selectors only labels here.

---
title: "VUE  3 Naming Convention"
date: "2022-08-15"
---

This is just a reminder of the naming conventions for Vue3:

- **component** name must be `PascalCase`,
- **Props**, form the outside (in the HTML) must be `kebab-case` like `title-value`, and they will be converted automatically in `camelCase` (and not in `PascalCase`)
- **events** emitted must be `kebab-case`

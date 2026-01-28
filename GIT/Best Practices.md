---
title: "GIT BEST PRACTICE"
date: "2020-02-08"
tags: 
  - "best"
  - "git"
  - "practice"
---

## Don't work on master only branches

It is better to use Branches for everything when you want to add something: use a branch, when you want to resolve a bug: use a branch etc... Try to not do commits on your master, but just merges, this way it is way easier to keep track of merging and changes:

- Create a new branch for each major addition to your project
- Don't create a branch if you can't give it a specific name

## Use two permanent branches

We used the permanent master branch as the foundation for all of these temporary branches, effectively making it the historian for our entire project. In addition to master, many programmers often add a second permanent branch called develop. This lets them use master to record really stable snapshots (e.g., public releases) and use develop as more of a preparation area for master.

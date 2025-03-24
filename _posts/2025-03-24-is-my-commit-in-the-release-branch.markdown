---
layout: post
title: Is my commit in the release branch?
date: 2025-03-24 20:21:28 +0100
categories: git
tags: git release
keywords: git log branch release
---

Today I stumbled upon having to know if a specific commit was present or not in the release branch. I of course checked stackoverflow, i didn't have it right in my mind.

```sh
VERSION=2.4
COMMIT_HASH=123acaef123acaed
git log origin/release/$(VERSION) | grep $(COMMIT_HASH) | wc -l
```

1. `git log` - that's I assume it is ok, just checking the `origin` branch
2. `grep` - also I think that's ok, just finding the commit hash from the git log list
3. `wc -l` - stands for word count and is a command-line utility that counts the number of lines, words, characters, and bytes in a file. and `-l` flag tells wc to count only the number of lines.

and there you go! You have effortlessly checked if your commit was present in a specific remote branch



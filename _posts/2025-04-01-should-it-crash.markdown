---
layout: post
title: "Should it crash?"
date: 2025-04-01 20:25:27 +0200
categories: swift
tags: swift crash optional unwrap
keywords: swift crash optional
---

Recently while doing some migration from objective-c to swift and I found a line of code that seemed to me that it should crash, and yet, it didn't crash on production. It was an objective-c code bridged to swift as an implicit unwrapped (!). So here is the snippet, more or less:

```swift
// The bridged generated interface from the objective-c
class BridgedClass {
    var importantNote: String! // contains a nil value
}

// ... then I had a function:

func doSomething(importantNote: String?) {
    print(note)
}

// ... and it was called

doSomething(importantNote: myBridgedInstance.importantNote) // Should it crash?
```

<!-- more -->

The answer as you may already know is **IT DOESN'T CRASH!**, but why? Isn't `myBridgedInstance.importantNote` accessing a `nil` value? Why it is printing `nil`?

So to understand that I went to a swift playground and in short what is happening underhood is:

```swift
doSomething(importantNote: myBridgedInstance.importantNote)
// this is same as:
let importantNote: String? = Optional(myBridgedInstance.importantNote)
print(importantNote)
```

And therefore it won't crash, as it is now an `Optional`. Crazy huh? However if by any chance the developer had changed the signature of the method to:

```swift
func doSomething(importantNote: String)
```

Then it would crash on his face!

## Music

A bit of this and that... old times

<iframe style="border-radius:12px" src="https://open.spotify.com/embed/track/7lOXNXztcb8CQ8UoroiNxW?utm_source=generator" width="100%" height="120" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" loading="lazy"></iframe>


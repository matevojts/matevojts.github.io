---
layout: post
title:  "Coding in Kotlin, like speaking in English"
date:   2020-11-20
excerpt_separator: <!--more-->
---
I've been using Kotlin for 2,5 years now, but it still amazes me how naturally this superb language allows me to write code.

Regularly I try to solve simple issues with writing the words into the IDE like into a chat. After typing a few characters (`writ` for example), I'll pick a suitable `writeThisBlogPost()` method from the suggestion list. 

Sometimes such a method does not exist, and I have to write my own implementation for that, but Kotlin's interface is so cool, that often it is already there for me.<!--more-->

## Sample
Let me show you one example from yesterday: we wanted to know if none of the items are loading.

If you came from the Java world maybe you'll write code like this:
```kotlin
val items: List<Item>

val isNothingLoading = !isLoading()

fun isLoading(): Boolean {
    for (item in items) {
        if (item.isLoading) {
            return true
        }
    }
    return false
}
```

As a Kotliner my first thought was this:
```kotlin
val items: List<Item>

val isNothingLoading = !items.any { it.isLoading }

```

Saying out loud what I wanted (**none** of the items are loading) I gave a shot to Kotlin, and it had this excellent built-in solution for my problem.
```kotlin
val items: List<Item>

val isNothingLoading = items.none { it.isLoading }

```

## Conclusion
I highly recommend learning new language features as I do: type your need into the IDE, and check the list of suggestions. If you've chosen the appropriate word, you'll often find something useful. 

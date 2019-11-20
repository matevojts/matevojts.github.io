---
layout: post
title:  "Join lines in Android Studio"
date:   2019-11-20
---

A warm welcome to you, who are reading the first post here, on my brand new blog.

In the previous days I was wondering what could be the best content for the first post in a blog, where I want to share my moments with Linux, Android, etc. Considering these requirements: it should be something deep technical and for sure a long one!

Mean time Android Studio just made me smile today while I was using a keybinding I found recently: the `Join lines` command, default keybinding: <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>J</kbd>

Starting point:
```kotlin
when (errorMessage) {
    .
    .
    .
    else {
        showServerError()
    }
}
```
In the `else` branch there was no major code so I just wanted to join with the next line - containing `showServerError()` - using the command mentioned above.

And guess what! It didn't just join the lines, but changed the curly braces to the arrow, like this:
```kotlin
when (errorMessage) {
    .
    .
    .
    else -> showServerError()
}
```
It followed the guidelines of Kotlin. Wow!

It was such a great experience, I just continued to press the keybinding, to join as many lines as I could. I'm sure you can imagine the result... 
---
layout: post
title:  "Type alias used to simplify future refactors"
date:   2022-01-11
excerpt_separator: <!--more-->
---

I had to introduce a new model in one of my projects, and by that time, I wasn't sure about its type (`Int` or `String`). I had to start the implementation to meet the deadline, so I choose to hide the type somehow. Kotlin's type aliases came to help me.

```kotlin
typealias Key = Int
```

<!--more-->

From the [official docs](https://kotlinlang.org/docs/type-aliases.html): Type aliases do not introduce new types. They are equivalent to the corresponding underlying types.

## Start coding ASAP

Using the type alias feature I could start the implementation of my app using `Key` everywhere, like this:

```kotlin
interface KeyStore {
    fun currentKey(): Flow<Key>
}


data class MyModel(
    val key: Key,
    val urls: List<String>,
)
```

I skipped the parts, where the type of `Key` is crucial, e.g. the implementation of the above `KeyStore`. In that class, I needed the exact type, as I wanted to persist it into SharedPreferences. It's not the same if it's an `Int` or `String`.

I knew that it will be visible on the UI directly in the future, so during binding I called `toString()` on it.
```kotlin
val key: LiveData<String> =
    keyStore
        .currentKey()
        .map { it.toString() }
        .asLiveData(Dispatchers.Default)
```

## Decision on the type

The only constant is change, and it materialized in my project as the type of `Key` became a `String` rather than an `Int`.

Maybe my decision on type alias usage will pay off here!

## Refactoring

Let's change:

- the type in `typealias`
- remove the redundant `toString()` calls on the usage side (Analyze -> Inspect Code in Android Studio helped me to find them)

Guess what? That's all!

No changes in my stores, viewModels, model classes, nowhere!

## Conclusion

Kotlin's type alias helped me to start coding quickly while skipping some domain-related decisions at the beginning of the development process. Meantime it saved me time on changing the type later.

Win-win.

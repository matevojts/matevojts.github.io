---
layout: post
title:  "How to avoid Android DataBinding NPE related crashes"
date:   2020-05-20
excerpt_separator: <!--more-->
---

During investigating and fixing a crash in one of the apps I'm contributing, I found an issue with DataBinding and custom adapters I'd like to share with you.

<!--more-->
# Initial state

Sample ViewModel with the data class I prepared to contain the info for the binding:
```kotlin
class ViewModel() {

    data class Parent(val parentId: String, val child: Child?) {

        data class Child(val childId: String)
    }

    val sampleData = Parent("parentId", null)

    @get:Bindable
    var parent: Parent = sampleData
        set(value) {
           .
           .
           .
        }

}

```

Initial adapter:
```kotlin
@BindingAdapter("initialBinding")
fun setInitialBinding(view: View, child: ViewModel.Parent.Child) {
    ...
}
```

Wire up adapter and ViewModel in the xml layout:
```xml
<View
    initialBinding="@{vm.parent.child}">
```

What we got runtime:
```
java.lang.IllegalArgumentException: Parameter specified as non-null is null: method kotlin.jvm.internal.Intrinsics.checkParameterIsNotNull, parameter child 
```

# Fix

For a quick fix, after checking another similar adapter I adjusted the code with a `?` in the adapter parameters like this:
```kotlin
@BindingAdapter("fixedBinding")
fun setFixedBinding(view: View, child: ViewModel.Prent.Child?) {
    ...
}
```

Everything went well, after the release we never saw again this kind of crash. I closed the issue in Crashlytics after monitoring it for a couple of days.

# Further improvements

However, my brain could not stop, and I was thinking further about:
1. How to avoid this kind of issue in the future?
2. How can I create a more robust solution?
3. How to convert this runtime exception to something that can be found compile time?

During this journey I found the real issue we have. We're not enforced in xml to feed our *initial* custom binding adapter with proper value, which is **non null** in our case.
So the compiler is not asking for something like this in the xml:
```xml
<View
    idealBinding="@{ vm.parent?.child ?: Parent("parentId", Child("childId")) }">
```

Finally, I understood, that this is the real problem: we have proper information about our data both in ViewModel and in the adapter, but the middleware (the xml) is not clever enough for us.

# Long term solution

To answer both 3 questions I had above I'd suggest the following: remove as much logic from xmls as possible, and pass to the custom adapters the bindable data completely like this:
```xml
<View
    goodBinding="@{vm.parent}">
```

And use a binding adapter like this:
```kotlin
@BindingAdapter("goodBinding")
fun setGoodBinding(view: View, parent: ViewModel.Parent) {
    val child: ViewModel.Parent.Child? = parent?.child
    ...
}
```

With this approach, you moved the logic into the adapter, which is a Kotlin file, where you can gain the advantage of the compiler's feature, that it will warn you: getting the `child` can give you a null result, which should be handled.

---

### I hope I could help you!

Máté

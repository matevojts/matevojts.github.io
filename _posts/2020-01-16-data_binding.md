---
layout: post
title:  "10 thoughts of mine about Android Data Binding"
date:   2020-01-16
excerpt_separator: <!--more-->
---

I've been using the [Data Binding for Android](https://developer.android.com/topic/libraries/data-binding) for 1 year, and I found it's a pretty handy tool for us to wire up xml layouts, Fragments, and our ViewModels.
However, I've been facing some issues during day-to-day usage I want to share with you.
<!--more-->

## Transformed `@Bindable` Boolean field name

Let's check the given code snippet for a typical binding usage for showing a loading state of our screen!

*Snippet from a ViewModel:*
```kotlin
@get:Bindable
    var isLoading: Boolean = false
        set(value) {
            if (value != field) {
                field = value
                notifyPropertyChanged(BR.loading)
            }
        }
```

In case the bindable field is a variable of your ViewModel. You can understand that the variable name `isLoading` becomes `loading` without the prefix after the code generation, as it is visible directly in the code here: `BR.loading`.
It means, that you'll have to refer to it in the xml like this: 
```xml
<Button
android:doSomethingForMe="@{viewModel.loading}"
```

Let's check another example, to show you the issue!

Guess what will happen, if the bindable parameter can be found in the constructor of the ViewModel like this:
```kotlin
class CreditCardViewModel(val card: CreditCard, @Bindable val isDeleteEnabled: Boolean) {

    fun deleteCard() {
        useCase.delete(card)
    }
}
```

You would expext refering the `isDeleteEnabled` from the layout file like this:
```xml
<Button
android:enabled="@{viewModel.isDeleteEnabled}"
android:onClick="@{() -> viewModel.deleteCard()}"
```

But it won't work, as the data binding generates code for your boolean constructor parameter with the name changed to `deleteEnabled`.
Notice, that it became different: without the `is` prefix, and the name will start with a lowercase `d`.

At the end of the day it will work only like this:
```xml
<Button
android:enabled="@{viewModel.deleteEnabled}"
android:onClick="@{() -> viewModel.deleteCard()}"
```

But it's not visible anywhere in your code, that the name will change, only available in the generated files!

## Error when using with `java --version >= JDK 11`

I tried to ask gradle to do me a favor with `./gradlew assemble` command, but I got the following error:
```
> Task :app:kaptDebugKotlin FAILED
e: java.lang.NoClassDefFoundError: javax/xml/bind/JAXBException
```

After a couple of minutes I found that some Java APIs, which are used by data binding became deprecated in Java 9, and completely removed in Java 11.
You can guess: I had jdk 11 installed on my laptop...

**Solutions**:
* Multiple suggestions on how to resolve the issue on [StackOverflow](https://stackoverflow.com/questions/43574426/how-to-resolve-java-lang-noclassdeffounderror-javax-xml-bind-jaxbexception-in-j).
It's important to mention, that I didn't try any of the solution(s) provided there yet.
* Use the Android Studio's built-in jdk, as I just did as a quick workaround.
* You can try out [Jabba](https://github.com/shyiko/jabba), a Java Version Manager. It's handy to change quickly within the installed jdk versions.
* Last but not least: don't use anything above Java 8 if you're an Android developer.

## Layout editor for xmls is often not available

It happens to me quite often, that the layout editor is not available, and the IDE says: *Waiting for build to finish*.

I'm using the editor only for creating some basic constraints at the beginning of implementing a new screen or modifying some, so it's quite annoying to me.

I just figured out, that if I copy the root layout to a separate file: without the `</layout>` tags and the imports, etc. the layout editor became stable. Maybe it's not the best way to deal with that issue, but in the few cases, I had to use it I feel it's ok.

## You have to learn a new middleware DSL

To be able to call your methods, and reach your data from ViewModels you have to use some strange DSL in your layout xmls, which are not straightforward to me in some cases, like:

*Sample 1:*
```xml
android:onClick="@{vm.doSomething()}"
```

*Sample 2:*
```xml
android:onClick="@{() -> vm.doSomething()}"
```

Please tell me for the first view which one is the correct...

Maybe you already know the answer, that *Sample 2* is the correct one, as I just gave you a hint during the explanation of the Transformed Boolean field name issue. Meanwhile, In my opinion, it's not clear enough to understand the structure in a short time.

And you'll receive such error message, which is *Not great, not terrible*.

`"msg":"Cannot find a setter for \u003candroid.view.View android:onClick\u003e that accepts parameter type \u0027void\u0027\n\n`

## Bindable properties and methods visibility is public in ViewModels

```kotlin
@get:Bindable
    var isLoading: Boolean = false
        set(value) {
            if (value != field) {
                field = value
                notifyPropertyChanged(BR.loading)
            }
        }
```

The `isLoading` variable in this sample is visible, as the [official documentation](https://kotlinlang.org/docs/reference/visibility-modifiers.html) states: If you do not specify any visibility modifier, public is used by default, which means that your declarations will be visible everywhere.

My feeling is, that this visibility issue can lead to some mistakes, as you can modify the `isLoading` property - bound to a view visible on screen - from places (and times) you shouldn't.

I didn't run into that problem yet, and with a well-designed architecture most of this kind of issue can be avoided, but still, I feel it dangerous.

It's important to mention, that it can be useful too, as you can call the same method from your fragment, and from the UI xmls too. For example, canceling something with a button on the screen, and from a fragment after a certain event - calling the same method in ViewModel.

## Rename and other kinds of refactors often not followed in xml

It happened to me multiple times, that after renaming a certain method in Android Studio the changes were not done in xml.

With the latest IDE I realized a bunch of improvements in this topic, but sometimes it still happens.

Watch out, and check your layouts, if you receive some error after renaming! ;)

## Navigation from xml to custom bind adapter is not possible in the usual way

Considering you have a custom binding adapter for whatever reason in your app as in this sample:
```xml
<TextView
    app:price="@{viewmodel.price}" />
```

Clicking the first part of the expression or hitting the *Declaration* hotkey in Android Studio (`Ctrl` + `B` by default) unfortunately won't redirect you to the adapter's file.

As a workaround, I often use a global search for the name of the custom adapter (for `price` in our case), but it's not the most comfortable to do ever.

## Two way binding - easy as it should be

I'm sure many of us created a UI for registration, where you should add your name, e-mail address, etc.
And many of us wanted to store the already typed pieces of information, and refill it after restoring state.
Or you used the same xml for editing user information.

Considering an xml like described above I'm pretty sure you should have a view like this:
```xml
<EditText
    android:text="@={vm.firstName}" />
```

The magic is done, as using the expression above you created a 2 way binding for the first name of the user.
This means, that the EditText will be updated as soon as it gets some new data from ViewModel, and vice versa.

It's really handy if you have some info on screen, which can be changed by the user, and from the ViewModel's side too, as it keeps both of them in sync.

## Further issues found

1. Forgotten View or model class imports in xml

    It happened to me a couple of times, that I forgot to import the necessary things in xml. Right now we're receiving quite correct error messages, but you can avoid reading them, if you add the proper imports at the right time, like this:
    ```xml
    <import type="android.view.View" />
    <import type="com.mysampleproject.ui.SomeConnectionState" />
    ```
    The first import is necessary all times.
    The second one is needed, if you want to use that particular class somehow in the xml expressions, like this:
    ```xml
    <Layout
    android:visibility="@{viewmodel.connectionState == SomeConnectionState.CONNECTED}" />
    ```
2. Missing the correct inflation in Fragments

    It's easy to forget that the inflation of the layout at `onCreateView` has to be done using the Binding.

    Just use this:
    ```kotlin
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?) =
    FragmentToDoBinding.inflate(inflater, container, false).root
    ```
    Instead of the good old one:
    ```kotlin
    inflater.inflate(R.layout.fragment_todo, container, false)
    ```
    
## Improvements noticed since I started to use Data Binding

There are several improvements since I started to familiarize myself with this kind of binding, like:
* The errors are given by the compiler are way more understandable, then just 1 year ago. It's a huge step, as my first impression that time was that it's absolutely not debuggable caused by the crazy error messages.
* As I mentioned already the rename and refactor issues are almost resolved

---

I hope I could help you avoid some issues in the future.

But you should never forget: a **clean build** can always help to make it compile. After a certain time, I assigned a hotkey to it too, as it turned out that sometimes the code generation is messed up something so much, that only a clean build helped.

Have a nice day!
---
layout: post
title:  "Change default JDK to Android Studio's bundled JDK"
date:   2020-10-12
excerpt_separator: <!--more-->
---

Recently I was looking for a solution to reduce a bit the enormous amount of RAM consumed by Android Studio and Gradle.

During a meetup of the [Kotlin Budapest User Group](https://www.meetup.com/Kotlin-Budapest/) I received a suggestion to change my default JDK to Android Studio's JDK and [this link](https://twitter.com/chrisbanes/status/1244613598284054528?lang=en) for help.
<!--more-->

I modified the method a bit, as I'm using Ubuntu, so the installation folder of Android Studio is different compared with Mac.

Appended to my `.bash_aliases`:
```
function set-studio-jdk() {
    export JAVA_HOME=/snap/android-studio/$1/android-studio/jre
}
```

Appended to my `.zshrc`:
```
set-studio-jdk "93"
```

Finally, I eliminated the `Daemon could not be reused` issues. However, I didn't check the drop of memory consumption, but I'm sure it has a significant impact.

Reminder for myself: I need to keep an eye on the IDE updates, as the number in the folder path will change, and I'll need to change it's manually in my `.zshrc`, but that's all.
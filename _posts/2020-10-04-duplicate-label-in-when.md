---
layout: post
title:  "Duplicate label in when"
date:   2020-10-04
excerpt_separator: <!--more-->
---
Maybe you already encountered the warning above if you're using Kotlin, and copy-paste often. <!--more-->

Here is an example of how it will look like:


<img src="https://matevojts.github.io//assets/images/duplicate_label_in_when.jpg" style="display: block; margin: auto;" />

I didn't understand first what the IDE wanted to tell me, so I googled for the error message but got no helpful suggestion. Then I started debugging, and I found this code snippet in another file of mine:

```kotlin
const val LINK_ONE = "link_one"
const val LINK_TWO = "link_one"
```

Yes, the good old copy-paste error. I created a new variable with copying the first line, changed the name of the variable, but forgot to change the link text itself.

Finally, the `Duplicate label in when` message made sense for me.

I hope I could help you, and next time your IDE gives you this warning you'll save some time.

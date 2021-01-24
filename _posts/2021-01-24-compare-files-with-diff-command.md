---
layout: post
title:  "Compare files with diff command"
date:   2021-01-24
excerpt_separator: <!--more-->
---
Recently I sent a text file to a colleague, and I received it back with a few, almost viewless changes.
I was looking for something that can compare the two files and help me understand the differences.

A quick search and an article gave me the solution:

### The `diff` command.  <!--more-->

The input:
```
diff my.json received.json
```

The output:
```
145c84
<             "id": "sample-order-id"
---
>             "id": "sample-package-id"
```

If you're using an advanced terminal like me it will help you with some colours, like this:

<img src="https://matevojts.github.io//assets/images/diff.jpg" style="display: block; margin: auto;" />

### Evaluating the output
In the 145th line of `my.json` there is  `"id": "sample-order-id"`, while in the 84th line in `received.json` there is `"id": "sample-package-id"`.

Given this data, I could act, and fix my code.

For further info, I'd suggest the `man diff` command to read the manual.

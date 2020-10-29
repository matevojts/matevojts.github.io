---
layout: post
title:  "Config files migrated to git"
date:   2020-10-29
excerpt_separator: <!--more-->
---
Recently I started to use another laptop for development, and I wanted to keep my configs in sync between all my devices.

I wanted to sync:
- `.bash_aliases`
- `.bashrc`
- `.gitconfig`
- `.ideavimrc`
- `.vimrc`
- `.zshrc`

Previously I was creating backup irregularly into my cloud storage, but I read an article about adding all these files into a git repository, which I found an interesting idea.<!--more-->

The difficulty was that I didn't want to initialize a git repo directly into my home folder, where these files are. **Linking files** (we'll figure out if hard or soft links) came to help me with this issue. If you ever used Windows maybe you already met with file links before: the desktop shortcuts for a particular software are just links to the actual file. It reminds me of a famous sentence of my cousin back from the mid 90's:
> It's there, but it's not there.

She just expressed the biggest issue with links: they can break, so the file it's pointing on is missing.

That's why I choose to use hard links - further reading in [this article](https://www.redhat.com/sysadmin/linking-linux-explained).

Standing in the repository folder with my config files I just needed to delete the original files from the home folder, and then add the hard links with this command:

```
ln .zshrc /home/mate/.zshrc
```

I did it for all the files, and it's working well. With this solution, I gave me the possibility to track my config changes, and distribute to other devices efficiently.

In addition, I committed the IntelliJ settings into this repository too, that's also an important file to keep.

---
layout: post
title: Airplay - The comeback
categories: [opensource]
published: true

---

After a long time about a month ago I resumed my work on airplay, my ruby gem to
interact with the Apple TV (and some others now).

Working on an open source project for more than 2 years is hard but doing so in
a project that relies on reverse engeneering a closed protocol is a PITA.

But here's the thing: I really love working on the airplay gem but I ran out of
energy some months ago because I couln't find a solution for the issues I was
having. And I basically quit... issues pile up and I was attempting a rewrite
without much luck.

Incredible people were the along the dark times like
[sodabrew](https://github.com/sodabrew) but thanks to a guy called Mike Kolich
airplay is what it is now. He mailed me from the blue talking me about how he
was working on a startup and they were using my first release of the gem to
convert regular Apple TVs as karaoke machines.

![](/posts_assets/comeback-sing.png)

That single event changed the destiny of the gem forever. There was someone
actually using my gem for something that I didn't think, something cool.
I needed to return to my roots. If I was going to make an airplay gem it's going
to be the best one.

A complete rewrite, multiple client handing, grouping, internal http server for
local content, a sample CLI, a co located Apple TV just for testing, milestones
and lots of documentation.
Just because someone contacted me... it was not just a download counter in
RubyGems, it's about the people.

![](https://pbs.twimg.com/media/BZTUAlhIQAAi_GG.jpg:large)

I need to thank the great developers, the ones that stayed awake for nights just
to find a bug, I need to do it more.
For that exact reason I built [Hugware](http://hugware.org) some time ago since
most developers will be extremelly happy with just a thank you.

[Airplay](https://github.com/elcuervo/airplay) is what is is now thanks to the
people around the project. I cheer today for the people that belived in me even
when I was not that sure.

There's a lot of future for the gem, for the CLI and for a bunch of spinoffs of
this but this was only possible only thanks to the community around this so this
blog post it's just to say... thank you.

---
layout: post
title: Ali vs Design
categories: dev
tags: golden-grid-system design ui
---
When I think of design as it relates to the internet, I think of the
interactions users have with software.  [Some sites][1] do a really great job of
concisely presenting information to the user.  And [they][2] work well on
screens that are way to big or a little too small.  With this in mind, I wanted
to learn more about desiging for the web.

**Disclaimer:  I am not a designer.  Continue reading at your own risk...or
something like that.**

I played with design on a little [web app][3] I built to play Billboard charts
as YouTube playlists.  I relied heavily on the [Golden Grid System][4] to create
the web app's design.  The Golden Grid System isn't a traditional CSS framework;
it's more of a guideline or a set of rules to building responsive websites.  I
was able to apply the Golden Grid System to the web app, but I used it more as a
framework than a guideline. I knew with another chance, I could right this
wrong.  Fortuantely for you (and me), I also wanted to spin up a new blog.
You're looking at my second attempt at using the Golden Grid System!

With the Golden Grid System, you split the screen into 18 evenly split columns.
The two outermost columns are reserved to be the outside margins of your layout.
That leaves 16 columns to play with, but a 16 column layout is really meant for
the largest screens.  When working with smaller screens, columns are combined
to create less overall columns.  Sometimes pictures, even crudley drawn
pictures, are better than words.

*Crudely Drawn Picture in Journal*

The columns are generally combined in half steps.  From 16 columns, we come down
to eight columns.  Then, from eight columns to four.  On a small screen like a
phone, you would probably use a four column layout.  As the screen size
increases, you can utilize more columns.  More space, more columns.

Then, there's the baseline grid.  I never knew about baseline grids, but my
[Google-Fu][5] tells me it's not a new idea.  A typical baseline grid has you at
a base font-size of 16px and a base base line-height of 24px.  So your text
would have to fit within the 24px line-height.  My interpretation of the
baseline grid within the Golden Grid System had me fitting content within rows
of my grid.  For example, the text of blog's header doesn't quite fit in three
rows of the grid.

*Snapshot from 4 column layout, no Firebug overlay*

So I force the text to take up the rest of the row it overflows into.  I also
add another row to add some more space between the header and the rest of the
document.

In addition to the baseline grid, the Golden Grid System also introduces a
zoomable baseline grid.  Since everything in my stylesheet is defined in ems,
font sizes can adjust as the screen size changes and everything should
change proportionately (except for a few rounding errors).  I didn't try to
implement a zoomable baseline grid when I didn't really know how to implement a
baseline grid.  It's kinda like [going for that first jump on skis when you just
got off the bunny hill][6].  Something to try later, I guess.

The other big piece to the Golden Grid System are elastic gutters.  Gutters
represent the space between columns.  The gutters in the Golden Grid System are
considered elastic because their widths are relative to page's font-size and not
the screen's width.  Since font sizes are specified as ems in my stylesheet,
content can transition smoothly as the screen size changes.

Elastic gutters are still a little mysterious to me, but as I understand it they
are created by placing a wrapper around content.  The wrapper has left and right
padding that is half the gutters' width.  For example, if I have content that
needs to fit within two columns, I would place a wrapper around the content that
is two columns wide.  Assuming I set my gutters to be 1.5 ems wide, the
wrapper's left and right padding would be 0.75 ems.  This has the effect of
centering my content between the two gutters.

*Gutter Drawing in Journal*

[1]: http://forecast.io
[2]: http://www.bulletsforever.com
[3]: http://stormyflower.com
[4]: http://goldengridsystem.com
[5]: http://alistapart.com/article/settingtypeontheweb
[6]: http://www.uproxx.com/wp-content/uploads/2013/02/ski-jump-fail-gif.gif

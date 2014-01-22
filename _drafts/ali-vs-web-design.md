---
layout: post
title: Ali vs Web Design
categories: dev
tags: golden-grid-system design ui
---
When I think of design as it relates to the internet, I think of the
interactions users will experience.  [Some sites][1] do a really great job of
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

In the Golden Grid System, the screen is divided into 18 evenly split columns.
The two outermost columns are reserved to be the outside margins of your layout.
That leaves 16 columns to play with, but a 16 column layout is really meant for
the largest screens.  When working with smaller screens, columns are combined
to create less overall columns.  Sometimes pictures, even crudley drawn
pictures, are better than words.

[![A Crudely Drawn Picture][5]][5]

The columns are generally combined in half steps.  From 16 columns, we come down
to eight columns.  Then, from eight columns to four.  On a small screen like a
phone, you would probably use a four column layout.  As the screen size
increases, you can utilize more columns.  More space, more columns.

Then, there's the baseline grid.  I never knew about baseline grids, but my
[Google-Fu][6] tells me it's not a new idea.  A typical baseline grid has you at
a base font-size of 16px and a base line-height of 24px.  So your text would
have to fit within the 24px line-height.  My interpretation of the baseline
grid within the Golden Grid System had me fitting content inside rows of the
grid.  For example, the text of blog's header doesn't quite fit in three rows
of the grid.

[![Header Overflow][7]][7]

So I force the text to take up the rest of the row it overflows into.  I also
add another row to add some more space between the header and the rest of the
document.

In addition to the baseline grid, the Golden Grid System also introduces a
zoomable baseline grid.  If everything in the stylesheet is defined in ems,
font sizes can adjust as the screen size changes and everything should
change proportionately (except for a few rounding errors).  I didn't try
implementing a zoomable baseline grid because [I had just learned baseline grids
were a thing][8].  Something to try later, I guess.

The other big piece to the Golden Grid System are elastic gutters.  Gutters
represent the space between columns.  The gutters in the Golden Grid System are
considered elastic because their widths are relative to page's font-size and not
the screen's width.  Since font sizes are specified as ems in the stylesheet,
content can transition smoothly as the screen size changes.

Elastic gutters are still a little mysterious to me, but as I understand it they
are created by placing a wrapper around content.  The wrapper has left and right
paddings that are half the gutters' width.  For example, if I have content that
needs to fit within two columns, I would place a wrapper around the content that
is two columns wide.  Assuming I set my gutters to be 1.5em wide, the
wrapper's left and right padding would be 0.75em.  This has the effect of
centering the content between the two gutters.

[![Down the Gutter][9]][9]

That's the jist of the Golden Grid System...as I understand it anyways.  Take a
look at the Golden Grid System's [website][4] and play around with it.  There's
a hamburger menu in the top right corner; this is the Golden Gridlet.  It's a
little bit of JavaScript that overlays a basic grid layout on top of the page.
Its useful for seeing how things fit within the grid.  I used it a lot when I
was building this blog.

Now that everyone knows what the Golden Grid System is, we can take a look at
what I came up with.

[Here][10] is the blog's main stylesheet.  It's written using Sass
[(Sass makes this whole thing tolerable)][11].  The stylesheet starts with a lot
of variables and presets.  Scroll a little bit and you'll see the [wrapper][12]
and the [first gird sighting][13].  The first layout I define is the four column
layout because I'm desiging [Mobile First&#8482;][14].  The four column layout
will generally be pretty simple because there's only so much you can do with
limited space.  Shrink your browser window and inspect the blog's html (go
ahead, [I'll wait][15]).  Compare it to the stylesheet so far and you'll see
there's not a lot of magic going on.

Things get a little more interesting when you make your window bigger (I would
tell you to make it bigger, but [I know you never shrank it to begin with][16]).
The first thing you'll notice is a change in the header.  The text will move
from bellow my beautiful face and line up magically next to me.  I split the
content of my header into left and right divs; left for me and right for the
text.  Since I defined each element to float left, they will naturally sit next
to each other.  The header should appear misaligned given [my header rules][17],
but it doesn't thanks to magic of [CSS media queries][18].  You can see [media
queries in action][19] towards the end of the four column layout definition. The
margins of both the left and right header divs change so the divs will be
aligned when they are sitting next to each other.  That happens when the screen
is wider than 455px.

Media queries are used again to define the [eight column layout][20].  984px is
my arbriatry breakpoint when the eight column layout will be used.  Now that I
have more space, my content doesn't need take up so much of the screen and I can
add few more columns to the outside margins.

I skipped over designing a  16 column layout.  I'm not using a large enough
screen, so it would be hard for me to design.  I also doubt you'll be reading
this on your living room TV.

...and that's my stylesheet!

The blog's design is pretty simple, and [that's by design][21].  The most
important thing on a blog are the posts. If nothing else, they should be
legible.  I think I accomplished that with this design.  Please let me know if
I'm wrong.  Seriously.  If you missed the disclaimer earlier, I'm not a
designer.  [I'm flying by the seat of my pants][22]!

[1]: http://forecast.io
[2]: http://www.bulletsforever.com
[3]: http://stormyflower.com
[4]: http://goldengridsystem.com
[5]: /assets/images/posts/ali-vs-web-design/collapsable_columns.jpg
[6]: http://alistapart.com/article/settingtypeontheweb
[7]: /assets/images/posts/ali-vs-web-design/header_overflow.png
[8]: http://media.tumblr.com/b5e946fd3c44fb224930594d79dc4e32/tumblr_inline_mo94a32ccO1qz4rgp.gif
[9]: /assets/images/posts/ali-vs-web-design/gutter.jpg
[10]: https://github.com/alimi/alimi.github.io/blob/7dd61f2cc3d25c827653c61a4b37221589250393/assets/stylesheets/scss/application.scss
[11]: http://sass-lang.com/guide
[12]: https://github.com/alimi/alimi.github.io/blob/7dd61f2cc3d25c827653c61a4b37221589250393/assets/stylesheets/scss/application.scss#L44:L56
[13]: https://github.com/alimi/alimi.github.io/blob/7dd61f2cc3d25c827653c61a4b37221589250393/assets/stylesheets/scss/application.scss#L58:L65
[14]: https://www.google.com/search?q=mobile+first
[15]: http://memecrunch.com/meme/7FX7/still-waiting/image.png
[16]: http://www.youtube.com/watch?v=oavMtUWDBTM
[17]: https://github.com/alimi/alimi.github.io/blob/7dd61f2cc3d25c827653c61a4b37221589250393/assets/stylesheets/scss/application.scss#L75:L90
[18]: https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Media_queries
[19]: https://github.com/alimi/alimi.github.io/blob/7dd61f2cc3d25c827653c61a4b37221589250393/assets/stylesheets/scss/application.scss#L106:L116
[20]: https://github.com/alimi/alimi.github.io/blob/7dd61f2cc3d25c827653c61a4b37221589250393/assets/stylesheets/scss/application.scss#L118:L131
[21]: http://www.youtube.com/watch?v=obKLdou0LH0
[22]: http://blog.wfmu.org/.a/6a00d83451c29169e20168e8116d94970c-800wi

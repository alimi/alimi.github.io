---
layout: post
title: Ali vs The Rails Asset Pipeline
categories: dev
tags: rails asset-pipeline
---
Your hero has been missing for some time, but it's not because he wasn't busy
fighting battles. I was gone in a far away land fighting the [Rails Asset
Pipeline][1].

The [Asset Pipeline][1] can help make your apps fast and responsive in
production environments, but configuring your app to use the [Asset Pipeline][1]
in production environments is...fun. A lot of people will completely skip over
the [Asset Pipeline][1] because it can be such a pain in the ass. I think that's
a mistake. Rails is so great because it gives you so many things out of the box
for free. So instead of being afraid of the [Asset Pipeline][1], embrace it!
It's actually pretty cool when it works.

Configuring the [Asset Pipeline][1] is difficult because it behaves differently
in development and production environments. In production enviornments, the
[Asset Pipeline][1] will do things like minifying JavaScript files and CSS
stylesheets to improve perfomance, but I'm not interested in reading minified files
when I'm trying to debug something in development. The [Asset Pipeline][1] is
smart enough to know this and it can give you the things you need to make
development easy while also optomizing for production. However, this can lead to
confusing bugs when you deploy to production.

To see this confusion in action, let's say you want to add a search form to a
page. The search form would be really cool if it had a search icon to submit the
form.

[![Google Search][2]][2]

There are so many ways you can go about doing this, but I want to do it with
CSS. But really I'm going to do it in [Sass][3].

{% highlight sass %}
$blue: #28A5C6;

.search-form {
  button {
    background: url('/assets/search.png') $blue no-repeat;
  }
}
{% endhighlight %}

I defined a rule for the `.search-form` class. Any button inside an element with the
`.search-form` class will have the search icon as its background. I put the
search icon, `search.png` in `app/assets/images`. I'm running everything locally
in development, so I'll get a pretty search form.

[![A pretty search form][4]][4]

Now, let's deploy this bad boy to production!

[![A broken search form][5]][5]

[![WTF][6]][6]

[ASSET PIPELINE][1]!!!

The icon is no where to be found. If we look at the server log, we get a nice
error message:

{% highlight ruby %}
ActionController::RoutingError (No route matches [GET] "/assets/search.png")
{% endhighlight %}

Looking back at my button stylesheet rule, I set the background image using
[CSS' url function][7].

{% highlight sass %}
background: url('/assets/search.png') $blue no-repeat;
{% endhighlight %}

This worked in development, because all assets can be accessed from the assets
path. But then why didn't it work in production? Since I'm using the [Asset
Pipeline][1], the app's assets go through [asset precompilation][8] when the app
is deployed to production. [Asset precompilation][8] is the process by which
Rails does all its magic that make apps so fast in production. Part of this
process involves creating a cache for assets. [Caching][9] is great because we can
serve assets faster, and the [Asset Pipeline][1] is even better because we
don't have to create caches!

So why can't I see the search icon in production? When the [Asset Pipeline][1]
caches assets, it adds a hash to the asset filename. So `search.png` becomes
something like `search-14cfc520e5db1343e158a783486eeba3.png`. The error message
says Rails can't find `search.png`, and that's right because `search.png`
doesn't exist. Thanfully, Rails can handle this. Instead of using [CSS' url
function][7] to include the image, we can use one of
[Rails' asset link helpers][10].

{% highlight sass %}
background: image-url('search.png') $blue no-repeat;
{% endhighlight %}

The `image-url` helper is smart enough to serve the regular image in development
and the cached image in production. Let's change that line of code and deploy to
production!

[![A pretty search form][4]][4]

Yay! It worked!

**Protip 1: Use [Rails' asset link helpers][10]**

Now we're ready to have some real fun with the asset pipeline! Let's say I want
to change the color of the search icon. To do that, I'll update the image.

[![New search icon][11]][11]

Cool, looks good in development. Let's see what happens when I deploy to
production...

[![A pretty search form][4]][4]

[![WTF][6]][6]

OMG [ASSET PIPELINE][1]!!!

What happened?! I did everything [DHH][12] told me to!

When I deployed to production, I had to run [asset precompilation][8] again. The
[Asset Pipeline][1] saw that the image changed, and it created a new
version of the cached image. But my stylesheet didn't change so it's still
referencing the old version of the cached image.

Once again, Rails can handle this! We just need to tell Rails that my stylesheet
depends on another asset. At the beginning of the stylesheet, we can add the
following line:

{% highlight sass %}
//= depend_on_asset 'search.png'
{% endhighlight %}

Let's deploy this change to production and see what happens...

[![New search icon][11]][11]

Yay! The [Asset Pipeline][1] is the best!

**Protip 2: Tell Rails when an asset depends on another asset**

That last case is tricky and really hard to detect, so let me introduce you
to [Sprockets Better Errors][13]. [Sprockets Better Errors][13] is great because
it will complain when you do things in development that won't work in
production. If I had [Sprockets Better Errors][13] configured for my development
environment, instead of everything working I would've seen an ugly exception:

{% highlight ruby %}
Asset depends on 'search.png' to generate properly but has not declared the
dependency
Please add: `//= depend_on_asset "search.png"` to '.../some_stylesheet.scss'
  (in .../some_stylesheet.scss)
{% endhighlight %}

It tells you what the problem is and tells you how to fix it!

**Protip 3: Use [Sprockets Better Errors][13]**

If you follow my three Protips&#8482;, you should have a more pleasureable experience
with the asset pipeline.

*I cannot guranntee you will have a more pleasureable expierence the asset
pipeline*

*This post was inspired by work I did on Baltimore Public Art Commons.
[Check it out!][14]*

[1]: http://guides.rubyonrails.org/asset_pipeline.html
[2]: /assets/images/posts/ali-vs-the-rails-asset-pipeline/search_icon.png
[3]: http://sass-lang.com/
[4]: /assets/images/posts/ali-vs-the-rails-asset-pipeline/pretty_search_form.png
[5]: /assets/images/posts/ali-vs-the-rails-asset-pipeline/broken_search_form.png
[6]: /assets/images/posts/ali-vs-the-rails-asset-pipeline/wtf_jackie_chan.jpg
[7]: https://developer.mozilla.org/en-US/docs/Web/CSS/uri
[8]: http://guides.rubyonrails.org/asset_pipeline.html#precompiling-assets
[9]: http://en.wikipedia.org/wiki/Cache_%28computing%29
[10]: http://guides.rubyonrails.org/asset_pipeline.html#css-and-sass
[11]: /assets/images/posts/ali-vs-the-rails-asset-pipeline/new_search_icon.png
[12]: http://david.heinemeierhansson.com/
[13]: https://github.com/schneems/sprockets_better_errors
[14]: https://github.com/BaltimorePublicArtCommons/baltimore_public_art_commons

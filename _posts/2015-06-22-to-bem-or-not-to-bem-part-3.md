---
layout: post
title: "To BEM, or Not to BEM: Part III"
---

I've described [the basics of BEM](/blog/to-bem-or-not-to-bem/) and provided
[an example of BEM](/blog/to-bem-or-not-to-bem-part-2/) already. Now it's time
to show something more. Something, that has moved us back to
[quick fixes](/blog/to-bem-or-not-to-bem/#quickfixes) effect and how we've dealt
with them.

## Case Study: Refactoring Flash Messages

You can see a flash message on [Wimdu](http://www.wimdu.com/)'s website in
a few places – some short text with some instructions for the user, for
instance, a message about having a voucher applied on the checkout page.

This is how the flash messages look:

![A flash message on Wimdu](/uploads/flash-message.jpg)

We've _BEMified_ the module already, so let's just quickly take a look at the
code:

{% highlight css %}
.flash {
  padding: 15px;
  border: 1px solid #f8fdf4;
  border-radius: 2px;
  color: #212121;
}

.flash__header {
  /* code */
}
{% endhighlight %}

{% highlight html %}
<div class="flash">
  <div class="flash__header">
    <h3 class="flash__title">
      Lorem ipsum dolor sit amet
    </h3>
  </div>
  <div class="flash__body">
    Lorem ipsum dolor sit amet...
  </div>
</div>
{% endhighlight %}

We have a lot of the flash messages that look slightly different and that's
where we use the modifiers. Quick preview of some of the flash messages on our
site:

{% highlight css %}
.flash {
  padding: 15px;
  border: 1px solid #f8fdf4;
}

.flash--error {
  border-color: #c23d4b;
  background-color: rgba(194, 61, 75, 0.1);
}

.flash--with-icon .flash__body {
  padding-left: 45px;
}
{% endhighlight %}

{% highlight html %}
<div class="flash flash--success"></div>

<div class="flash flash--error"></div>

<div class="flash flash--info"></div>

<div class="flash flash--panel"></div>

<div class="flash flash--with-icon"></div>
{% endhighlight %}

Looks quite nice, doesn't it? Not so fast...

### Code Wars: The Bootstrap Strikes Back

After a while, we had to add a flash message with an icon, that indicates a
success result of a user's action. This is how it looks:

![A flash success message with an icon](/uploads/flash-message-success.jpg)

A quick look at the code:

{% highlight html %}
<div class="flash flash--success flash--with-icon flash--voucher">
  <i class="flash__icon"></i>
  <div class="flash__header">
    <h3 class="flash__title">Lorem ipsum dolor sit amet</h3>
  </div>
  <div class="flash__body">
    Lorem ipsum dolor sit amet...
  </div>
</div>
{% endhighlight %}

A block with **three** modifiers applied?! This reminds us of the
[quick fixes that we wanted to avoid](/blog/to-bem-or-not-to-bem/#quickfixes) in
the first place! What can we do about it? CSS pre-processors come in handy.

### "Hacking" BEM

At Wimdu we're using [SASS](http://sass-lang.com/) with SCSS syntax (looks
really similar to plain CSS) as our pre-processor. It works great and offers
a lot of nice features ([variables](http://sass-lang.com/guide#topic-2),
[extends](http://sass-lang.com/guide#topic-7),
[nesting](http://sass-lang.com/guide#topic-3),
[mixins](http://sass-lang.com/guide#topic-6), etc.), that can keep your CSS code
cleaner and easier to maintain. What can we do about our flash message? Let's
use some of the benefits of the pre-processor –
[placeholders](http://thesassway.com/intermediate/understanding-placeholder-selectors).

{% highlight scss %}
// Placeholders

%flash-with-icon {
  // code
}

%flash-success {
  // code
}

.flash {
  // code
}

.flash--with-icon {
  @extend %flash-with-icon;
}

.flash--success {
  @extend %flash-success;
}

.flash--voucher {
  @extend %flash-with-icon;
  @extend %flash-success;
  // code
}
{% endhighlight %}

All the properties that we used to have added for the modifiers, we moved to
placeholders. To limit the modifiers applied for this `flash` block, we've
created a new one called `voucher`, which extends properties of
`%flash-with-icon` and `%flash-success`. Thanks to that, we could remove two
modifiers from the block. What's the final result?

{% highlight html %}
<div class="flash flash--voucher">
  <i class="flash__icon"></i>
  <div class="flash__header">
    <h3 class="flash__title">Lorem ipsum dolor sit amet</h3>
  </div>
  <div class="flash__body">
    Lorem ipsum dolor sit amet...
  </div>
</div>
{% endhighlight %}

Looks much better.

## Final Thoughts

BEM is easy to learn, it doesn't require any extra setup, helps you to simplify
CSS selectors and organise your code better. You don't have to style by elements
anymore, you won't be confused about how to name an element inside a block and
all the class names are unique per block.

> There are only two hard things in Computer Science: cache invalidation and naming things.
> 
> <cite>– Phil Karlton</cite>

On the other hand, sometimes it's hard to implement BEM in smaller projects.
As usual, people will be confused about how to name a block and in order to
avoid the [quick fixes mess](/blog/to-bem-or-not-to-bem/#quickfixes), you have
to use CSS pre-processors.

> Should I start using BEM?

**Yes.**

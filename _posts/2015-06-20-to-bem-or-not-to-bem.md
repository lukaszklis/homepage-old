---
layout: post
title: "To BEM, or Not to BEM: Part I"
---

I've been working as a front-end developer since 2007. Since then, a lot of
frameworks, grid systems and methodologies have been developed to help
developers' worklife. [Bootstrap](http://getbootstrap.com),
[Foundation](http://foundation.zurb.com),
[Semantic UI](http://semantic-ui.com) – those are examples of only some of
those. Tools like these have allowed us to rapidly create prototypes or web
applications. Doing front-end became easy. Did it really?

## Hello Bootstrap

Let's take a closer look at one of the most famous CSS framework - Bootstrap.
You've picked a task where you're supposed to create a button on your site.

{% highlight html %}
<button class="btn">
  Click me
</button>
{% endhighlight %}

Save a HTML file, refresh the browser and voilá! A nice button shows up.

![A basic button from Bootstrap](/uploads/basic-button.jpg)

Moving on to the next task, you've been asked to make the button larger, make it
more prominent and fill the entire width of the container. *This requires
more work* – you may think. Quick look at Bootstrap's documentation and all you
have to do is add some extra classes:

{% highlight html %}
<button class="btn btn-lg btn-primary btn-block">
  Click me
</button>
{% endhighlight %}

Once again, save, refresh and task seems to be done.

![A more prominent button](/uploads/more-prominent-button.jpg)

## Quick Fixes

You keep working on your project for the next couple of months. Bootstrap, as a
CSS framework of choice, seems to be perfect. Everybody is happy – a client, a
PO and you.

Suddenly, you've been assigned to a task where you have to fix one thing in
your layout. Eventually, you'll end up with code like below:

{% highlight html %}
<button class="btn btn-lg btn-primary btn-block btn-cta btn-orange">
  Click me
</button>
{% endhighlight %}

After pushing such code to the repository, you start to wonder what's the
purpose of your work and life. Is it really something that you want to do?
Aren't out there any better solutions which may help you keep the codebase clean
and easier to maintain?

## BEM to the Rescue!

**BEM** - **B**lock **E**lement **M**odifier - a methodology created by
[Yandex](http://yandex.ru) in 2005 (yeah, BEM is already 10 years old!). The
methodology comes with some guidelines on how to name elements in your templates
and stylesheets.

What's a block, an element and a modifier? Let's take a look at the example
layout below:

![An example of a page layout](/uploads/example-layout.jpg)
Source: <http://bem.info/method/definitions/>

### B for Block

While implementing beautiful designs provided to you by the designer, you try to
split the page into modules. You do your best to find common patterns between
them, so you could re-use as much code as possible. Such a module is what we
call a block. You can find a few of them in the example layout:

![An example layout with blocks marked](/uploads/layout-with-blocks.jpg)
Source: <http://bem.info/method/definitions/>

As you can see, blocks can be nested, as shown in the screenshot below:

![A closer look at the header block](/uploads/header-with-blocks.jpg)
Source: <http://bem.info/method/definitions/>

A header block contains blocks for logo, search form, navigation menu and user's
login.

### E for Element

What about the elements? Elements are always a part of a block and you should
never use them outside of it. Where can you find elements in the example layout?
Let's take a closer look at the navigation in the header:

![A closer look at the header's navigation block](/uploads/header-with-elements.jpg)
Source: <http://bem.info/method/definitions/>

### M for Modifier

As mentioned before, you always do your best to find common parts of different
blocks across the page, however sometimes you have to apply some custom styles
for a specific block. Let's take a look at the navigation menus in the header
and the footer:

![A closer look at the header's and footer's navigation block](/uploads/layout-with-modifiers.jpg)
Source: <http://bem.info/method/definitions/>

Both of the navigation menus have a common part, i.e. `float: left;` and perhaps
a color of a link. As you can see, the navigation menu in the bottom of the page
has some additional spacing applied between the links and that's when a modifier
comes in handy.

## Syntax

BEM doesn't force you to use a specific syntax. However, the creator of the
methodology (Yandex) proposed a syntax that you could follow.

### The Yandex Way

{% highlight html %}
<div class="block__element"></div>
<div class="block__element_mod_value"></div>
{% endhighlight %}

Yandex uses double underscore character to distinguish a block from an element,
and `_mod_` to distinguish a modifier from a block or an element.

### CSSguidelin.es

{% highlight html %}
<div class="block__element"></div>
<div class="block__element
            block__element--modifier"></div>
{% endhighlight %}

Harry Roberts' [CSSguidelin.es](http://cssguidelin.es) uses double underscore
character to distinguish a block from an element, and double dash character to
distinguish a modifier from a block or an element. Also, for the sake of
modifiers' readability, a block or an element to which a modifier is applied,
is duplicated.

### Your Way

As mentioned before, BEM offers some freedom in terms of separators choice.
Whatever suits you best and is valid with W3C's specification is fine. For
instance, when you're a fan of emoticons, why not go for the syntax below?

{% highlight html %}
<div class="block-_-element"></div>
<div class="block-_-element-o_O-modifier"></div>
{% endhighlight %}

## BEM in Practice

Take a look at the follow up posts with case study of refactoring front-end code
at [Wimdu](http://www.wimdu.com):

* [To BEM, or not to BEM: Part II](/blog/to-bem-or-not-to-bem-part-2/)
* [To BEM, or not to BEM: Part III](/blog/to-bem-or-not-to-bem-part-3/)

## Read More

* <http://bem.info>
* <http://getbem.com>
* <http://cssguidelin.es>
* <http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/>
* <http://www.smashingmagazine.com/2014/07/17/bem-methodology-for-small-projects/>
* <http://www.smashingmagazine.com/2012/04/16/a-new-front-end-methodology-bem/>
* <https://css-tricks.com/bem-101/>

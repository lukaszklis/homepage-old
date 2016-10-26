---
layout: post
title: "To BEM, or Not to BEM: Part II"
---

After understanding the [basics of BEM](/blog/to-bem-or-not-to-bem/), it's time
to take a look at the methodology in practice. At [Wimdu](http://www.wimdu.com),
we're using it on daily basis to create new functionality and move old modules
into a new architecture. In the beginning we've been using Bootstrap a lot and
we've started to get rid of it over time. We are using
[CSSguidelin.es's syntax](/blog/to-bem-or-not-to-bem/#cssguidelines) as it seems
to be the easiest to read and maintain for our team.

## Case Study: Refactoring an Offer Tile

Wimdu offers a possibility to rent your flat and use already existing listings
once you're planning to go for vacations. We display the listings in a block on
search results page and homepage, which we call an offer tile.

### The Past

Let's go back in time to May '13, when we used to have an old offer tile.

![An old offer tile on search results page](/uploads/old-offer-tile.jpg)

Looks not too bad, let's take a closer at the code:

```css
.thumbnail-search .offer-description {
  /* code */
}

.thumbnail-search .price {
  /* code */
}

.thumbnail-search .price-tag {
  /* code */
}

.thumbnail-search .price-tag span {
  /* code */
}

.thumbnail-search .primary {
  /* code */
}
```

```html
<div class="search-result thumbnail thumbnail-search">
  <a class="offer-image-link" href="#">
    <img class="offer-image" src="#" alt="">
  </a>
  <div class="favorite-search-result"></div>
  <div class="offer-description">
    <div class="h3 title"></div>
    <div class="subtitle">
      Bielefeld, Germany
    </div>
    <div class="description"></div>
    <div class="offer-rating"></div>
    <div class="offer-additional-details"></div>
  </div>
  <div class="price price-per-night">
    <div class="price-tag"></div>
    <div class="price-info"></div>
  </div>
  <div class="offer-actions"></div>
</div>
```

As you can see, there is no clear indication which class name should be used
inside another and how to name new elements, so you could avoid conflicts in
your code. CSS doesn't look so bad, however we haven't been using all of the
class names for styling.

The search results page is not the only place where we are using an offer tile.
On our homepage we have a carousel with a list of last booked/viewed apartments
on our website.

![An old offer tile on homepage](/uploads/old-offer-tile-hp.jpg)

As you can see, the offer tile looks similar, it's missing some of the elements
that we used to have on search results page. What about the code?

```css
.offer-tile img {
  /* code */
}

.offer-tile .price-tag {
  /* code */
}

.offer-tile .price {
  /* code */
}

.offer-tile .price-info {
  /* code */
}

.offer-tile .location {
  /* code */
}
```

```html
<li class="offer-tile">
  <a href="#">
    <img src="#" alt="">
    <div class="price-tag">
      <div class="price"></div>
      <div class="price-info"></div>
    </div>
    <div class="title"></div>
    <div class="location">
      Copenhagen, Denmark
    </div>
    <div class="rating"></div>
  </a>
</li>
```

Unfortunately, the code looks completely different. Some of the elements have
lost a class name at all, some of them have been re-ordered (`price` is no
longer a parent of all `price-` prefixed elements). In CSS we're using a
different class name to style the module and we're applying some styles to a
HTML element.

In the code of both offer tiles above, you can find a lot of things that are bad
for your project: styling by elements, strong CSS selectors, class names
duplication in other blocks, confusion about how to name new elements inside a
block. It was painful to maintain it and we had to improve it.

![BEMify all the things!](/uploads/bemification.jpg)

### The New Offer Tile

Let's take a look at the search results page:

![A new offer tile](/uploads/the-new-offer-tile.jpg)

As you can see, we've redesigned it since May '13 (that's good!), but what about
the code?

```css
.offer {
  /* code */
}

.offer__image-column {
  /* code */
}

.offer__details-column {
  /* code */
}

.offer__details {
  /* code */
}

.offer__title {
  /* code */
}

.offer__subtitle {
  /* code */
}

.offer__rating {
  /* code */
}

.offer--tile .offer__details {
  /* code */
}
```

```html
<div class="offer offer--tile">
  <div class="offer__image-column">
    <img class="offer__image" src="#" alt="">
  </div>
  <div class="offer__details-column">
    <div class="offer__details">
      <h3 class="offer__title"></h3>
      <div class="offer__subtitle">
        Berlin, Germany
      </div>
      <div class="offer__rating"></div>
      <div class="offer__description"></div>
    </div>
    <div class="price price--mini">
      <div class="price__tag"></div>
      <div class="price__info"></div>
    </div>
  </div>
</div>
```

There is a **huge** difference. We've created a block called `offer` and found
all styles that are common for all of the occurrences and created modifiers
whenever we had to adjust styling a bit. All elements inside a block have a
prefix, there are no more confusions about how to name elements inside a block
and we're sure, that we won't have any conflicts between two different blocks.
As we had to adjust styling for search results page, in CSS we use the modifier
only to modify elements that we need to (in this case `.offer__details`
element).

What about the homepage?

![A new offer tile on homepage](/uploads/the-new-offer-tile-hp.jpg)

Looks neat, what about the code?

```css
.offer--carousel .offer__details {
  /* code */
}

.offer--carousel .offer__title {
  /* code */
}

.offer--carousel .offer__rating {
  /* code */
}
```

```html
<div class="offer offer--carousel">
  <div class="offer__image-column">
    <img class="offer__image" src="#" alt="">
  </div>
  <div class="offer__details-column">
    <div class="offer__details">
      <h3 class="offer__title"></h3>
      <div class="offer__subtitle">
        Berlin, Germany
      </div>
      <div class="offer__rating"></div>
    </div>
    <div class="price price--mini">
      <div class="price__tag"></div>
      <div class="price__info"></div>
    </div>
  </div>
</div>
```

Code looks exactly like the one that we're using on the search results page.
Finally! `price` block also has the same structure and all elements that require
styling have a class added. If you take a look at the CSS, you can see a huge
difference. We've added only 3 selectors to modify some of the elements that are
being used on both, search results page and homepage.

After this refactoring, we've improved the code base and eliminated all the bad
parts that you've seen in the old offer tile: class names are unique per block
(no conflicts), no confusion about how to name things anymore, easy to maintain,
styling by class names only, power of selectors limited to the minimum.

## BEM in Practice

Take a look at the next follow up blog post with a case study of refactoring
front-end code at [Wimdu](http://www.wimdu.com):

- [To BEM, or not to BEM: Part III](/blog/to-bem-or-not-to-bem-part-3/)

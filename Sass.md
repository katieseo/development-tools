## 5 Sass features
#### 1. Nesting (parent selector, media queries)

```
.main {
  &-title {           -----> .main-title
    color: aqua;
  }

  #{&}__container {   -----> .main .main__container
    color: aqua;
  }
  
  @media (min-width: 50rem) {
    color: pink;      ----->  dont need to reselect .main {}
  }
}

p {
  &+& {               -----> p + p
    margin-top: 1rem;
  }
}

```
#### 2. Mixins

```
@mixin media($width) {
  @media (min-width: $width) {
    @content;
  }
}

main {
  @include media("50rem") {
    color: pink;
  }
}
```
```
@use "sass:map";
$breakpoints: (
  small: 30rem,   //480px
  medium: 48rem,  //768px
  large: 64rem,   //1024px
);

@mixin media($key) {
  $width: map.get($breakpoints, $key);

  @media (min-width: $width) {
    @content;
  }
}

main {
  @include media(large) {
    color: pink;
  }
}
```
```
@mixin color-scheme($text, $bg){
  background-color: $bg;
  color: $text;
}

@ex1 {
  @include color-scheme(yellow, black);
}

@ex2 {
  @include color-scheme(black, yellow);
}
```
```
@use 'sass:list';

$scheme-default: $color-neutral-light, $color-primary;
$scheme-secondary: $color-neutral-dark, $color-secondary;
$scheme-accent: $color-neutral-dark, $color-secondary, $color-extra;

@mixin color-scheme($text, $bg){
  @if li.length($bg) == 1 {
    background-color: $bg;
  } @else {
    background-image: linear-gradient(to right, $bg);
  }
  
  color: $text;
}

@ex1 {
  @include color-scheme($scheme-default ...);
}

@ex2 {
  @include color-scheme($scheme-secondary ...);
}

@ex3 {
  @include color-scheme(black, (white, yellow, purple));
}
```

+++
```
@mixin transition-ease {
  transition: all 0.5s ease-in-out;
}
```
```
@mixin flex-center($direction) { //pass argument
  align-items: center;
  display: flex;
  flex-direction: $direction;
  justify-content: center;
}

.main {
  @include flex-center(row);
}
```
```
@mixin theme($dark-theme: true){
  @if $dark-theme {
    background: darken(white, 100%);
    color: lighten(black, 100%);
  }
}

.dark {
  @include theme($dark-theme: true)
}
```
#### 3. Partials

_reset.scss
@import './resets';

* check @use

#### 4. Built-in Functions

```
color: rgba(aqua, 0.5);

color: mix(aqua, black, 50%);
```

#### 5. Loops

```
@for $i from 1 through 5 {
  .menu-nav-item:nth-child(#{$i}) {
    transition-delay: ($i * 0.1s) + 0.15s;
  }
}
```
```
.row {
  display: flex;
  flex-wrap: wrap;
}

[class^="col-"] {
  flex-basis: 100%;
}

$grid-total: 12;
@media (min-width: 50rem) {
  @for $i from 1 through $grid-total {
    .col-#{$i} {
      flex: 0 0 (100%/12) * $i;
    }

    .col-offset-#{$i} {
      margin-left: (100%/12) * $i;
    }
  }
}
```

## More

#### Variables
$font-weight: 400

#### Extend
```
.div {
  @extend .main;
}
```
#### Math Operations
```
.main {
  width: 50%-25%;
}
```
#### map-get
```
$font-weight: (
  "regular": 400,
  "bold": 600
);

div {
  font-weight: map-get($font-weight, bold);
}
```
#### functions (return value)
```
@function weight($weight-name) {
  @return map-get($font-weight, $weight-name)
}

.div {
  font-weight: weight(bold);
}
```
```
@function set-text-color($color) {
  @if (lightness($color) > 40%) {
    @return #000;
  } @else {
    @return #fff;
  }
}

a {
  color: set-text-color($primary-color);
}
```

////----Variables
$font-weight: 400



////----Maps
map-get

  $font-weight: (
    "regular": 400,
    "bold": 600
  );

  div {
    font-weight: map-get($font-weight, bold);
  }
  
  
////----Nesting
  
&

//.main .main__container
.main {
  width: 80%;
  
  #{&}__container {
    color: skyblue;

    &:hover {
      color: lightcoral;
    }
  }
}



////---- partials

_reset.scss
@import './resets';




////---- functions (return value)

@function weight($weight-name) {
  @return map-get($font-weight, $weight-name)
}


.div {
  font-weight: weight(bold);
}

------

@for $i from 1 through 5 {
  .menu-nav__item:nth-child(#{$i}) {
    transition-delay:  ($i * 0.1s) + 0.15s;
  }
}

------

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


/////---- Mixin (define styles)

1)-------
@mixin flex-center($direction) { //pass argument
  align-items: center;
  display: flex;
  flex-direction: $direction;
  justify-content: center;
}

.main {
  @include flex-center(row);
}

2)-------
@mixin theme($dark-theme: true){
  @if $dark-theme {
    background: darken(white, 100%);
    color: lighten(black, 100%);
  }
}

.dark {
  @include theme($dark-theme: true)
}

3)-------
@mixin mobile {
  @media (min-width: 800px) {
    @content
  }
}

.main {
  @include flex-center;
  @include mobile {
    flex-direction: column
  }
}

------

@mixin media-md {
  @media screen and (min-width: 768px) {
    @content;
  }
}

@include media-md {
  .home {
    ....
  }
}

------

@mixin transition-ease {
  transition: all 0.5s ease-in-out;
}



////---- Extend
.div {
  @extend .main;
}



////---- Math Operations
.main {
  width: 50%-25%;
}

# Testing SCSS Media Queries

## The problem

In another project the media queries got messed up in the compiled CSS file. Like:

```CSS
@media (min-width: 768px) and (max-width: 1023px) and (max-width: 767px) {
    html body .header {
        margin-top: var(--spacer-32);
    }
}
```

A guess is that this is all due to nested media queries:

```SCSS
@include media($tablet...) {
  display: grid;

  .header {
    @include responsive-spacing(margin-top, large);
    @include responsive-spacing(padding-top, large);
    @include border(border-top);
  }
}
```

Where `responsive-spacing` has also media queries inside:

```SCSS
@mixin responsive-spacing($selector: margin-bottom, $variant: large, $map-type: '') {
	$line-height: '';
	$line-height-mobile: '';

	@if ($map-type == content) {
		$line-height: '-20';
		$line-height-mobile: '-18';
	}

	$map: (
		large: var(--spacer-32#{$line-height}),
		small: var(--spacer-24#{$line-height}),
		horizontal: var(--gutter-24),
		none: 0
	);

	$map-mobile: (
		large: var(--spacer-32#{$line-height-mobile}),
		small: var(--spacer-16#{$line-height-mobile}),
		horizontal: var(--gutter-16),
		none: 0
	);

	@include media('<tablet') {
		#{$selector}: map-get($map-mobile, $variant);
	}

	@include media('>=tablet') {
		#{$selector}: map-get($map, $variant);
	}
}
```

Also others faced this problem: https://medium.com/front-end-developers/the-solution-to-media-queries-in-sass-5493ebe16844

## Solution

Forget SASS. Really. It's React time. When all these and [more problems](https://github.com/metamn/test-scss-imports) are solved inside the box.

SASS and BEM were good mates enduring a good couple of years which in tech terms is an age.
Time to move to Javascript: http://metamn.io/beat/on-old-and-new-stacks/

## Tests

### Test 1

```SCSS
@include mobile {
  background: red;
}

@include tablet {
  background: blue;

  @include portrait {
	background: green;
  }

  @include landscape {
	background: yellow;
  }
}

@include laptop {
  background: grey;
}

@include desktop {
  background: lightblue;
}
```

No anomaly detected in the compiled CSS.

```bash
cs@cs-swift:~/work/test-scss-media-query$ grep -rn '@media' production/assets/styles/site.min.css
330:    @media only screen and (-ms-high-contrast: none), (-ms-high-contrast: active) {
341:      @media only screen and (orientation: portrait) {
344:      @media only screen and (max-width: 767.9px) {
361:    @media only screen and (orientation: landscape) {
364:    @media only screen and (min-width: 768px) and (max-width: 1024px) {
399:    @media only screen and (max-width: 767.9px) {
618:    @media only screen and (-ms-high-contrast: none), (-ms-high-contrast: active) {
629:      @media only screen and (orientation: portrait) {
632:      @media only screen and (max-width: 767.9px) {
649:    @media only screen and (orientation: landscape) {
652:    @media only screen and (min-width: 768px) and (max-width: 1024px) {
687:    @media only screen and (max-width: 767.9px) {
824:    @media only screen and (-ms-high-contrast: none), (-ms-high-contrast: active) {
835:      @media only screen and (orientation: portrait) {
838:      @media only screen and (max-width: 767.9px) {
855:    @media only screen and (orientation: landscape) {
858:    @media only screen and (min-width: 768px) and (max-width: 1024px) {
893:    @media only screen and (max-width: 767.9px) {
947:  @media only screen and (orientation: portrait) {
951:  @media only screen and (max-width: 767.9px) {
1042:    @media only screen and (-ms-high-contrast: none), (-ms-high-contrast: active) {
1053:      @media only screen and (orientation: portrait) {
1056:      @media only screen and (max-width: 767.9px) {
1073:    @media only screen and (orientation: landscape) {
1076:    @media only screen and (min-width: 768px) and (max-width: 1024px) {
1111:    @media only screen and (max-width: 767.9px) {
1164:    @media only screen and (max-width: 767.9px) {
1231:    @media only screen and (max-width: 767.9px) {
1236:@media only screen and (max-width: 767.9px) {
1240:@media only screen and (min-width: 768px) and (max-width: 1024px) {
1243:  @media only screen and (min-width: 768px) and (max-width: 1024px) and (orientation: portrait) {
1246:  @media only screen and (min-width: 768px) and (max-width: 1024px) and (orientation: landscape) {
1250:@media only screen and (min-width: 1024.1px) and (max-width: 1600px) {
1254:@media only screen and (min-width: 1600.1px) {
1663:  @media only screen and (min-width: 1024.1px) and (max-width: 1600px) {
1668:  @media only screen and (min-width: 1600.1px) {
1814:          @media only screen and (-ms-high-contrast: none), (-ms-high-contrast: active) {
1825:            @media only screen and (orientation: portrait) {
1828:            @media only screen and (max-width: 767.9px) {
1845:          @media only screen and (orientation: landscape) {
1848:          @media only screen and (min-width: 768px) and (max-width: 1024px) {
1883:          @media only screen and (max-width: 767.9px) {
2019:          @media only screen and (-ms-high-contrast: none), (-ms-high-contrast: active) {
2030:            @media only screen and (orientation: portrait) {
2033:            @media only screen and (max-width: 767.9px) {
2050:          @media only screen and (orientation: landscape) {
2053:          @media only screen and (min-width: 768px) and (max-width: 1024px) {
2088:          @media only screen and (max-width: 767.9px) {
2141:        @media only screen and (orientation: portrait) {
2145:        @media only screen and (max-width: 767.9px) {
2236:          @media only screen and (-ms-high-contrast: none), (-ms-high-contrast: active) {
2247:            @media only screen and (orientation: portrait) {
2250:            @media only screen and (max-width: 767.9px) {
2267:          @media only screen and (orientation: landscape) {
2270:          @media only screen and (min-width: 768px) and (max-width: 1024px) {
2305:          @media only screen and (max-width: 767.9px) {
2358:          @media only screen and (max-width: 767.9px) {
2425:          @media only screen and (max-width: 767.9px) {
2429:      @media only screen and (max-width: 767.9px) {
2432:      @media only screen and (min-width: 768px) and (max-width: 1024px) {
2435:  @media only screen and (min-width: 768px) and (max-width: 1024px) and (orientation: portrait) {
2438:  @media only screen and (min-width: 768px) and (max-width: 1024px) and (orientation: landscape) {
2441:      @media only screen and (min-width: 1024.1px) and (max-width: 1600px) {
2444:      @media only screen and (min-width: 1600.1px) {
2452:      @media only screen and (min-width: 1024.1px) and (max-width: 1600px) {
2457:      @media only screen and (min-width: 1600.1px) {
cs@cs-swift:~/work/test-scss-media-query$
```

### Test 2

```SCSS
@include desktop {
  @include section-padding;
}

.article {
  @include article;
  @include article-thumb;
  @include section-padding;
}
```

Results: One similar `strange` compilation found:

```
2467:  @media only screen and (min-width: 1600.1px) and (min-width: 1024.1px) and (max-width: 1600px) {
```

```bash
cs@cs-swift:~/work/test-scss-media-query$ grep -rn '@media' production/assets/styles/site.min.css
330:    @media only screen and (-ms-high-contrast: none), (-ms-high-contrast: active) {
341:      @media only screen and (orientation: portrait) {
344:      @media only screen and (max-width: 767.9px) {
361:    @media only screen and (orientation: landscape) {
364:    @media only screen and (min-width: 768px) and (max-width: 1024px) {
399:    @media only screen and (max-width: 767.9px) {
618:    @media only screen and (-ms-high-contrast: none), (-ms-high-contrast: active) {
629:      @media only screen and (orientation: portrait) {
632:      @media only screen and (max-width: 767.9px) {
649:    @media only screen and (orientation: landscape) {
652:    @media only screen and (min-width: 768px) and (max-width: 1024px) {
687:    @media only screen and (max-width: 767.9px) {
824:    @media only screen and (-ms-high-contrast: none), (-ms-high-contrast: active) {
835:      @media only screen and (orientation: portrait) {
838:      @media only screen and (max-width: 767.9px) {
855:    @media only screen and (orientation: landscape) {
858:    @media only screen and (min-width: 768px) and (max-width: 1024px) {
893:    @media only screen and (max-width: 767.9px) {
947:  @media only screen and (orientation: portrait) {
951:  @media only screen and (max-width: 767.9px) {
1042:    @media only screen and (-ms-high-contrast: none), (-ms-high-contrast: active) {
1053:      @media only screen and (orientation: portrait) {
1056:      @media only screen and (max-width: 767.9px) {
1073:    @media only screen and (orientation: landscape) {
1076:    @media only screen and (min-width: 768px) and (max-width: 1024px) {
1111:    @media only screen and (max-width: 767.9px) {
1164:    @media only screen and (max-width: 767.9px) {
1231:    @media only screen and (max-width: 767.9px) {
1236:@media only screen and (max-width: 767.9px) {
1240:@media only screen and (min-width: 768px) and (max-width: 1024px) {
1243:  @media only screen and (min-width: 768px) and (max-width: 1024px) and (orientation: portrait) {
1246:  @media only screen and (min-width: 768px) and (max-width: 1024px) and (orientation: landscape) {
1250:@media only screen and (min-width: 1024.1px) and (max-width: 1600px) {
1254:@media only screen and (min-width: 1600.1px) {
1663:  @media only screen and (min-width: 1024.1px) and (max-width: 1600px) {
1668:  @media only screen and (min-width: 1600.1px) {
1814:          @media only screen and (-ms-high-contrast: none), (-ms-high-contrast: active) {
1825:            @media only screen and (orientation: portrait) {
1828:            @media only screen and (max-width: 767.9px) {
1845:          @media only screen and (orientation: landscape) {
1848:          @media only screen and (min-width: 768px) and (max-width: 1024px) {
1883:          @media only screen and (max-width: 767.9px) {
2019:          @media only screen and (-ms-high-contrast: none), (-ms-high-contrast: active) {
2030:            @media only screen and (orientation: portrait) {
2033:            @media only screen and (max-width: 767.9px) {
2050:          @media only screen and (orientation: landscape) {
2053:          @media only screen and (min-width: 768px) and (max-width: 1024px) {
2088:          @media only screen and (max-width: 767.9px) {
2141:        @media only screen and (orientation: portrait) {
2145:        @media only screen and (max-width: 767.9px) {
2236:          @media only screen and (-ms-high-contrast: none), (-ms-high-contrast: active) {
2247:            @media only screen and (orientation: portrait) {
2250:            @media only screen and (max-width: 767.9px) {
2267:          @media only screen and (orientation: landscape) {
2270:          @media only screen and (min-width: 768px) and (max-width: 1024px) {
2305:          @media only screen and (max-width: 767.9px) {
2358:          @media only screen and (max-width: 767.9px) {
2425:          @media only screen and (max-width: 767.9px) {
2429:      @media only screen and (max-width: 767.9px) {
2432:      @media only screen and (min-width: 768px) and (max-width: 1024px) {
2435:  @media only screen and (min-width: 768px) and (max-width: 1024px) and (orientation: portrait) {
2438:  @media only screen and (min-width: 768px) and (max-width: 1024px) and (orientation: landscape) {
2441:      @media only screen and (min-width: 1024.1px) and (max-width: 1600px) {
2444:      @media only screen and (min-width: 1600.1px) {
2452:      @media only screen and (min-width: 1024.1px) and (max-width: 1600px) {
2457:      @media only screen and (min-width: 1600.1px) {
2462:    @media only screen and (min-width: 1600.1px) {
2467:  @media only screen and (min-width: 1600.1px) and (min-width: 1024.1px) and (max-width: 1600px) {
2472:  @media only screen and (min-width: 1600.1px) and (min-width: 1600.1px) {
cs@cs-swift:~/work/test-scss-media-query$
```

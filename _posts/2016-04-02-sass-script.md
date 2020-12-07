---
layout: post
title: Sass - <strong>SassScript</strong>
subtitle: CSS를 프로그래밍 언어와 같이 작성할 수 있게 확장한 SassScript
categories: Sass
section: Sass
seq: 4
subseq: 2
description: SassScript는 CSS에서는 불가능한 연산, 변수, 함수 등의 확장 기능을 의미한다.
---

* TOC
{:toc}

![sass-logo](/img/sass-logo.png)

SassScript는 CSS에서는 불가능한 연산, 변수, 함수 등의 확장 기능을 의미한다.

# 1. 데이터 타입

프로퍼티 값으로 사용할 수 있는 값에는 각각의 데이터 타입(Data type)이 존재한다. SassScript는 7가지의 데이터 타입을 제공한다.

숫자형
: e.g) 1.2, 13, 10px

문자열
: CSS는 2종류의 문자열을 사용할 수 있다. 따옴표를 사용하는 경우("Lucida Grande", 'http://sass-lang.com')와 사용하지 않는 경우(bold, sans-serif)가 있다. Sass는 2종류의 문자열 모두를 인식할 수 있으며 트랜스파일링 후의 CSS에는 Sass에서 사용한 문자열이 그대로 출력된다.<br>e.g. "Lucida Grande", 'http://sass-lang.com', sans-serif

컬러
: e.g. blue, #04a3f9, rgba(255, 0, 0, 0.5)

boolean
: e.g. true, false

null
: 프로퍼티 값에 값이 null인 변수가 지정되면 해당 룰셋은 트랜스파일링되지 않는다.

list
: margin과 padding 프로퍼티값 지정에 사용되는 0 auto와 font-family 프로퍼티값 지정에 사용되는 Helvetica, Arial, sans-serif 등은 공백 또는 콤마 구분된 값의 list이다.<br>e.g. 1.5em 1em 0 2em, Helvetica, Arial, sans-serif

map
: 객체와 유사한 방식으로 `map-get` 함수를 사용하여 원하는 값은 추출할 수 있다.<br>e.g. (key1: value1, key2: value2)

```scss
// map
$foundation-palette: (
  primary: #E44347,
  mars: #D7525C,
  saturn: #E4B884,
  neptune: #5147D7
);

.mars {
  color: map-get($foundation-palette, mars);
}

// => .mars { color: #D7525C; }
```

데이터 타입을 확인하려면 빌트인 함수인 [type-of](./sass-built-in-function#21-data-type-%EC%B7%A8%EB%93%9D)를 사용한다.

# 2. 변수

Sass에서는 변수를 사용할 수 있다. 문자열, 숫자, 컬러(#aa443f) 등을 사전에 변수에 저장하고 필요할 때 불러 사용할 수 있다.

변수명은 `$`로 시작한다.

```scss
$site_max_width: 960px;
$font_color: #333;
$link_color: #00c;
$font_family: Arial, sans-serif;
$font_size: 16px;
$line_height: percentage(20px / $font_size);

body {
  color: $font_color;

  // Property Nesting
  font: {
    size: $font_size;
    family: $font_family;
  }

  line-height: $line_height;
}

#main {
  width: 100%;
  max-width: $site_max_width;
}
```

```css
body {
  color: #333;
  font-size: 16px;
  font-family: Arial, sans-serif;
  line-height: 125%;
}

#main {
  width: 100%;
  max-width: 960px;
}
```

# 3. 변수의 Scope

변수에는 유효범위(scope)가 존재한다. 코드 블록 내에서 선언된 변수는 지역변수가 된다. 지역변수의 유효범위는 자신이 속한 코드 블록과 하위 코드 블록이다.

```scss
$width: 960px; // 전역 변수

header {
  width: $width;
  margin: 0 auto;
}

#main {
  $color: #333; // 지역 변수
  width: $width;
  margin: 20px auto;
  section {
    p {
      color: $color;

      a:link {
        color: $color;
      }
    }
  }
}

footer {
  width: $width;
  margin: 0 auto;
  color: $color; // Error: Undefined variable: "$color".
}
```

위 예제에서 $width는 top level에 기술되었으므로 전역 변수다. 전역변수는 전역은 물론 하위의 어떤 코드 블록 내에서도 유효하다.

위 예제를 트랜스파일링하면 Undefined variable: "$color"라는 에러가 발생한다. 이는 &#35;main에서 선언한 $color는 &#35;main 내에서만 유효한 지역 변수이기 때문이다.

코드 블록 내에서 선언한 변수를 전역 변수로 지정하는 방법은 아래와 같다.

```scss
#main {
  $color: #333 !global; // 전역 변수
  width: $width;
  ...
```

# 4. 연산자

## 4.1 숫자 연산자

| 연산자    	| 설명
|:---------:|:------------- |
| +	        | 덧셈
| -	        | 뺄셈
| *	        | 곱셈
| /	        | 나눗셈
| %	        | 나머지
| ==        | 동등
| !=        | 부등

아래의 예제를 살펴보자.

```scss
$width: 100px;

#foo {
  width: $width + 10; // 110px
}

#bar {
  width: $width + 10in; // 1060px
}
```

변수 $width의 값 100px에 10 또는 10em과 같이 다른 단위의 값을 연산하여도 에러없이 연산이 수행된다. 이때 연산자의 왼쪽 값을 기준으로 단위가 설정된다.

$width에 10em을 더하면 어떻게 될까?

```scss
$width: 100px;

#foo {
  width: $width + 10em; // 100px + 10em => Error: Incompatible units: 'em' and 'px'.
}
```

트랜스파일링 결과 Incompatible units: 'em' and 'px'.이라는 에러를 출력한다.

Sass 연산은 대상을 변환하여 연산할 수 없는 경우 에러를 출력한다.

%, em, rem, vh, vw, vmin, vmax과 같이 상대적인 값을 Sass는 알지 못한다. 상대적인 값의 결과값은 브라우저만이 알 수 있기 때문이다.

따라서 상대적인 값을 갖는 단위의 연산은 동일한 단위를 갖는 값과의 연산만이 유효하다.

```scss
#foo {
  width: 5% + 10%; // 15%
}
```

CSS3의 calc 함수(IE9 이상 사용가능)를 사용하면 이런 문제를 해결할 수 있다.

```scss
#foo {
  width: calc(25% - 5px);
}
```

아래의 CSS를 살펴보자.

```css
p {
  /*
    font: font-style font-variant font-weight font-size/line-height font-family
  */
  font: italic bold 12px/30px Georgia, serif;
}
```

CSS에서의 `/`는 나눗셈의 의미가 아니라 값을 구분하는 의미를 갖는다.

따라서 Sass의 `/` 연산자를 사용하기 위해서는 몇가지 조건이 필요하다.

- 변수에 대해 사용
- 괄호 내에서 사용
- 다른 연산의 일부로서 사용

```scss
p {
  // font와 border-radius의 '/'는 CSS문법에 맞는 표현이므로 연산되지 않는다.
  font: italic bold 12px/30px Georgia, serif;
  // 타원형 둥근 모서리
  border-radius: 10px 20px/20px;
  /*
  border-top-left-radius: 10px 20px;
  border-top-right-radius: 20px;
  border-bottom-right-radius: 10px 20px;
  border-bottom-left-radius: 20px;
  */

  $width: 1000px;

  width: $width / 2;            // 변수에 대해 사용 →　width: 500px;
  height: (500px / 2);          // 괄호 내에서 사용 →　height: 250px;
  margin-left: 5px + 8px / 2px; // 다른 연산의 일부로서 사용 →　margin-left: 9px;
}
```

변수를 CSS의 /와 함께 사용하고자 하는 경우 `#{}`(Interpolation)를 사용한다.

```scss
p {
  $font-size: 12px;
  $line-height: 30px;
  font: #{$font-size}/#{$line-height};  // 12px/30px
}
```

## 4.2 컬러 연산자

모든 산술 연산자는 컬러값에도 사용할 수 있다.

```scss
p {
  color: #010203 + #040506;
  // R: 01 + 04 = 05
  // G: 02 + 05 = 07
  // B: 03 + 06 = 09
  // => #050709
}

p {
  color: #010203 * 2;
  // R: 01 * 2 = 02
  // G: 02 * 2 = 04
  // B: 03 * 2 = 06
  // => #020406
}

p {
  color: rgba(255, 0, 0, 0.75) + rgba(0, 255, 0, 0.75);
  // alpha(투명도)는 연산되지 않는다
  // color: rgba(255, 255, 0, 0.75);
}
```

rgba의 alpha값은 연산되지 않는다. alpha값의 연산을 위해서는 [opacify 함수 또는 transparentize 함수](./sass-built-in-function#65-alpha-연산)를 사용한다.

- opacify 함수: 첫번째 인수로 전달받은 alpha값에 두번째 인수를 더해 불투명도를 증가시킨다.(더 불투명해진다)

- transparentize 함수: 첫번째 인수로 전달받은 alpha값에 두번째 인수를 빼서 불투명도를 감소시킨다.(더 투명해진다)

```scss
$translucent-red: rgba(255, 0, 0, 0.5);

p {
  color: opacify($translucent-red, 0.3);
  // => color: rgba(255, 0, 0, 0.8);

  background-color: transparentize($translucent-red, 0.25);
  // => background-color: rgba(255, 0, 0, 0.25);
}
```

## 4.3 문자열 연산자

`+` 연산자는 자바스크립트와 같이 문자열을 연결할 수 있다.

```scss
p {
  cursor: e + -resize;  // e-resize
}
```

따옴표가 있는 문자열과 없는 문자열을 함께 사용하는 경우 좌항의 문자열을 기준으로 따옴표를 처리한다.

```scss
p:before {
  content: "Foo " + Bar;        // "Foo Bar"
  font-family: sans- + "serif"; // sans-serif
}
```

## 4.4 불린 연산자

| 연산자	  | 설명
| :---------: |:-------------:|
| &&	        | and
| &#124;&#124;| or
| !	          | not

- [if](./sass-css-extention#31-if)
- [@if](./sass-css-extention#31-if)

## 4.5 리스트 연산자

리스트를 위한 별도의 연산자는 제공되지 않지만 [리스트 함수](http://www.sass-lang.com/documentation/Sass/Script/Functions#list-functions)를 사용하여 필요한 처리를 수행할 수 있다.

-[list functions](./sass-built-in-function#4-list-functions)

# 5. 함수

[Sass Built-in Functions](./sass-built-in-function)를 참조하기 바란다.

# 6. Interpolation: #{}

인터폴레이션은 변수의 값을 문자열 그대로 삽입한다. 인터폴레이션에 의해 삽입된 문자열은 연산의 대상으로 취급되지 않는다.

변수는 프로퍼티값으로만 사용할 수 있으나 `#{}`을 사용하면 프로퍼티값은 물론 셀렉터와 프로퍼티명에도 사용할 수 있다.

```scss
$name: foo;
$attr: border;

p.#{$name} {            // p.foo
  #{$attr}-color: blue; // border-color: blue;
}

.someclass {
  $font-size: 12px;
  $line-height: 30px;
  // 연산의 대상으로 취급되지 않도록
  font: #{$font-size} / #{$line-height}; // 12px / 30px
}
```

# 7. Ampersand(&)

`&`는 부모요소를 참조하는 셀렉터이다.

```scss
a {
  color: #ccc;

  &.home {
    color: #f0f;
  }

  &:hover {
    text-decoration: none;
  }

  // & > span (X)
  > span {
    color: blue;
  }

  span {
    color: red;
  }
}
```

위 Sass의 트랜스파일링 결과는 아래와 같다.

```css
a {
  color: #ccc;
}

a.home {
  color: #f0f;
}

a:hover {
  text-decoration: none;
}

a > span {
  color: blue;
}

a span {
  color: red;
}
```

상세한 내용은 [The Sass Ampersand](https://css-tricks.com/the-sass-ampersand/)을 참조하기 바란다.

# 8. !default

!default flag는 할당되지 않은 변수의 초기값을 설정한다.

```scss
$content: null;
$content: "Non-null content" !default;

#main {
  content: $content; // "Non-null content"
}
```

이미 값이 할당되어 있는 변수에 !default flag를 사용하면 적용되지 않는다.

```scss
$content: "First content";
$content: "Second content?" !default;
$new_content: "First time reference" !default;

#main {
  content: $content; // "First content"
  new-content: $new_content; // "First time reference"
}
```

이러한 특성은 [partial](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#Partials__partials)에 매우 유용하다.

2개의 파일 _font.scss와 main.scss를 생성해 보자. main.scss은 내부에서 _font.scss을 import한다.

```scss
// _font.scss
$font-size: 16px !default;
$line-height: 1.5 !default;
$font-family: "Helvetica Neue", "Helvetica", "Arial", sans-serif !default;

body {
  font: #{$font-size}/$line-height $font-family;
}
```

```scss
// main.scss
$font-family: "Lucida Grande", "Lucida Sans Unicode", sans-serif;

@import "font";
```

위 코드의 트랜스파일링 결과는 아래와 같다. !default는 변수에 값이 할당되지 않았을 때 사용할 기본값을 지정할 때 사용한다. 위 예제의 경우, main.scss에서 변수에 값을 할당하였기 때문에 !default와 같이 사용한 변수값은 무력화된다.

```css
body {
  font: 16px/1.5 "Lucida Grande", "Lucida Sans Unicode", sans-serif; }
```

만일 font.scss의 $font-family 변수에 !default 설정이 없었다면 후위에 선언된 font.scss의 $font-family 변수값이 적용돠어 아래와 같은 결과가 생성되었을 것이다

```css
body {
  font: 16px/1.5 "Helvetica Neue", "Helvetica", "Arial", sans-serif; }
```

# Reference

* [Sass](http://sass-lang.com/)

* [The Sass Way](http://www.thesassway.com/)

* [Getting Started with SASS ](https://scotch.io/tutorials/getting-started-with-sass)

* [Sass & Compass Color Functions](http://jackiebalzer.com/color)

* [Using pure Sass functions to make reusable logic more useful](http://thesassway.com/advanced/pure-sass-functions)

* [The Sass Ampersand](https://css-tricks.com/the-sass-ampersand/)

* [partial](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#Partials__partials)

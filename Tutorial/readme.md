# Sass (.scss)

[sass-lang](http://sass-lang.com/)  

## Sass拥有许多perks( 过滤器)，使我们能够编写简洁的，可读的代码。在这一课中，我们将探讨三个概念：  
- Variables
- Mixins
- Nests

### Nesting
选择器
属性 :
```scss
.banner {
    font-family: 'Pacifico', cursive;
    height: 400px;
    background-image: url("lemonade.jpg");
    border : {
        top: $standard-border;
        bottom: $standard-border;
    }
    .slogan {
        position: absolute;
        border: $standard-border;
        top: 200px;
        left: 25%;
        width: 50%;
        height: 200px;
        background-color: $translucent-white;
        span {
            position: absolute;
            text-align: center;
            line-height: 200px;
        }
    }
}
```
### Variables
> 在SCSS变量让你自己选择的标识符分配给特定的值。
>   可以分配给一个变量在CSS中不同的数据类型。
>   颜色, 数字, 文本字符串, 布尔值, null(空值), 列表( list), 映射(map)
>   1.列表可以通过空格或逗号分隔。
>   注意：您也可以用括号包围一个列表，并创建由列表组成的列表
>   2.映射与列表非常相似，但是每一个对象是一个键 - 值对。
>   注意：在映射中，一个键的值可以是列表或另一个映射。

```scss
$translucent-white: rgba(255,255,255,0.3);
$icon-square-length: 300px;
$standard-border: 4px solid black;
.div{
    font-family: 'Pacifico', cursive;
    height: 400px;
    background-image: url("lemonade.jpg");
    border : {
        top: $standard-border;
        bottom: $standard-border;
    }
}
```
```scss
1.5em Helvetica bold;
/* or */
Helvetica, Arial, sans-serif;

(key1: value1, key2: value2);
```

###  伪类( :hover), 伪元素(::before,  ::after)
&

```scss
.notecard{ 
    &:hover{
        @include transform (rotatey(-180deg));  
    }
}
```
### Mixins （@mixin， @include）
> 在Sass中，mixin让你做CSS声明组,你想要在整个网站中重用的。
>   注意：Mixin的名字和所有其他Sass标识使用的**连字符**和**下划线**可以互换 ?。

```scss
@mixin backface-visibility {
    backface-visibility: hidden;
    -webkit-backface-visibility: hidden;
    -moz-backface-visibility: hidden;
    -ms-backface-visibility: hidden;
    -o-backface-visibility: hidden;
}
/* usage */
.notecard {
    .front, .back {
        width: 100%;
        height: 100%;
        position: absolute;
        @include backface_visibility;
        /* backface-visibility === backface_visibility ??? */
    }
}
```
###  Mixins 也有接收一个参数的能力.

```scss
/* backface-visibility: visible|hidden|initial|inherit; */ 
$visibility: hidden;

@mixin backface-visibility($visibility) {
    backface-visibility: $visibility;
    -webkit-backface-visibility: $visibility;
    -moz-backface-visibility: $visibility;
    -ms-backface-visibility: $visibility;
    -o-backface-visibility: $visibility;
}
/* 为其参数,传递一个值的语法如下： */
@include backface-visibility(hidden);
```
###  Mixin 参数可以被分配一个默认值,在Mixin定义，通过使用一个特殊的符号。
> 当Mixin被包括,如果没有值传递,缺省值被分配给参数。

```css
@mixin backface-visibility($visibility: hidden) {
    backface-visibility: $visibility;
    -webkit-backface-visibility: $visibility;
    -moz-backface-visibility: $visibility;
    -ms-backface-visibility: $visibility;
    -o-backface-visibility: $visibility;
}
```
### 一般来说，以下是有关参数和混入5个重要的事实：
> 1. Mixins可以采取多个参数。
> 2. Sass允许你明确你的@include声明定义每个参数
> 3. 当明确指定的值，你可以给他们发送乱序.
> 4. 如果一个Mixin定义参数的组合,没有默认值，你应该首先定义那些没有默认值的。
> 5. Mixins 可以嵌套

```scss

@mixin dashed-border($width, $color: #FFF) {
    /*two argument only one has default value*/
    border: {
        color: $color;
        width: $width;
        style: dashed;
    }
}
span { //only passes non-default argument
    @include dashed-border(3px);
}
p { //passes both arguments
    @include dashed-border(3px, green);
}
div { //passes out of order but explicitly defined
    @include dashed-border(color: purple, width: 5px); 
}
```
###  Sass允许你传递多个参数, 以列表或映射格式。

```scss
/* list */
@mixin stripes($direction, $width-percent, $stripe-color, $stripe-background: #FFF) {
    background: repeating-linear-gradient(
        $direction,
        $stripe-background,
        $stripe-background ($width-percent - 1),
        $stripe-color 1%,
        $stripe-background $width-percent
    );
}
/* map */
$college-ruled-style: ( 
    direction: to bottom,
    width-percent: 15%,
    stripe-color: blue,
    stripe-background: white
);
/* 然后,我们可以传递这些参数,在一个映射中,用跟随的...符号: */
.definition {
    width: 100%;
    height: 100%;
    @include stripes($college-ruled-style...);
 }
```
### 优先考虑可读性
> 注意：记住总是优先考虑可读性,, 在写更少的代码之上。
> 如果你发现它增加了你代码库的清晰度,这种方法是唯一有用。

### 字符串插值
> 在 Sass，字符串插值是放置一个字符串变量在其它两个字符串的中间的过程

```scss
@mixin photo-content($file) {
    content: url(#{$file}.jpg); //string interpolation
    object-fit: cover;
}
//....
.photo { 
    @include photo-content('titanosaur');
    width: 60%;
    margin: 0px auto; 
}
``` 

###  Sass 允许mixins内部使用 & 选择器。
> 1. ＆选择器获取在mixin被包括的点,被分配的父的值。
> 2. 如果没有父选择器，那么该值为null且Sass将抛出一个错误。

```scss
@mixin text-hover($color){
    &:hover {
        color: $color; 
    }
}
/* uasge */
.word { //SCSS: 
    display: block; 
    text-align: center;
    position: relative;
    top: 40%;
    @include text-hover(red);
}
``` 

### 

> 他们具有能力，接收参数，给这些参数分配默认值，并接收上述的任何的格式参数，是最可读，方便你使得mixin Sass's最受欢迎的指令。

> &选择器 *是一个 Sass结构，它允许更灵活的表现通过引用父选择器,,when用CSS伪元素和类时。

> 字符串插值是胶水，可以让你在另一个字符串中间插入一个字符串，当它是在一个可变格式。
  它的应用有所不同，但插值能力特别有用，用于传递文件名。

###  functions, arithmetic, and color operations 



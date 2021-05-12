[TOC]
## sass安装
sass依赖于Ruby环境,因此需要先安装ruby
1. [下载Ruby](https://rubyinstaller.org/downloads/)
2. 安装时注意勾选`Add Ruby executables to your PATH`
3. Ruby自带RubyGems系统,用于安装基于Ruby的软件,cmd命令行中输入`gem install sass`即可完成sass安装

## 使用
sass文件后缀名是`.scss`,意思是Sassy CSS
### 基本用法
#### 定义变量
以$开头
```css
$blue: #1875e7;
div {
    color: $blue;
}

/* 如果变量需要镶嵌在字符串中,就必须需要写在#{}中 */
$side: left;
.round {
    border-#{$side}-radius: 5px;
}
```

#### 计算功能
sass允许在代码中使用算式
```css
body {
　　　　margin: (14px/2);
　　　　top: 50px + 100px;
　　　　right: $var * 10%;
　　}
```

#### 嵌套
1. 选择器嵌套
```css
div h1 {
    color : red;
}

<!-- 可以写成嵌套格式 -->
div {
　　　　h1 {
　　　　　　color:red;
　　　　}
　　}
```

2. 属性嵌套
例如border-color属性
注意border后面必须加冒号
```css
p {
　　　　border: {
　　　　　　color: red;
　　　　}
　　}
```

3. 嵌套的代码块内,可以使用&引用父元素
```css
/* a:hover可以写成一下: */
a {
    &:hover {
        color: ffb3ff;
    }
}

```

#### 注释
1. 标准的CSS注释`/* comment */`会保留到编译后的文件

2. 单行注释 `// comment`只保留在sass文件中,编译后被省略

3. 在`/*`后面加一个感叹号,表示这是"重要注释".即使是压缩模式编译,也会保留这行注释,通常可用于声明版权信息
```css
/*!
　　　　重要注释！
　　*/
```
#### 代码重用
##### 继承
允许一个选择器继承另一个选择器,使用@extend
```css
.class1 {
    border: 1px solid #ddd;
}

/* class2继承class1 */
.class2 {
    @extend .class1;
    font-size:120%;
}
```
##### Mixin
mixin是可以重用的代码块.
`@mixin`定义,`@include`调用
```css
/* 使用@mixin命令定义一个代码块 */
@mixin left {
    float: left;
    margin-left: 10px;
}

/* 使用@include命令调用这个mixin */
div {
    @include left;
}
```

可以指定参数和缺省值
```css
@mixin left($value: 10px) {
    float: left;
    margin-right: $value;
}

/* 使用时根据需要加入参数 */
div {
    @include left(20px);
}
```
一个mixin实例,用于生成浏览器前缀
```css
@mixin rounded($vert, $horz, $radius: 10px) {
    border-#{$vert}-#{$horz}-radius: $radius;
    -moz-border-radius-#{$vert}#{horz}: $radius;
    -webkit-border-#{$vert}-#{$horz}-radius: $radius;
}

/* 调用 */
#navbar li {
    @include rounded(top, left);
}
#footer {
    @include rounded(top, left, 5px);
}
```

#### 颜色函数
#### 插入文件
使用`@import`命令插入外部文件
`@import "path/fileName.scss";`

### 高级用法
#### 条件语句
`@if`条件判断 && `@if`+`@else`
```css
p {
    @if 1+1 == 2 {
        border: 1px solid;
    }
    @if 5<3 {
        border: 2px dotted;
    }
}

@if lightness($color) > 30% {
    background-color: #000;
} @else {
    background-color: #fff;
}
```

#### 循环语句
1. for循环`@for`
2. while循环
3. each循环
```css
/* @for循环 */
@for $i from 1 to 10 {
    .border-#{$i} {
        border: #{$i}px solid blue;
    }
}

/* @while循环 */
$i: 6;
@while $i > 0 {
    .item-#{$i} {
        width: 2em * $i;
    }
    $i: $i - 2;
}

/* @each命令 */
@each $member in a, b, c, d {
    .#{$member} {
        background-image: url("/image/#{$member}.jpg");
    }
}

```

#### 自定义函数
```css
@function double($n) {
    @return $n * 2;
}
#sidebar {
    width: double(5px);
}
```
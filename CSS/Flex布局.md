# Flex 布局

传统布局：{% em %}基于盒模型，依赖 display属性 + position属性 + float属性{% endem %}

Flex布局的出现使得任何容器的盒模型的操作变得更简单，提供了极大的灵活性

# Flex 语法

## 基本概念

采用Flex布局的元素，称为Flex容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称"项目"。
![flex1](/assets/CSS/flex布局/flex1.jpg)

* main axis  ： 水平的主轴，简称——主轴
    * main start ：主轴开始的位置（从左到右还是从右到左）
    * main end   ：主轴结束的位置
    * main size  : 单个项目（子元素）占据的主轴空间
* cross axis ： 垂直的交叉轴，简称——交叉轴
    * cross start ：交叉轴开始的位置（从上到下还是从下到上）
    * cross end   ：交叉轴结束的位置
    * cross size   : 单个项目（子元素）占据的交叉轴空间

## 容器属性
* [容器属性](#容器属性)
  * [flex-direction-主轴的方向](#flex-direction)
  * [flex-wrap—换行](#flex-wrap)
  * [flex-flow-以上两种属性的简写](#flex-wrap)
  * [justify-content-主轴上的对齐方式](#justify-content)
  * [align-items-交叉轴的对齐方式](#align-items)
  * [align-content-多根轴线对齐方式](#align-content)

### flex-direction

{% em %}flex-direction属性决定主轴的方向（即项目的排列方向）{% endem %}

> .box {
  flex-direction: row | row-reverse | column | column-reverse;
}

> row（默认值）：主轴为水平方向，从左到右

>row-reverse：主轴为水平方向，从右到左

>column：主轴为垂直方向，从上到下

>column-reverse：主轴为垂直方向，从下到上

### flex-wrap

{% em %}默认情况下，项目都排在一条线。flex-wrap属性定义，如果一条轴线排不下，如何换行{% endem %}

> .box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
> nowrap（默认）：不换行。

> wrap：换行，第一行在上方。

> wrap-reverse：换行，第一行在下方。


### flex-flow

{% em %}flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap{% endem %}

> .box {
  flex-flow: flex-direction || flex-wrap;
}

### justify-content

{% em %}justify-content属性定义了项目在主轴上的对齐方式（水平方向上的）{% endem %}

> .box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}

![flex2](/assets/CSS/flex布局/flex2.jpg)

> flex-start（默认值）：左对齐

> flex-end：右对齐

> center： 居中

> space-between：两端对齐，项目之间的间隔都相等。

> space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。


### align-items

{% em %}align-items属性定义项目在交叉轴上如何对齐。{% endem %}

> .box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}

![flex3](/assets/CSS/flex布局/flex3.jpg)

> row（默认值）：主轴为水平方向，从左到右

>row-reverse：主轴为水平方向，从右到左

>column：主轴为垂直方向，从上到下

>column-reverse：主轴为垂直方向，从下到上

### align-content

{% em %}align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用{% endem %}

> .box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}

![flex3](/assets/CSS/flex布局/flex3.jpg)

> rflex-start：与交叉轴的起点对齐。

> flex-end：与交叉轴的终点对齐。

> center：与交叉轴的中点对齐。

> space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。

> space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。

> stretch（默认值）：轴线占满整个交叉轴。

### align-content

{% em %}align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。{% endem %}

> .box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}

![flex4](/assets/CSS/flex布局/flex4.jpg)

> flex-start：与交叉轴的起点对齐。

> flex-end：与交叉轴的终点对齐。

> center：与交叉轴的中点对齐。

> space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。

> space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。

> stretch（默认值）：轴线占满整个交叉轴。



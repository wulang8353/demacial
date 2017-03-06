# Flex 布局

传统布局：{% em %}基于盒模型，依赖 display属性 + position属性 + float属性{% endem %}

Flex布局的出现使得盒模型的操作变得更简单，提供了极大的灵活性

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

## 特性语法

* [flex-direction-主轴的方向](#flex-direction)
* [flex-wrap—换行](#flex-wrap)
* [justify-content-主轴上的对齐方式](#justify-content)
* [align-items-交叉轴的对齐方式](#align-items)
* [align-content-多根轴线对齐方式](#align-content)

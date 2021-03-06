# CSS笔记

### CSS各种居中实现方式

CSS居中是每次布局都需要面对的问题，但是同一个居中方法并不是任何元素都能使用的，内联元素和块级元素的居中方式各不相同，下面我就对它们分别进行讨论和总结。

#### 内联元素

##### 内联元素的特点

- 和其他元素都在一行上
- 设置高度height 无效，可以通过line-height来设置
- 设置margin和padding只有左右有效，上下无效
- **宽度就是它的文字或图片的宽度，不可改变**
- 内联元素只能容纳文本或者其他内联元素

以下实例都是基于下面的html代码：

```html
    <div class="out">
        <div class="in2">sss</div>
        <span class="in">
            居中元素
        </span>
    </div>
```

##### 水平居中

1. 父元素样式设置为text-align:center，里面包含的行内元素居中,如果父元素内部还存在包含文字且不定宽的块元素，那么这个块元素也会居中。

   ```css
        .out{
             text-align: center;
            }
   ```

2. 父元素样式设置为display:flex; justify-content:center，里面包含的行内元素居中。

   ```css
        .out{
            display:flex;
            justify-content:center
        }
   ```

##### 垂直居中

1. **单行文本**：可将其父元素的高度和行高设置为相等的值 height = line-height

   ```css
        .out{
           height: 100px;
           line-height: 100px;
        }
   ```

2. **多行文本**：用一个span标签将所有的文字封装起来，再用一个容器包裹span，设置属性display: table-cell;vertical-align: middle。这种方法同样适用于使图片居中。

   ```css
       .out{
           display: table-cell;
           vertical-align: middle;
        }
   ```

3. 父元素样式设置为display:flex; align-items: center，里面包含的行内元素居中。

   ```css
        .out{
            display:flex;
            align-items: center;
        }   
   ```

#### 块级元素

##### 块级元素的特点

- 总是在新行上开始，占据一整行
- 高度，行高以及外边距和内边距都可控制
- 宽度缺省是它的容器的100%，除非设定一个宽度
- 它可以容纳内联元素和其他块元素

以下实例都基于下面的html代码：

```html
        <div class="out">
            <div class="in">
                居中元素
            </div>
        </div>
```

##### 水平居中

###### 定宽块级元素水平居中

1. 父元素样式设置为display:flex; justify-content:center，则里面包含的块元素居中

   ```css
          .out{
            display:flex;    
            justify-content:center;
        }  
   ```

2. 该元素样式设置为 margin：0 auto；

   ```css
          .in{
            width: 100px;  
            margin: 0 auto;
        }  
   ```

3. 该元素样式设置为 position: relative;left: 50%；margin-left: -0.5*width（负的该元素宽度的一半）

   ```css
          .in{
            width: 100px;
            position: relative;
            left: 50%;
            margin-left: -50px;
        }
   ```

4. 上述方法把relative改为absolute也同样适用，根据实际情况选择适合自己的方法。

   ```css
         .in{
            width: 100px;
            position: absolute;
            left: 50%;
            margin-left: -50px;
        }
   ```

###### 不定宽块级元素水平居中

1. 对于包含文字的块元素可将其父元素设置为text-align:center

   ```css
        .out{
           text-align:center;    
        }
   ```

2. 若不包含文字，可把该块级元素变成行内元素，即设置display:inine，再给其父元素设置text-align:center

   ```css
        .out{
           text-align:center;    
        }
        .in{
           display:inine;    
        }
   ```

3. 父元素样式设置为display:flex; justify-content:center，则里面包含的块元素居中

   ```css
        .out{
           display:flex;    
           justify-content:center;
        }
   ```

4. 该元素样式设置为 position: absolute;left: 50%;transform: translate(-50%,0)

   ```css
        .in{
           position: absolute; 
           left: 50%;  
           transform: translate(-50%,0);
        }
   ```

##### 垂直居中

###### 定高块级元素垂直居中

1. **父元素样式设置为display:flex; align-items: center，则里面包含的块元素居中**

   ```css
       .out{
           display:flex; 
           align-items: center;
       }
   ```

2. 该元素样式设置为 position: relative;top: 50%；margin-top: -0.5*height（负的该元素高度的一半）

   子元素没宽度的话 宽度会撑开到父元素

   ```css
        .in{
           height:100px;
           position: relative; 
           top: 50%;  
           margin-top:-50px;
        }
   ```

3. 该元素的父元素的position值设置为relative，将该元素样式设置为position: absolute;top:50%;margin-top: -0.5*height

   子元素没宽度的话 宽度不会撑开

   ```css
        .out{
             position: relative;    
          }
        .in{
             height:100px;
             position: absolute; 
             top:50%;   
             margin-top: -50px
          }
   ```

4. 该元素的父元素的position值设置为relative，将该元素样式设置为position: absolute;top:0;bottom: 0;left: 0;right: 0;margin: auto;

   这个好像不太行

   ```css
        .out{
            position: relative;    
         }
        .in{
            height:100px;
            position: absolute; 
            top:0;   
            bottom: 0;
            left: 0;
            right: 0;
            margin: auto;
         }
   ```

###### 不定高块级元素垂直居中

1. 父元素样式设置为display:flex; align-items: center，则里面包含的块元素居中

   ```css
        .out{
            display:flex; 
            align-items: center;
        }
   ```

2. 该元素样式设置为 position: relative;top: 50%;transform: translate(0,-50%)

   ```css
        .in{
            position: relative;
            top: 50%;
            transform: translate(0,-50%);
        }
   ```

3. 该元素的父元素的position值设置为relative，将该元素样式设置为position: absolute;top:50%;translate(0,-50%)

   ```css
        .out{
             position: relative;
           }
        .in{
            position: absolute;
            top: 50%;
            transform: translate(0,-50%);
          }
   ```

------

#### 小结

- 总结到这里就会发现令元素居中的办法千奇百怪，其中无论水平还是垂直，无论有没有指定宽高，总能奏效的办法就是**display:flex**办法，这也是flex布局的优势之一，有时候使用flex布局甚至比bootstrap这种框架更加顺手，有机会我也会对flex做个总结。
- 另外在上面的很多居中的办法中都用到了position: absolute，但是其实不建议经常使用绝对定位进行布局，因为它脱离了文档流，有时会造成元素的塌陷。因为关于position的不同取值带来的效果经常使人困扰，后面我也会对它进行总结。
- 还有，居中的办法有这么多，但并不代表你需要懂得每一种，甚至去学会一些奇怪或者复杂的居中方式，而是在布局的过程中选择最适合、最简单、你用的最顺手的方法实现你想要的效果，俗话说黑猫白猫，能抓住耗子的就是好猫，居中方式也是一样的道理。

链接：https://www.jianshu.com/p/342497cfc100

### flex布局

flex布局也叫弹性盒子，是由W3C提出的一种新的布局方式，目前兼容IE10+以上的浏览器，任何一个盒子包括行内元素都可以指定为Flex布局，只需要在它的父元素设置display:flex;即可

```css
.box{
  display:flex;
}
```

注意，在设置了flex布局以后，其所有的子元素的float，clear和vertical- align属性都将失效

父元素设置flex后，会成为flex容器（box），它的所有子元素默认称为容器成员（item），容器默认存在两根轴线，水平的主轴线和垂直的交叉轴线

![img](https://upload-images.jianshu.io/upload_images/2808208-76920500e99115d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

在父元素（box）设置flex后，会有6个属性

#### box

> **flex- direction**
>
> **flex-wrap**
>
> **flex-flow**
>
> **justify- content**
>
> **align-items**
>
> **align-content**

下面我们来依次分析这几个属性

##### fiex-direction

该属性决定主轴的方向，也就是项目的排列方向，它有4个参数 ，**主轴方向改变后交叉轴的方向也会改变**

- flex-direction:row;这个是默认值，默认主轴为水平方向，起点在左端

- flex-direction:row-reverse;主轴为水平方向，起点在右端

- flex-direction:column;主轴在垂直方向，起点在上端

- fiex-direction:column-reverse;主轴在垂直方向，起点在下端

  

  ![img](https://upload-images.jianshu.io/upload_images/2808208-6e6c1e3f79e823a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/912/format/webp)

  flex-direction.png

##### flex-wrap

该属性决定子元素如果不在一条轴线上如何换行

- nowrap:该属性是默认属性，不换行，如果我们设定的item大小在一行上回超出box，那么不会溢出也不会换行，而是item会变形，从而适配当前box

- wrap:正常换行，在第一行正下方

- wrap-reverse:在第一行正上方换行，第一行会默认从底部开始

  

  ![img](https://upload-images.jianshu.io/upload_images/2808208-f05d37d7a20feaf0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/835/format/webp)

  wrap.png

##### flex-flow

该属性是flex-direction和flex-wrap属性的简写，默认值是row nowrap;

```
.box{
  display:flex;
  flex-flow:row nowrap;
}
```

##### justify-content

该属性定义了项目在主轴上的对齐方式

- flex-start：该属性是默认值，从左端对齐
- flex-end：该属性规定从右端对齐
- center：该属性规定居中对齐
- space- between：该属性规定两端对齐，子元素之间的间隔都相等
- space- around：该属性规定每个子元素的两侧的间隔相等，所以子元素之间的间隔比子元素与边框之间的间隔大一倍



![img](https://upload-images.jianshu.io/upload_images/2808208-64788779311dd113.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/861/format/webp)

justify-content.png

##### align-items

该属性定义项目在交叉轴上如何对齐

- flex-start：交叉轴的起点对齐

- flex-end：交叉轴的终点对齐

- center：交叉轴的中点对齐

- baseline：子元素的第一行文字的基线对齐

- stretch：默认值，如果子元素未设置高度或auto，将占满整个父容器的高度

  

  ![img](https://upload-images.jianshu.io/upload_images/2808208-8ebaf9409330bdc1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/785/format/webp)

  align-items.png

##### align-content

定义了多根轴线的对齐方式，如果父元素只有一根轴线，该属性不起作用

- flex-start：与交叉轴的起点对齐
- flex-end：与交叉轴的终点对齐
- cengter：与交叉轴的中点对齐
- space- between：与交叉轴的两端对齐，轴线之间的间隔平均分布
- space-around：每跟轴线之间的距离相等所以轴线之间的间隔比与边框线之间的间隔大一倍
- stretch：默认值，轴线占满整个交叉线

#### item

在父元素设置了flex属性后，子元素会有以下6个属性

> order
>
> flex-grow
>
> flex-shrink
>
> flex- basis
>
> flex
>
> align-self

下面我们来依次了解这些属性

##### order

该属性定义项目的排列顺序，数值越小，排列越靠前，默认为0

![img](https://upload-images.jianshu.io/upload_images/2808208-2550baf0c4d21716.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/338/format/webp)

order.png

##### flex-grow

该属性定义项目的放大比例，默认为0，即使存在剩余空间，也不放大，如果所有子元素都设置该属性为1，那么他们将会等分剩余的空间，如果一个其中一个子元素的flex-grow属性设置为2，那么它将会比其他的子元素多一倍



![img](https://upload-images.jianshu.io/upload_images/2808208-18a0ae845f220fd9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/409/format/webp)

flex-grow.png

##### flex-shrink

该属性定义了项目的缩小比例，默认为1，也就是说，如果空间不足，该项目将会缩小，如果所有项目的缩小比例都为1，那么如果空间不足，所有项目都将会等比例缩小，如果其中一个设置为0，那么在空间不足的情况下，其它项目会缩小， 该项目不会缩小，该属性设置负值无效

##### flex- basis

属性定义在分配多余空间之前，项目占据的主轴空间，浏览器根据这个属性，计算主轴是否有多余的空间，它的默认值为auto，即项目本来的大小，该属性可以设置和width或length属性一样的值，那么项目将占据固定的空间

flex

该属性是flex- grow，flex-shrink和flex- basis的简写，默认值为0 1 auto，后两个属性可选

##### align-self

该属性允许单个项目有与其他项目不一样的对齐方式，可覆盖 align-items属性，默认值为auto，表示继承父元素的 align-items属性，如果没有父元素，那么等同于 stretch

```
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```



![img](https://upload-images.jianshu.io/upload_images/2808208-37e05c9620ffe941.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/812/format/webp)

align-self.png

该属性可能取6个值，除了auto，其他都与align-items属性完全一致。



# BFC详解

## 一、BFC是什么？

在解释 BFC 是什么之前，需要先介绍 Box、Formatting Context的概念。

**Box: CSS布局的基本单位**

Box 是 CSS 布局的对象和基本单位， 直观点来说，就是一个页面是由很多个 Box 组成的。元素的类型和 display 属性，决定了这个 Box 的类型。 不同类型的 Box， 会参与不同的 Formatting Context（一个决定如何渲染文档的容器），因此Box内的元素会以不同的方式渲染。让我们看看有哪些盒子：

> - block-level box:display 属性为 block, list-item, table 的元素，会生成 block-level box。并且参与 block fomatting context；
> - inline-level box:display 属性为 inline, inline-block, inline-table 的元素，会生成 inline-level box。并且参与 inline formatting context；
> - run-in box: css3 中才有， 这儿先不讲了。

**Formatting context**

Formatting context 是 W3C CSS2.1 规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。最常见的 Formatting context 有 Block fomatting context (简称BFC)和 Inline formatting context (简称IFC)。
CSS2.1 中只有 `BFC`和 `IFC`, [CSS3](http://www.cnblogs.com/lhb25/category/146075.html) 中还增加了 `GFC`和 `FFC。`

**BFC 定义**

BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

**BFC布局规则：**

> 1. 内部的Box会在垂直方向，一个接一个地放置。
> 2. Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
>
> - 边距折叠只会发生在上下边距，左右边距是不会发生折叠的
> - 边距折叠只发生邻接的上下边距中，也即兄弟节点或者父子节点
> - 发生边距折叠的两个节点必须同处于一个bfc布局中
> - 发生边距折叠的两个父子节点没有border或者padding隔开
> - 只有普通文档流中块框的垂直外边距才会发生外边距合并，行内框、浮动框- 或绝对定位之间的外边距不会合并。
>
> 1. 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
> 2. BFC的区域不会与float box重叠。
> 3. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
> 4. 计算BFC的高度时，浮动元素也参与计算

## 二、哪些元素会生成BFC?

> 1. 根元素
> 2. float属性不为none
> 3. position为absolute或fixed
> 4. display为inline-block, table-cell, table-caption, flex, inline-flex
> 5. overflow不为visible

## 三、BFC的作用及原理

**1. 自适应两栏布局**

```html
<style>
    body {
        width: 300px;
        position: relative;
    }
 
    .aside {
        width: 100px;
        height: 150px;
        float: left;
        background: #f66;
    }
 
    .main {
        height: 200px;
        background: #fcc;
    }
</style>
<body>
    <div class="aside"></div>
    <div class="main"></div>
</body>
```



![img](https://upload-images.jianshu.io/upload_images/12665637-bbc084a97d955be3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/304/format/webp)

根据BFC布局规则第3条：

> 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

因此，虽然存在浮动的元素aslide，但main的左边依然会与包含块的左边相接触。

根据BFC布局规则第四条：

> BFC的区域不会与float box重叠。

我们可以通过通过触发main生成BFC， 来实现自适应两栏布局。

```
.main {
    overflow: hidden;
}
```

当触发main生成BFC后，这个新的BFC不会与浮动的aside重叠。因此会根据包含块的宽度，和aside的宽度，自动变窄。效果如下：



![img](https://upload-images.jianshu.io/upload_images/12665637-b4c1c2099ef4f891.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/302/format/webp)

这个方法可以避免文字环绕效果

```
<div style="height: 100px;width: 100px;float: left;background: #ff5555"></div>
    <div style="width: 200px; height: 200px;background: #eee">我是一个没有设置浮动,
        也没有触发 BFC 元素,我是一个没有设置浮动,
        也没有触发 BFC 元素,</div>
```



![img](https://upload-images.jianshu.io/upload_images/12665637-baa6b17fb07cd22a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/214/format/webp)

这时候其实第二个元素有部分被浮动元素所覆盖，(但是文本信息不会被浮动元素所覆盖) 如果想避免元素被覆盖，可触第二个元素的 BFC 特性，在第二个元素中加入 overflow: hidden，就会变成：



![img](https://upload-images.jianshu.io/upload_images/12665637-fd32cd7c66e86f70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/314/format/webp)

**2. 清除内部浮动**

```
<style>
    .par {
        border: 5px solid #fcc;
        width: 300px;
    }
 
    .child {
        border: 5px solid #f66;
        width:100px;
        height: 100px;
        float: left;
    }
</style>
<body>
    <div class="par">
        <div class="child"></div>
        <div class="child"></div>
    </div>
</body>
```



![img](https://upload-images.jianshu.io/upload_images/12665637-05ff95e1f2ffdf84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/326/format/webp)

根据BFC布局规则第六条：

> 计算BFC的高度时，浮动元素也参与计算

为达到清除内部浮动，我们可以触发par生成BFC，那么par在计算高度时，par内部的浮动元素child也会参与计算。

```
.par {
    overflow: hidden;
}
```





![img](https://upload-images.jianshu.io/upload_images/12665637-e2327e07dd434330.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/316/format/webp)

\3. 防止垂直 margin 重叠



```
<style>
    p {
        color: #f55;
        background: #fcc;
        width: 200px;
        line-height: 100px;
        text-align:center;
        margin: 100px;
    }
</style>
<body>
    <p>Haha</p>
    <p>Hehe</p>
</body>
```



![img](https://upload-images.jianshu.io/upload_images/12665637-23a99c956a713020.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/209/format/webp)

两个p之间的距离为100px，发送了margin重叠。
根据BFC布局规则第二条：

> Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠

我们可以在p外面包裹一层容器，并触发该容器生成一个BFC。那么两个P便不属于同一个BFC，就不会发生margin重叠了。

```
<style>
    .wrap {
        overflow: hidden;
    }
    p {
        color: #f55;
        background: #fcc;
        width: 200px;
        line-height: 100px;
        text-align:center;
        margin: 100px;
    }
</style>
<body>
    <p>Haha</p>
    <div class="wrap">
        <p>Hehe</p>
    </div>
</body>
```



![img](https://upload-images.jianshu.io/upload_images/12665637-bd0e711cd0213c37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/205/format/webp)

## 总结

其实以上的几个例子都体现了BFC布局规则第五条：

> BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

因为BFC内部的元素和外部的元素绝对不会互相影响，因此， 当BFC外部存在浮动时，它不应该影响BFC内部Box的布局，BFC会通过变窄，而不与浮动有重叠。同样的，当BFC内部有浮动时，为了不影响外部元素的布局，BFC计算高度时会包括浮动的高度。避免margin重叠也是这样的一个道理。
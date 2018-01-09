# SVG是什么
一种XML定义的开放标准的二维图形语言，可以用来描述三种类型的图形对象：矢量图形（直线，曲线构成的图形），图片和文本；支持使用js、css(包括动画)对其进行控制，并且支持事件（比如onmouseover，onclick)。ie9以下支持不太好，需要adobe的插件，其他标准浏览器支持情况较好。

# 如何绘制一个环形饼图  
1. svg的基本图形有：圆，椭圆，线，多边形，折线和矩形  
![基本图形](https://github.com/Namicici/web-tech/blob/master/svg/images/shape.png)  
2. svg中涉及到画圆的方式：圆，椭圆
3. 这些图形中属性支持圆心，半径设置等，太过简单，无法满足我们的需求
4. svg中还有一个path，能够被填充，跟踪，剪裁；path描述了几种几何形态：
* moveto（new point)
* lineto(draw a straight line to)
* curveto(draw a curve to)  
![曲线](https://github.com/Namicici/web-tech/blob/master/svg/images/curve.png)  
* arc (elliptical or circular arc)
* closepath (close the current shape by drawing a line to the last moveto)  
其中arc（A）表示可以画椭圆或者圆弧，符合我们需要
5. A的参数包括：
* rx ： 橢圓的 x 軸半徑 ( 根據不同的終點換算成比例 )
* ry ： 橢圓的 y 軸半徑 ( 根據不同的終點換算成比例 )
* x-axis-rotation ： 弧線與 x 軸的夾角
* large-arc-flag ： 1 為大角度弧線，0 為小角度弧線 ( 必須有三個點 )
* sweep-flag ： 1 為順時針方向，0 為逆時針方向
* x ： 終點 x 座標
* y ： 終點 y 座標

# 本例中的画法  
1. 圆环的直径取画布宽度的1/3，这样实际圆环的直径就是画布宽度的1/3，`diameter = width/3`
2. 设置厚度值为STROKE_WIDTH
3. 由于有选中效果，同角度放射效果；我们实际还会再画一个选中效果圆环，选中效果环和实际圆环中间的间隔为GAP，厚度一样；这样选中圆环直径`selectDiameter = diameter + strokeWidth*2 + GAP*2`  
![圆环坐标尺寸](https://github.com/Namicici/web-tech/blob/master/svg/images/pie.png)  
根据以上数据我们可以获得两个圆的起点坐标，圆心坐标
4. 根据传递过来的数据我们可以得出每一份数据再全部数据中所占的比例，也就是角度；已知圆心，半径和偏移角度，可以计算出目标点坐标。
    ``  
        endPoint.x = e.x + r * Math.sin(a * Math.PI / 180)  
        endPoint.y = e.y - r * Math.cos(a * Math.PI / 180)  
    ``   
5. 动画  
    animate'元素用于随着时间的推移对单个属性或属性进行动画化  
所以我们需要对属性做动画，那么对什么属性做动画呢？  
![stash](https://github.com/Namicici/web-tech/blob/master/svg/images/stash.png)  
path有属性stroke-dasharray和stroke-dashoffset   

    ``  
        <?xml version="1.0" encoding="UTF-8"?>  
        <svg width="224px" height="223px" viewBox="0 0 224 223" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">    
            <title>stash</title>  
            <desc>Created with Sketch.</desc>  
            <defs></defs>  
            <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd" stroke-dasharray="60,50">  
                <circle id="stash" stroke="#50E3C2" stroke-width="24" cx="112" cy="111" r="100"></circle>  
            </g>  
        </svg>  
    ``  

offset是偏移  
![stash offset](https://github.com/Namicici/web-tech/blob/master/svg/images/stash-offset.png)  
我们可以将将弧长的stash和gap设置为弧长长度，然后再偏移弧长长度量，那么就刚好看不见对应的弧长，然后让对应的offset量不断减少，就可以看见对应的圆环从起点慢慢进入  
    ``  
        <animate  
            attributeName="stroke-dashoffset"  
            :form="item2.dash"  
            to="3"  
            :begin="item2.begin"  
            :dur="item2.dur"  
            fill="freeze"  
        />  
    ``  
动画中的begin可以指定上一个动画结束后开始，to值不为0可以出现间隙，fill为freeze动画完成后保持完成时的状态  

![动画效果](https://github.com/Namicici/web-tech/blob/master/svg/images/svg-animation.gif)  
6. 接下来我们要完成选中图例提示

![选中图例提示](https://github.com/Namicici/web-tech/blob/master/svg/images/svg-legend.png)  
这里设计矩形框和文字， 矩形可以使用rect建立，可以设置透明程度，文字需要创建文字text。  
希望根据鼠标点击的位置来决定图例显示在哪儿，那么需要对path绑定click事件，并且记录下点击位置  

# 参考  
[官网](https://www.w3.org/TR/SVG11/intro.html#W3CCompatibility)  
[svg研究之路](http://www.oxxostudio.tw/articles/201406/svg-05-path-2.html)  
[轻松打开svg动画大门](https://isux.tencent.com/svg-animate.html)  

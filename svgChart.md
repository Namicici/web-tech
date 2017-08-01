# SVG是什么
一种XML定义的开放标准的二维图形语言，可以用来描述三种类型的图形对象：矢量图形（直线，曲线构成的图形），图片和文本；支持使用js、css(包括动画)对其进行控制，并且支持事件（比如onmouseover，onclick)。ie9以下支持不太好，需要adobe的插件，其他标准浏览器支持情况较好。

# 如何绘制一个环形饼图  
1. svg的基本图形有：圆，椭圆，线，多边形，折线和矩形
2. svg中涉及到画圆的方式：圆，椭圆
3. 这些图形中属性支持圆心，半径设置等，太过简单，无法满足我们的需求
4. svg中还有一个path，能够被填充，跟踪，剪裁；path描述了几种几何形态：moveto（new point), lineto(draw a straight line to), curveto(draw a curve to), arc (elliptical or circular arc) and closepath (close the current shape by drawing a line to the last moveto)  
其中arc（A）表示可以画椭圆或者圆弧，符合我们需要
5. A的参数包括：
"
rx ： 橢圓的 x 軸半徑 ( 根據不同的終點換算成比例 )
ry ： 橢圓的 y 軸半徑 ( 根據不同的終點換算成比例 )
x-axis-rotation ： 弧線與 x 軸的夾角
large-arc-flag ： 1 為大角度弧線，0 為小角度弧線 ( 必須有三個點 )
sweep-flag ： 1 為順時針方向，0 為逆時針方向
x ： 終點 x 座標
y ： 終點 y 座標
"

# 本例中的画法  
1. 圆环的直径取画布宽度的1/3，这样实际圆环的直径就是画布宽度的1/3，diameter = width/3
2. 设置厚度值为STROKE_WIDTH
3. 由于有选中效果，同角度放射效果；我们实际还会再画一个选中效果圆环，选中效果环和实际圆环中间的间隔为GAP，厚度一样；这样选中圆环直径selectDiameter = diameter + strokeWidth*2 + GAP*2
![圆环坐标尺寸](https://github.com/Namicici/web-tech/blob/master/pie.svg)  
根据以上数据我们可以获得两个圆的起点坐标，圆心坐标
4. 根据传递过来的数据我们可以得出每一份数据再全部数据中所占的比例，也就是角度；已知圆心，半径和偏移角度，可以计算出目标点坐标。
"
endPoint.x = e.x + r * Math.sin(a * Math.PI / 180)
endPoint.y = e.y - r * Math.cos(a * Math.PI / 180)
"

5. 动画

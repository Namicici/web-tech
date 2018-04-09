# PC端的自适应是怎么回事儿  
[自适应网页设计](http://www.ruanyifeng.com/blog/2012/05/responsive_web_design.html)  
所谓自适应就是自动识别屏幕宽度，并做相应调整。这里的屏幕宽度主要是指屏幕像素。  
比如在不同的分辨率下呈现不一样的布局：  
!(https://github.com/Namicici/web-tech/blob/master/adaptive/images/response.png)   
* 所以这里涉及几个问题：  
1. 要分辨出当前窗口的分辨率  
2. 元素要随着屏幕像素大小改变  
3. 对于内容超过屏幕可以容纳的需要自动换行显示    
* 解决上诉几个问题的方案：  
1. 媒体查询，区分出屏幕；这在css中可以解决``@media screen and (max-device-width: 400px){XXXXXX}``
2. 使用百分比;不能使用固定像素，因为元素的长款应该根据屏幕的大小作出相应调整。所以``width: x%``要使用百分比来表示  
3. 使用浮动块级元素，防止溢出出现滚动条；``float:left``.  

对于同一个元素如果要在各个屏幕下都显示出对应的布局，那么意味着对同一个元素作用多种屏幕下的css；比如1300以上的屏幕有css类``grid-lg-2``，600-1300的有``grid-md-4``, 600以下有``grid-xs-10``，这些css中的类分别放在对应的媒体查询中。那么使用的时候就需要把这三个类都作用于对应的元素中，当屏幕发生变化的时候@media检测到对应屏幕后，对应的class生效而且他屏幕下的class不生效，这样就实现了不同屏幕下看到的元素的尺寸在响应变化以适应屏幕布局。 
``<div class="grid-lg-2 grid-md-4 grid-xs-10"></div>``   

另外这里有一个细节``<meta name="viewport" content="width=device-width, initial-scale=1" />``这里的viewport的width设置为了device-width，可以不设置么？或者用其他的属性？ PC上可以不设置的，但是mobile的默认视窗大小不是设备屏幕大小  

另外现在也有flex布局，参考[flex布局介绍](https://github.com/PawelLin/css-layout)  

# 移动端的适配问题  

## 参考  
[dpi、ppi、尺寸、分辨率等关系](http://www.cnblogs.com/liuwenbohhh/p/4522852.html)  

## 概念理解  
1. 尺寸，iPhone6: 4.7寸是指可视屏幕对角线的长度， 可视区：2.3 * 4.1  iPhone 6 plus 5.5寸， 2.7 * 4.8  
!(https://github.com/Namicici/web-tech/blob/master/adaptive/images/inch.png)  
2. 分辨率：横竖方向上的最大像素数， 分辨率大小本身不能反应屏幕的好坏，得结合尺寸使用，相同尺寸下分辨率越大说明显示越细腻。 
3. ppi： 每英寸的像素数。值越大显示越细腻  
Retina Display 意为视网膜显示屏，名称为苹果独享（已注册商标），其他公司不能使用此名称，根据科学研究发现，人眼每英寸最对可以看到大约 300 个像素，多了就分辨不出来了，也就是 ppi 的值为 300，Retina Display 所表示的就是，ppi 大于 300 的显示屏。  
!(https://github.com/Namicici/web-tech/blob/master/adaptive/images/retina.png)  
4. dpi： dpi 全称 dots per inch，意为每英寸的点数，在打印行业，一般指的是每英寸墨滴的数量，dpi 的数值越小，打印出来的质量越差，在 iPhone 屏幕设计中，和 Points 数值有关。 
5. Points：iPhone中引入的坐标点，为了便于设计的统一。所以dpi和ppi中间有一个渲染比例。  
对于同样大小的区域，物理像素的个数根据屏幕的不同有不同，我们在css中的1px是指点素1px，实际对应的物理像素可能是1个像素也可能是4个像素（根据渲染比不同而不同）  
!(https://github.com/Namicici/web-tech/blob/master/adaptive/images/points.png)  
6. device-pixel-ratio：就是上面提到的渲染比  
7. viewport视窗，在桌面浏览器中viewport就是浏览器的窗口大小；但是在移动端有点儿复杂，引入了两个概念：virtual viewport和layout viewport；其中virtual viewport就是屏幕上可以看到的部分，而layout viewport就是css布局采用的宽度（PC的页面到移动端就有滚动条了）， ![默认情况下的视窗大小](https://github.com/Namicici/web-tech/blob/master/adaptive/images/viewport.png)  

# 我们说的移动端适配是适配什么  
比如UI出了iphone6下的视觉稿，在其他设备上元素比例保持跟iphone6一致，注意这里是比例。如果全部用px，在iphone其他幸好的元素大小（尺寸）跟iphone6一致，但是在iphone4/5下屏幕更小，希望是元素比例缩小，这样协调一些，所以这里涉及了适配问题。  
我们需要解决两个问题，一是找到一种可以按比例缩放的中间单位（px是不行了，因为用px后尺寸大小会在不同屏幕下一样大小，iphone就是这样做的），二是如何设定缩放比例  
* 对于问题一，rem是相对于html的font-size尺寸，比如font-size是16px，那么1rem就是16px
* 对于问题二：  
[手淘适配方案](https://github.com/amfe/article/issues/17)  
[手淘方案详解](http://div.io/topic/1092)  
手淘有两点注意的地方：  
1. 为何要在viewport中设置scale  
2. 基准rem的计算  
3. 字体大小如何处理  
* 对于问题1， 考虑iphone6 dpr为2， 视觉稿是750的，如果给出了一个border是1px那么实际在css中应该使用0.5px（逻辑宽度是375），但是0.5px渲染的时候会变成0。解决这种情况的办法就是：viewport中的width设置为750，但是缩小0.5（1/2）。  
* 对于问题2我们只需要规定一种比例算法就好，手淘中是逻辑宽度*Dpr/10，所以对于iphone6来说rem是75px（这里乘了Dpr就是因为1px的问题可能页面缩放了）  
*  手淘对字体大小认为不同设备字体大小一样，不安比例缩放，也就是说不用rem来表示，但是由于不同的dpr页面有缩放，所以需要根据dpr将字体再缩放回去  

## 对于图片问题  
对于位图而言，像素对应的是物理像素（跟我们上面讲的css像素不同），那么对于200px的图片，在dpr为1的时候200个像素一一对应。但是对于dpr>1的情况，200像素实际会对应到200*4=800个像素上面去，那么对于位图中的1一个像素就会就近取色，然后在对应到4个物理像素上去，由于是就近取色的，所以存在失真导致图片模糊  
### 参考
但是这里有一些特殊情况，如果viewport中不设置width，那么缩放的时候是采用默认分辨率缩放的，对于iphone6来说逻辑像素*Dpr=物理像素，但是对于一些逻辑像素*Dpr != 物理像素的情况呢  
[在谈retina下1px的问题](https://www.w3cplus.com/css/fix-1px-for-retina.html)  
其中iphone6+是在1242的基础上缩小了约13%的  

[不做缩放，指通过改变root font size来做](http://blog.csdn.net/weihaifeng163/article/details/66974490)  
核心的算法就是 realWidth/designWidth * rem2px / defaultFontSize * 100%  
其中realWidth就是实际设备的逻辑宽度， designWidth是视觉稿的宽度， rem2px是视觉稿宽度的目标font-size， defaultFontSize是浏览器默认root字体大小  
比如假设750下的1rem=100px，那么对于iphone6来说：375/750*100/16*100%=321.5%，那么html(font-size:312.5%;)  
如果不用比例，而是用px，315/750*100= 50px; 这个时候1rem = 50px；如果750的视觉高中设计的margin-top:20px; 那么转化为rem:20/200=0.2rem  

[我们目前css中是如何使用逻辑像素和渲染比的](https://github.com/Namicici/web-tech/blob/master/adaptive/images/css-media.jpg)  
那么我们的程序中是如何设计的呢？  
我们的方法类似上面的这种情况，375/750*200=100px,这里多了rem2px的目标参数是200（所谓的目标参数就是跟视觉稿尺寸一致的时候的rem to px的转化值，这个值是可以人为设定的）    
但是对于414的屏幕: 414/750*200 = 111x，但是我们用了100px，我们取的都是近似值，不过准确点的话可以近似到110px（个人社区h5）   
开发的时候我们只需要根据视觉稿的尺寸来计算即可，比如对于750的视觉稿，如果有宽度是20px，那么对应的rem值就是20/200。其中对于1080的屏幕为140px这个没懂？？？？桌面PC上的适配？？？  

[vw方案-vue中vw适配方案](https://www.w3cplus.com/mobile/vw-layout-in-vue.html)
1vw为1%  
这里涉及的就是转化问题，使用postcss  

## iPhone 6 plus 特殊 如何解释   
![iPhone参数](https://github.com/Namicici/web-tech/blob/master/adaptive/images/iphoneSum.jpg) 


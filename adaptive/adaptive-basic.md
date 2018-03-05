# PC端的自适应是怎么回事儿  

# 移动端的适配问题  

## 参考  
[dpi、ppi、尺寸、分辨率等关系](http://www.cnblogs.com/liuwenbohhh/p/4522852.html)  

## 概念理解
1. 尺寸，iPhone6: 4.7寸是指可视屏幕对角线的长度， 可视区：2.3 * 4.1  iPhone 6 plus 5.5寸， 2.7 * 4.8  
2. 分辨率：横竖方向上的最大像素数， 分辨率大小本身不能反应屏幕的好坏，得结合尺寸使用，相同尺寸下分辨率越大说明显示越细腻。 
3. ppi： 每英寸的像素数。值越大显示越细腻  
4. dpi： 每英寸的点数。值越大打印越细腻  
5. Points：iPhone中引入的坐标点，为了便于设计的统一。所以dpi和ppi中间有一个渲染比例，1个pt根据渲染比例的不同可能渲染到2/3个像素上去，以此可以用一份设计稿适配不同的屏幕  
6. device-pixel-ratio：就是上面提到的渲染比  
7. viewport视窗，在桌面浏览器中viewport就是浏览器的窗口大小；但是在移动端有点儿复杂，引入了两个概念：virtual viewport和layout viewport；其中virtual viewport就是屏幕上可以看到的部分，而layout viewport就是css布局采用的宽度（PC的页面到移动端就有滚动条了）， ![默认情况下的视窗大小](https://github.com/Namicici/web-tech/blob/master/adaptive/images/viewport.jpg)  

# 我们说的移动端适配是适配什么  
比如UI出了iphone6下的视觉稿，在其他设备上元素比例保持跟iphone6一致，注意这里是比例。如果全部用px，在iphone其他幸好的元素大小（尺寸）跟iphone6一致，但是在iphone4/5下屏幕更小，希望是元素比例缩小，这样协调一些，所以这里涉及了适配问题。  
我们需要解决两个问题，一是找到一种可以按比例缩放的中间单位（px是不行了，因为用px后尺寸大小会在不同屏幕下一样大小，iphone就是这样做的），二是如何设定缩放比例  
* 对于问题一，rem是相对于html的font-size尺寸，比如font-size是16px，那么1rem就是16px
* 对于问题二：  
![手淘适配方案](https://github.com/amfe/article/issues/17)  
    
![我们目前css中是如何使用逻辑像素和渲染比的](https://github.com/Namicici/web-tech/blob/master/adaptive/images/css-media.jpg)  
* device-width指的就是逻辑宽度，是apple定义的Points的宽度；在移动端css中的px表示的是逻辑像素，在dpr为2的时候css中的1px实际渲染的时候对应到了2个物理像素上面去了


## iPhone 6 plus 特殊 如何解释  
![iPhone参数](https://github.com/Namicici/web-tech/blob/master/adaptive/images/iphoneSum.jpg) 



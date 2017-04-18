# Six-kinds-of-gray-algorithm
#### 六种有趣的灰度算法<br>
# 三原色与灰度
原色是指不能透过其他颜色的混合调配而得出的“基本色”。一般来说叠加型的三原色是红色、绿色、蓝色，以不同比例将原色混合，可以产生出其他的新颜色。这套原色系统常被称为“RGB色彩空间”，亦即由红（R）绿（G）蓝（B）所组合出的色彩系统。当这三种原色以等比例叠加在一起时，会变成灰色；若将此三原色的强度均调至最大并且等量重叠时，则会呈现白色。灰度就是没有色彩，RGB色彩分量全部相等。
# 获取图片像素数据
算法以前端为例
```
//伪代码
var img = new Image();
img.src = 'xxx.jpg';
var myCanvas = document.querySelector(canvasId);
var canvasCtx = myCanvas.getContext("2d");
canvasCtx.drawImage(img, 0, 0, img.width, img.height);
//图片的像素数据
var data = canvasCtx.getImageData(0, 0, img.width, img.height);
```
使用getImageData()返回一个ImageData对象，此对象有个data属性就是我们要的数据了，数据是以Uint8ClampedArray 描述的一个一维数组，包含以 RGBA 顺序的数据，数据使用 0 至 255（包含）的整数表示。 所以，一个像素会有4个数据（RGBA）,RGB是红绿蓝，A指的是透明度。可以看到图片的数据很长，如果一次性处理很多图片的时候，计算量相当可观，所以例子中会使用worker，把繁重的计算任务交给后台线程。
# 算法的基本步骤
1.取得每一个像素的red，green，blue值。<br>
2.使用灰度算法，算出一个灰度值。<br>
3.用这个灰度值代替像素原始的red，green，blue值。<br>
举个例子，我们这里用到的灰度算法是：
```
Gray = (Red + Green + Blue) / 3
```
代码：
```
//伪代码
for(var Pixel in Image){
  var Red = Image[Pixel].Red
  var Green = Image[Pixel].Green
  var Blue = Image[Pixel].Blue

  var Gray = (Red + Green + Blue) / 3

  Image[Pixel].Red = Gray
  Image[Pixel].Green = Gray
  Image[Pixel].Blue = Gray
}
```
# 算法一-平均法
这是最常见的灰度算法，简单暴力，把它放到第一位。公式是：
```
Gray = (Red + Green + Blue) / 3
```
这个算法可以生成不错灰度值，因为公式简单，所以易于维护和优化。但是因为简单快速，从人眼的感知角度看，图片的灰度阴影和亮度方面做的还不够好。所以，我们需要更复杂的运算。
# 算法二-基于人眼感知
人眼对不同光感知程度不同。人的眼睛内有几种辨别颜色的锥形感光细胞，分别对黄绿色、绿色和蓝紫色的光最敏感。虽然眼球中的椎状细胞并非对红、绿、蓝三色的感受度最强，但是由肉眼的椎状细胞所能感受的光的带宽很大，红、绿、蓝也能够独立刺激这三种颜色的受光体。<br>
人类对红绿蓝三色的感知程度依次是： 绿>红>蓝，所以平均算法从这个角度看是不科学的。应该按照人类对光的感知程度为每个颜色设定一个权重，它们的之间的地位不应该是平等的。<br>
一个图像处理通用的公式是：
```
Gray = (Red * 0.3 + Green * 0.59 + Blue * 0.11)
```
现在对图像灰度处理的最佳公式还存在争议，有一些类似的公式：
```
Gray = (Red * 0.2126 + Green * 0.7152 + Blue * 0.0722)
```
```
Gray = (Red * 0.299 + Green * 0.587 + Blue * 0.114)
```
# 算法三-去饱和
为了更通俗易懂，有时我们选择HLS模型描述颜色，这三个字母分别表示Hue(色调)、Saturation(饱和度)、Lightness(亮度)。色调，取值为：0 - 360，0(或360)表示红色，120表示绿色，240表示蓝色，也可取其他数值来指定颜色。饱和度，取值为：0.0% - 100.0%，它通常指颜色的鲜艳程度。亮度，取值为：0.0% - 100.0%，黑色的亮度为0。去饱和的过程就是把RGB转换为HLS，然后将饱和度设为0。因此，我们需要取一种颜色，转换它为最不饱和的值。这个数学公式比本文介绍的更复杂，这里提供一个简单的公式，一个像素可以被去饱和通过计算RGB中的最大值和最小值的中间值：
```
Gray = ( Math.max(Red, Green, Blue) + Math.min(Red, Green, Blue) ) / 2
```
去饱和后，图片立体感减弱，但是更柔和。
# 算法四-分解
# 算法五-单一通道
# 算法六-自定义灰度阴影

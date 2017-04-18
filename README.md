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
# 算法二-基于人眼感知
# 算法三-去饱和
# 算法四-分解
# 算法五-单一通道
# 算法六-自定义灰度阴影

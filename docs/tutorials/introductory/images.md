# 图像教程

有关使用Matplotlib打印图像的简短教程。

## 启动方式

首先，让我们从IPython开始。它是对标准Python提示的最佳增强，它与Matplotlib有着很好的兼容性。现在可以在shell或IPython Notebook上启动IPython。 

随着IPython的启动，我们现在需要连接到GUI事件循环。这告诉IPython在哪里（和如何）显示图像。要连接到GUI循环，请在IPython提示符下执行 **%matplotlib** magic。 关于它在[IPython的GUI事件循环](http://ipython.org/ipython-doc/2/interactive/reference.html#gui-event-loop-support)文档中的确切含义有更详细的说明。

如果您使用的是IPython Notebook，则可以使用相同的命令，但人们通常使用特定的参数来使用%matplotlib技法：

```python
In [1]: %matplotlib inline
```

这将打开内联打印，其中打印图形将显示在笔记本中。这对互动有重要的影响。对于内联打印，单元格下方输出打印的单元中的命令不会影响打印。例如，无法从创建打印的单元格下方的单元格更改颜色贴图。但是，对于打开单独窗口的其他后端(如Qt5)，创建打印的单元格将更改打印-它是内存中的活动对象。

本教程将使用matplotlib的命令式打印界面pylot。此界面保持全局状态，对于快速轻松地尝试各种打印设置非常有用。另一种选择是面向对象的接口，它也非常强大，通常更适合大型应用程序开发。如果您想了解面向对象的接口，我们的[使用指南](https://matplotlib.org/tutorials/introductory/usage.html)是一个很好的起点。现在，让我们继续使用命令式方法：

```python
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
import numpy as np
```

## 将图像数据导入Numpy数组

枕形库支持加载图像数据。在本地，Matplotlib仅支持PNG图像。如果本机读取失败，下面显示的命令将返回[Pillow](https://pillow.readthedocs.io/en/latest/)。

此示例中使用的图像是一个PNG文件，但请记住对您自己的数据的枕头要求。

下面是我们要操作的图片：

![黑白图片](/static/images/tutorials/stinkbug.png)

这是一个24位的RGB PNG图像(R、G、B每个8位)。根据您获取数据的位置，您最可能遇到的其他类型的图像是RGBA图像(允许透明)或单通道灰度(亮度)图像。您可以右键单击它并选择“将图像另存为”，将其下载到您的计算机上，以便完成本教程的其余部分。

我们开始吧.。

```python
img = mpimg.imread('../../doc/_static/stinkbug.png')
print(img)
```

输出：

```python
[[[0.40784314 0.40784314 0.40784314]
  [0.40784314 0.40784314 0.40784314]
  [0.40784314 0.40784314 0.40784314]
  ...
  [0.42745098 0.42745098 0.42745098]
  [0.42745098 0.42745098 0.42745098]
  [0.42745098 0.42745098 0.42745098]]

 [[0.4117647  0.4117647  0.4117647 ]
  [0.4117647  0.4117647  0.4117647 ]
  [0.4117647  0.4117647  0.4117647 ]
  ...
  [0.42745098 0.42745098 0.42745098]
  [0.42745098 0.42745098 0.42745098]
  [0.42745098 0.42745098 0.42745098]]

 [[0.41960785 0.41960785 0.41960785]
  [0.41568628 0.41568628 0.41568628]
  [0.41568628 0.41568628 0.41568628]
  ...
  [0.43137255 0.43137255 0.43137255]
  [0.43137255 0.43137255 0.43137255]
  [0.43137255 0.43137255 0.43137255]]

 ...

 [[0.4392157  0.4392157  0.4392157 ]
  [0.43529412 0.43529412 0.43529412]
  [0.43137255 0.43137255 0.43137255]
  ...
  [0.45490196 0.45490196 0.45490196]
  [0.4509804  0.4509804  0.4509804 ]
  [0.4509804  0.4509804  0.4509804 ]]

 [[0.44313726 0.44313726 0.44313726]
  [0.44313726 0.44313726 0.44313726]
  [0.4392157  0.4392157  0.4392157 ]
  ...
  [0.4509804  0.4509804  0.4509804 ]
  [0.44705883 0.44705883 0.44705883]
  [0.44705883 0.44705883 0.44705883]]

 [[0.44313726 0.44313726 0.44313726]
  [0.4509804  0.4509804  0.4509804 ]
  [0.4509804  0.4509804  0.4509804 ]
  ...
  [0.44705883 0.44705883 0.44705883]
  [0.44705883 0.44705883 0.44705883]
  [0.44313726 0.44313726 0.44313726]]]
```

注意这里的dtype-Float32。Matplotlib已将每个通道的8位数据重新缩放为0.0到1.0之间的浮点数据。另外，Pillow可以使用的唯一数据类型是uint8。Matplotlib绘图可以处理Float32和uint8，但PNG以外的任何格式的图像读/写仅限于uint8数据。为什么是8位？大多数显示器只能渲染每通道8位的颜色渐变。为什么它们只能呈现8位/通道？因为这是人眼所能看到的。更多在这里(从摄影的角度)：[发光景观位深度教程](https://luminous-landscape.com/bit-depth/)。

每个内部列表代表一个像素。在这里，对于RGB图像，有3个值。由于是黑白图像，R、G和B都是相似的。RGBA(其中A是Alpha或透明度)，每个内部列表有4个值，一个简单的亮度图像只有一个值(因此只是一个2-D数组，而不是3-D数组)。对于RGB和RGBA图像，matplotlib支持Float32和uint8数据类型。对于灰度，matplotlib仅支持Float32。如果数组数据不符合这些描述之一，则需要重新缩放它。

## 将数字数组绘制为图像

因此，您将数据放在一个numpy数组中（通过导入或生成它）。 让我们渲染吧。 在Matplotlib中，这是使用[imshow()](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.imshow.html#matplotlib.pyplot.imshow)函数执行的。 在这里，我们将抓住绘图对象。 此对象为您提供了一种从提示中操作绘图的简便方法。

```python
imgplot = plt.imshow(img)
```

![使用matplotlib打印的图片](/static/images/tutorials/sphx_glr_images_001.png)

你也可以绘制任何numpy数组。

### 将伪彩色方案应用于图像图

Pseudocolor可以成为增强对比度和更容易可视化数据的有用工具。这在使用投影仪进行数据演示时尤其有用 - 它们的对比度通常非常差。

伪彩色仅与单通道，灰度，亮度图像相关。 我们目前有一个RGB图像。由于R，G和B都是相似的（请参阅上面或您的数据），我们可以选择一个数据通道：

```python
lum_img = img[:, :, 0]

# This is array slicing.  You can read more in the `Numpy tutorial
# <https://docs.scipy.org/doc/numpy/user/quickstart.html>`_.

plt.imshow(lum_img)
```

![伪彩色方案](/static/images/tutorials/sphx_glr_images_002.png)

现在，使用亮度（2D，无颜色）图像，应用默认色图（也称为查找表，LUT）。 默认名称为viridis。 还有很多其他选择。

```python
plt.imshow(lum_img, cmap="hot")
```

![伪彩色方案2](/static/images/tutorials/sphx_glr_images_003.png)

注意：您还可以使用set_cmap()方法更改现有绘图对象上的颜色映射：

```python
imgplot = plt.imshow(lum_img)
imgplot.set_cmap('nipy_spectral')
```

![伪彩色方案3](/static/images/tutorials/sphx_glr_images_004.png)

注意：但是，请记住，在具有内联后端的IPython笔记本中，您无法更改已经呈现的绘图。 如果你在一个单元格中创建imgplot，则不能在稍后的单元格中调用set_cmap()并期望更早的绘图。 确保在一个单元格中一起输入这些命令。 plt命令不会更改早期单元格的图形。

还有许多其他色彩方案可用。 查看[色彩映射的列表和图像](https://matplotlib.org/tutorials/colors/colormaps.html)。

### 颜色比例参考

了解颜色代表什么价值是有帮助的。 我们可以通过添加彩条来实现。

```python
imgplot = plt.imshow(lum_img)
plt.colorbar()
```

![伪彩色方案4](/static/images/tutorials/sphx_glr_images_005.png)

这会为您现有的图形添加一个颜色条。 如果您更改切换到不同的色彩映射，则不会自动更改 - 您必须重新创建绘图，然后再次添加颜色栏。

### 检查特定数据范围

有时，您希望增强图像的对比度，或者扩大特定区域的对比度，同时牺牲颜色的细节，这些颜色不会有太大变化，或者无关紧要。 找到有趣区域的好工具是直方图。要创建图像数据的直方图，我们使用[hist()](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.hist.html#matplotlib.pyplot.hist)函数。

```python
plt.hist(lum_img.ravel(), bins=256, range=(0.0, 1.0), fc='k', ec='k')
```

![直方图](/static/images/tutorials/sphx_glr_images_006.png)

最常见的情况是，图像的“有趣”部分位于峰值附近，您可以通过剪裁峰值上方和/或下方的区域来获得额外的对比度。在我们的直方图中，看起来高端中没有太多有用的信息(图像中没有很多白色的东西)。让我们调整上限，以便有效地“放大”直方图的一部分。我们通过将clim参数传递给imshow来实现这一点。也可以通过调用图像打印对象的“set_clim()”方法来实现这一点，但是在使用IPython Notebook时，请确保在与PLOT命令相同的单元格中执行此操作-它不会更改以前单元格中的打印。

可以在打印调用中指定客户端。

```python
imgplot = plt.imshow(lum_img, clim=(0.0, 0.7))
```

![伪彩色图](/static/images/tutorials/sphx_glr_images_007.png)

还可以使用返回的对象指定客户端。

```python
fig = plt.figure()
a = fig.add_subplot(1, 2, 1)
imgplot = plt.imshow(lum_img)
a.set_title('Before')
plt.colorbar(ticks=[0.1, 0.3, 0.5, 0.7], orientation='horizontal')
a = fig.add_subplot(1, 2, 2)
imgplot = plt.imshow(lum_img)
imgplot.set_clim(0.0, 0.7)
a.set_title('After')
plt.colorbar(ticks=[0.1, 0.3, 0.5, 0.7], orientation='horizontal')
```

![伪彩色图](/static/images/tutorials/sphx_glr_images_008.png)

### 数组插值方案

根据不同的数学方案，插值计算像素“应该”的颜色或值。 发生这种情况的一个常见地方是调整图像大小。 像素数会发生变化，但您需要相同的信息。 由于像素是离散的，因此缺少空间。 插值就是你填充那个空间的方式。 这就是为什么当你把它们炸掉时，你的图像有时看起来像是像素化的原因。 当原始图像和扩展图像之间的差异更大时，效果更明显。 让我们拍摄我们的照片并缩小它。 我们有效地丢弃像素，只保留少数像素。 现在，当我们绘制它时，数据会被炸成屏幕上的大小。 旧像素不再存在，计算机必须以像素绘制以填充该空间。

我们将使用我们用于加载图像的Pillow库来调整图像大小。

```python
from PIL import Image

img = Image.open('../../doc/_static/stinkbug.png')
img.thumbnail((64, 64), Image.ANTIALIAS)  # resizes image in-place
imgplot = plt.imshow(img)
```

![伪彩色图子图](/static/images/tutorials/sphx_glr_images_009.png)

这里我们有默认的插值，双线性，因为我们没有给imshow()任何插值参数。

我们试试其他一些。 这里是“最近的”，没有插值。

```python
imgplot = plt.imshow(img, interpolation="nearest")
```

![模糊图](/static/images/tutorials/sphx_glr_images_010.png)

和双三次：

```python
imgplot = plt.imshow(img, interpolation="bicubic")
```

![模糊图2](/static/images/tutorials/sphx_glr_images_011.png)

在吹制照片时经常使用双立方插值 - 人们往往更喜欢模糊而不是像素化。

## 下载本文的所有示例

- [下载python源码: images.py](https://matplotlib.org/_downloads/319b2b9027c123790a024608ef1578e7/images.py)
- [下载Jupyter notebook: images.ipynb](https://matplotlib.org/_downloads/7bec845b7f84613778f69a9a8413e0ab/images.ipynb)
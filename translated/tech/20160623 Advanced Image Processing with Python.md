Python高级图像处理
======================================

![](http://www.cuelogic.com/blog/wp-content/uploads/2016/06/Image-Search-Engine.png)

构建图像搜索引擎并不是一件容易的任务。这里有几个概念、工具、想法和技术需要实现。主要的图像处理概念之一是逆图像查询(RIQ)或者说逆图像搜索。Google、Cloudera、Sumo Logic 和 Birst等公司在使用逆图像搜索中名列前茅。通过分析图像和使用数据挖掘RIQ提供了很好的洞察分析能力。

### 顶级公司与逆图像搜索

有很多顶级的技术公司使用RIQ来产生最好的收益。例如：在2014年Pinterest第一次带来了视觉搜索。随后在2015年发布了一份白皮书，揭示了其架构。逆图像搜索让Pinterest获得对时尚对象的视觉特征和显示类似的产品建议的能力。

众所周知，谷歌图片使用逆图像搜索允许用户上传一张图片然后搜索相关联的图片。通过使用先进的算法对提交的图片进行分析和数学建模。然后和谷歌数据库中无数的其他图片进行比较得到相似的结果。

**这是opencv 2.4.9特征比较报告一个图表：**

![](http://www.cuelogic.com/blog/wp-content/uploads/2016/06/search-engine-graph.jpg)

### 算法 & Python库

在我们使用它工作之前，让我们过一遍构建图像搜索引擎的Python库的主要元素：

### 专利算法

#### 尺度不变特征变换算法（SIFT - Scale-Invariant Feature Transform）

1. 非自由功能的一个专利技术，利用图像识别符，以识别相似图像，甚至那些从不同的角度，大小，深度和规模点击，它们被包括在搜索结果中。[点击这里][4]查看SIFT详细视频。
2. SIFT能与从许多图片中提取的特征的大型数据库正确的匹配搜索条件。
3. 从不同的方面来匹配相同的图像和匹配不变特征来获得搜索结果是SIFT的另一个特征。了解更多关于尺度不变[关键点][5]

#### 加速鲁棒特征(SURF - Speeded Up Robust Features)算法

1. [SURF][1] 也是一种非自由功能的专利技术而且还是一种“加速”的SIFT版本。不像SIFT，

2. SURF依赖于Hessian矩阵行列式的位置和尺度。

3. 在许多应用中，旋转不变性是不是一个必要条件。所以找不到这个方向的速度加快了这个过程。

4. SURF包括几种特性，在每一步的速度提高。比SIFT快三倍的速度，SIFT擅长旋转和模糊化。然而它不擅长处理照明和变换视角。

5. Open CV，一个程序功能库提供SURF相似的功能，SURF.compute()和SURF.Detect()可以用来找到描述符和要点。阅读更多关于SURF[点击这里][2]

### 开源算法

#### KAZE 算法

1. KAZE是一个开源的非线性尺度空间的二维多尺度和新的特征检测和描述算法。在加性算子分裂的有效技术（AOS）和可变电导扩散是用来建立非线性尺度空间。

2. 多尺度图像处理基础是简单的创建一个图像的尺度空间，同时用正确的函数过滤原始图像，提高时间或规模。

#### AKAZE (Accelerated-KAZE) 算法

1. 顾名思义，这是一个更快的图像搜索方式，找到匹配的关键点在两幅图像之间。AKAZE 使用二进制描述符和非线性尺度空间来平衡精度和速度。

#### BRISK (Binary Robust Invariant Scalable Keypoints) 算法

1. BRISK 非常适合描述关键点的检测与匹配。

2. 是一种高度自适应的算法，基于尺度空间的快速检测器和一个位字符串描述符，有助于加快搜索显着。

3. 尺度空间关键点检测与关键点描述帮助优化手头相关任务的性能

#### FREAK (Fast Retina Keypoint)

1. 这个新的关键点描述的灵感来自人的眼睛。通过图像强度比能有效地计算一个二进制串级联。FREAK算法相比BRISK，SURF和SIFT算法有更快的计算与较低的内存负载。

#### ORB (Oriented FAST and Rotated BRIEF)

1. 快速的二进制描述符，ORB具有抗噪声和旋转不变性。ORB建立在FAST关键点检测器和BRIEF描述符之上，有成本低、性能好的元素属性。

2. 除了快速和精确的定位元件，有效地计算定向的BRIEF，分析变动和面向BRIEF特点相关，是另一个ORB的特征。

### Python库

#### Open CV

1. Open CV提供学术和商业用途。一个开源的机器学习和计算机视觉库，OpenCV便于组织利用和修改代码。

2. 超过2500个优化算法，包括国家最先进的机器学习和计算机视觉算法服务与各种图像搜索--人脸检测、目标识别、摄像机目标跟踪，从图像数据库中寻找类似图像、眼球运动跟随、风景识别等。

3. 像谷歌，IBM，雅虎，IBM，索尼，本田，微软和英特尔这样的大公司广泛的使用OpenCV。

4. OpenCV拥有python，java，C，C++和MATLAB接口，同时支持Windows，Linux，Mac OS和Android。

#### Python图像库 （PIL）

1. Python图像库（PIL）支持多种文件格式，同时提供图像处理和图形解决方案。开源的PIL为你的Python解释器添加图像处理能力。
2. 标准的图像处理能力包括图像增强、透明和屏蔽作用、图像过滤、像素操作等。

详细的数据和图表，请看OpenCV 2.4.9 特征比较报告。[这里][3]

### 构建图像搜索引擎

图像搜索引擎可以从预置集图像库选择相似的图像。其中最受欢迎的是谷歌的著名的图像搜索引擎。对于初学者来说，有不同的方法来建立这样的系统。提几个如下：

1. 采用图像提取、图像描述提取、元数据提取和搜索结果提取，建立图像搜索引擎。
2. 定义你的图像描述符，数据集索引，定义你的相似性度量，然后搜索和排名。
3. 选择要搜索的图像，选择用于进行搜索的目录，所有图片的搜索目录，创建图片特征索引，评估相同的搜索图片的功能，在搜索中匹配图片，并获得匹配的图片。

我们的方法基本上就比较grayscaled版本的图像，逐渐移动到复杂的特征匹配算法如SIFT和SURF，最后沉淀下来的是开源的解决方案称为BRISK。所有这些算法提供了有效的结果，在性能和延迟的细微变化。建立在这些算法上的引擎有许多应用，如分析流行统计的图形数据，在图形内容中识别对象，以及更多。

**例如**：一个图像搜索引擎需要由一个IT公司作为客户机来建立。因此，如果一个品牌的标志图像被提交在搜索中，所有相关的品牌形象搜索显示结果。所得到的结果也可以通过客户端分析，使他们能够根据地理位置估计品牌知名度。但它还比较年轻，RIQ或反向图像搜索尚未被完全挖掘利用。

这就结束了我们的文章，使用Python构建图像搜索引擎。浏览我们的博客部分来查看最新的编程技术。

数据来源：OpenCV 2.4.9 特征比较报告(computer-vision-talks.com)

(Ananthu Nair 的指导与补充)

--------------------------------------------------------------------------------

via: http://www.cuelogic.com/blog/advanced-image-processing-with-python/

作者：[Snehith Kumbla][a]
译者：[Johnny-Liao](https://github.com/Johnny-Liao)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]: http://www.cuelogic.com/blog/author/snehith-kumbla/
[1]: http://docs.opencv.org/3.0-beta/doc/py_tutorials/py_feature2d/py_surf_intro/py_surf_intro.html
[2]: http://www.vision.ee.ethz.ch/~surf/eccv06.pdf
[3]: https://docs.google.com/spreadsheets/d/1gYJsy2ROtqvIVvOKretfxQG_0OsaiFvb7uFRDu5P8hw/edit#gid=10
[4]: https://www.youtube.com/watch?v=NPcMS49V5hg
[5]: https://www.cs.ubc.ca/~lowe/papers/ijcv04.pdf

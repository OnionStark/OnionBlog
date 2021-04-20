---
title: 巨量多边形场景渲染——相关论文阅读笔记
date: 2020-09-20 10:50:59
categories: 计算机图形学
---

# [GigaVoxels:A Voxel-Based Rendering Pipeline For Efficient Exploration Of Large And Detailed Scenes](http://gigavoxels.inria.fr/Publications/2011/Cra11/CCrassinThesis_EN_Web.pdf)

文章作者针对巨量多边形场景的渲染提出了一种基于体素的渲染管线，其总体框架如图1所示所示。

![图1 GigaVoxels渲染框架](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200916150637384.png)

巨量多边形场景的渲染主要面临3个问题：

1. 巨量多边形的模型如何渲染
2. 模型数据如何存储
3. 模型数据如何加载到显存

针对这三个问题，文章提出了一种新的紧凑数据结构来表示模型数据，一种高效的渲染算法以及一种out-of-core的数据流传输机制和数据生成方案。

## 基于体素的几何表示

巨量多边形场景模型通常包含大量的几何数据信息，占用大量的空间，而内存和显存的空间是有限的。为了解决这一问题，文章提出了一种紧凑的数据存储结构。其中，将三角形面表示的原始模型转化为基于体素的几何表示。根据BRDF着色模型，体素主要包含模型的颜色、法线、透明度等信息。这一步主要通过GPU Producer模块来实现转化。

## 基于八叉树的体素MIP-map pyramid

将模型转化为基于体素的表示后，为了实现数据的紧凑存储以及运行渲染时的快速遍历，采用基于八叉树的MIP-map pyramid来组织体素数据，如图2所示。

![图2 体素数据的组织](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200916160221312.png)

## 体素数据的渲染

文章采用ray-casting的体渲染方式来对场景进行渲染。

![ray-casting](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200916161732082.png)

屏幕的每一个像素发射一条光线，对与光线相交的体素按特定的footprint进行采样，完成该像素最终的着色。

![光线遍历](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200916162537931.png)

上文提到的基于八叉树的存储可以有效的加速光线与体素的相交检测。同时，MIP-map pyramid也可以根据视点距离来提供一种类似于LOD的效果，不同的是，该操作可以通过硬件加速完成，而LOD需要美术人员进行额外操作，并且在渲染时过渡更加平滑，而LOD则存在一些popping artifacts的问题。

## out-of-core的数据管理

由于该框架的渲染是基于视点的，虽然模型整体的体素数据可能会很大，但是在渲染时可能只有一小部分的体素数据需要访问。因此，文章提出了一种out-of-core的数据管理机制。在渲染时，根据视点位置来动态加载需要的体素数据。

![image-20200916164057406](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200916164057406.png)

这样的设计使得该渲染框架可以拓展到巨量多边形的渲染。

## 性能

文章针对一些模型进行了实验，其实验环境为NVIDIA GTX480 GPU 和 Intel Core 2 Duo E6850 CPU @3GHz。效果可以在其[网站](http://gigavoxels.inrialpes.fr/videos.html)进行观看，该渲染框架的整体开销可以分为数据的动态加载和渲染两方面，对于数据加载方面的性能数据，如下图所示。

![image-20200916165124910](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200916165124910.png)

底部的统计图表示在4倍移动速度下的场景遍历时的性能数据。可以看到体素数据的IO是主要的性能瓶颈，特别是当移动速度变快的情况下，体素数据需要进行快速的加载和替换，需要进行的IO操作也更频繁。

针对渲染的性能，文章也与传统的光栅化渲染性能进行了比较，如下图所示。

![12M triangles场景渲染](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200916165837453.png)

![13.5M triangles 场景渲染](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200916165934020.png)

与光栅化渲染相比，该渲染框架的FPS平均提升了20-30倍。特别是当视点距离较远时，其渲染的加速效果更明显。

## 总结与想法

这篇文章提出了一种与传统光栅化不同的渲染方式。把几何表示的模型转化为体素表示的模型，并用稀疏八叉树结合MIP-map pyramid来组织，这样做一方面有效利用了存储空间，另一方面可以提供一种基于视点位置的动态体素数据加载，突破内存和显存空间的局限性，使得该渲染框架可以拓展到更巨量模型的渲染上。

与传统光栅化渲染相比，由于该渲染框架基于view-ray动态加载模型数据，因此可以避免vertex shader阶段大量的vertex transform开销。并且当三角形面小于一个像素时，光栅化存在Over-shading而造成额外开销，而ray-casting方式体渲染则只取决与遍历场景八叉树的开销，因此不会存在这样的问题。

但是该渲染框架也存在一些问题。首先它采用了与传统渲染框架完全不同的渲染方式，可能与当前的渲染方法集成起来存在问题。比如模型的一些参数可能需要重新考虑，并且把几何表示的模型转化为体素表示的模型本身也需要一定的开销。同时，该方法针对巨量多边形场景的渲染存在优势，但是当顶点数据较少的场景，硬件光栅化应该还是会比该渲染方法更高效。如果能将该渲染方法与光栅化渲染相结合，根据三角形面覆盖的像素数来动态决定渲染方式可能会更具通用性。该文章提出的解决方案仅适用于静态场景的渲染，动态物体的渲染比如角色动画等，文章并没有涉及。

# [三维模型网格简化技术研究](http://gb.oversea.cnki.net/KCMS/detail/detailall.aspx?filename=1019616787.nh&dbcode=CMFD&dbname=CMFDTEMP)

论文的思路如下：

1. 基于区域生长的思想对网格模型进行平面区域检测，并求出误差矩阵。
   * 输入模型的顶点信息和面信息，根据法向量夹角和距离等因素的阈值作为约束条件来判断不同的三角形网格是否属于同一个近平面。
   * 二次误差度量定义为顶点到其相关联平面的距离平方和，用来描述成本的概念。基于边折叠的网格简化过程，当折叠产生的新顶点成本最低时，当前顶点即为折叠后的最优顶点位置。

2. 对网格点的锐度进行计算，基于锐度对误差矩阵施加影响。
3. 基于Voronoi图提出局部网格优化方法，目的是使化简后的三角网格具有更高的正则度，使整体网格更加规整平均。
4. 基于纹理映射的原理，提出纹理信息保留因子，同样基于此对误差矩阵施加影响。

可以看出，该论文对网格模型的简化主要根据锐度来保留细节特征，对属于同一个近平面的三角形网格执行主要的化简，并进行网格的局部优化。模型简化的效率如下图所示。

![image-20200917091329563](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200917091329563.png)

![image-20200917091420823](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200917091420823.png)

该论文的缺点是算法复杂度很高，每次化简操作都要遍历所有顶点。

# [GPU-based Multiple-Choice Scheme for Mesh Simplification](https://www.researchgate.net/publication/325328975_GPU-based_Multiple-Choice_Scheme_for_Mesh_Simplification)

同样是基于二次误差度量的边折叠算法，其算法的思路是：并不总是求出二次误差最小的顶点，而是利用GPU的并行性提供一组成本较低的候选顶点，同时并行折叠无冲突的边，提高算法效率，同时一定程度保证网格化简的质量。实验环境为CPU i7-6700 3.4GHz 和GPU GTX970，效果如下图所示：

![image-20200917114111909](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200917114111909.png)

![image-20200917114134921](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200917114134921.png)

该算法较上一个算法而言利用GPU的并行性提升了执行速度，其化简质量也有一定保证，但其时间开销还是不满足实时性。

# [GPU-Accelerated Real-Time Mesh Simplification Using Parallel Half Edge Collapses](https://link.springer.com/chapter/10.1007/978-3-319-29817-7_10)

文章提出了一个基于视点的并行half edge collapse三角形网格简化算法。思路是先对模型顶点进行分类，将顶点分为待删除的点集R以及其对应的补集S(要保留的点集)，执行half edge collapse的过程就是将R中的顶点用其在S中相连的邻居顶点来代替。分类的标准同样是基于二次误差度量，但引入了视点距离的影响。算法迭代运行，更新顶点的二次误差值，并对剩余顶点进行重分类，不断简化网格。文章使用顶点的边界计算和检测来避免简化过程中可能会造成的拓扑不一致或网格折叠问题，同时预防并行执行的死锁情况。在实验环境为Geforce GTX 670 的条件下，算法的效果如下图所示：

![image-20200917171847248](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200917171847248.png)

![image-20200917171900828](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200917171900828.png)

算法对不同模型的性能如下图：

![image-20200917172014423](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200917172014423.png)

从实验结果可以看出，算法的时间开销基本满足实时性。但该算法的性能和效果依赖于拓扑结构，只适用于流形网格。

# [Progressive compression of arbitrary textured meshes](https://perso.liris.cnrs.fr/guillaume.lavoue/travaux/revue/CGF16.pdf)

渐进网格是三维网格简化的一种应用，其本质都是通过边折叠来简化模型，但渐进网格记录了边折叠过程，可以用于后续通过点分裂操作来还原原模型。该论文的改进点在于产生的简化网格模型保持了贴图信息，不会在贴图边界处产生裂缝。该算法还对模型的几何、连通性、以及uv坐标信息进行编码，使得简化后的模型具有高压缩比，有利于减少存储空间，加快传输速度。算法的效果如下图所示：

![image-20200918152553418](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200918152553418.png)

该算法还有一个优势是适用于任意多边形网格，且对于非流形网格也有效。在3.4GHz Intel Core i5的测试环境下，性能数据如下表所示：

![image-20200918153104252](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20200918153104252.png)
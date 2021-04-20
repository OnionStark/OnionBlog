---
title: 巨量面数场景渲染优化算法的设计与实现——网易实习项目
date: 2021-04-19 23:50:48
categories: 计算机图形学
img: ./medias/featureimages/1.jpg
summary: 网易实习项目总结
cover: true
tag: 
 - 计算机图形学
 - 渲染优化
 - 巨量多边形场景
---

# 巨量面数场景渲染优化算法的设计与实现

好久没更新博客，距从网易离职有一段时间，这段时间一直在玩怪猎起飞，果然划水它不香吗。不过也很怀念在网易奋斗的那些日子，主要是有钱拿哈哈。其实最近也干了一些正事，比如春招尝试投了下暑期实习，没想到被上海天美T2工作室给捞了，目前正准备去上海实习，希望一切顺利吖。不啰嗦了，下面就对网易的实习内容作一下总结。

## 1. Voxel-GPUDriven混合渲染管线介绍

### 1.1 GPU-Driven Rendering

在传统渲染管线中，场景管理一般在CPU中进行。SceneGraph位于CPU可访问的内存中，CPU进行可见性剔除等操作获取可见对象并将其数据传输到GPU中，等待GPU渲染完成。由于性能限制，在CPU上进行的剔除只能做到Object级别，且消耗颇高。随着硬件的发展，GPU的计算能力远超于CPU，因此把更多CPU端的计算放到GPU端，是性能优化的一个方向。相较于CPU端的剔除，GPU可以进行更细粒度、激进的剔除来替代CPU进行drawcall提交，从而解放CPU端的性能，同时也能获得更好的剔除效果。

### 1.2 Voxel Rendering

 目前主流的模型表示形式是三角网格，而自2009年《我的世界》获得空前的成功，体素（Voxel）进入开发者的眼球，成为另一种构建模型的可行方式。简单来说体素就是像素的三维版本。在二维中，用颜色的二维数组来表示一个影像。在三维中也可以用体素的三维数组来表示一个栅格化的三维空间，每个体素可以存储该空间位置的颜色、法线、材质等属性。

### 1.3 Voxel与GPU-Driven渲染的混合

在光栅化渲染管线中，每次drawcall提交的三角面数是影响渲染效率的因素之一。而实际场景中很多提交的三角面最后并不会渲染到画面中。因此，对于拥有千万级三角面的场景渲染，一个直观的解决方案就是进行可见性剔除，尽可能地只处理影响最终画面的三角面。而GPU-Driven可以提供Triangle Level的剔除，可以最大程度的减少需要处理的三角面数。

而光栅化渲染管线的另一个性能瓶颈是关于Micropolys的渲染，主要有以下几个原因：1.基于光栅化的原理，当一个三角形为像素大小时，它可能会错过像素中心，根本不会被光栅化，浪费了顶点着色期间所做的所有工作。![image-20201119114707689](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20201119114707689.png)

2.在Shading阶段，即使它被光栅化了，目前大多数图形硬件都是以2×2像素四边形为单位进行三角形渲染的（在 NVIDIA 硬件上，是以8个这样的四边形为一组的，即一组32个线程）。如果其中某些片元（fragment）没有覆盖到三角形，那么它们的输出将会被忽略掉，从而浪费了一些线程的计算量。对此，可以在GPU-Driven中进行Small Primitive Culling，即剔除掉小于特定像素阈值范围的三角形。然而，这一步属于比较激进的Culling，由于这些三角形的缺失，会造成最终画面中出现Holes。

对于Micropolys的渲染问题，UE5的方案是采用软光栅渲染。这里我采用另一种方案——结合Voxel Rendering的RayCasting方法，即对于Small Primitive Culling造成的Holes，在对应像素位置进行RayCasting。由于RayCasting方法的效率与RenderTarget的像素以及Sparse Voxel Octree的层级数有关，因此，当Holes像素数为少量时，其性能消耗理论上可以低于Small Primitive Culling所节省的消耗，从而既提升了性能，又保证最终渲染画面的质量。

## 2. Voxel-GPUDriven混合渲染管线的实现

### 2.1 数据结构的定义

由于混合管线涉及GPU-Driven与Voxel RayCasting两种不同的渲染方式，所需要的数据结构并不相同。针对GPU-Driven方法，Culling需要将模型按每64个三角面组成的Cluster为单位。而RayCasting方法需要将模型转化为Sparse Voxel Octree结构。

#### 2.1.1 Cluster

Cluster结构的定义如下：

```c++
struct Cluster
{
    float coneAngleCosine;
    XMFLOAT3 aabbMin, aabbMax;
    bool valid;
    XMFLOAT3 coneCenter, coneAxis;
};
```

coneAngleCosine、valid、coneCenter、coneAxis变量为Cluster结构定义了一个圆锥体，用于背面剔除。

aabbMin、aabbMax表示Cluster中三角面的AABB。

由于每个Cluster固定为64个三角面，因此在Culling过程中，可以使用Cluster结构体数组的下标来对模型的三角面进行索引。因此不需要额外的变量来记录Cluster所包括的三角面索引。

单个Cluster总占用空间为53bytes。

#### 2.1.2 Sparse Voxel Octree

当把模型体素化渲染时，体素数据的分辨率决定了其渲染的细致程度。然而，体积数据是以立方级数增长的，所需的存储容量很高。例如一个 256^3^大小的体素空间含一千六百多万个体素（16M个）；而1024^3^就会增长至10亿个体素（1G 个）。即使每个体素只占1字节，也需要大量储存空间。因此，可以使用Sparse Voxel Octree结构去压缩这些原始体素数据。

Sparse Voxel Octree的结点结构定义如下：

![image-20210203152539706](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210203152539706.png)

可以看到，Sparse Voxel Octree采用分离式的设计，分为Octree与Attachment两部分。

Octree存储SVO的结构信息，每个结点占用4bytes。第一位V用来标识当前结点下是否存在叶结点，第二位L用来标识当前结点是否为叶结点。当该结点为叶结点时，后30位用来存储当前voxel对应的Attachment结点的索引；当该节点为非叶结点时，后30位用来存储当前结点的第一个子结点的索引。

Attachment用来存储SVO的数据，每个结点占用8bytes。当前Attachment结点包含法线与颜色数据，每种数据压缩为4bytes大小。

这样设计的好处是解耦SVO的结构与数据信息：当对SVO进行遍历时，只需要对Octree结点进行遍历，最后根据遍历的结果，从Attachment结点获取数据；另一个好处是便于拓展，当前Attachment仅存储了模型的法线与颜色信息，后续若需要增加额外的信息，只需改动Attachment结点的结构，而无需对Octree结构进行修改。

### 2.2 渲染流程总览

Voxel-GPUDriven混合渲染的流程如下图所示:

![image-20210125100257095](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210125100257095.png)

* Prepare Pass用于初次导入模型时，对模型数据进行处理生成Cluster与SVO结构。该过程为离线构建，耗时较长。但构建的结果可以保存为二进制文件用于下次导入。

* MeshCulling Pass输入模型的Clusters，以Cluster为单位执行Frustum Culling， Backface Culling和Small Primitive Culling。输出需要绘制的三角面索引范围。

* Rasterization Draw Pass根据MeshCullingPass的输出，按照传统光栅化管线渲染未被裁剪的三角面，同时标记出Small Primitive Culling造成的Holes像素的位置。

* RayCasting Pass在Holes像素位置执行RayCasting，完成最终渲染。

接下来对各个渲染阶段的细节作介绍。

### 2.3 Prepare Pass

Prepare Pass主要对初次导入的模型数据构建Cluster与SVO结构。

#### 2.3.1 Cluster的构建

输入模型数据时，以64个三角面为单位构建Cluster。需要考虑的是怎样选取64个三角面。一种选择是直接按照模型输入的Indices列表，每64个三角面为一个Cluster。然而，Cluster的划分会影响接下来MeshCulling Pass的剔除效果。因此，这里采用的方法是以**三角面的位置**为主要因素，尽量将相邻区域内的三角面归为同一个Cluster。具体的划分过程如下：将模型按照其BoundingBox划分为多个Cube，如下图所示。

![image-20210125105427821](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210125105427821.png)

每个三角面按照其重心坐标，归属于某个Cube。构建时采用贪心算法，遍历所有Cube，将同一个Cube或其相邻Cube中的64个三角面组成一个Cluster。最后按照Cluster的顺序，重新排序模型的Indices列表。

对于每个Cluster，根据其包含的所有三角面计算AABB和Cone。其中，Cone用于MeshCulling Pass中的背面剔除，构建过程如下：以Cluster的AABB的中点为起点，将Cluster中每个三角面的负法向量相加，得到ConeAxis；计算ConeAxis与所有三角面的交点，取距离AABB中点最远的交点为ConeCenter，如下图所示。

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210125113804957.png" alt="image-20210125113804957" style="zoom:67%;" />

同时保存ConeAxis与所有三角面的负法向量夹角的最大值，作为ConeAngle，得到如下Cone，

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210125114055677.png" alt="image-20210125114055677" style="zoom: 67%;" />

如图所示，当Camera位于灰色区域的Cone范围内，即位于当前Cluster中所有三角面的背面位置，因此当前Cluster可以被安全剔除。

综上所述，Cone的构建与Cluster内三角面的法向量有关，且并不是所有Cluster中的三角面都可以构成Cone，例如当Cluster存在两个相对的三角面，此时不存在可以安全剔除的区域。因此，引入一个valid变量来标识当前Cluster是否存在有效的Cone结构。

#### 2.3.2 SVO的构建

SVO结构用于voxel的渲染，其过程主要包括模型数据的体素化、SVO的构建两个步骤。

##### 2.3.2.1 体素化

体素化过程借助了渲染流程的光栅化处理，将整个体素化的过程并行化，速度极快，具体步骤如下：

首先计算网格模型的AABB包围盒，

<img src="https://cdn.jsdelivr.net/gh/ZeusYang/CDN-for-yangwc.com@1.1.13/blog/Voxelization/2.png" alt="img" style="zoom: 50%;" />

获取了模型的包围盒之后，就需要根据这个包围盒设置观察角度和投影平面，这关系到后面的体素化结果。同时为了保证正确地体素化模型，采用的投影方式是正交投影。首先要选择一个观察方向和投影平面，AABB包围盒有六个面，其中前和后、上和下、左和右的投影结果是一样的，因此实际的选择只有三个平面，分别是前、上、右（或者后、下、左），如下图所示。

![image-20210125142532581](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210125142532581.png)

设置好投影矩阵和视图矩阵之后，需要特别注意的是，应该关闭深度测试和背面剔除，确保所有的三角形不被剔除，从而使得全部的三角形都被处理，最后得到正确的体素化结果。

如上图所示，每个三角形面片在不同包围盒投影面上的投影结果不同，根据三角形的朝向不同，投影到平面上的三角形大小也各不相同。因此我们需要选取一个投影方向，在该投影方向上三角形的投影面积最大，这样就能够确保所有的三角形面片被充分地体素化。这一步可以在几何着色器中完成，这里有一个技巧，直观上说是根据三角形的投影面积来渲染采用哪个投影相机，实际上没有必要真正地去计算三角形的投影面积，**可以直接根据当前三角形的世界空间法线朝向来决定投影方向**。举个例子，当法线向量的x分量比其余两个分量大时，则当前的三角形肯定投影到x轴方向的投影平面上的面积更大。更深入的理解：设法线向量为n=(nx,ny,nz)，将法线向量n与(1,0,0)、(0,1,0)、(0,0,1)分别做点乘，结果为nx、ny、nz，而法线向量分别与该三个基向量点乘的意义为法线向量在x、y、z轴上的投影值，该值越大则三角形投影到该平面上的面积也越大。

然而，经过上述处理，会造成另外一个问题，由于模型的每个三角形都是各自根据在每个平面上的投影面积来选择投影相机，这意味着两个相邻的三角形片面可能选取了不同投影相机，使得三角形面片之间因为体素化投影平面的不同而产生过渡问题，从而出现孔洞，即有些部分没有被体素化到。问题产生的根源在于光栅化处理，一个像素是否作为当前图元的光栅片元，是通过判断当前图元是否覆盖了该像素中心来完成的。对于那些没有覆盖像素中心的片元，不作为该图元的光栅片元送入片元着色器做进一步的处理，因而模型的一些部分可能会被丢失，从而造成孔洞。为了解决这个问题，在几何着色器中实现一种被称为**保守光栅化**（Conservative Rasterization）的算法。

通常的硬件光栅化，都是默认只取那些中心被图元覆盖的像素单元。而保守光栅化则将所有被图元覆盖（无论是否覆盖到像素单元的中心点）的像素单元都作为光栅化的片元，从而确保图元覆盖的所有区域都被光栅化，故名思意，这就是“保守”一词的由来。如下图所示，

![image-20210125151644129](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210125151644129.png)

通常情况下硬件默认的光栅片元是绿色部分，边缘红色部分的片元没有被光栅化，导致的体素化结果出现孔洞。为了修补体素化的孔洞，必须使得被图元哪怕一点点覆盖到的像素（就是图中的红色部分）都作为当前图元的光栅化结果，这个过程就是保守光栅化算法。

实现保守光栅化一个简单直观的思路就是手动扩充三角形图元面片。如上图所示，里面的三角形是最初要光栅化的三角形，为了使得边缘红色的像素也包含进来，扩张最初的三角形得到外面的那个三角形，这个三角形比原来的三角形稍微大一点，此时若将该扩大的三角形送入硬件默认的光栅化单元进行处理，则红色像素也被当作光栅片元，从而达到了保守光栅化的目的。

扩大三角形和剔除像素整个过程都是在裁剪空间中进行的，也就是经过摄像机空间变换和投影变换之后。故而三角形的包围盒只需二维即可，然后需要适当地扩大一点，以免剔除红色的像素片元。

```glsl
float4 calcAABB(float4 pos[3], float2 pixelDiagonal)
{
	float4 aabb;
	aabb.xy = min(pos[2].xy, min(pos[1].xy, pos[0].xy));
	aabb.zw = max(pos[2].xy, max(pos[1].xy, pos[0].xy));

	aabb.xy -= pixelDiagonal;
	aabb.zw += pixelDiagonal;
	return aabb;
}
```

接下来对于给定的三角形的三个顶点，要适当地扩大三角形。总体的思路就是：**首先计算三角形的三条边与原点构成的齐次空间的平面，然后适当挪动这三个平面，接着就计算偏移后的这三个齐次平面的交线，最后计算三条交线与三角形平面的交点，从而得到扩大后的三角形的三个顶点。**整个计算过程都是在裁剪空间中进行的，所以忽略顶点的z分量，但是上面又提到了齐次平面一词，因此采用一个齐次平面来描述三角形边的线段。所谓齐次平面，就是把顶点的齐次分量w和x、y分量合并一起来表示一条线段，直观来看，这就是一个齐次空间的平面，但实际上是一段二维空间的直线。如下所示：
$$
Ax_c+By_c+Cw_c=0\tag{1}
$$
公式(1)就是一个齐次空间的过原点的平面方程，但是它实际上是一个二维空间的直线方程。这是因为采用的都是正交投影，正交投影并没有透视除法之类的处理，因为正交投影都是线性变换，故而wc=1，所以公式(1)表示的过原点的齐次空间的平面方程就是如下所示的二维直线方程：
$$
Ax_c+By_c+C=0\tag{2}
$$
之所以采用齐次空间的平面方程，是为了方便计算。首先根据三角形的三条边计算三个齐次空间的平面，已知该齐次空间的平面过原点，平面方程的(A,B,C)就是该平面的法线向量，因此直接做叉乘计算可得平面的法线，如下所示：

```glsl
	float3 edgePlanes[3];
	edgePlanes[0] = cross(pos[0].xyw - pos[2].xyw, pos[2].xyw);
	edgePlanes[1] = cross(pos[1].xyw - pos[0].xyw, pos[0].xyw);
	edgePlanes[2] = cross(pos[2].xyw - pos[1].xyw, pos[1].xyw);
```

然后对这三个平面分别进行偏移。直观上来说，分别令三角形的三条边在其法线的方向上挪一段距离，这个距离由像素单元格的大小（即下面的halfPixel）在法线方向的投影决定，如下所示：

```glsl
	edgePlanes[0].z -= dot(halfpixel, abs(edgePlanes[0].xy));
	edgePlanes[1].z -= dot(halfpixel, abs(edgePlanes[1].xy));
	edgePlanes[2].z -= dot(halfpixel, abs(edgePlanes[2].xy));
```

接着计算三个齐次平面的交线向量，这个不难理解，两个平面的交线必然垂直于这两个平面的法线向量，因而交线向量可由这两个平面的法线向量做叉乘得到：

```glsl
	float3 intersection[3];
	intersection[0] = cross(edgePlanes[0], edgePlanes[1]);
	intersection[1] = cross(edgePlanes[1], edgePlanes[2]);
	intersection[2] = cross(edgePlanes[2], edgePlanes[0]);
	intersection[0] /= intersection[0].z;
	intersection[1] /= intersection[1].z;
	intersection[2] /= intersection[2].z;
```

最后根据上面的三条射线向量与初始三角形所在的平面求交点，从而得到最终扩大后的三角形的三个顶点。由于是正交投影，所以上面求到的三条射线向量的x分量和y分量就是扩大后三角形顶点的x分量和y分量，即交点的x、y已知，需要求z值。一个三维平面方程如下所示，从直观的几何意义上来说，(A,B,C)就是平面的法线向量，D就是原点到平面的直线距离。
$$
Ax+By+Cz+D=0\tag{3}
$$
已知初始三角形的三个点，可以求出它的法线向量，然后原点到平面的直线距离就等于平面上的点在法线向量方向上的投影长度，这里要特别注意符号，具体看下面的代码：

```glsl
	float4 trianglePlane;
	trianglePlane.xyz = normalize(cross(pos[1].xyz - pos[0].xyz, pos[2].xyz - pos[0].xyz));
	trianglePlane.w = -dot(pos[0].xyz, trianglePlane.xyz);
```

已知交点的x和y，代入平面方程(3)求得z值。
$$
z=-\frac{Ax+By+D}{C}\tag{4}
$$

```glsl
	float z[3];
	z[0] = -(intersection[0].x * trianglePlane.x + intersection[0].y * trianglePlane.y + trianglePlane.w) / trianglePlane.z;
	z[1] = -(intersection[1].x * trianglePlane.x + intersection[1].y * trianglePlane.y + trianglePlane.w) / trianglePlane.z;
	z[2] = -(intersection[2].x * trianglePlane.x + intersection[2].y * trianglePlane.y + trianglePlane.w) / trianglePlane.z;
	pos[0].xyz = float3(intersection[0].xy, z[0]);
	pos[1].xyz = float3(intersection[1].xy, z[1]);
	pos[2].xyz = float3(intersection[2].xy, z[2]);
```

最终，求得扩大后的三角形的三个顶点，还需要对三个顶点做逆视图投影变换，将裁剪空间的顶点变换到世界空间，得到扩大后的三角形的世界坐标，因为最终目的是根据世界空间坐标做体素化的处理。与此同时，还将在裁剪空间的扩大三角形的顶点传到片元着色器，因为要剔除不必要的片元。

对于最后片元着色器中通过的片元，计算它在3DTexture中的位置，将其位置、颜色、法线信息写入到Fragment Buffer中，用于构建Sparse Voxel Octree。Fragment Buffer的结构如下：

![image-20210203170711060](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210203170711060.png)

单个Fragment Data长度为12btyes，用于记录voxel的位置、颜色和法线信息。

##### 2.3.2.2 压缩体素数据

上文提到构建Sparse Voxel Octree的目的是压缩原始体素数据，最大化节省内存空间。上一节体素化后，片元着色器输出了由每个体素信息组成的Fragment Buffer。因此仅对实际存在数据的Voxel构建SVO，从而过滤无数据存在的Voxel。

该过程在Compute Shader中完成，利用GPU的并行性。每个GPU线程根据其ID从上一步的Fragment Buffer中提取一个片元数据来处理，根据其片元位置，从根结点出发，逐层调用一次Dispatch进行构建，根据FragmentData的voxelPos字段，在Octree结构的相应位置填入其片元的颜色和法线信息。这里遇到了个问题：由于上一步体素化后，可能存在相同位置的体素数据。正常情况下，对于写入到同一个体素位置的信息，需要将它们取平均值。然而不同GPU线程间的处理具有独立性，后完成的线程会覆盖之前完成的线程的处理结果。因此这里需要用到原子操作。而DX对于Shader中的原子操作接口只支持uint或int两种数据类型。这里采用的解决方案是将颜色或法线信息封装为单个uint类型，需要对体素数据进行均值化时将SVO中的数据进行拆封。以法线信息为例，对应的封装代码如下：

```glsl
uint PackNormalFloat4(float4 val)
{
	val.xyz = (val.xyz + 1.0f) / 2;
	uint r = round(clamp(val.r, 0.0, 1.0) * 255.0);
	uint g = round(clamp(val.g, 0.0, 1.0) * 255.0);
	uint b = round(clamp(val.b, 0.0, 1.0) * 255.0);
	uint a = round(val.a);
	return (
		(uint(a) & 0x000000FF) << 24U |
		(uint(b) & 0x000000FF) << 16U |
		(uint(g) & 0x000000FF) << 8U |
		(uint(r) & 0x000000FF));
}
```

在构建SVO过程中，利用InterlockedCompareExchange接口，完成体素信息的原子性写入。具体代码如下所示：

```glsl
	uint packed_normal = PackNormalFloat4(float4(fragmentData[DispatchThreadID.x].normal.xyz, 1.0f));
	uint previousStoredValue = 0;
	uint currentStoredValue;
	float4 currValue;
	float3 average;
	uint count;
	InterlockedCompareExchange(Attachment[idx].normal, previousStoredValue, packed_normal, currentStoredValue);
	while(currentStoredValue != previousStoredValue)
	{
		previousStoredValue = currentStoredValue;
		currValue = UnpackNormalFloat4(previousStoredValue);
		average = currValue.rgb;
		count = currValue.a;
		average = (average * count + fragmentData[DispatchThreadID.x].normal.xyz) / (count + 1);
		packed_normal = PackNormalFloat4(float4(average, (count + 1)));
		InterlockedCompareExchange(Attachment[idx].normal, previousStoredValue, packed_normal, currentStoredValue);
	}
```

大致思路是线程不断轮询待填入位置的值，保存上一次查询的待填入位置的值，在此基础上计算该线程需要写入的预期值，写入前比较上次查询的值与当前待写入位置的值，若没有变化则表示在此期间没有其他线程写入到该位置，可以执行写入操作；否则重新计算预期值，并不断查询，直到满足写入条件。这样就能确保SVO中存储的是体素数据均值化后的结果，避免后写入的体素数据覆盖之前的体素数据。

##### 2.3.2.3 mipmap体素数据

上一步得到的SVO中，仅有叶结点存储了Voxel的数据。而在之后的RayCasting中，需要根据距离的远近来对体素的渲染实现一个LOD的效果，这要求不需要遍历到SVO的叶结点就能拿到体素的数据。因此需要填充非叶子结点的Attachment结构数据，即对上一步生成的SVO进行mipmap处理。

该过程在Compute Shader中完成。每个GPU线程处理一个叶结点，自底向上逐层查找其父结点，对其父结点对应位置的Attachment数据进行均值化处理。

最后得到的SVO中，非叶结点的Attachment数据存储了其子结点Attachment数据均值化的结果，而叶结点的Attachment数据存储了原始的Voxel数据。

### 2.4 MeshCulling Pass

MeshCulling Pass是在Compute Shader中完成的。其输入是Prepare Pass中构建的Cluster数组，输出是Rasterization Draw Pass中需要用到的DrawIndexedArgument结构体，其定义如下图所示，该结构体需要的变量根据未被剔除的Cluster来填充。

```c++
struct DrawIndexedArguments
{
	DrawIndexedArguments();
	DrawIndexedArguments(UINT indexCountPerInstance, UINT instanceCount,
		UINT startIndexLocation, INT baseVertexLocation, UINT startInstanceLocation);

	UINT m_IndexCountPerInstance;
	UINT m_InstanceCount;
	UINT m_StartIndexLocation;
	INT m_BaseVertexLocation;
	UINT m_StartInstanceLocation;
};
```

MeshCulling Pass中主要包括Frustum Culling、Backface Culling和Small Primitive Culling三部分。

#### 2.4.1 Frustum Culling

视锥体剔除的基本思想：判断对象是否在相机视锥体内（相交也算），在则不剔除，不在则剔除。判断方法具体如下：Compute Shader中传入当前视锥体的六个面，每个GPU线程处理一个Cluster，根据Cluster的AABB，对于每个视锥体的平面，测试AABB和平面的关系，如果在一次测试中包围盒位于背面，说明包围盒位于视锥体外，就可以终止测试。如果包围盒位于所有平面正面，说明包围盒位于视锥体中，其余情况说明包围盒和视锥体的平面相交。

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210125191013803.png" alt="image-20210125191013803" style="zoom:50%;" />

其对应的代码如下：

```glsl
bool TestAABBAgainstFrustum(float4 frustumPlanes[6], AABB aabb)
{
	bool insideOrOverlap = TestAABBAgainstPlane(frustumPlanes[0], aabb) &&
		TestAABBAgainstPlane(frustumPlanes[1], aabb) &&
		TestAABBAgainstPlane(frustumPlanes[2], aabb) &&
		TestAABBAgainstPlane(frustumPlanes[3], aabb) &&
		TestAABBAgainstPlane(frustumPlanes[4], aabb) &&
		TestAABBAgainstPlane(frustumPlanes[5], aabb);

	return insideOrOverlap;
}
```

#### 2.4.2 Backface Culling

在Cluster的构建一节中，根据Cluster中包含的三角面的法向量创建了Cone结构。在Backface Culling中，利用Cone结构判断当前视点位置是否位于该Cluster的安全剔除范围内。具体代码如下：

```glsl
if (g_NodeBuffer[index].valid && TestBackface(camera_position, coneCenter, g_NodeBuffer[index].coneAxis, g_NodeBuffer[index].coneAngleCosine))
{
		InterlockedAdd(BackfaceCullNum[0], 1);
		continue;
}

bool TestBackface(float3 eye, float3 coneCenter, float3 coneAxis, float coneAngleCosine)
{
	float3 testVec = normalize(eye - coneCenter);
	if (dot(testVec, coneAxis) > coneAngleCosine)
	{
		return true;
	}
	return false;
}
```

先检查Cluster的valid位，判断当前是否存在有效的Cone。若存在则调用TestBackface函数进行测试。函数内部通过比较coneCenter到视点位置的向量与coneAxis的点积与coneAngleCosine，若小于coneAngleCosine，表示当前视点处于Cluster内所有三角面的背面，可以将该Cluster剔除；否则，无法安全剔除该Cluster。

#### 2.4.3 Small Primitive Culling

Small Primitive Culling的目的是剔除最终渲染后覆盖范围小于某个像素阈值的Cluster。

其原理是通过计算Cluster的AABB的8个顶点在屏幕空间的投影，求出其在屏幕空间的AABB。最后根据屏幕空间AABB在x轴或y轴的跨度来判断是否剔除，其代码如下所示：

```glsl
bool TestSmallPrimitive(float4x4 mv,float4x4 project, float4x4 viewport, AABB box, uint clipSize, out float4 pmin, out float4 pmax)
{
	float4 aabbMin = float4(FLT_MAX, FLT_MAX, FLT_MAX, 1.0f);
	float4 aabbMax = float4(FLT_MIN, FLT_MIN, FLT_MIN, 1.0f);
	float4 coner[8];
	float4x4 mvp = mul(project, mv);
	for (int i = 0; i < 8; ++i)
	{
		coner[i] = float4(box.radius, 1.0f) * g_BoxOffset[i] + float4(box.center, 1.0f);
		coner[i] = mul(mvp, coner[i]);
		coner[i] = coner[i].xyzw / coner[i].w;
		coner[i] = mul(viewport, coner[i]);
		aabbMin = min(aabbMin, coner[i]);
		aabbMax = max(aabbMax, coner[i]);
	}
	pmin = aabbMin;
	pmax = aabbMax;
	return ((aabbMax.x - aabbMin.x) < clipSize) || ((aabbMax.y - aabbMin.y) < clipSize);
}
```

上述代码中，clipSize变量可由用户输入进行调整，当Cluster的屏幕空间AABB在x轴或y轴小于clipSize规定的阈值时，该Cluster会被剔除。

当clipSize取值过大时，会对最终渲染画面造成Holes，如下图所示：

![image-20210126104546876](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210126104546876.png)

由于Cluster被剔除掉而产生的Holes，需要标记其像素位置，用于后续Voxel渲染时在对应像素执行RayCasting操作。因此，在这里采用Stencil Texture来记录Holes像素位置，其代码如下所示：

```glsl
if (TestSmallPrimitive(model_view, project, viewport, aabb, g_clipSize, pixelMin, pixelMax))
{
	for (int j = (int)pixelMin.y; j < (int)pixelMax.y+1; j++)
	{
		for (int i = (int)pixelMin.x; i < (int)pixelMax.x + 1; i++)
		{
			StencilTexture[int2(i, j)] = 1u;
		}
	}
	InterlockedAdd(SmallCullNum[0], 1);
	continue;
}
```

当Cluster由于TestSmallPrimitive函数被剔除时，根据其屏幕空间中AABB的位置，在Stencil Texture中标记受到影响的像素。对于上图的剔除结果，对应的Stencil Texture标记结果如下:

![image-20210126104646943](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210126104646943.png)

图中红色位置对应上图中Holes像素的位置，对比可以发现，该方法所得到的Stencil Texture是一个保守的结果。

### 2.5 RasterizationDraw Pass

Rasterization Draw Pass根据剔除结果，将剩余的Cluster进行光栅化渲染。在MeshCulling Pass中，执行Frustum Culling、Backface Culling、Small Primitive Culling后，余下的Cluster将根据其索引，填充DrawIndexedArgument结构体。该结构体的对象作为Rasterization Draw Pass中，ExecuteIndirect函数的输入参数。

ExecuteIndirect是GPU-Driven管线中的关键API。通过这一调用，可以直接传入GPU中的Buffer作为DrawCall调用的参数，这样就避免了GPU回传数据到CPU后再执行DrawCall调用所造成的额外开销，从而获得性能的提升。

### 2.6 RayCasting Pass

Voxel的渲染方法参考了NVIDIA的论文[Efficient Sparse Voxel Octrees](https://research.nvidia.com/sites/default/files/pubs/2010-02_Efficient-Sparse-Voxel/laine2010tr1_paper.pdf) 的实现。RayCasting过程在Pixel Shader中完成。首先明确一点，GPU上没法做递归，因此这里引入了Stack的概念。

RayCasting实际上是对Sparse Voxel Octree遍历求交的过程，从SVO的根结点开始，向下每遍历一层结点，将信息存储到一个Stack中，这样模拟递归的效果。当一条射线穿过3D空间的时候，先从SVO的根结点进行初始化，找到最早进入的是8个子结点空间里的哪一个。具体方法是根据入射点相对当前结点空间的中点的位置判断子结点的index。根据上述提到的SVO结构的定义，Octree的非叶结点存储了其第一个子结点的索引，因此可以从根结点开始向下递归直至找到与射线相交的叶结点。

在GPU中可以通过循环的方法实现SVO的递归过程。在射线对SVO进行遍历求交的过程大致需要考虑3种情况:

* 第一种是在一个stack内从一个子空间前进到另一个子空间，叫做advance。
* 第二种是在前进到某个子空间的时候发现该子空间还存在子结点，所以我们进入这个子stack，叫做push。
* 第三种是从子空间向前进的时候发现已经走出这个stack的范围了，于是跳回父stack再向前进，叫做pop。

下图通过一个2维空间遍历的过程来对上述算法进行举例说明：

![image-20210203194749077](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210203194749077.png)

如图所示，射线先从根结点1出发，执行push操作两次先后进入结点2、结点3的子空间，然后到达叶结点4；随后执行两次advance操作依次遍历结点5和结点6；当遍历完结点6后，射线已经遍历完结点3的子空间，依次执行pop操作到达结点7；依次类推，射线按照图中数字的顺序遍历SVO直到走出根节点的空间。

循环过程的伪代码如下：

```pseudocode
(tmin, tmax) ← (0,1)
t' ← project cube(root, ray)
t ← intersect(t, t')
h ← t'max
parent ← root
idx ← select child(root, ray, tmin)
(pos, scale) ← child cube(root, idx)
while not terminated do
	tc ← project cube(pos, scale, ray)
	if voxel exists and tmin ≤ tmax then
		if voxel is small enough then return tmin
		tv ← intersect(tc, t)
		if tvmin ≤tvmax then
			if voxel is a leaf then return tvmin
			if tcmax < h then stack[scale] ← （parent, tmax)
			h ← tcmax
			parent ← find child descriptor(parent, idx)
			idx ← select child(pos, scale, ray, tvmin)
			t ← tv
			(pos, scale) ← child cube(pos, scale, idx)
			continue
		end if
	end if
	oldpos ← pos
	(pos, idx) ← step along ray(pos, scale, ray)
	tmin ← tcmax
	if idx update disagrees with ray then
		scale ← highest differing bit(pos, oldpos)
		if scale ≥ smax then return miss
		(parent, tmax) ← stack[scale]
		pos ← round position(pos, scale)
		idx ← extract child slot index(pos, scale)
		h ← 0
	end if
end while	
```

上述伪代码包括了初始化阶段以及一段射线遍历每个单独体素的循环：

* 算法1-7行用于初始化，tmin和tmax表示ray的有效跨度，project cube(root, ray)求出根结点与ray两个相交位置的值t'，intersect(t, t')表示取t和t'的交集求出当前相交结点位置的tmin与tmax值。h被初始化为当前相交节点的tmax值，用于对当前结点空间stack的边界进行定义。对于第一个相交的子结点，可以表示为parent（初始化为root结点）以及对应的子结点idx。idx可以通过比较tmin与当前结点中心位置的tx，ty和tz来初始化。最后，根据相交的子结点，更新pos与scale值。

* 第8-35行用循环来实现ray遍历Octree求交的过程。

  * 当子结点存在时，10-12行用于更新当前遍历到的结点在ray上的跨度，其中，11行判断当前结点相对于视点位置的大小，当结点足够小时，停止向下遍历，直接返回当前结点的数据实现LOD效果。若跨度有效，则13-22行用于向其子结点递归，对应push操作，向当前stack层保存当前结点的信息，同时根据子结点更新对应的变量。

  * 若当前子结点不存在，24-26行对应advance操作，继续遍历当前结点的下一个子结点。
  * 27-34行对应pop操作，即在advance操作执行后，若idx更新后的值与ray方向不符，表示ray已走出当前结点，因此需要返回上层stack，通过读取上层stack的信息，更新对应的变量。

对所有的像素都进行上述操作开销很大，因此这里仅对Holes像素位置RayCasting。这里需要用到Small Primitive Culling时生成的Stencil Texture，每个像素先检查Stencil Texture对应位置的值，仅为1时执行RayCasting操作，否则discard该像素。对于上述Stencil Texture，RayCasting Pass的渲染结果如下图：

![image-20210126164504273](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210126164504273.png)

将渲染结果与光栅化渲染的结果混合，效果如下所示：

![image-20210126164759128](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210126164759128.png)

## 3. 渲染结果与性能分析

### 3.1 实验环境及配置

接下来对Voxel-GPUDriven混合渲染管线进行测试，实验环境及硬件配置如下所示：

|                            |                        |                        |        |              |
| -------------------------- | ---------------------- | ---------------------- | ------ | ------------ |
| CPU                        | GPU                    | DirectX feature level  | Memory | Video memory |
| Intel Core i5-4590 @3.3GHz | NVIDIA GeForce GTX 750 | D3D_FEATURE_LEVEL_11_0 | 16GB   | 1GB          |
|                            |                        |                        |        |              |

### 3.2 模型信息

测试过程中用到的模型信息如下表所示：

|          |                                                              |           |           |
| -------- | ------------------------------------------------------------ | --------- | --------- |
| 模型名称 | 缩略图                                                       | 顶点数    | 三角面数  |
| bunny    | <img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210204141446679.png" alt="image-20210204141446679" style="zoom: 33%;" /> | 72,027    | 144,046   |
| city     | <img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210204150446299.png" alt="image-20210204150446299" style="zoom:33%;" /> | 605,612   | 1,209,773 |
| mountain | <img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210204164537747.png" alt="image-20210204164537747" style="zoom:33%;" /> | 2,420,867 | 4,841,488 |
|          |                                                              |           |           |

### 3.3 模型空间占用情况分析

Voxel-GPUDriven混合管线的渲染需要对原始模型生成cluster和svo数据，在voxel分辨率为128x128x128的情况下，针对上述三个模型，生成的额外数据和空间占用情况如下所示：

|          |                    |                     |
| -------- | ------------------ | ------------------- |
| 模型名称 | Cluster数/占用空间 | SVO结点数/占用空间  |
| bunny    | 2251 / 0.114M      | 2,060,912 / 23.585M |
| city     | 18,903 / 0.955M    | 1,593,872 / 18.240M |
| mountain | 75,649 / 3.824M    | 738,064 / 8.446M    |
|          |                    |                     |

Clusters的空间占用与模型的三角面数有关；而SVO的空间占用与Voxel的分辨率以及模型占有的Voxel数量有关，与模型的三角面数无关。

### 3.4 剔除效果分析

在MeshCulling阶段，涉及的剔除有视锥体剔除、背面剔除和小三角面剔除，下面以bunny模型为例，分别对三种剔除效果正确性进行验证。

#### 3.4.1 视锥体剔除正确性验证

在GPU上实现的视锥体剔除是以Cluster为单位的剔除，在仅开启视锥体剔除的情况下，当模型整体都位于视锥体范围内时，视锥体剔除的Cluster数为0：

![image-20210205111257450](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210205111257450.png)

当模型部分位于视锥体范围内时，如下图所示：

![image-20210205111204108](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210205111204108.png)

此时视锥体剔除了1029个Cluster，剩余78,190个三角面，剔除率为45.7%。

当模型完全位于视锥体之外时，剔除效果如下图所示：

![image-20210205111638495](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210205111638495.png)

此时模型的所有三角形都不提交至渲染管线。

#### 3.4.2 背面剔除正确性验证

背面剔除直观的效果是把模型位于视角背面的三角面剔除。当仅开启背面剔除时，按如下图的视角，背面剔除的效果如下所示：

![image-20210205112340110](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210205112340110.png)

其背面的视角如下图：

![image-20210205112535489](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210205112535489.png)

可以看到背面的大多数三角面已经被剔除。

#### 3.4.3 小三角面剔除正确性验证

当三角面在屏幕空间的投影小于预定阈值时会被剔除，而在透视投影中投影面积和三角面与视点的距离有关：当三角面离视点距离越远，其投影面积越小。因此，当仅开启小三角面剔除时，保持给定的阈值不变，随着视角的拉远，被剔除的三角面应越多。以下图所示的视角为初始情况：

![image-20210205113526809](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210205113526809.png)

此时小三角面剔除数为0，当视角拉远后，剔除效果如下图所示：

![image-20210205113711900](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210205113711900.png)

![image-20210205113730848](https://gitee.com/wei_yang_song/image-resources/raw/master/img/image-20210205113730848.png)

由上图可见，当视角拉远时，越来越多的三角面的投影面积小于特定阈值，但也给最后的画面造成了许多holes，影响渲染的正确性。因此小三角面剔除需要与RayCasting相结合来保证渲染效果。

#### 3.4.4 剔除效果综合分析

上面几节分别对各个剔除效果的正确性进行验证。在实际使用中，三种剔除同时开启，在各种不同情况下，尽可能多的剔除三角面，从而获得性能的提升。

### 3.5 渲染性能分析

对上述三个模型在不同视角下渲染，测试其渲染性能，测试结果如下：

![image-20210420000316987](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2021042000-03-24-dbb8b7d2a132c2ea1e453f6afbcbe480-image-20210420000316987-9de12d.png)

![image-20210420000406242](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2021042000-04-06-26013d814b2f435532b18231cf340c8c-image-20210420000406242-5eecd4.png)

![image-20210420000424877](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2021042000-04-24-b888935bdc77352f483420c75433e75d-image-20210420000424877-4d4dba.png)

bunny模型为小模型，面片数为10w级别；city和mountain模型对应大场景模型，面片数分别为100w和500w级别。通过分析上述测试结果，Rasterization阶段依旧是整个渲染最主要的开销，且其开销的大小与渲染的面片数有关，当剩余三角面较多时，其开销增大；而当剔除了较多三角面时，其开销会有所下降。而RayCasting的开销与面片数无关，主要与需要RayCasting的像素数和SVO的层数有关。MeshCulling阶段的开销则很小且较为固定，不会随视角的改变而变化，主要与Cluster数有关。
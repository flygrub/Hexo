title: Unity UGUI 图片模糊
date: 2016-01-28 09:21:58
tags: [Unity,UGUI]
---
最近遇到导入带有透明度或毛边效果的图片，在视图中显示模糊。整理UGUI中图片格式属性修改方案。

把图片拖入Unity工程中，图片默认的属性如下图：
![初始格式](PageImage/UnityUGUIImageBlur_First.jpg)
这里Texture Type默认格式为Texture。
这种纹理格式是3D物体最常用的纹理，不过UGUI需要修改为Sprite(2D and UI)。
Sprite Mode保持默认Single

>如果要切割图片需要修改为Multiple，打开Sprite Editor面板，点击左上角的Slice，点击Slice按钮，默认的配    置会自动把图集切割成一张一张小图，切割适用于美工给的图片是一整图集。
Packing Tag 打包图集名，相同Tag的图片会被打包成一张图
Pixels Per Unit 每单位像素值，默认就好
Pivot 中心点，一般是Center，根据需求修改

Generate Mip Maps 把勾去掉，Mit Maps多级纹理，是逐步缩小图像版本的一个列表，远离相机的物体使用较小的纹理版本，这个对于UGUI来说没什么用处，还会造成图片模糊和多使用33％以上的内存。
Filter Mode 纹理过滤模式保持默认Bilinar

>Point 点模式，纹理在近距离变成块状，UGUI分辨率缩小时，图片会边模糊
Bilinear 双线性，纹理在近距离变模糊，UGUI下分辨率缩小时，模糊效果比Point好太多
Trilinear 三线性，类似双线性，但纹理也在不同的mipmap层次之间变模糊，在UGUI中不适用

Max Size 导入纹理的最大尺寸。美工往往更愿意使用巨大的纹理，用这个调整纹理降到合适的大小。
Format 纹理格式

>Compressed 压缩，压缩RGB纹理，这是最常见的漫反射纹理格式。UGUI中大部分图片都使用这个格式。
Truecolor 真彩色，这是最高的质量。通常透明度和颜色渐变的图片使用这个格式，这个格式消耗也大

修改之后的图片属性如下：
![修改后格式](PageImage/UnityUGUIImageBlur_End.jpg)

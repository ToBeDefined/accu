# 空间滤波基础

某些邻域处理工作是操作邻域的图像像素值以及相应的与邻域有相同维数的子图像的值. 这些子图像可以被称为**滤波器**, **掩模**, **核**, **模板** 或 **窗口**. 在滤波器子图像中的值是系数, 而不是像素值.

空间滤波的机理就是在待处理图像上逐点地移动掩模. 在每一点, 滤波器的响应通过事先定义的关系来计算. 对于线性空间滤波, 其相应由滤波器系数与滤波掩模扫过区域的相应像素值的乘积之和给出.

![img](/img/pil/spatial_filter/spatial_filter.jpg)

线性空间滤波处理经常被称为"掩模与图像的卷积", 类似的, 滤波模板有时也成为"卷积模板", "卷积核" 一词也常用于此.

实现空间滤波邻域处理时的一个重要考虑因素就是**滤波中心靠近图像轮廊时发生的情况**. 考虑一个简单的大小为 n * n 的方形掩模, 当掩模中心距离图像边缘为 (n-1)/2 个像素时, 该掩模至少有一条边与图像轮廓相重合. 如果掩模的中心继续向图像边缘靠近,那么掩模的行或列就会处于图像平面之外. 有很多方法可以处理这种问题. 最简单的方法就是将掩模中心点的移动范围限制在距离图像边缘不小于 (n-1)/2个像素处. 这种做法将使处理后的图像比原始图像稍小, 但滤波后的图像中的所有像素点都由整个掩模处理. 如果要求处理后的输出图像与原始图像一样大, 那么所采用的典型方法是, 用全部包含于图像中的掩模部分滤波所有像素. 通过这种方法, 图像靠近边缘部分的像素带将用部分滤波掩模来处理. 另一种方法就是在图像边缘以外再补上一行和一列灰度为零的像素点(其灰度也可以为其他常值), 或者将边缘复制补在图像之外. 补上的那部分经过处理后去除. 这种方法保持了处现后的图像弓原始图像尺寸大小相等, 但是补在靠近图像边缘的部分会带来不良影响, 这种影响随着掩模尺寸的增加而增大. 总之, 获得最佳滤波效果的惟一方法是使滤波掩模屮心距原图像边缘的距离不小于 (n-1)/2 个像素.
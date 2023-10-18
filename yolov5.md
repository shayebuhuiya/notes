**目标检测**

* 类别标签
* 置信度得分

**定位与检测**

* 定位：找到检测图像中带有一个给定标签的单个目标
* 检测：周到图像中带有给定标签的所有目标

**性能指标**

检测精度

* Precision, Recall, F1 score
* IoU (Intersection over Union)
* P-R curve (Precision-Recall curve)
* AP (Average Precision)
* mAP (mean Average Precision)

检测速度

* 前传耗时
* 每秒帧数FPS(Frames Per Second)
* 浮点数运算量(FLOPS)

**混淆矩阵**

![image-20231018153715757](D:\notes\Xiao\图片\混淆矩阵.png)**损失函数**

* 分类损失：classification loss
* 定位损失：localization loss
* 置信度损失：confidence loss

总损失函数：
$$
classification\ loss + localication\ loss + confidence\ loss
$$

### CNN

![image-20231018164123350](D:\notes\Xiao\图片\cnn网络图.png)

**卷积**：通过多个卷积核可以获得图像的多个特征

**池化**：避免过拟合

**激活**：通过激活函数relu使线性化变得有意义

通过三步操作后将矩阵扁平化并输入全连接网络进行训练
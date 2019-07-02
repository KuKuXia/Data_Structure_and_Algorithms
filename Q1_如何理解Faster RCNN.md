# 如何理解Faster RCNN

目前学术和工业界的目标检测算法分成３类：

1. 传统的目标检测算法：Cascade + HOG/DPM + Haar/SVM以及上述方法的诸多改进、优化
2. 候选区域/框+深度学习分类：通过提取候选区域，　并对相应区域进行以深度学习方法为主的分类的方案，　如：
   1. R-CNN (Selective Search + CNN + SVM)
   2. SPP-net (ROI Pooling)
   3. Fast R-CNN (Selective Search + CNN + ROI)
   4. Faster R-CNN (RPN + CNN + ROI)
   5. R-FCN等系列方法

3. 基于深度学习的回归方法：　
   1. Yolo/SSSD/DenseBox等方法
   2. 结合RNN算法的RRC Detection
   3. 结合DPM的Deformable CNN等

## Faster RCNN主要结构

Faster RCNN 已经将特征抽取(feature extraction), proposal提取，bounding box regression (rect refine), 'classification都整合在一个网络中，使得综合性能有较大提高，在检测速度方面尤为明显

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155421007015280035.jpg)

Faster RCNN其实可以分为4个主要内容：

1. Conv layers。作为一种CNN网络目标检测方法，Faster RCNN首先使用一组基础的conv+relu+pooling层提取image的feature maps

2. feature maps。该feature maps被共享用于后续RPN层和全连接层。

3. Region Proposal Networks。RPN网络用于生成region proposals。该层通过softmax判断anchors属于foreground或者background，再利用bounding box regression修正anchors获得精确的proposals。
   Roi Pooling。该层收集输入的feature maps和proposals，综合这些信息后提取proposal feature maps，送入后续全连接层判定目标类别。

4. Classification。利用proposal feature maps计算proposal的类别，同时再次bounding box regression获得检测框最终的精确位置。

   

Note: 在Faster RCNN Conv layers中对所有的卷积都做了扩边处理（ pad=1，即填充一圈0），导致原图变为 (M+2)x(N+2)大小，再做3x3卷积后输出MxN 。正是这种设置，导致Conv layers中的conv层不改变输入和输出矩阵大小。


# DrivenData-Identifying-Accents-in-Spectrograms-of-Speech
DrivenData平台AI Contest  Identifying Accents in Spectrograms of Speech 

## 1. 问题描述
图像分类问题：将来自三个国家的语音信息转化为图谱，对其进行识别测试。训练集数量为4500，测试集数量为5376
## 2. 算法思想
### 2.1 解题思路
（1）训练集数量不多，因此需采用图像增强方式        
（2）采用keras框架，构建卷积神经网络模型，选取 DenseNet201、InceptionV3、Xception、InceptionResNetV2、ResNet50 五个网络进行单独训练，分别得到五个输出结果        
（3）加权模型融合，以模型分类精度为标准，分配模型权重，对输出结果进行加权投票，得到最终结果 
### 2.2 卷积神经网络模型

| 基础模型 | 模型概述 |    
| :-----:| :---- |   
| ResNet50 | ResNet利用残差模块来解决由于深度网络层数增加而引发的准确率下降问题，且训练表现优异，有助于解决梯度消失和梯度爆炸问题 |   
| Densenet | DenseNet最大化网络中所有层之间的信息流，网络中的所有层两两都进行了连接，并且使用少量的卷积核就可以生成大量的特征 |       
| Inception V3 | 研究在增加网络规模的同时保证计算高效率，其最重要的改进是分解，用1xn结合nx1来代替nxn的卷积，既可以加速计算，处理高维特征|  
| InceptionResNet V2 | Inception-ResNetV2的改进是使用Inception系列中残差连接的网络使得训练加速收敛，精度更高|    
| Xception | 利用空间变换、通道变换设计出易迁移、计算量小、能适应不同任务，且精度较高的 | 

### 2.3 加权投票法
加权投票法参考 http://mlwave.com/kaggle-ensembling-guide/， 以模型精度作为依据，分别赋予4,3,2,1的权重， GitHub项目 Kaggle-Ensemble-Guide，具体如下：                 

| 基础模型 | 模型得分 | 模型权重 |     
| :-----| ----: | :----: |   
| ResNet | 0.78 | 1 |     
| Densenet | 0.78 | 1 |       
| Inception V3 | 0.79 | 2 |  
| InceptionResNet V2 | 0.79 | 3 |   
| Xception | 0.80 | 4 | 

## 3. 比赛总结
后期看了几个解题思路，其中对提升精度最有帮助的在于 **图像增强**，分析图像可知 ，         
（1）语音数据图谱进行水平翻转对于口音分类并无影响，因此可进行 **水平翻转** 进行增强       
（2）制造缺失值，随机将图像**部分置零**（水平偏移应该也可）         
（3）如果不做模型融合的话，就简单对多模型输出做**加权融合**，相比于单纯的投票法精度要高，同时比较具有合理性     
总的来说，图像类问题的图像增强手段要依据 图像特点来说，若单纯的物体分类，则可通过旋转、裁剪等手段，但如果是语音频谱类，则不适当的操作有可能引起图像他原本的数据丢失，或者反而会对网络分类带来负面的影响

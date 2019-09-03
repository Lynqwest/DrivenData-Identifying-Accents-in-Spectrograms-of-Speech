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
### 2.3 加权投票法
加权投票法参考 http://mlwave.com/kaggle-ensembling-guide/， GitHub项目 Kaggle-Ensemble-Guide
## 3. 比赛总结
后期看了几个解题思路，其中对提升精度最有帮助的在于 **图像增强**，分析图像可知 ，         
（1）语音数据图谱进行水平翻转对于口音分类并无影响，因此可进行 **水平翻转** 进行增强       
（2）制造缺失值，随机将图像**部分置零**（水平偏移应该也可）         
（3）如果不做模型融合的话，就简单对多模型输出做**加权融合**，相比于单纯的投票法精度要高，同时比较具有合理性     

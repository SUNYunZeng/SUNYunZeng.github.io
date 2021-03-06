---
title: '自己第一篇SCI文章： LSI-LSTM——个体出行目的地预测模型'
comments: true
toc: true
date: 2021-03-15 12:51:50
categories: 算法
tags: 深度学习
sticky: 0
cover: https://sunyunzeng.com/%E8%87%AA%E5%B7%B1%E7%AC%AC%E4%B8%80%E7%AF%87SCI%E6%96%87%E7%AB%A0-LSI-LSTM-%E4%B8%AA%E4%BD%93%E5%87%BA%E8%A1%8C%E7%9B%AE%E7%9A%84%E5%9C%B0%E9%A2%84%E6%B5%8B%E6%A8%A1%E5%9E%8B/%E5%9B%BE3.png
---

# 内容导读

个体驾驶目的地预测对于个性化服务推荐、出行导航、交通调度等基于位置的服务具有重要的参考意义。然而，**个体出行不仅具有长时间依赖性，而且受到出行空间上下文的影响**，使得精准实时的驾驶目的地预测面临较大挑战。目前，长时间依赖关系建模已存在一定的技术手段，**但空间因素对出行过程的影响仍然较少被考虑到。事实上，出发地与沿途的城市功能区、路网关键节点均一定程度反映并影响着司机的出行意图**。为此，本文提出一种基于注意力机制和长短期记忆（LSTM）的个体驾驶目的地实时预测模型LSI-LSTM。该模型在兼顾个体出行长时间依赖性学习的同时，从轨迹点的位置语义、轨迹点与目的地间的空间关联关系两个维度建模空间上下文。具体来说，**使用高德POI和TF-IDF算法建立轨迹语义提取方法t-LSE，以获得轨迹点所在区域的城市功能区类型；基于转向角、速度、已行驶距离三类驾驶状态评价轨迹点的位置重要性，并利用轨迹空间注意力机制t-SAM进一步捕获与潜在目的地有强空间关联的轨迹点。基于上述城市功能区类型和位置重要性，LSI-LSTM对轨迹点的空间上下文进行建模，在不需要外部路网数据的情况下实现细粒度空间出行特征的学习**。基于私家车移动轨迹数据的实验结果表明，本文模型LSI-LSTM的预测精度显著优于所选基准模型（隐马尔科夫、随机森林与LSTM）；同时，消融实验验证了t-LSE与t-SAM的有效性。此外，本文进一步探讨了出行熵、出行距离等因素对模型预测能力的影响。

本文的主要创新如下：
1.	LSI-LSTM综合考虑时间依赖、出行位置语义及空间关联对个体尺度目的地预测的影响。
2.	轨迹位置语义提取算法t-LSE为空间出行特征中如何有效耦合城市功能性提供了参考。t-LSE有助于丰富出行位置的先验知识，为更好地捕获语义层面上的出行规律提供支撑。
3.	轨迹空间注意力机制t-SAM使得LSI-LSTM能够以数据驱动的方式实时推测司机的驾驶意图。这得益于t-SAM能够在不依赖路网数据的情况下，利用驾驶状态数据识别位置重要性并发掘潜在空间关联关系。

# 引言

个体驾驶出行目的地预测作为人类移动分析领域研究的分支，近年来逐渐成为研究热点。该研究关注如何根据一段尚未完成的轨迹及其上下文信息，快速、精准地预测可能抵达的目的地。个体出行过程具有复杂的时间依赖与空间关联关系，同时受到所处的空间上下文影响。个体出行长短距离混杂，并且具有长短期时间依赖关系。传统概率模型如隐马尔可夫，其概率推断的模型设计，难以捕获轨迹的长期依赖；而循环神经网络模型能够自动学习长期依赖关系，已成为主流的预测方法。**但现有预测模型对轨迹的位置语义及空间关联关系的研究和利用尚显不足。**

受到路网拓扑的约束及个体出行偏好选择的影响，轨迹点之间存在空间关联关系。**但现有研究主要关注出发位置与目的地间的空间关联关系，而忽略了中间轨迹点与各候选目的地间空间关联的差异性；然而这种差异性往往有助于出行过程的表达。位置语义作为特定区域的城市功能描述，能够辅助揭示司机的频繁出行模式，但较少有个体出行目的地预测模型考虑上述因素的作用。**

为此，本文提出一种顾及轨迹点的位置语义及空间关联关系的个体出行目的地预测模型LSI-LSTM，并通过模型对比及消融实验验证LSI-LSTM在多组轨迹数据集上的有效性。

# 模型设计

本文的模型LSI-LSTM由输入模块、出行模式学习模块与目的地预测模块三部分组成，如图 1 所示：
<center>{% asset_img 图1.png %}</center>
<center>图1 LSI-LSTM模型架构图</center>

其中输入模块负责对轨迹数据、出行时空语义、驾驶状态进行编码与组织，得到移动特征序列。出行模式学习模块利用双层LSTM捕获移动特征序列的长期依赖关系，并基于轨迹空间注意力机制探测并强化关键位置的轨迹特征，最终获得个体出行模式的特征表达。目的地预测模块采用线性全连接残差网络实现特征空间到二维坐标空间的转换，输出目的地预测结果。为了建模空间上下文，输入模块和出行模式学习模块分别实现了本文提出的轨迹位置语义提取方法t-LSE和轨迹空间注意力机制t-SAM，具体介绍如下。

1）轨迹位置语义提取算法 t-LSE
t-LSE算法分为位置语义地图构建和轨迹位置语义赋值两步。算法主要流程如图2所示：
<center>{% asset_img 图2.png %}</center>
<center>图2 轨迹位置语义提取算法t-LSE</center>

为了提高位置语义提取与赋值的效率，预先构建一幅覆盖整个研究区域的位置语义格网地图，其中每个格网单元代表特定的划分区域。t-LSE统计每个单元格内各类POI数量，并基于TF-IDF算法提取该单元格的位置语义向量，该向量的每一维取值表示对应POI类型的重要程度。相对于仅使用唯一的POI标签（如，占比最多的POI类型），语义向量能够更加全面地反映特定区域的城市功能类型。提取的深圳市位置语义示意地图如图3表示，**从展示出来的关键地标建筑（如区域A中的深圳大学）可以看出位置语义格网地图能够一定程度反映城市功能性。**
<center>{% asset_img 图3.png %}</center>
<center>图3 位置语义地图</center>

理论上，出发区域的位置语义能够辅助模型揭示司机潜在的出行偏好；途径的典型城市功能区也可能影响出行意图。因此，t-LSE基于语义地图为出发区域及所有中间轨迹点赋予位置语义。首先利用DBSCAN算法对所有出发轨迹点聚类，识别出各个出发区域。然后取同一类簇内所有轨迹点位置语义的均值作为该区域的位置语义。为避免位置语义稀疏问题，采用轨迹点所在格网内最重要的POI类型作为中间轨迹点位置语义。

2）轨迹空间注意力机制t-SAM
t-SAM通过驾驶状态推测各轨迹点所在空间位置对于目的地选择而言的重要性，进而为重要的轨迹点赋予较高的注意力权重，实现司机出行模式的学习与表达。通常而言，**位于路口等关键位置的轨迹点与目的地之间往往有更紧密的空间关联性，能够反映司机在前进方向的选择，进而影响最终目的地的选择**。图4给出司机在具有4个路口的简化路网结构中对7个候选目的地的选择过程。
<center>{% asset_img 图4.png %}</center>
<center>图4 简化路网中的驾驶过程示例</center>

驾驶过程中途径路口点变化会导致候选目的地抵达概率的变化（仅考虑路网约束）。如图4所示，司机从A地出发，四个出行阶段所在路口位置对应的最可能目的地分别为H、B与C、D与E、D。结合关键位置及该位置司机的出行方向选择信息可有效地缩小潜在目的地的搜索范围。

t-LSE在**无需路网数据**的情况下，利用驾驶状态（速度、转向角、已行驶距离）识别位于“重要空间位置”上的轨迹点，捕获关键移动特征。基于先验知识可知，在关键路口位置或转向处，司机往往会减速慢行且前进方向发生变化，因此低速、转向角大的轨迹点常位于重要的路网位置；司机行驶距离越远，距离最终目的地也越近，轨迹点位置重要程度也相对更高。图5展示了基于某司机真实驾驶状态得到的位置重要性示例，颜色越深表示位置越重要。
<center>{% asset_img 图5.png %}</center>
<center>图5 轨迹点在三类驾驶状态下的位置重要性可视化</center>

如图5所示，结合转向角与驾驶速度，重要位置的轨迹点 p_5、p_12、p_25  均被成功识别。基于三类驾驶状态，t-LSE构建轨迹点位置重要性计算规则，并作为注意力机制的启发项，计算得到各个轨迹点特征的注意力权重。


# 实验分析


为了验证模型及相关算法的有效性，本文选择深圳市内四名私家车司机2018年全年的驾驶移动轨迹记录为研究数据，其统计信息（年出行OD熵、不同的OD点簇数量、年出行距离、轨迹平均距离、轨迹总数量）如表1及图6所示：
<center>表1 四名司机的年度出行统计</center>
<center>{% asset_img 表一.png %}</center>

<center>{% asset_img 图6.png %}</center>
<center>图6 四名司机的年度出行轨迹、OD弧、活动热点空间分布可视化</center>

1）	模型有效性验证实验

本文将LSI-LSTM与三类经典的目的地预测模型（隐马尔科夫HMM、随机森林RF及LSTM）进行对比，结果如图7所示：
<center>{% asset_img 图7.png %}</center>
<center>图7 模型有效性对比</center>

实验结果表明，**无论是平均绝对误差MAE、平均均方根误差RMSE还是平均相对误差MRE，LSI-LSTM的预测精度均显著高于三种基准模型**。概率模型RF与HMM将连续稠密的轨迹数据转化为精简的出行特征，有效降低了出行过程建模的复杂性和冗余，因此比LSTM表现更好。此外，由于RF与HMM无法捕获长期的时间依赖和建模复杂的出行模式，导致面向出行熵和出行总距离较大的个体预测效果一般。结合表1的出行OD熵来看，**司机的整体预测精度与出行OD熵紧密相关**。出行OD熵度量了司机出行的不规律性，OD熵越高，司机的出行模式越复杂，模型整体预测精度就越低。

2）t-LSE和t-SAM的有效性验证实验

本文采用消融实验来验证算法t-LSE和t-SAM的有效性，实验的结果如图8所示：
<center>{% asset_img 图8.png %}</center>
<center>图8 模型消融实验</center>

图8实验结果表明，**t-LSE和t-SAM均有效地提高了模型的预测精度，而综合这两种算法的LSI-LSTM预测精度最好**。与t-LSE相比，t-SAM对模型精度的提升更为明显。尽管t-LSE能够刻画轨迹所在位置的城市功能性，但无法揭示用户真正活动的场所类型，导致模型难以在社会活动空间上挖掘用户的移动规律；t-SAM通过识别具有显著位置重要性的轨迹点，能够实时、准确地对出行过程进行细粒度表达。

3）t-LSE和t-SAM的影响因素实验

t-LSE需要构建位置语义格网地图，而格网的划分粒度影响着位置语义的代表性及准确性。为此，本文探讨了不同格网粒度对位置语义有效性的影响，实验结果如图9所示：
<center>{% asset_img 图9.png %}</center>
<center>图9 不同语义地图格网粒度对四名司机平均预测精度的影响</center>

图9的实验结果表明，**太粗或太细的格网粒度均会影响位置语义对城市功能区的表达**。太细格网粒度划分下的位置语义代表性下降，而太粗粒度会导致轨迹点在不同区域的位置语义区分度降低。针对四名司机的轨迹数据而言，250m-500m是较为合适的格网划分区间，不同应用场景下最优格网划分需要通过格网粒度实验获得。

t-SAM中位置重要性计算依赖于三类驾驶状态，为了探讨不同驾驶状态对t-SAM的影响，本文设计对比实验，结果如图10所示：
<center>{% asset_img 图10.png %}</center>
<center>图10 不同驾驶状态下四名司机的平均预测精度</center>

结果表明，**三种驾驶状态均能显著提高t-SAM的有效性**，因为行驶距离可以反映长短距离的移动模式，而驾驶速度及转向角可以体现出行的显著移动特征。综合这三种驾驶状态可以更全面地描述轨迹的显著特征，预测精度也最高。

为了进一步探讨t-SAM的工作原理，本文根据t-SAM学习到的各轨迹点注意力权重与预测距离误差计算空间关联度，并通过地图可视化呈现，如图11所示：
<center>{% asset_img 图11.png %}</center>
<center>图11 某司机各轨迹点格网与候选目的地格网的空间关联度的空间分布及关系图</center>

**图11 a)为各个轨迹格网的累积空间关联度**，数值越大，格子颜色越深，则表示该位置与各目的地的关联性也越大，而其中颜色较深的几个位置均位于路口附近，也验证t-SAM可以基于轨迹自身状态启发式地识别关键路网位置。**图11 b) 为子区域A中各轨迹网格与目的地格网332的空间关联关系强度分布图**，其中轨迹格网241、301、317、354与目的地格网332的关联度最大，它们均分布于决定未来走向的关键路口附近。**图11 c) 展现了轨迹格网346与360与候选目的地格网的空间关联度**，可以看出当出行轨迹经过路口轨迹格网361后，前进方向上的格网346与360分别与目的地格网229与332具有强空间关联度，这与路网拓扑约束的结果吻合。**图11 d) 为不同轨迹格网与目的地格网之间的空间关联度热力表**，颜色越深表明关联越紧密，可以发现路网中临近的轨迹格网对目的地有相似的关联度，如轨迹格网317与332、300及301。可视化结果表明通过对轨迹点与目的地间的空间关联关系的探测与学习，t-SAM可以一定程度上辅助用户出行模式的表达。

4）其它影响因素实验

**不同出行长度及子轨迹的切分数量会影响到预测精度**，为了探讨这两个因素对模型模型效果的影响，本文开展两项对比实验。

a.	出行总距离对模型精度的影响
个体存在长距离出行与短距离出行并存的情况，为了探讨出行总距离对模型精度的影响，本文对比四种模型（LSI-LSTM、HMM、RF与LSTM）在四名司机的数据集上的综合表现，结果如图12所示。
<center>{% asset_img 图12.png %}</center>
<center>图12 不同出行距离下模型的MAE和MRE表现</center>

图12表明，随着出行总距离的增加，四种模型的MAE均逐渐增加；除了LSTM，其它三个模型的MRE均不断下降。这主要是因为随着出行距离的增加，用于模型训练的轨迹总数量不断减少（如图12子图左上角嵌套的小图可知），因此MAE上升。MRE逐渐降低的原因在于MAE增加率比出行距离增加率小。整体而言，LSI-LSTM在不同的出行距离下，在MAE及MRE上均表现最优。

b.	子轨迹生成数量对模型精度的影响
子轨迹是一段已出行但并未完成的从出发点到中间点的轨迹序列，为了模拟用户的出行过程，一条完整的出行轨迹被切分为不同长度的子轨迹集合。为了探讨子轨迹的生成数量对模型精度的影响，本文针对不同子轨迹切分数量进行精度分析实验，如图13所示：
<center>{% asset_img 图13.png %}</center>
<center>图13 子轨迹生成数量对比实验</center>

实验结果表明，当子轨迹生成的数量越多，模型可以学习更丰富的出行细节，相应的预测精度也越高。但是随着子轨迹生成数量的增加，模型预测精度的提高有限，而模型训练时间却呈线性增长。因此，最合适的子轨迹生成数量可以综合预测精度与模型训练效率进行设置。

# 讨论与总结

针对目的地预测任务中对于空间因素利用不足的问题，本文提出一种**综合轨迹点位置语义与空间关联**的预测模型，并通过实验验证分析了模型及相关算法的有效性。未来工作可以围绕如下两个方面展开：

1.	深层位置语义提取。城市功能区类型仍属于浅层的位置语义，而更深层个体社会活动位置语义（如“在餐厅吃饭”）的提取，可以更好地揭示司机的出行行为偏好。
2.	冷启动问题。个体目的地预测模型在数据采集初期面临数据稀疏问题，利用群体轨迹数据对模型进行预训练将提供一种解决思路。


# 引用

## 论文链接

[LSI-LSTM: An attention-aware LSTM for real-time driving destination prediction by considering location semantics and location importance of trajectory points](https://www.sciencedirect.com/science/article/pii/S092523122100134X)


**引用txt格式：**

Zhipeng Gui, **Yunzeng Sun**, Le Yang, Dehua Peng, Fa Li, Huayi Wu, Chi Guo, Wenfei Guo, Jianya Gong,
LSI-LSTM: An attention-aware LSTM for real-time driving destination prediction by considering location semantics and location importance of trajectory points,
Neurocomputing,
Volume 440,
2021,
Pages 72-88,
ISSN 0925-2312,
https://doi.org/10.1016/j.neucom.2021.01.067.
(https://www.sciencedirect.com/science/article/pii/S092523122100134X)

## 代码链接

https://github.com/ZPGuiGroupWhu/Human-Mobility-Analysis/tree/master/Travel-Destination-Prediction/LSI-LSTM

## 首发微信公众号

首发地址：https://mp.weixin.qq.com/s/ynMufJEbiZ1lEFNUqE7_QA

团队GitHub网站链接：https://github.com/ZPGuiGroupWhu

珞珈时空计算团队微信公众号：**<font color='blue'>Luojia-STC</font>**

珞珈时空计算团队网站链接：http://www.luojia-spatial.com/index.html

<center>{% asset_img 微信公众号.png %}</center>
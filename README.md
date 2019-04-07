# NumberStringsRecognizerInFinancialBills

[![LICENSE](https://img.shields.io/badge/License-Apache--2.0-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-windows-lightgrey.svg)]()
[![Python3](https://img.shields.io/badge/Python-3-green.svg)]()

基于卷积神经网络识别金融票据中的手写数字串

本项目为2019大学生计算机设计大赛参赛项目

待比赛结束后将会把项目完整的释出 敬请期待

___________________________

# 目录

__[开发人员名单](#开发人员名单)__

__[设计方案](#设计方案)__

__[作品安装说明](#作品安装说明)__

__[手写单数字符识别](#手写单数字符识别)__

__[手写数字串识别](#手写数字串识别)__

_________________________________

# 开发人员名单

[DolorHunter](https://github.com/DolorHunter) 

[zyMarshtomp](https://github.com/MarshtompCS)

## 设计方案

**金融票据的自动识别是模式识别的重要应用领域。几乎涉及了模式识别与图 像处理的所有重要分支，是一个综合的研究课题，具有理论和实用两方面
的价值。**

**由图可见金融票据单中有很多的手写数字, 对于这些数字计算机无法直接识别, 只能依靠人工识别输入计算机, 费时费力. 因此我们利用了人工智能神经网络技术对于手写数字进行神经网络模型训练, 并通过训练好的模型对金融票据中的数字进行识别. 这样就不需要人工识别手写数据, 再输入计算机, 方便高效. 这个技术不仅实用, 更具备一定的商业意义.**

- [x] 本作品通过运用基于tf神经网络的单数字符识别模型, 通过一系列的预处理操作, 可以用来识别金融票据中的手写数字串(流水号/ 金额/ 工单号等的信息).
	
- [x] 方案通过tf神经网络和mnist数据集训练出对于单数字符具有高识别度的模型(**正确率98.01%**). 然后再将待识别图片的单列手写数字串进行反相/去噪声/变形/切分等预处理, 从而分离各个单数字符. 再将各个单数字符分别用模型识别, 从而达到对于单列手写数字串的识别.(**理论正确率98.01%^n**).

*作品的意义不仅仅在于利用预处理的方式省去制作手写数字串训练集, 整理训练集等的繁琐步骤, 更能通过预处理操作让理论正确率达到98.01%^n, 因此使得本作品具备更多的科研和商用意义.*

## 作品安装说明

模块和环境:	 Tensorflow 1.0   	 Python 3.7 		 PIL 		 Numpy

帮助: `README`(文件和目录说明)和`README.md`帮助文档

下载命令: 
	
	$ git clone https://github.com/DolorHunter/NumberStringsInFinancialBillsRecognizor.git


## 手写单数字符识别

方案参照了主流的神经网络设计八股(准备, 前传, 反传, 迭代)进行总体设计. 

通过调用tensorflow的库函数来实现神经网络的参数优化和模型训练以达到识别数字的功能. 我使用mnist数据集对模型参数进行训练. mnist数据集有超过60000个标记样本以供训练. 同时mnist数据集的可靠性也很高, 很适合作为项目的数据集.

神经网络的模型通过喂入mnist数据集中的样本来训练, 通过前向传播和反向传播的计算, 逐渐的修改模型. 其中使用了如: 滑动平均值ema, 正则化损失函数防止过拟合, 指数衰减学习率等高效算法, 让模型修改的更加贴合实际情况. 

训练产生的模型将储存于model目录下. 模型实际训练时常约30min, 为方便多次分时训练和后续增加的训练, 因此使用了断点续训的方式. 

测试应用的图片文件储存与pic目录, 为手写数字(白底黑字), 用来测试模型的识别的准确性, 也可以根据实际情况上网下载图片进行识别测试.

方案通过app.py文件调用已经训练好的模型对自定义的数字图片进行识别. 经过测试, 识别成功率超过了98.01%, 超过了预期的设计构想. 方案成功.

## 手写数字串识别 

**预处理:**

`反相, 去噪声:`

预处理中将图片黑白颠倒, 并去除噪声, 故可识别的图片为白底黑字, 符合日常使用需求.

`切片:`

假定数字串的每个数字之间存在空隙. 图片从左到右逐列识别, 当图片开始出现黑色时, 认为单数字符出现, 标记该列为出现单数字符的列, 并储存; 当图片全列为白
时, 认为单数字符结束, 标记该列为单数字符消失的列, 并储存. 为了便于说明, 将识别黑色(出现)称为黑检测, 将识别白色(结束)称为白检测. 黑白检测通过bool型变量进行切换. 将数字串的个数和位置信息存储于一维数组. 利用一维数组的数据进行中值切片.

`小图变形:`

对于一维数组中的信息进行提取, 将每一个单数字符存储至一个二维数组. 将二维数组变形成28*28, 再将二维数组变形成1*784的数组, 传入模型进行识别.

`小图输入单子符识别模型`

**输出**

`输出预测的手写数字串结果. 理论识别率为98.01%^n.`

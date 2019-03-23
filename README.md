

# 天池比赛：[津南数字制造算法挑战赛【赛场一】](https://tianchi.aliyun.com/competition/entrance/231695/introduction) Drop 队（初赛17名，复赛结果未知）代码，初赛加复赛

## 2019年3月9号更新——更新说明：

增加了复赛程序，包括收率预测和最优参数生成。

+ __收率预测：__

  与初赛类似，但有所改动，改动如下：

  （1）最终异常数据并没有用其他特征来预测，只是简单的改变为缺失值，最后发现效果甚至好于用其他特征预测。

  （2）未使用 id 特征。

  （3）经过多次特征选择最终决定用 9 个特征，因此略去了特征选择的过程。

+ __最优参数生成：__

  测试了两种方法，并最终采用粒子群算法：

  （1）梯度下降：通过数值方法计算每个特征的梯度，然后按照梯度下降的方法更新参数，但是最终发现梯度下降对于树模型基本没用，猜测因为特征略微改变后不影响分裂后的结果，因此大部分时候梯度为 0。

  （2）粒子群算法：每个粒子代表一组参数，优化目标为最优参数，寻找最优的粒子。并测试了不同的参数，确保找到最优的参数。

## 更新文件说明：

复赛.ipynb 为本地程序，包括包括收率预测和最优参数生成。

文件夹“津南数字制造算法挑战赛+17+Drop”为最终提交程序，不包括最优参数生成，只包括收率预测。

# 以下为原始内容：

-------------------------

天池比赛：[津南数字制造算法挑战赛【赛场一】](https://tianchi.aliyun.com/competition/entrance/231695/introduction)初赛 17 名 Drop 队代码

+ 作者: 陶亚凡 陕西科技大学

我的 github 和 博客：

github: https://github.com/taoyafan

博客: https://me.csdn.net/taoyafan

+ 队友（Blue 电子科技大学的） github 和 博客：

github：https://github.com/BluesChang

博客：https://blueschang.github.io

程序各个部分很大程度的参考了鱼佬的 baseline

感谢队友的很多贡献, 感谢鱼佬和他的的 baseline, 感谢林有夕大佬让我在群里不停的学到新知识.

因为我这是第一次参加 ML 的比赛, 看着鱼佬 baseline 开始学习 pandas, sklearn 还有相关知识, 所以水平实在有限. 希望各位大佬给点意见或建议, 有什么问题或者可以改进的地方请告知我, 灰常感谢. 

一直想开源, 但是成绩太差, 趁着还在首页, 赶快开源了, 我也是抱着学习的心态, 也没想着拿奖, 所以这个程序也没啥保留~

## 环境要求

+ __系统__

ubuntu 16.04

+ __python 版本__

python 3.5 或以上

+ __需要的库__

numpy, pandas, lightgbm, xgboost, sklearn

## 文件说明

包括[初赛](https://github.com/taoyafan/jinnan/tree/master/%E5%88%9D%E8%B5%9B)和复赛（等比赛结束后开源），初赛中包括[本地最终程序](https://github.com/taoyafan/jinnan/blob/master/%E5%88%9D%E8%B5%9B/%E6%9C%80%E7%BB%88%E7%A8%8B%E5%BA%8F.ipynb)（.ipynb）、[提交的程序](https://github.com/taoyafan/jinnan/tree/master/%E5%88%9D%E8%B5%9B/%E6%B4%A5%E5%8D%97%E6%95%B0%E5%AD%97%E5%88%B6%E9%80%A0%E7%AE%97%E6%B3%95%E6%8C%91%E6%88%98%E8%B5%9B%2B20%2BDrop)（.py）和 [历史程序](https://github.com/taoyafan/jinnan/tree/master/%E5%88%9D%E8%B5%9B/history)。

建议看[本地最终程序](https://github.com/taoyafan/jinnan/blob/master/%E5%88%9D%E8%B5%9B/%E6%9C%80%E7%BB%88%E7%A8%8B%E5%BA%8F.ipynb)，里面标题、注释和输出都比较全，方便阅读。

[提交的程序](https://github.com/taoyafan/jinnan/tree/master/%E5%88%9D%E8%B5%9B/%E6%B4%A5%E5%8D%97%E6%95%B0%E5%AD%97%E5%88%B6%E9%80%A0%E7%AE%97%E6%B3%95%E6%8C%91%E6%88%98%E8%B5%9B%2B20%2BDrop)为本地程序转成.py格式，然后稍加改善（main函数、结构、路径、输入约束等），介绍可以看里面的readme。

 [历史程序](https://github.com/taoyafan/jinnan/tree/master/%E5%88%9D%E8%B5%9B/history)里面都是记录了我学习的历程，从最初学习鱼佬的程序开始，有些有用有些没用。

## 程序说明

+ __程序思路:__ 

读取数 -> 手动处理训练集明显异常数据 -> 数据清洗 -> 特征工程 -> 训练

__数据清洗:__

删除缺失率过高的数据 -> 处理字符时间(段) -> 计算时间差 -> 处理异常值 -> 删除单一类别占比过大的特征

__特征工程:__

构建新特征 -> 利用特征之间的相关性预测 nan 值 -> 后向特征选择

__训练:__

使用 lgb 和 xgb 自动选择最优参数, 然后融合

+ __数据通路:__

开始学鱼佬的 baseline 自己写着写着变量名太多了,前后运行总是出现各种小错误, 所以对整体结构改了好多次, 最终使用了 pipe line, 包括了整个数据清洗和特征工程部分, 所以变量名少了, 各个部分也不存在耦合了, 所以我觉得有必要介绍下数据通路:

数据读取得到 train, test ----> 合并得到 full ---> 经过 pipe line 得到 pipe_data ---> 训练集测试集分割得到 X_train 和 X_test ---> 训练得到结果 oof 和 predictions

## 运行说明

在复赛的一级目录下还需要 data 和 result 文件夹，分别放训练、测试数据和最终生成结果，最终生成的结果文件名为：测试名\_模型名\_结果\_特征数量_时间.csv（提交的程序按照官方要求命名）

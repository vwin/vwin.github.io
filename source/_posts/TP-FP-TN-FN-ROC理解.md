---
title: TP FP TN FN ROC理解
toc: true
date: 2018-11-11 21:37:20
tags: [tp,fn,tn,fp,roc]
categories: [技术]
description:
---
机器学习中算法评估的几个概念，tp,fp,tn,fn,roc,auc,F1值的理解
<!--more-->

1. True Positive （真正, TP）被模型预测为正的正样本；可以称作判断为真的正确率
2. True Negative（真负 , TN）被模型预测为负的负样本 ；可以称作判断为假的正确率
3. False Positive （假正, FP）被模型预测为正的负样本；可以称作误报率
4. False Negative（假负 , FN）被模型预测为负的正样本；可以称作漏报率
5. True Positive Rate（真正率 , TPR）或灵敏度（sensitivity） 
TPR = TP /（TP + FN） 
正样本预测结果数 / 正样本实际数
6. True Negative Rate（真负率 , TNR）或特指度（specificity） 
TNR = TN /（TN + FP） 
负样本预测结果数 / 负样本实际数
7. False Positive Rate （假正率, FPR） 
FPR = FP /（FP + TN） 
被预测为正的负样本结果数 /负样本实际数
8. False Negative Rate（假负率 , FNR） 
FNR = FN /（TP + FN） 
被预测为负的正样本结果数 / 正样本实际数
9. 精确度（Precision）： 
P = TP/(TP+FP) ; 反映了被分类器判定的正例中真正的正例样本的比重
10. 准确率（Accuracy） 
A = (TP + TN)/(P+N) = (TP + TN)/(TP + FN + FP + TN);
反映了分类器统对整个样本的判定能力——能将正的判定为正，负的判定为负
11. 召回率(Recall)，也称为 True Positive Rate: 
R = TP/(TP+FN) = 1 - FN/T; 反映了被正确判定的正例占总的正例的比重
12. ROC曲线指受试者工作特征曲线 / 接收器操作特性曲线(receiver operating characteristic curve), 是反映敏感性和特异性连续变量的综合指标,是用构图法揭示敏感性和特异性的相互关系，它通过将连续变量设定出多个不同的临界值，从而计算出一系列敏感性和特异性，再以敏感性为纵坐标、（1-特异性）为横坐标绘制成曲线，曲线下面积越大，诊断准确性越高。在ROC曲线上，最靠近坐标图左上方的点为敏感性和特异性均较高的临界值。
13. 精确率(Precision）是指在所有系统判定的“真”的样本中，确实是真的的占比，就是TP/(TP+FP)。
14. 召回率（Recall）是指在所有确实为真的样本中，被判为的“真”的占比，就是TP/(TP+FN)。
15. FPR（False Positive Rate），又被称为“Probability of False Alarm”，就是所有确实为“假”的样本中，被误判真的样本，或者FP/(FP+TN)
16. F1值是为了综合考量精确率和召回率而设计的一个指标，一般公式为取P和R的harmonic mean:2*Precision*Recall/(Precision+Recall)。
17. ROC=Receiver Operating Characteristic，是TPR vs FPR的曲线；与之对应的是Precision-Recall Curve，展示的是Precision vs Recall的曲线。

参考：
[知乎](https://www.zhihu.com/question/30643044/answer/161955532)

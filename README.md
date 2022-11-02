# 电商评论文本挖掘
## 前言
该项目是我在大三选修课（大数据分析与应用）的实验报告，我觉得文本挖掘的项目网上相对来说不是很多，我也是参考了许多Github上的项目，以及自己的一些所学，而完成的一个基础的文本挖掘项目。我觉的整体批注写的还比较全面，便于业余选手，或者只是想了解了解文本挖掘、机器学习、调调包的同学们学习。欢迎大家交流。
## 目录结构
好评：好评数据（只上传了一些）。  
工具：所有代码中国另外需要读取的东西（我改变了原有的目录结构，如果下载下来要跑的话记得更改读取文件的目录）。  
差评：差评数据（只上传了一些）。  
README.md
plus_motion.ipnb：源代码。
## README目录
+ [1.实验目的](#1实验目的)
+ [2.数据介绍](#2数据介绍)
+ [3.数据清洗](#3数据清洗)
+ [4.利用机器学习和文本挖掘技术完成情感分析模型搭建](#4利用机器学习和文本挖掘技术完成情感分析模型搭建)
+ [5.利用情感极性判断与程度计算来判断情感倾向](#5利用情感极性判断与程度计算来判断情感倾向)
+ [6.利用词频和TF-IDF挖掘出正负文本中的关键点情况](#6利用词频和TF-IDF挖掘出正负文本中的关键点情况)
+ [7.利用文本挖掘相关算法找到平台中用户讨论的集中点](#7利用文本挖掘相关算法找到平台中用户讨论的集中点)
## 1.实验目的
利用所学，对如下数据进行文本挖掘，主要实现一下四个方面内容：
1. 利用机器学习和文本挖掘技术完成情感分析模型搭建。
2. 利用情感极性判断与程度计算来判断情感倾向。
3. 利用词频和TF-IDF挖掘出正负文本中的关键点情况。
4. 利用文本挖掘相关算法找到平台中用户讨论的集中点。
## 2.数据介绍
主要由商品id，评论时间、评论内容、点赞数、评论打分构成。  
![image](https://user-images.githubusercontent.com/57103853/199514297-e7cafadf-e4a0-4408-9228-6be814662c02.png)
## 3.数据清洗
1. 使用unique观察商品id、点赞数、评论打分的情况。  
![image](https://user-images.githubusercontent.com/57103853/199515338-a315920a-a4fd-425e-b060-30d085de5ea9.png)
2. 查看评论打分的具体分布，发现4非常少，所以将4归为5分。并且为了方便，将5变为1，将1变为0。  
![image](https://user-images.githubusercontent.com/57103853/199515378-1ae6e6a3-b1a4-446c-bfea-a8bbef6daddc.png)  
![image](https://user-images.githubusercontent.com/57103853/199515494-e5038ffb-0120-4b22-aace-dc2655ae0ec4.png)
3. 观察每个商品好评与差评的数量分布，可以发现一个商品好评、差评最多1000条，一个商品好评>=差评。  
![image](https://user-images.githubusercontent.com/57103853/199515514-d440b3a8-1500-4b9f-91f4-4132abdd73e0.png)
4. 观察每年的商品数量，可以发现主要集中在2021年附近。  
![image](https://user-images.githubusercontent.com/57103853/199517362-f8168ace-b09a-4a42-a267-89a44804f88f.png)
5. 观察评论长度分布。  
![image](https://user-images.githubusercontent.com/57103853/199515589-386a8ecc-8dbb-4271-927d-1c32da5ff42b.png)
6. 观察评论长度大小分布情况。  
![image](https://user-images.githubusercontent.com/57103853/199515615-be23347b-74de-4b5b-be9a-389c0fdfde7c.png)
7. 根据以上观察，去掉一些长度过短、过长的评论。  
![image](https://user-images.githubusercontent.com/57103853/199515657-4b06410d-e30a-47e3-82eb-a98c0e07b6fa.png)
8. 删除重复值。  
![image](https://user-images.githubusercontent.com/57103853/199515691-0575a377-61b4-46f0-8057-2b0a7908f462.png)
9. 删除标点符号或英文字母过多的评论。  
![image](https://user-images.githubusercontent.com/57103853/199515736-69409104-b482-4ddb-a4d4-c61a184ab407.png)
10. 处理HTML符号。  
![image](https://user-images.githubusercontent.com/57103853/199515777-d6711faa-9681-40a4-a244-ff2dc8d2f163.png)
11. 处理*（可能是违禁语、价格、品牌），好评：差评=1：2，应该保留。但每个*的数量还不一样，为了避免jieba分出来多个，将每个出现*的都变为1个*。  
![image](https://user-images.githubusercontent.com/57103853/199515831-6040b9d6-8fc3-4be8-a2fc-5498a1fe49ce.png)
12. 将“京东”、“奶粉”字样去除，所有商品都是奶粉，都来自于京东。  
![image](https://user-images.githubusercontent.com/57103853/199515875-ead7213d-95a4-4acc-8f8d-60139eccc69e.png)
13. jieba分词。  
![image](https://user-images.githubusercontent.com/57103853/199523639-c898bd68-d22c-43c5-b050-135d2512ed00.png)  
![image](https://user-images.githubusercontent.com/57103853/199523677-d706a7d9-2e14-4544-ac05-232ba7df19b7.png)
14. 根据停用词，处理分词后的结果。  
![image](https://user-images.githubusercontent.com/57103853/199523716-1024cbc9-e4d0-4307-a19a-f9d144df8cf4.png)
15. 利用jieba.posseg进行分词后的词性识别。
![image](https://user-images.githubusercontent.com/57103853/199523759-433aa87c-3450-4948-a9e6-8d36b6a194cc.png)  
![image](https://user-images.githubusercontent.com/57103853/199523783-1814b196-191d-4e3e-b34f-213ea223a34d.png)
16. 数据清洗后的结果。
![image](https://user-images.githubusercontent.com/57103853/199523895-a2b2920c-e625-4d71-80a9-3e4c7649dbc6.png)
## 4.利用机器学习和文本挖掘技术完成情感分析模型搭建
1. 根据不同函数情感挖掘的情况，对逻辑回归LogisticRegression、多项式贝叶斯MultionamialNB、随机森林RandomForestClassifier进行进一步探索。
2. 首先对文字使用TF-IDF进行处理变为数字。  
![image](https://user-images.githubusercontent.com/57103853/199523947-b85ca2c1-c60b-4a18-b135-d9e836e0690e.png)  
![image](https://user-images.githubusercontent.com/57103853/199523812-5d05e537-c6d7-4877-9c81-9bdedbf83584.png)
3. 对数据进行PCA降维，从累计贡献率可以看出，使用PCA降维的效果欠佳，几乎每个特征都不能消除，特征之间都几乎不具备共线性，所以不使用PCA降维。  
![image](https://user-images.githubusercontent.com/57103853/199523970-45e3dc4a-3752-4568-9829-6b7a70072dfa.png)  
![image](https://user-images.githubusercontent.com/57103853/199524021-83e354ee-a8e5-417c-89d3-2090550a4918.png)
4. 使用RandomForestClassifier模型的feature_importance_属性来找出前N个最重要的特征。  
![image](https://user-images.githubusercontent.com/57103853/199524045-922bb346-5324-4739-8246-9d6aa4b8f7d7.png)
5. 根据RandomForestClassifier的重要性特征排序，分别每隔100，进行ROC比较，找到合适的特征个数，前900个已经不错。
![image](https://user-images.githubusercontent.com/57103853/199524517-c1559d21-1aef-45b0-8c28-376d2875abe4.png)  
![image](https://user-images.githubusercontent.com/57103853/199524543-82386d6b-7c42-42cb-a570-ef6513bd0ded.png)  
![image](https://user-images.githubusercontent.com/57103853/199524710-e0253860-6aad-4cd5-865d-e7796ba1b660.png)  
![image](https://user-images.githubusercontent.com/57103853/199524781-ab5d7884-672b-4075-8621-0df7055c4add.png)
6. 同理使用卡方检验选出前N重要的特征，前800个特征已经相当不错。  
![image](https://user-images.githubusercontent.com/57103853/199524846-ee7c0426-7ed2-4a70-ad48-0cdee927446f.png)
7. 将RandomForestClassifier的前900特征，与SelectKBest前800特征，合并，形成新的特征。  
![image](https://user-images.githubusercontent.com/57103853/199524894-077d1c8b-4879-428b-9a1e-fc6dcddce25f.png)
8. 合并后计算ROC-AUC曲线，效果还不错。  
![image](https://user-images.githubusercontent.com/57103853/199524924-e215ac44-991e-4d79-b2ee-d88e2733de00.png)
9. 用得到的新特征开始进行三个模型训练，并分别利用网格搜索、随机网格搜索、交叉验证来进行调参，并通过计算准确率、查全率等，绘制混淆矩阵，绘制ROC-AUC曲线进行对比，最后发现逻辑回归的整体性能表现最好。  
![image](https://user-images.githubusercontent.com/57103853/199524991-37404d27-b977-448e-9155-eb2696af9d4c.png)  
![image](https://user-images.githubusercontent.com/57103853/199525020-49f65fe3-5449-45dc-800e-a6ef328429f7.png)  
![image](https://user-images.githubusercontent.com/57103853/199525038-11cf11c4-255c-468b-a2f9-d7e816c6f272.png)  
![image](https://user-images.githubusercontent.com/57103853/199525063-07a0a901-fe6f-4baf-b051-6a8520d5d64c.png)  
![image](https://user-images.githubusercontent.com/57103853/199525073-8e79eb0d-ca51-435f-8059-a6d1523a7db5.png)  
![image](https://user-images.githubusercontent.com/57103853/199525096-08813206-3aac-4799-9f6d-051a4f74acc3.png)
## 5.利用情感极性判断与程度计算来判断情感倾向
1. 主要思路：对文档分词，找出文档中的情感词、否定词以及程度副词；然后判断每个情感词之前是否有否定词及程度副词，将它之前的否定词和程度副词划分为一个组；如果有否定词将情感词的情感权值乘以-1，如果有程度副词就乘以程度副词的程度值；最后所有组的得分加起来，大于0的归于正向，小于0的归于负向，得分的绝对值大小反映了积极或消极的程度。
2. 读取否定词、程度副词、情感极性词典。  
![image](https://user-images.githubusercontent.com/57103853/199525162-9597dfcf-80d3-46fb-a3b9-ff4af7662374.png)
3. 根据情感词计算情感分数。  
![image](https://user-images.githubusercontent.com/57103853/199525213-6832458d-a449-4ff4-adf5-96eadd838d0e.png)
4. 计算正确率，为0.835，效果还不错。  
![image](https://user-images.githubusercontent.com/57103853/199525244-f59688bb-9f00-4d76-ad4c-2d4c44f4ad07.png)  
![image](https://user-images.githubusercontent.com/57103853/199525262-161b98b4-e6b6-44e6-b313-cdc50a53204e.png)
## 6.利用词频和TF-IDF挖掘出正负文本中的关键点情况
1. 利用Tf-idf将文字转为有权重的特征值。  
![image](https://user-images.githubusercontent.com/57103853/199525304-08909575-d78e-4945-86e6-fef7feaaacf9.png)
2. 根据权重排名，取出权重比较大的特征。
![image](https://user-images.githubusercontent.com/57103853/199525411-32ed625e-6c18-4c04-b78e-a5586b93ae8f.png)  
![image](https://user-images.githubusercontent.com/57103853/199525435-e26446f2-ceee-45e6-ad8f-8c3c021113bf.png)  
![image](https://user-images.githubusercontent.com/57103853/199525453-5412af99-7a88-4757-8135-746bc212c5d7.png)
![image](https://user-images.githubusercontent.com/57103853/199525481-44d7717d-d165-4793-8e6c-34b567a801e7.png)
3. 绘制云词图可视化展示。  
![image](https://user-images.githubusercontent.com/57103853/199525535-2f329447-c1d0-4199-9663-9d8a77d928dc.png)  
![image](https://user-images.githubusercontent.com/57103853/199525560-0870dda0-bd55-44c3-9292-0643e617a366.png)
4. 生成共现矩阵，利用共现矩阵查看具体一个词说了什么，大约1200-300，重要性高的词调高end。  
![image](https://user-images.githubusercontent.com/57103853/199525599-2e0a7aa8-24a9-449b-b4fd-f96c7c63ecfd.png)
![image](https://user-images.githubusercontent.com/57103853/199525630-b86ff050-c6fe-459d-9b7c-a17fc573f2de.png)  
![image](https://user-images.githubusercontent.com/57103853/199525658-9c7f6282-1aa8-4fed-9680-a740c2f486c8.png)
5. 可以得到以下结论，顾客对以下方面比较满意/在意：宝宝喜欢，老人喜欢（少部分奶粉是老人奶粉）；味道、口感、接近母乳；营养配方，身体免疫力，清火，消化吸收；正品；包装；物流速度，快递员服务（顾客喜欢下单第二天就送达）；活动力度，价格，性价比，比超市便宜；大品牌；溶解性好，结块不严重；日期新鲜；自营；满意度：飞鹤>伊利>安佳>蒙牛(>德运)；满意度：脱脂>全脂；满意度：进口>国产；独立小包装；可以和麦片搭配销售。  
顾客对以下方面比较不满意/在意，差评数据量不太大，很多都是上面好评原因的对立面，下面为几个主要原因：客服服务，售后；勺子；奶粉的质量、口味；买完就降价；没送赠品；保质期；结块；包装问题，快递问题；不满意：飞鹤>伊利；假货，不是原装进口。
## 7.利用文本挖掘相关算法找到平台中用户讨论的集中点
1. 利用奇异值分解对评论主题分析。  
![image](https://user-images.githubusercontent.com/57103853/199525761-3b59bb4a-9cff-4ada-be2c-f7f684d97a3f.png)  
![image](https://user-images.githubusercontent.com/57103853/199525792-431f08fb-b49d-4c50-90ca-765838b13d78.png)  
![image](https://user-images.githubusercontent.com/57103853/199525835-6ed34836-b1e8-4e30-b32a-9d985cecb9b3.png)  
![image](https://user-images.githubusercontent.com/57103853/199525859-0f079e51-309b-4571-b8dd-35c428e2ddfa.png)
2. 好评的主要主题为：牛奶的配方有营养，味道好；牛奶便于吸收，溶解性好，清火，有营养；口感好，香味十足；接近母乳，更喜欢飞鹤；物流快，包装好。分布为主题一最多。    
![image](https://user-images.githubusercontent.com/57103853/199525947-21842ce4-b4f6-4908-9a24-f9e5507bf9a8.png)
3. 差评的主要主题为：味道不好，客服不满意，包装不好，勺子不好；客服不好，买完就降价，包装不好，赠品不好；勺子不好，客服不满意，买完就降价，快递不配送，少赠品。分布为主题一最多。  
![image](https://user-images.githubusercontent.com/57103853/199526041-a2b4c263-0a94-49a0-9d54-35eef15f3af7.png)
